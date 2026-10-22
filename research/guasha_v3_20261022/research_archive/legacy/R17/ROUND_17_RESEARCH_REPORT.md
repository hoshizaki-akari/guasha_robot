# FAIRINO FR5 刮痧机器人博士级算法研究｜第十七轮研究报告 R17.0

> **日期**：2026-10-18。**本轮定位**：公开真实 RGB-D 数据的**原始文件身份、跨目录谱系、训练/测试泄漏与注册代码审计**，而非宣称 FR5 实机定位进展。**实际执行**：完整读取 R16 报告、讲义及 ZIP 内决策追踪；通过只读 GitHub 接口读取固定提交的两个完整目录元数据（616+206 项）、7份关键文本源码、9份真实 PNG 的二进制内容并在连接器运行环境解析 PNG IHDR 与逐块 CRC；本地运行精确组合计数、分组切分和 38 项单元测试。**未完成**：这9张 PNG 在本地容器下载与像素级双解码、原始→配准成品的逐像素重生成、独立三维几何真值、FR5 采集/力控/人体试验。所有百分比是**数据划分风险的数学概率**，不是机器人误差、算法精度或已发生泄漏的证明。
>
> **证据等级**：`I-F` 本地前轮全文；`G-M` 固定 Git 提交的完整目录元数据；`G-B` 连接器实际取得二进制字节并做 PNG 结构/CRC 审计（未解压像素）；`G-S` 远端完整文本源码核读；`D-F` 官方相关技术条目正文；`P-A` 仅论文摘要/出版信息；`C-R` 本地确实运行的组合数学/代码；`U` 尚缺独立实体证据。**没有将摘要称为全文**。

## 0. 执行摘要：本轮为什么改变优先级

R16 已在本地真实 Middlebury 静态双目上证明“像素级拒绝门改善错误率并不自动给出完整路径”。R17 原计划直接取得 TUM Scribble 原始 PNG 与独立靶标。**新证据表明，在比较深度补洞和重观测算法之前，还必须先控制数据集内重复采集造成的训练/测试泄漏，以及 RGB/深度注册成品的生成谱系。**这不是用数据治理替代机器人研究，而是防止下一步真实数据实验产生虚假的泛化结论。

对 [TUM RGB-D Scribble Benchmark](https://github.com/tum-vision/rgbd_scribble_benchmark) 的固定提交 `466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f` 实际审计发现：`LabeledImages` 含 154 组四模态文件（共616份）；`UnregisteredDepth` 有206份深度文件，其中52个文件名未出现在已标注组。`bedroom_12` 和 `bedroom_3` 的 RGB 与配准深度 **Git blob 完全相同**，但 scribbles 与语义 gt blob 不同。未注册深度中有**8组两两完全相同的文件**。这是**重复数据/多份标注**，不能未经调查断言标注错误或样本不独立的物理成因。

若**假设**对154个标注条目固定随机划分123训练、31测试，唯一已知完全重复的 RGB-D 对跨划分概率为 **32.3657%**；若对206个未注册文件固定随机划分165/41，8对完全重复文件至少一对跨划分的组合概率为 **95.7568%**。这些是“假设随机划分”的精确计算，不是数据集实际划分或模型已泄漏的实测结果。实际采用同一采集组原子划分的本地程序在1000个种子上均为零跨组泄漏（仅验证定义好的精确重复组）。

**路线修订**：`源文件与组身份资格 → RGB/深度角色和成品生成谱系 → 人工有序点选/实体画线 → 目标视角可见性与深度资格 → 同场景独立三维验证 → 实测刮板边缘/姿态 → 非人体厂家恒力强基线与双向卸载`。自动点位识别和复杂接触估计继续后置。

## 1. R16 报告、教学讲义与决策记录：逐项继承但允许推翻

实际读取 `/mnt/data/robotics_round16/ROUND_16_RESEARCH_REPORT.md`、`ROUND_16_TEACHING_GUIDE.md`，并从 R16 ZIP 提取 `DECISION_TRACE_R15_R16.md`。R16 的 `REAL_STEREO_PROXY`、`GT_UNKNOWN`、风险—覆盖率、固定路径、FR5 HOLD 和旧仓库禁止写入全部是起点而非不可挑战的结论。

| R16 决策组 | R17 新证据或反证 | R17 判断 |
|---|---|---|
| ADR-R16-01/02 真实代理层、无效真值独立 | 实际取得 TUM PNG 字节及 Git blob，但未解压原像素/获得深度真值 | **修订**：新增 `REAL_RGBD_BINARY_AUDITED`，不可升级 `REAL_RGBD_GEOMETRY_VERIFIED` |
| ADR-R16-03/04/05 简单基线与风险—覆盖 | 精确重复文件可污染任意算法的跨样本比较 | **保留并前置**：评估必须先通过组级划分资格 |
| ADR-R16-06 禁止 oracle 路径 | RGB-D 重复样本会使“盲测试”不盲 | **保留**，增加禁止按单文件名独立切分 |
| ADR-R16-07/08 TUM 生成谱系未决、冻结毫米精度 | 注册代码、README 和成品命名无法闭合生产链 | **保留未决**，不能宣称发布数据配准错误 |
| ADR-R16-09/10 人工双入口、实测刮板 | 本轮无实体画线/刮板轮廓 | **保留但未通过** |
| ADR-R16-11/12 官方恒力强基线、禁止旧仓库写入 | 无 FR5 台架、新仓库授权未确认 | **保留 HOLD/只读** |

## 2. 文献、官方接口、数据与源代码证据账本 L17-01—L17-16

| ID | 可复核来源 | 实际核读范围/证据级别 | 能支持与不能支持的判断 |
|---|---|---|---|
| **L17-01** | 本地 R16 报告、讲义、ZIP 决策记录 | `I-F` 全文 | 继承研究门槛；不是外部实测 |
| **L17-02** | [TUM Scribble README](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/README.org) | `G-S` 全部文本 | 官方称154组、640×480、深度毫米；`UnalignedDepth` 是文档称谓，仓库实为 `UnregisteredDepth` |
| **L17-03** | [LabeledImages](https://github.com/tum-vision/rgbd_scribble_benchmark/tree/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/LabeledImages) 与 [UnregisteredDepth](https://github.com/tum-vision/rgbd_scribble_benchmark/tree/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/UnregisteredDepth) | `G-M` 两个完整目录的文件名、字节数、blob SHA | 精确相同 blob 与目录计数；不能据此检测近似重复或物理重拍 |
| **L17-04** | 上述两目录的9份指定 PNG | `G-B` 实际取得文件 base64→二进制，IHDR/全部块 CRC | 640×480、16-bit 灰度深度/8-bit 索引笔迹；**没有**在本地解压像素 |
| **L17-05** | [registerDepth.cpp](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/Calibration/RegisterDepth/registerDepth.cpp) | `G-S` 完整源码 | 调用实参 `Color,IR,distColor,Rt` 与 API 参数角色有冲突；不能证明成品由此程序生成 |
| **L17-06** | [注册 README](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/Calibration/RegisterDepth/README.md) | `G-S` 全文 | 示例循环遍历 `LabeledImages/*depth.png`，输出 `_registered.png`；不是发布成品的可追溯运行日志 |
| **L17-07** | [calib_results.yml](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/Calibration/RegisterDepth/calib_results.yml) | `G-S` 全部参数 | Color/IR K 主点不同、Rt 含非零平移；尚无靶标外参独立验收 |
| **L17-08** | [export_stereo_calibration.m](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/Calibration/RegisterDepth/export_stereo_calibration.m) 和 [test_registration.m](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/466a42d4ec2df23cd9e4feb2824fa31e1eec6a7f/Calibration/RegisterDepth/test_registration.m) | `G-S` 两份全文 | 前者含绝对路径，后者引用未列入标注目录的 `livingroom_05`；可能为开发示例，不是生成证据 |
| **L17-09** | [OpenCV 4.13 `registerDepth`](https://docs.opencv.org/4.13.0/d2/d3a/group__rgbd.html) | `D-F` 相关完整函数签名与参数解释 | 第一个 K 是未注册深度相机，第二个是目标 RGB，相机间 Rt 方向有明确定义 |
| **L17-10** | [Git 对象存储官方说明](https://git.github.io/htmldocs/user-manual.html) | `D-F` Git blob 对象格式相关段落 | 同 blob SHA 是相同对象内容；**Git blob SHA 不是原文件 `sha256sum`** |
| **L17-11** | [Barz & Denzler, Do We Train on Test Data?, 2020](https://www.mdpi.com/2313-433X/6/6/41) | `P-A` 摘要与网页信息；正文页面限流未核读 | 图像重复污染评价已有研究，不能将去重称为本项目原创算法 |
| **L17-12** | [Kaufman et al., Leakage in Data Mining, 2012](https://doi.org/10.1145/2382577.2382579) | `P-A` 摘要与出版信息 | 数据泄漏的基本问题已有定义；未复现论文实验 |
| **L17-13** | [FAIRINO Python 力控手册 4.0.0](https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html) | `D-F` `FT_Control`、保护/找面相关条目 | 厂家恒力为强基线；手册参数不是人体安全阈值 |
| **L17-14** | [Middlebury 2014 / scikit-image Motorcycle](https://scikit-image.org/docs/stable/api/skimage.data.html#skimage.data.stereo_motorcycle) | R16 `I-F` + R16 本地真实像素已运行 | 保留 R16 单场景真实视差代理证据，不能代替 TUM 彩深验证 |
| **L17-15** | [NIST TN1297 合成标准不确定度](https://www.nist.gov/pml/nist-technical-note-1297/nist-tn-1297-5-combined-standard-uncertainty) | `D-F` 相关方法正文 | 共模标定/重复图像不应被当成独立误差样本 |
| **L17-16** | [2026 automotive dataset leakage study](https://research.chalmers.se/en/publication/553456) | `P-A` 摘要/研究页面，未核读全文 | 近重复与分组评估仍是当代问题；不能套用其车型数据的错误率 |

**比较与适用假设**：Git blob 去重对**字节完全相同**是确定性且廉价的；视觉感知哈希、特征向量与时空邻近聚类能扩大到近重复，但会引入阈值与误并组风险；仅按文件名随机切分假设每个文件独立，当前至少一个 RGB-D 反例已推翻该假设。独立仿体/独立测量仪才是**几何精度**评价的强证据，不能被 blob 审计替代。

## 3. 数据集对象定义与目录结构：什么是一个独立样本

定义 `FileObject=(path,git_blob,bytes,commit,modality)`，`CaptureGroup` 是由同一物理采集或字节一致的 RGB-D 所形成的**最小不可跨训练/测试边界的组**，`AnnotationVersion` 则允许同一采集具有多份不同笔迹/语义标注。`OrderedStroke` 仍须显式记录人工起点、终点、方向、分叉，不可由标注文件名猜测。**文件数、标注条目数、物理采集数、独立测试单元数绝不自动相等。**

| 实际目录/模态 | 文件数 | 已证实的精确重复 | 可解释范围 |
|---|---:|---|---|
| `LabeledImages/*_image.png` | 154 | `bedroom_12` 与 `bedroom_3` RGB blob 相同 | 至少1对 RGB 文件完全相同 |
| `LabeledImages/*_depth.png` | 154 | 同一对深度 blob 相同 | 至少1对配准深度文件完全相同 |
| `LabeledImages/*_scribbles.png` | 154 | 0 对完全相同 blob | **不能**推出不同标注都正确或都不同内容 |
| `LabeledImages/*_gt.png` | 154 | 0 对完全相同 blob | 这是语义标签，不是三维深度真值 |
| `UnregisteredDepth/*_depth.png` | 206 | 8 对完全相同 blob | 52个名称不在154组中；不代表52次独立采集 |

本轮按文件名交集确认154个已标注深度在未注册目录均有同名文件，且两目录同名深度的 blob **0 个相同**；这是两个不同版本的文件身份，**不证明**其中一个一定是正确配准。`LabeledImages` 的154组*4模态=616条元数据，且 `UnregisteredDepth` 共206条，均来自实际完整目录列表，而非抽样推断。

## 4. 真实二进制审计：已取得什么，仍没取得什么

通过只读 GitHub 连接器实际请求9份 PNG 的 base64 内容，解析为字节，检查 PNG 签名、每个 chunk 的 CRC32、IHDR 分辨率/位深/颜色类型。**9/9 PNG 结构和 CRC 检查通过**；3份已配准深度与3份未注册深度均为640×480、16-bit 灰度；3份 scribbles 为640×480、8-bit 索引颜色。这里的“通过”只证明**传输字节与文件结构一致**，不证明 RGB 对齐、深度物理正确、语义标签正确。

| 场景 | 配准深度 Git blob / 字节 | 未注册深度 Git blob / 字节 | Scribbles Git blob / 字节 | 本轮二进制资格 |
|---|---|---|---|---|
| `bedroom_12` | `c597834d…` / 166670 | `25877a84…` / 579731 | `517d8287…` / 3153 | IHDR/CRC 通过；无像素级重算 |
| `kitchen_22` | `1b41ecde…` / 206646 | `5dfcb610…` / 615968 | `82da80ad…` / 2700 | IHDR/CRC 通过；无像素级重算 |
| `livingroom_01` | `bf772b20…` / 129840 | `893f4f38…` / 465827 | `36d5052b…` / 2349 | IHDR/CRC 通过；无像素级重算 |

**证据跃迁与未过门**：R16 没有本地 TUM 原始 PNG；R17 已在连接器环境读取9份真实 PNG 字节和 CRC，故 `RAW_BYTES_REMOTE_VERIFIED=TRUE`。但本地容器对外下载失败，**没有**将这些文件保存成本地可独立运行的 PNG；`LOCAL_PIXEL_REDECODE=FALSE`，`REGISTERED_VS_UNREGISTERED_REPROJECTION=FALSE`。Git blob SHA 由 Git 对象头与文件内容共同计算；本地未对原始 PNG 另算 SHA256，不能把 blob SHA 冒称为 SHA256。

## 5. 注册生成链的三处未闭合：源码问题不等于数据集错误

实际核读 `registerDepth.cpp`，其调用是 `rgbd::registerDepth(cameraMatrixColor, cameraMatrixIR, distCoeffsColor, Rt, image, Size(640,480), registeredDepth, false)`。OpenCV 官方签名的第一 K 是**未注册深度相机**、第二 K 是**目标相机**；如果变量名与真实物理角色一致，这一调用与意图“IR深度→Color图”不符。但**代码是否用于发布文件、Rt方向是否由上游反向约定、变量名是否误标均未确认**。

实际核读注册 README：批量示例输入 `../../LabeledImages/*depth.png`，而仓库有 `UnregisteredDepth`；程序写入后缀 `_registered.png`，发布文件是 `_depth.png`。实际核读 `test_registration.m`：引用 `LabeledImages/livingroom_05_depth.png`，该名称不在154个已标注组中，但出现在未注册目录。`export_stereo_calibration.m` 含特定开发机器的绝对路径，导出键名与当前 YAML 不是可直接一键衔接的证据。**这些是生成谱系的缺口，不是发布 PNG 错误的证明。**

| 检查项 | 源码/目录观测 | 可证伪的解释 | 本轮结论 |
|---|---|---|---|
| 相机内参角色 | `Color, IR` vs API `Depth, Target` | 代码没被用、变量命名错、或注册目标不同 | `ROLE_UNRESOLVED` |
| 输入目录 | README 循环 `LabeledImages` | 示例代码针对中间文件，非生产脚本 | `INPUT_LINEAGE_UNRESOLVED` |
| 输出命名 | 代码 `_registered.png` vs 发布 `_depth.png` | 存在未归档重命名/处理步骤 | `OUTPUT_LINEAGE_UNRESOLVED` |
| 测试场景 | `livingroom_05` 不在标注目录 | 旧开发样例或未标注数据 | `TEST_FIXTURE_STALE_OR_EXTERNAL` |
| 标定导出 | 绝对路径与不同键名 | 开发期脚本而非当前配置生成器 | `CALIB_LINEAGE_UNRESOLVED` |

## 6. 组合数学：固定大小随机划分到底有多大泄漏风险

设总文件数为 `N`，其中 `m` 组两两完全相同、互不重叠的文件对，剩余 `N-2m` 为单文件；从中**等概率且固定大小**选出 `t` 个测试文件。若要求所有重复对都留在同一侧，则选择 `j` 对整体进入测试，再从单文件中选 `t-2j` 个。故：

**R17-01** `P(no_cross) = [Σ_{j=0}^{m} C(m,j) C(N-2m,t-2j)] / C(N,t)`，其中越界组合数取0。

**R17-02** `P(any_cross) = 1 - P(no_cross)`。

**R17-03** `E[cross_pairs] = m × 2t(N-t) / [N(N-1)]`。

**R17-04** 对唯一一对重复的154个标注条目，`P(cross) = 2×31×123/(154×153) = 0.3236567354`。

**R17-05** 对8对重复的206个未注册文件，`P(any_cross) = 0.9575677568`，`E[cross_pairs] = 2.5631067961`（测试41/训练165）。

| **假设划分协议** | 总数/重复对/测试数 | 至少一对跨训练测试的精确概率 | 预期跨界对数 |
|---|---|---:|---:|
| 标注 RGB-D 按文件名随机固定123/31 | `N=154,m=1,t=31` | **32.3657%** | 0.3237 |
| 未注册深度按文件名随机固定165/41 | `N=206,m=8,t=41` | **95.7568%** | 2.5631 |
| 以已知重复对为原子组再分配 | 相同 N、组不拆分 | **0%（按定义）** | 0 |

注意：概率是**事前随机划分风险**，不是“已运行训练泄漏率”；它只覆盖**字节完全一致**，不能检测视角相近/时间相近/重复采集；分组后0%仅对已知组成立。`bedroom_12`/`bedroom_3` 的两套标注不同，泄漏可能**高估**或**降低**算法表现，不能预设只会提升分数。

## 7. 实际运行的算法比较与反例：文件切分 vs 组切分

本地 `audit_round17.py` 使用冻结的连接器观测摘要 `source_observations.json` 计算精确组合概率，并实现**组原子分配**：先将同一已知 blob 对合并为一个 `CaptureGroup`，再按种子打乱组，整组进入训练或测试。对标注154条与未注册206条各测试1000个种子，**已知完全重复组跨界数均为0**；38/38项 Python 单元测试通过（含离线路径/非人体力台资格拒绝逻辑）。该程序**没有**重新从网络读取全部822条元数据，也没有运行任何视觉学习器；本地代码测试的对象是冻结观察摘要与数学逻辑。另用 `qualification_gate.py` 纯离线测试时间戳、标定、可见性、笔顺、独立参考、非人体力台前提的拒绝逻辑，**不会发送运动指令**。

| 方法 | 适用假设 | 已验证的性质 | 不能解决的问题 |
|---|---|---|---|
| `FILE_RANDOM_SPLIT` | 每个文件独立 | 精确泄漏概率可计算 | 实际存在相同 RGB-D blob |
| `BLOB_ATOMIC_SPLIT` | 完全相同 blob 必须同组 | 已知8对/1对1000种子零跨组 | 近重复、同次采集不同编码 |
| `RGBD_CAPTURE_GROUP_SPLIT` | RGB/深度成对 + 采集会话标识 | **设计协议，尚未在全量像素上验证** | 缺真实会话ID/时钟 |
| `SESSION_SUBJECT_DEVICE_HOLDOUT` | 跨会话/重装/仿体泛化是目标 | **预注册未来实验** | 当前 TUM 无 FR5/人体仿体会话 |
| `PERCEPTUAL_NEAR_DUP_CLUSTER` | 感知相似可提示同源 | **未运行**；只能作为人工复核线索 | 相似但独立的样本误并组 |

**反例**：同一 `bedroom_12`/`bedroom_3` RGB-D 图像被拆到训练和测试，即使语义 `gt` 不同，也无法把测试结果当作“完全未见过该物理图像”的泛化能力。反之，若仅看 `scribbles`/`gt` 的 blob SHA，它们没有重复，会错误地放行；因此**分组键必须优先使用 RGB-D 观测身份，而非标注身份**。

## 8. 注册误差与数据泄漏是两条不同的因果链

注册几何是 `K_D, K_C, T_CD, z_D, pixel` 的物理投影问题；泄漏是 `CaptureGroup, split, evaluation` 的统计独立性问题。前者错会产生位置偏差；后者错会产生过于乐观或不可解释的算法泛化结论。两者可同时发生，但不能相互替代。

**R17-06** `p_D = z_D K_D^{-1} [u_D,v_D,1]^T`（深度相机回投影）。

**R17-07** `p_C = R_CD p_D + t_CD ;  [u_C,v_C,1]^T ∼ K_C p_C`（到彩色相机；`∼` 为齐次比例）。

**R17-08** `Σ_{p_C} ≈ J_z Σ_z J_z^T + J_K Σ_K J_K^T + J_T Σ_T J_T^T`（局部一阶近似；若误差相关，必须加入交叉协方差项）。

即使 `P(any_cross)=0`，也不能推出 `Σ_{p_C}` 小；即使注册误差很小，也不能推出测试集独立。固定提交和 blob 可核对**文件身份**，不能证明时间戳、曝光或工具接触面正确。

## 9. 从 RGB-D 到人工有序三维轨迹的可执行资格

首阶段两个入口必须共享同一 `OrderedStroke` 结构：`MANUAL_CLICK` 明确人工像素、顺序和端点；`PHYSICAL_MARK` 必须识别**画在非人体仿体表面**的实际墨线，且由人确认分叉与方向。TUM scribbles 是数字标注，不能充当实体墨线实验。每个轨迹样本应带 `capture_group_id, source_blob, camera_frame_id, clock_domain, timestamp_ns, calibration_id, registration_lineage_id, depth_valid, target_visible, independent_gt_status, ordered_index`。

**R17-09** `p_B(s) = T_BE(t_s) T_EC p_C(s)`；其中 `T_BE` 为基座到法兰位姿，`T_EC` 为法兰到相机手眼外参，必须在同一时刻/时钟域有效。

**R17-10** `PASS_path = ∧_{i=1}^{n} (registered_i ∧ visible_i ∧ depth_valid_i ∧ calibrated_i ∧ ordered_i) ∧ gap_ok ∧ reference_qualified`；这是**保守研究资格逻辑**，不是医学安全认证。

| 路径状态 | 必要证据 | 不足时动作 |
|---|---|---|
| `RAW_BYTES_REMOTE_VERIFIED` | 原始字节/CRC/固定 Git 提交 | 允许离线数据治理，**不允许运动** |
| `PIXEL_REDECODED_LOCAL` | 本地双解码+SHA256+深度单位 | 失败 `DATA_BLOCKED` |
| `REGISTERED_TARGET_VERIFIED` | 同场景靶标/角色/外参/遮挡证据 | 失败 `REOBSERVE` |
| `ORDERED_3D_PATH_QUALIFIED` | 有序整段+独立几何误差/完整性 | 失败 `HOLD` |
| `TOOL_CONTACT_QUALIFIED` | 实测刮板有限边缘/姿态/撤退路径 | 失败 `HOLD` |
| `NONHUMAN_FORCE_BENCH_QUALIFIED` | FR5/独立力台/急停/审查 | 未通过禁止人体接触 |

## 10. 可复现数据接口：版本化、可拒绝、禁止混用证据层

建议只读离线 `DatasetRecordV17`、`RegistrationEvidenceV17`、`PathEvidenceV17` 和 `ForceEvidenceV17` 四类结构；接口必须包含单位、时钟域、坐标系、标定版本、数据证据级别、不可变来源标识和人工确认。`GitBlob` 只存**Git对象身份**，与 `SHA256File` 独立字段；`semantic_gt` 不得填进 `depth_gt`。遇到未定义相机角色、原始文件缺失、注册目标不明、整段路径缺深度或重复组跨测试边界，返回可审计错误而不是自动补洞或发送运动指令。

| 接口字段/枚举 | 单位或取值 | 不变量/拒绝条件 |
|---|---|---|
| `git_commit, git_blob, sha256_file` | 字符串，分别记录 | blob≠SHA256；未知不伪造 |
| `capture_group_id, annotation_version` | ID | 同组不可跨训练/测试；标注版本不当采集组 |
| `K_depth, K_rgb, T_rgb_depth` | px, SE(3) | 源/目标角色、方向、分辨率必须明确 |
| `depth_unit, depth_valid` | mm或m，布尔 | 0/NaN/Inf/饱和码统一处理但不可混淆 |
| `timestamp_ns, clock_domain, T_base_flange` | ns, ID, SE(3) | 未校时不能升为机器人轨迹 |
| `ordered_index, stroke_direction, branch_choice` | 序号/枚举 | 不可用像素图连通性代替人工笔顺 |
| `status, reason_code` | `HOLD/REOBSERVE/DATA_BLOCKED` | 资格失败禁止自动接触运动 |

## 11. 几何误差、相关性与统计验收：不能用文件数冒充样本数

若同一物理场景有多份标注或多帧近似重复，像素/文件不独立。统计比较的重抽样单元应是**采集会话/仿体/重新装夹批次**，而不是几百万像素。给定会话级误差 `e_j`，报告每组完整路径失败、P95、最大错误接受、重拍次数与人工确认耗时；再做组级配对置信区间。没有真实 FR5 会话时**不报告**其置信区间。

**R17-11** `n_eff ≈ n / [1+(k-1)ρ]`（等组大小 `k`、组内相关 `ρ` 的设计效应近似；并非本轮实测参数）。

**R17-12** `U_path = w_e E_path + w_r R_bad_accept + w_c C_reobserve + w_h C_human`，权重必须事先由工程风险与任务要求制定，不能在测试集上调到新方法胜出。

例如同一采集重复两次且组内相关趋近1，两个文件不能提供两份独立信息；即使准确率在两份文件上都很高，也不能声称跨场景泛化。`w_e` 是几何误差代价、`w_r` 错误接受风险代价、`w_c` 重观测成本、`w_h` 人工确认成本，当前都未标定。

## 12. 刮痧板接触几何与姿态规划：为何本轮仍冻结优化器

真实刮痧板的工作边缘是有限曲线 `r_G(a)` 而不是一个数学点；安装变换 `T_EG`、边缘厚度、圆角半径、装夹重复性、局部背部法向与可达性都需要物理测量。未获得这些量时，使用“固定 TCP 接触点”只能作为未来对照基线，不能代表真实板边缘覆盖。

| 方案 | 前提 | 评价/否决 |
|---|---|---|
| 固定 TCP + 固定姿态 | 已实测板边缘、曲面足够平缓 | 简单强基线；覆盖/刮板碰撞不合格则否决 |
| 有限边缘逐点姿态 | `r_G(a)`、法向、边缘切向可测 | 比较接触覆盖与姿态变化、撤退可达性 |
| 接触几何优化器 | 独立仿体真值、装夹重测、固定基线不足 | 留出批次有净收益才进入，否则冻结 |

本轮不设任何人体接触力/板角度数值。第一个可做的物理工作是**卡尺/扫描刮板工作边缘和装夹重复性**，而不是直接优化人体轨迹。

## 13. FR5 六维力觉与工作程/返程策略：厂家接口是强基线而非实测

[FAIRINO 官方 Python 力控手册](https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html) 提供 `FT_Control`、力觉读取/零点与保护相关接口；这只说明 API 存在，**没有**说明本轮已调用它。设计上的安全顺序是：`NO_CONTACT → APPROACH_NONHUMAN → WORK_FORCE → UNLOAD_VERIFY → RETRACT → RETURN_CLEAR`；“返程轻力”只允许作为**非人体台架对照分支**，不默认比离面返程安全。实际接触前必须完成独立力台、急停、速度/力上限风险审查、失联停机、采样延迟测量；任何失败直接 HOLD。

| 状态/候选控制 | 进入条件 | 失败/退出条件 |
|---|---|---|
| `WORK_FORCE` 厂家 `FT_Control` | 非人体仿体、独立测力、硬件保护已审查 | 超限/丢帧/姿态无资格立即停机 |
| `UNLOAD_VERIFY` | 工作程结束且力觉有效 | 未证实卸载则禁止换向 |
| `RETURN_CLEAR` 离面返程 | 卸载、撤退空间可达 | 碰撞风险则停机 |
| `RETURN_LIGHT` 轻力返程对照 | **仅非人体**、独立审查/预注册 | 力峰/接触丢失劣于离面则淘汰 |

## 14. 可证伪假设 H17-01—H17-10：本轮逐项判断

| 假设 ID | 命题 | 证据与判定 |
|---|---|---|
| **H17-01** | TUM 已标注154组对应154个字节独立的 RGB-D 观测 | **推翻**：`bedroom_12`/`bedroom_3` RGB和深度 blob相同 |
| **H17-02** | 同一 RGB-D 的不同标注可用标注 blob 区分独立采集 | **推翻**：该对 scribbles/gt blob 不同而观测相同 |
| **H17-03** | 未注册目录恰好有154个文件 | **推翻**：实际206，其中52个未标注名称 |
| **H17-04** | 按文件名固定大小随机切分不会出现完全重复跨界 | **推翻为普遍保证**：组合概率32.37%/95.76% |
| **H17-05** | 已知完全重复组原子切分可消除这些组的跨界 | **组件级保留**：1000种子零跨组；不覆盖近重复 |
| **H17-06** | TUM 九份选定 PNG 无法在任何环境取得真实字节 | **推翻绝对说法**：连接器读入9份并CRC；本地仍不可下载 |
| **H17-07** | 发布配准深度肯定由当前 `registerDepth.cpp` 直接生成 | **未证实**：目录、输出名、角色未闭合 |
| **H17-08** | 源码角色冲突足以证明发布数据错误 | **拒绝推断**：缺生产日志/独立靶标 |
| **H17-09** | 通过文件身份审计即可证明毫米级 FR5 背部轨迹 | **推翻逻辑蕴含**：缺几何/时空/工具真值 |
| **H17-10** | 可以在无硬件控制权限下完成只读数据审计 | **保留组件级**：23项测试；非实体安全认证 |

## 15. 研究决策 ADR-R17-01—ADR-R17-12：保留、修订、推翻与逆转条件

| 决策 ID | 本轮动作 | 将来改变决策的证据 |
|---|---|---|
| **ADR-R17-01** | **新增** 数据集 `capture_group_id` 与 `annotation_version` 分离 | 物理采集日志证明更细粒度独立性 |
| **ADR-R17-02** | **新增** 精确重复 blob 必须原子分组，禁止文件级随机切分用于泛化宣称 | 组级外推目标改变且风险论证通过 |
| **ADR-R17-03** | **新增** 近重复只列为未完成审计，不宣称0近重复 | 完整图像可下载后跨编码相似度+人工复核 |
| **ADR-R17-04** | **修订** R16 的 TUM 证据：9份原始 PNG 远端字节/CRC通过，**本地像素仍未通过** | 本地双解码、文件 SHA256、像素一致通过 |
| **ADR-R17-05** | **保留** 注册内参角色冲突为疑点，不判发布数据错误 | 成品生成日志/原始→成品重生成/独立靶标 |
| **ADR-R17-06** | **新增** 数据集分组与注册谱系为三维轨迹实验前置门 | 另一个有独立会话ID/配准谱系的数据集可替代 |
| **ADR-R17-07** | **保留** 人工点选/实体墨线先行，自动识别后置 | 双入口独立物理实验通过后再启动 |
| **ADR-R17-08** | **保留** 整段路径资格/失败拒绝，禁止仅凭 P95 放行 | 风险审查证明更强替代指标 |
| **ADR-R17-09** | **冻结** 有限刮板复杂优化器，先测真实边缘 | 实测边缘+盲留出证明固定姿态基线不足 |
| **ADR-R17-10** | **保留** 厂家 `FT_Control` 为强基线、卸载后离面返程优先 | 非人体独立测力证据支持更优策略 |
| **ADR-R17-11** | **禁止** 将随机切分泄漏概率当实际模型误差或实机精度 | 只有实际模型/实体数据才可报告对应性能 |
| **ADR-R17-12** | **禁止** 写旧 `robot`；新仓库存在与权限未获明确确认，不提交 | 明确新仓库标识、存在且具有授权后仅写新仓库 |

## 16. 第十八轮工作包 WP18-A—WP18-F：预注册进入/退出/止损

| 工作包 | 可证伪实验/对照 | 进入与退出条件；失败动作 |
|---|---|---|
| **WP18-A 本地 TUM 原图与去重** | 固定 commit 下载9 PNG+全目录，SHA256、PIL/OpenCV 双解码、RGB-D blob/近重复聚类 | 进入：合法获取原始字节；退出：全量像素可本地复算、组级切分审计；否则 `DATA_BLOCKED` |
| **WP18-B 注册成品谱系与同场景靶标** | 未注册→目标 RGB 角色两种假设、Rt方向、z-buffer、原成品差异；独立标定靶标 | 进入：K/Rt/原始深度/目标图像及版本；退出：能解释成品生成链且独立几何误差满足预注册预算；否则 `HOLD` |
| **WP18-C 非人体实体墨线与人工点选** | 曲面仿体两入口、有序端点/分叉盲测、重观测成本；固定点深度强基线 | 进入：B通过；退出：整段有序三维误差/完整率/人工成本通过；否则 `REOBSERVE` |
| **WP18-D 真实刮板边缘与姿态** | 实测 `r_G(a),T_EG`、重装批次；固定姿态 vs 有限边缘姿态 | 进入：工具/曲面可计量；退出：覆盖/可达/撤退及误差预算通过；否则冻结优化 |
| **WP18-E FR5 非人体恒力台架** | 独立测力、时戳/零点/保护，厂家 `FT_Control` 工作程/卸载/离面返程 vs 轻力返程 | 进入：硬件急停、风险审查、独立限力；退出：预注册力峰/失联/换向指标通过；否则停机 |
| **WP18-F 博士创新与跨域盲测** | 按仿体/重装/会话分组留出；简单拒绝重拍 vs 复杂模型；统计组级 CI | 进入：A—C 证据门通过；退出：盲留出净收益+成本/安全可解释，否则停止复杂化 |

**进入/退出数字**必须在拿到真实独立测试集前由仪器不确定度、风险分析、任务需求共同制定；不能把 R16 的 `2px`、本轮的 `32.37%` 或厂家示例力值当成人体允许阈值。失败不是论文“负结果”被隐藏，而应在 `HOLD/REOBSERVE/DATA_BLOCKED` 中显式记录。

## 17. 可复现材料、验收、GitHub 与最终限制

**已运行**：只读 GitHub 固定提交完整目录枚举（616+206元数据），核读注册/标定源码；连接器实际取得9份 PNG 二进制并完成签名/IHDR/全部块 CRC 检查；本地 `audit_round17.py` 精确组合计数、已知组原子分配（2种人口各1000种子）、`python -m unittest -v test_round17 test_gate` **38/38通过**。`source_observations.json` 是**从连接器观测人工冻结的摘要**，不是本地自动抓取的完整822条 manifest；本地可重算的是数学与组级软件逻辑。研究包包含未来可联网的原始 GitHub URL 与固定提交，**不重新分发第三方 PNG**。此前 R16 的 Middlebury 本地像素验证继续有效，但没有在本轮冒称为 TUM 像素验证。

**仍未通过**：TUM PNG 本地字节/像素双解码、生产注册重生成、独立同场景几何真值、真实实体墨线、FR5 RGB-D/位姿/六维力原始日志、实测刮板、非人体力台与任何人体安全/伦理审批。`9/9 PNG CRC` 只属于文件完整性；`38/38测试` 只属于离线软件；不能外推为毫米级三维轨迹或人体安全。

**GitHub**：只读研究数据来自公开 TUM 仓库，不是用户要求的新研究仓库。上一轮仅能看到旧 `robot` 等仓库；本轮仍无**明确的新独立研究仓库标识及授权**，所以没有 GitHub 写入、没有触碰旧 `robot`。双文档 0—17 章、L17-01…16、R17-01…12、所有原表、H17-01…10、ADR-R17-01…12、WP18-A…F 由本地结构检查覆盖；结构覆盖不等于真实零基础学生理解测试。

**本轮判定**：**数据来源/精确重复风险与注册生成链审计有实际新证据，理论与离线软件工作完成；TUM 本地像素级几何与 FR5 实体任务未完成。** 本轮的博士研究价值是发现会使未来算法评价失真的可证伪前提，而非提出已验证的新刮痧控制算法。