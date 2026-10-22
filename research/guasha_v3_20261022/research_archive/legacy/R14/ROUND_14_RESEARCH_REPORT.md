# FAIRINO FR5 刮痧机器人博士级算法研究｜第十四轮研究报告 R14.0

> **执行日期**：2026-10-18；**研究阶段**：非人体、离线、源码—标定契约核验。**硬件目标**：FR5、Eye-in-Hand RGB-D、腕部六维力觉、有限宽度刮痧板。**先行任务**：人工点选和实体画线的有序三维曲面轨迹；自动点位识别后置。**核心发现**：在前轮反复统计数字 scribble 深度空洞的公开数据仓库中，深度配准示例的相机矩阵实参顺序与 OpenCV 官方 API 语义存在可复核的**条件性冲突**。这不证明数据集成品深度图有错，但足以使“注册目标坐标系已证实”的资格保持未通过。
>
> **证据分层**：`S-F` 公开完整源文件已由只读 GitHub 连接器取得并核对 Git blob SHA；`D-F` 官方 API/SDK 网页正文已核读；`P-A` 论文摘要/机构介绍；`I` R13 继承；`M` 解析推导；`X-L` 本地确定性数值/合成单元测试；`D` 尚未执行。**没有**下载真实 PNG 到本地，没有复现 OpenCV 二进制执行，没有测量真实 FR5、人体、仿体、刮板或力控性能。报告中的“12.18–12.49 像素”是**两个假定投影契约的计算差**，不是实测图像误差。

## 0. 执行摘要、证据强度与路线变化

R13 证明：图像数字标注上可达缺测点的 P95 跳数会受到四/八邻接和选择性分母影响；即使深度非零，也不能推断其三维正确。R14 没有重复制造缺测指标，而是核查更上游的问题：**深度值与人工指定的 RGB 像素，是否真的属于同一相机投影平面？**本轮实际取得 TUM RGB-D Scribble Benchmark 的注册示例 C++、校准 YAML、说明文件和 OpenCV 3.4 对应实现。官方接口要求先传深度相机内参，再传目标 RGB 内参；仓库示例以变量名判断却先传 `cameraMatrixColor`，后传 `cameraMatrixIR`。该不一致需用独立棋盘格/彩深边界测试判定是否为实际错误，不能仅靠变量名定罪。

基于仓库给出的两套 640×480 内参和变换，对 9 个**人为指定**的源像素—深度组合计算两种实参顺序的输出位置。结果两种投影差的范数为 **12.1777–12.4859 px**；它只说明接口错配在该条件下有可观测后果，**不能写成“数据集误差12像素”或“FR5误差若干毫米”**。仓库还保留不同用途/分辨率的 factory 与 MATLAB 标定，且官方说明曾对半分辨率标定作两倍缩放，说明“看到一个 K 就用”不是可接受的数据接口。

**本轮决策**：新增 `DepthRegistrationEvidenceV14`/`CalibrationProvenanceV14`，将 `source_camera`、`target_camera`、`K` 对应分辨率、`T_target_source`、深度尺度、算法版本和独立配准靶标验证纳入硬门；R13 的图距离统计仅保留为**源文件层的描述统计**，在未确认 RGB—深度配准前不能提升为三维轨迹可信证据。下一轮第一优先不再是新增图像缺口指标，而是**配准契约实测/复算、真实 PNG 本地落地和独立几何真值**。

## 1. 上轮文档、教学与决策的实际继承

已读取 `/mnt/data/robotics_round13/ROUND_13_RESEARCH_REPORT.md` 全部 0–17 章、`ROUND_13_TEACHING_GUIDE.md` 的结构及核心教学内容，并核查 `ROUND_13_REPRODUCIBLE_PACKAGE.zip` 清单。R13 的 `ADR-R13-01/04` 坚持 `OrderedStroke ≠ SupportGraph`；`ADR-R13-05/06` 保留直接深度+拒绝及独立真值；`ADR-R13-07/08` 把厂家恒力作为强基线并要求卸载后返程；`ADR-R13-10` 明确本地真实 PNG、实体线、工具与 FR5 闸门均未通过；`ADR-R13-11` 禁写旧 `robot` 仓库。

本轮**保留** R13 的三个真实场景缺测像素统计为连接器侧观测记录，不重新宣称本地复算。**修订**其证据使用范围：数字 scribble 与标称注册深度图在同一像素位置相交，最多证明该文件中有无深度数字；没有独立验证彩深对齐就不能称它们支持真实同一表面点。**推翻**任何隐含的“既然文件名为 depth，便可不带内参角色与注册版本直接回投影”工程假设。上轮 WP14-A（本地 PNG）、WP14-B（独立真值）、WP14-C（实体有序线）、WP14-D（工具测量）、WP14-E（FR5 台架）、WP14-F（创新止损）均未被本轮假冒完成。

| R13 继承事项 | R14 证据 | 判断 |
|---|---|---|
| 有序笔划不等于像素连通图 | 源码审计不改变用户笔顺的不可推断性 | **保留** |
| 非零深度≠可信三维点 | 新发现注册目标与内参角色也须核验 | **加强** |
| 直接深度+拒绝为强基线 | 仅在配准资格成立后才能进入几何基线比较 | **修订进入条件** |
| 真实 PNG 本地双实现 | 网络/原始文件仍未在本地取得 | **不通过** |
| 独立三维真值、实体线、刮板轮廓 | 没有新实物测量 | **不通过** |
| 官方恒力基线与离线 HOLD | 核读官方 SDK，未下发 FR5 指令 | **保留，实体未通过** |

## 2. 高相关文献、源代码、官方资料的阅读深度账本 L14-01—L14-15

本轮对**完整源文件**与**学术论文全文**严格分开：GitHub 的 `.cpp/.yml/.m` 是完整文件，不能称作“论文全文”；OpenCV 和厂家文档是官方 API 正文；摘要不能替代论文实验方法核读。2026-10-18 访问核验如下。

| ID | 来源与原始链接 | 本轮实际核验层级 | 直接证据/限制 |
|---|---|---|---|
| **L14-01** | [Hazirbas 等，TUM RGB-D Scribble Benchmark (2018)](https://github.com/tum-vision/rgbd_scribble_benchmark) | `D-F` 完整 README | 154 室内场景、640×480、深度以毫米存储；`gt` 是**语义分割标签**，不是独立激光深度真值 |
| **L14-02** | [注册示例 registerDepth.cpp](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/RegisterDepth/registerDepth.cpp) | `S-F` 全文件，Git blob `d303ce96a28adb8a462deb5074a4c664dcdfb8a0` | 调用的前两项变量依次为 `cameraMatrixColor`、`cameraMatrixIR`；不等于证明发布的 PNG 用此代码生成 |
| **L14-03** | [注册用 calib_results.yml](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/RegisterDepth/calib_results.yml) | `S-F` 全文件，blob `6bcae19efb5a75b761dc947a3886477b3fa1f26e` | RGB 主点(319.5,239.5)、IR 主点(314.5,235.5)、共同焦距574.0528 px、Rt 平移米 |
| **L14-04** | [注册工具 README](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/RegisterDepth/README.md) | `S-F` 全文件，blob `11091ccdfdeff7276f9570c768dfa9bcc3579e1a` | 工具宣称用于深度配准并写出 `_registered.png`；未证明其参与现有标注文件生产 |
| **L14-05** | [Calibration/README.md](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/README.md) | `S-F` 全文件，blob `8a443a0634b0c4526af1a43c7f3478acfe19c0cd` | 作者明确说明标定图曾用一半分辨率，相关分辨率量作×2；不能无条件使用半分辨率 K |
| **L14-06** | [factory_calibration.yml](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/factory_calibration.yml) | `S-F` 全文件，blob `76f0e47624598d24d21b16842ecd931dc34d6ef7` | M1/M2 焦距约285.94 px，角色、尺度和使用链须单独核对 |
| **L14-07** | [matlab_calibration.yml](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/matlab_calibration.yml) | `S-F` 全文件，blob `d2cb023ca83993895558db31fa842160d7d1da30` | M1/M2 焦距约546/580 px；不可和注册用 K 当作同一标定直接混合 |
| **L14-08** | [test_registration.m](https://github.com/tum-vision/rgbd_scribble_benchmark/blob/master/Calibration/RegisterDepth/test_registration.m) | `S-F` 全文件，blob `71c6bd9b15bdab45a9deb67ce952de485ff8b8c9` | 示例把深度按自身极值归一化到[0,1]后画散点，**展示图不是米制点云精度验证** |
| **L14-09** | [OpenCV 3.4 RGB-D registerDepth 官方 API](https://docs.opencv.org/3.4.20/d2/d3a/group__rgbd.html) | `D-F` 完整函数签名与参数定义 | 第1项深度 K，第2项目标 RGB K，第3项目标畸变；Rt 为深度→目标；官方文档直接支持接口契约 |
| **L14-10** | [OpenCV contrib 3.4 depth_registration.cpp](https://github.com/opencv/opencv_contrib/blob/3.4/modules/rgbd/src/depth_registration.cpp) | `S-F` 全文件取得、重点核读注册实现，blob `4cf13e3c89d2cf7997f0bba50884f5b905cb5c54` | `CV_16U` 深度内部×0.001 转米；不能再无依据怀疑示例缺少毫米→米转换 |
| **L14-11** | [iBims-1 官方介绍](https://www.asg.ed.tum.de/en/lmf/ibims1/) | `D-F` 官方介绍正文 | 独立激光扫描深度真值可用于边界/平面评价；**未下载数据，也不能和 TUM Scribble 不同场景逐像素比较** |
| **L14-12** | [Koch 等，Evaluation of CNN-based Single-Image Depth Estimation Methods (2018)](https://arxiv.org/abs/1805.01328) | `P-A/I` 摘要与前轮方法记录，**本轮未通读论文全文** | 已有平面、边界和深度误差评测；不能宣称原创评价指标 |
| **L14-13** | [FAIRINO Python Force Control SDK 4.0.0](https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html) | `D-F` 本轮核读 12.1–12.12、12.19–12.22 | `FT_Control`、力觉原始/参考系、零点、负载、`FT_Guard`；接口不代表这台 FR5 已验证 |
| **L14-14** | [NIST TN1297 Combined Standard Uncertainty](https://www.nist.gov/pml/nist-technical-note-1297/nist-tn-1297-5-combined-standard-uncertainty) | `D-F` 官方方法页 | 不同来源与相关项要进入不确定度预算；没有 FR5 的实际协方差 |
| **L14-15** | [Franaszek & Cheok，局部手眼标定 (2020)](https://www.nist.gov/publications/using-locally-adjustable-hand-eye-calibrations-reduce-robot-localization-error) | `P-A` NIST 机构摘要，**未读全文** | 机器人局部手眼误差补偿已有研究，不能将“校准很重要”当博士原创 |

## 3. 研究问题、可辨识对象与防止偷换概念

`Q14-A`：公开注册代码的相机角色/实参顺序是否与 OpenCV 官方契约一致？**可回答：在源码变量语义下不一致。**`Q14-B`：在作者提供的 K/Rt 下，顺序互换能否导致非零、可计算的输出像素差？**可回答：能，纯投影计算。**`Q14-C`：现有 `LabeledImages/*_depth.png` 是否真的由该示例生成、是否发生实际错配？**不能回答：没有生产日志、源图—成品对照和独立靶标。**`Q14-D`：人工点选/实体线能否达到准确三维轨迹？**不能回答：没有本机图像、真值、实物笔顺。**`Q14-E`：FR5 双向力控是否达标？**不能回答：只有官方接口，未运行台架。

**观察层级**：`source-contract`（源码 API 顺序）≠`projected-sensitivity`（人为 3D 点计算）≠`registered-file-quality`（真实图像误差）≠`surface-path-quality`（真实三维曲面轨迹）≠`contact-safety`（真实工具与人体安全）。不能把前两层当后面三层。R13 三个场景 1482/1003/803 个数字标注像素及缺深度统计为**先前连接器侧结果**，本轮没有重新打开这些 PNG，故本轮不新增真实像素指标。

## 4. 源码审计：接口角色冲突、文件用途与最小反例

OpenCV `rgbd::registerDepth` 的文档参数顺序是 `(unregisteredCameraMatrix, registeredCameraMatrix, registeredDistCoeffs, Rt, unregisteredDepth, outputSize, ...)`。`unregistered` 对应**深度 IR 相机**，`registered` 对应**目标 RGB 相机**。TUM 示例中的两内参变量分别命名 `cameraMatrixColor`、`cameraMatrixIR`，调用时却按 **Color→IR** 排列。注册配置的两个主点差为 `(5,4)` 像素，焦距相同。若变量名与数据流角色相符，调用契约就被交换；若变量命名或成品数据流另有约定，则需要实际靶标复核，**不能从文字推定发布数据错误**。

注册工具说明写明产生新的 `_registered.png` 文件，根 README 列出 `LabeledImages`、`UnregisteredDepth` 两类数据；但目前没有建立从某个源 PNG 到某个发布 PNG 的哈希级**生成谱系**。`test_registration.m` 将深度归一化至 `[0,1]` 作散点显示，不能用其散点图当米制验证。另有 `factory_calibration.yml`、`matlab_calibration.yml` 与 `RegisterDepth/calib_results.yml` 三类标定；不能以名称相似就混用。`Calibration/README.md` 对标定图片半分辨率的提醒是直接证据，不是本轮推测。

| 检查点 | 观察到的完整源文件事实 | 尚缺证据 |
|---|---|---|
| 注册函数前两参数 | OpenCV 深度 K→目标 K；示例 Color→IR | 彩深靶标和代码执行结果 |
| 目标相机坐标系 | 文档要求目标 RGB，示例输出文件名 `_registered` | 该文件实际被哪个后续流程消费 |
| 深度单位 | 源深度 `uint16` 毫米；OpenCV 3.4 对 `CV_16U` 内部乘 0.001 | 真实输入 PNG 编码、运行版本与输出复核 |
| 标定分辨率 | 某些 factory K 焦距约286 px；另有 full-res K 约574 px | M1/M2 与 Color/IR 的实际角色映射 |
| 真实三维参考 | iBims 官方有激光真值，但与此数据集不是同场景 | 同场景独立测量/已注册彩深靶标 |

## 5. 数学模型、坐标变换与公式 R14-1—R14-4

采用列向量、右乘齐次像素。`D` 是深度相机坐标系，`C` 是 RGB 相机坐标系，`B` 是机器人基座，`E` 是末端。已知深度相机内参 `K_D`、RGB 内参 `K_C`、深度像素 `q_D=(u_D,v_D,1)^T`、**米制**深度 `z_D`、变换 `T_CD=(R_CD,t_CD)`，无畸变近似为：

\[
\mathbf p_D=z_DK_D^{-1}\mathbf q_D,\qquad \mathbf p_C=R_{CD}\mathbf p_D+\mathbf t_{CD}.\tag{R14-1}
\]

\[
\pi(K,\mathbf p)=\left(\frac{(K\mathbf p)_x}{(K\mathbf p)_z},\frac{(K\mathbf p)_y}{(K\mathbf p)_z}\right),\qquad \mathbf q_C=\pi(K_C,R_{CD}z_DK_D^{-1}\mathbf q_D+\mathbf t_{CD}).\tag{R14-2}
\]

把示例调用解释为**角色互换**的纯数学替代，保持同一 `Rt`，计算 `q_C^{swap}=π(K_D,R_CD z K_C^{-1}q_D+t_CD)`，对比正确角色 `q_C^{ref}`：

\[
\Delta_{uv}=\mathbf q_C^{swap}-\mathbf q_C^{ref},\quad d_{px}=\|\Delta_{uv}\|_2.\tag{R14-3}
\]

将 RGB 平面已配准深度送到 Eye-in-Hand 世界坐标，必须同时确认 `T_BE(t)`、`T_EC`、时间戳与深度代表的**目标坐标系**：

\[
\mathbf p_B(t)=T_{BE}(t)T_{EC}\begin{bmatrix}z_C K_C^{-1}\mathbf q_C\\1\end{bmatrix},\quad z_C\text{ 必须为 RGB 坐标系轴向深度。}\tag{R14-4}
\]

**特别注意**：对源 IR 深度进行 `T_CD` 后，RGB 轴向深度应是 `p_C,z`，而不是机械复用 `z_D`；工具工作边缘的几何资格仍需在 RGB/基座坐标一致的真实曲面上计算。畸变、遮挡、重采样和软组织运动未在这些简式中建模。

## 6. 本轮实际运行的九点投影敏感性审计

本地运行 `registration_contract_audit.py`（NumPy、**不调用 OpenCV 或机器人**），直接使用 L14-03 的 `K_C/K_D/Rt` 数值。深度取**人为设定** 0.5、0.7、1.0 m，IR 像素取中心 `(314.5,235.5)`、左上 `(100,100)`、右下 `(540,380)`；每个深度与三个像素组合，共 **9 个确定性案例**。两种契约使用同一 Rt，仅交换 K 的角色，计算公式 R14-1—3。

| 假定源深度 z (m) | 3 个测试像素的 `d_px` 范围 | 对象与解释 |
|---:|---:|---|
| 0.5 | 12.1777–12.1898 px | 角色互换的数学投影差；不是测量误差 |
| 0.7 | 12.3426–12.3554 px | 角色互换的数学投影差；不是配准性能 |
| 1.0 | 12.4725–12.4859 px | 角色互换的数学投影差；不是 FR5 精度 |
| **合计 9 案例** | **最小12.1777 / 中位12.3488 / 最大12.4859 px** | 由所给 K/Rt 和人工测试点确定；不可对真实场景作统计推广 |

一个有用的**解析极限反例**：令 `R=I,t=0`，由于两 K 焦距相同且主点差为 `(5,4)`，正确映射相对输入为 `(+5,+4)`，互换为 `(-5,-4)`，两者相差 **(10,8) px**，范数 `√164≈12.8062 px`。这表明接口角色不是可以忽略的标签。真实 Rt 的非零平移/旋转使九点差落在上表区间。`Rt` 的 3×3 旋转块因 YAML 数值截断，`det(R)=1.00006264`、`||RᵀR−I||_F=8.86×10⁻⁵`；这是一项文件数值一致性观察，不足以断言实际外参质量。

**审计不是误差实验**：未使用真实像素对应、未使用 OpenCV 的遮挡/离散栅格、没有靶标真值；不报告成功率、置信区间或毫米级 FR5 定位。源码和投影结果之间的条件依赖必须完整保留。

## 7. 标定分辨率、内参版本与尺度陷阱 R14-5—R14-6

源仓库 `Calibration/README.md` 指出标定图分辨率只有 RGB-D 图像的一半，因而对分辨率相关量作两倍处理。对于**无裁剪、仅整倍数缩放且采用一致像素中心约定**的理想图像，尺度 `s` 对内参的变换是：

\[
K'=\operatorname{diag}(s,s,1)K,\quad f'_x=sf_x,\ f'_y=sf_y,\ c'_x=sc_x,\ c'_y=sc_y.\tag{R14-5}
\]

严格像素中心 `(u+0.5)` 约定可能需要额外半像素平移，且裁剪/畸变/对齐改变时不能只乘 2。`factory_calibration.yml` 中 M1 焦距约 285.944 px；`RegisterDepth/calib_results.yml` 的 RGB/IR 焦距约 574.053 px；`matlab_calibration.yml` 的 M1/M2 焦距约 546/580 px。**M1/M2 与 RGB/IR 角色并无在本轮建立的同一标定等价关系**，所以数值不同不是“哪个更准确”的证据。

条件性演示：固定假定 `z=0.7m`、同一 RGB 像素 `(319.5,239.5)`，分别用注册 RGB K、MATLAB M1 K、未缩放 factory M1 K、简单×2 factory M1 K 回投影；与注册 RGB K 的 `x/y` 差分别约为 `(-6.196,+2.670)mm`、`(+392.484,+281.460)mm`、`(+1.411,-11.692)mm`。**这是不允许混用 K 的反事实数值演示，绝不是标定实测误差**；即便×2仍有差异，说明不能把半分辨率处理等同于所有版本同一化。

\[
\delta\mathbf p\approx J_{uv}\delta(uv)+J_z\delta z+J_K\delta\boldsymbol\kappa+J_T\delta\boldsymbol\xi,\quad\Sigma_p\approx J\Sigma J^\top.\tag{R14-6}
\]

`κ` 是内参参数向量，`ξ` 是外参扰动；若版本错配是**共同系统偏差**，重复观测不会自动把它平均掉。若 `K` 的像素坐标平面与深度值所在坐标系不同，公式 R14-4 也无法通过平滑补救。

## 8. 深度单位、遮挡、离散注册与结果资格

本轮进一步核读 OpenCV contrib 3.4 的 `depth_registration.cpp` 实现：对 `CV_16U` 输入，注册内部按 **0.001 m/整数单位** 处理；对 `CV_32F` 则按 1.0 m/单位。故**推翻**“示例未显式 `/1000` 必然导致 1000 倍错位”的猜测；在该实现与 `uint16` 前提下，库内部已经转换。另一个 `test_registration.m` 先除1000，再把深度按该幅图最小最大值缩放到 `[0,1]`，因此其点云散点图**不具备米制误差评价资格**。

深度配准还涉及 `p_C,z`、深度遮挡的前后关系、落入输出像素的离散取整、无数据标记和边界混合；`depthDilation=false` 只是示例选项，不是“所有洞都真实”的保证。要验证真实图，必须保存未注册 IR 深度、同步 RGB、目标彩深棋盘格/边缘、两种注册输出、源代码版本和相机标定版本，进行同一目标平面误差与遮挡误差比较。

| 候选“正确性”检查 | 能证明什么 | 不能证明什么 |
|---|---|---|
| Git blob SHA 一致 | 取到的文件版本与清单相同 | 文件数值正确或配准正确 |
| `uint16` × 0.001 | 与 OpenCV 3.4 分支单位契约一致 | 深度尺度已经被独立计量校准 |
| 公式投影差非零 | 参数角色交换具有可计算后果 | 发布的成品 PNG 确实错配 |
| RGB/IR 靶标对齐 + 独立测量 | 同场景坐标系与几何残差可检验 | 人体接触压力和安全性 |
| 力控 SDK 接口存在 | 有可作为基线的函数 | FR5 当前固件性能、接触安全 |

## 9. 方法比较：源契约检查、真实靶标、重观测、补洞与复杂优化

| 方法 | 关键假设 | 优点 | 主要失败模式/当前资格 |
|---|---|---|---|
| **B14-0 文件名直接采深度** | RGB 与 depth 同像素即同表面 | 快速但不可追溯 | **撤销放行资格**；没有注册目标证据 |
| **B14-1 角色/单位/分辨率静态契约审计** | 变量名与 API 角色能追溯 | 本轮实际执行；低成本、可拒绝明显矛盾 | 不能知道生产数据是否用了该代码；只作硬门前置 |
| **B14-2 正/反实参顺序投影比较** | 已知 K、Rt、深度尺度 | 9 点数学敏感性已计算 | 无真实真值、栅格和遮挡；不能排名真实算法 |
| **B14-3 独立彩深靶标/激光仿体** | 目标与源同时可见，独立测量可追溯 | 能判定角色与真实 3D 误差 | 本轮未获得，下一轮**最高优先** |
| **B14-4 有序线重观测** | 可重新拍摄且坐标/软组织可配准 | 比盲补洞更易审计 | 没有实物数据；只在 B14-3 后评价 |
| **B14-5 RGB 引导补洞/神经模型** | RGB 边缘对应几何、域迁移成立 | 可能提升覆盖 | 会制造自信但错误的点；继续冻结 |
| **B14-6 厂商 `FT_Control` 基线** | SDK/固件/传感器兼容且独立安全审查 | 已有功能，避免重做 PID | 未实测；非人体力控台架才可比较 |

算法排序不是按“代码复杂度”而是按**证据消除顺序**：首先排除数据契约错配，再量化同场景真值，再比较直接深度/重观测/补洞；第二部分刮板姿态规划必须等待曲面与真实边缘；第三部分工作程/返程控制先与厂家基线和离面返程比较。

## 10. 第一部分：人工点选、实体画线和三维曲面接口 R14-7

人工点选入口输出 `OrderedStroke`：起点、终点、点序、方向、交叉分支选择、操作时间；实体画线入口需额外输出墨线像素检测置信度、人工确认的笔顺、分段/遮挡和实体表面运动记录。两者**共享同一**配准与三维重建接口，不把数字语义 scribble 当实体墨线检测真值。像素 `uv` 必须显式带 `pixel_frame=RGB_RECTIFIED`、`registration_target=RGB`、`K_version`、`depth_encoding`、`depth_frame`、`T_CD`、`clock_domain`；否则 `depth[uv]` 只能是“数组取值”，不是“物理同一条射线”。

\[
\operatorname{PathEligible}=\bigwedge_{i=0}^{N}(e_{\rm order,i}\land e_{\rm rgbd,i}\land e_{\rm depth,i}\land e_{\rm 3Dref,i})\land e_{\rm path\ continuity}.\tag{R14-7}
\]

路径完整性是**全路径**约束；拒绝一个缺测段不等于补出它。未知笔顺、分叉或边界深度混合必须进入 `REOBSERVE/ASK_OPERATOR/HOLD`，不能把最短图路自动转成机械臂路线。自动点位识别以后只替换路径候选生产者，不豁免上述证据门。

## 11. 第二部分：刮板真实边缘、姿态规划与误差覆盖 R14-8

真实工具在机器人末端是一个刚体，工具坐标 `T_EG`（法兰/末端→刮板）及边缘轮廓 `r_G(a)` 需实测，不能用“单 TCP 点+法向”代替有限宽度边缘。设机器人姿态 `T_BE(s)`、沿工具边缘参数 `a`，真实工作边缘在基座坐标为：

\[
\mathbf x_B(s,a)=T_{BE}(s)T_{EG}\begin{bmatrix}\mathbf r_G(a)\\1\end{bmatrix},\quad a\in[a_{min},a_{max}].\tag{R14-8}
\]

要比较 `x_B(s,a)` 与可信局部曲面间的间隙、穿透和法向夹角，必须有**注册可信的表面**、边缘几何、重复装夹与误差预算。对有限宽度刮板，中心线“有深度”不能推出边缘全部接触。复杂偏航/曲率优化继续冻结；只有在独立仿体真值上证明简单固定姿态/边缘门控不够，才允许增加优化器。

## 12. 第三部分：六维力觉、工作程恒力、卸载与返程 R14-9

FAIRINO 官方 Python SDK 4.0.0 第12章列出 `FT_GetForceTorqueOrigin`、`FT_GetForceTorqueRCS`、`FT_SetZero`、`FT_SetRCS`、负载与重心配置、`FT_Guard`、`FT_Control`。力传感器读数要携带**坐标系、零点版本、重力/工具载荷配置、时间戳、量纲**。腕部合力/合矩仍不能唯一恢复边缘局部压力；R6/R7 的不可辨识结论保留。

\[
\mathbf w_O=\begin{bmatrix}\int_{\mathcal C}\mathrm d\mathbf f\\\int_{\mathcal C}(\mathbf r-\mathbf o)\times\mathrm d\mathbf f\end{bmatrix},\qquad \operatorname{RETURN\_eligible}=e_{\rm unload}\land e_{\rm force\_fresh}\land e_{\rm independent\_limit}.\tag{R14-9}
\]

阶段策略为 `HOLD→APPROACH→WORK→UNLOAD→RETURN_LIGHT/RETURN_OFF_SURFACE→RETRACT`，但这是**离线状态模型**。`RETURN_LIGHT` 与 `RETURN_OFF_SURFACE` 是待比较基线，不是人体可执行指令。程序 `provenance_gate.py` 始终返回 `robot_command=None`，不导入 FAIRINO SDK；`eligible_for_offline_plan_review=True` 也绝不表示真实控制许可。任何未验证注册、缺独立几何、缺工具边缘、未知六维力坐标系、超时或卸载失败都必须保持 HOLD。真实机器人仍需独立急停/限位、人员监督和厂家风险评审。

## 13. 可证伪假设 H14-01—H14-09 与本轮判断

| ID | 假设/可证伪检验 | 本轮证据与判断 |
|---|---|---|
| **H14-01** | 仓库注册调用与 OpenCV 参数契约一致；比对官方文档与完整源码 | **在变量语义下推翻**：Color→IR 与官方 IR→Color 不符；真实运行效果未判定 |
| **H14-02** | 互换 K 对投影位置没有影响；九个固定点重算 | **推翻**：12.1777–12.4859 px 条件性差异 |
| **H14-03** | 发现调用顺序矛盾即证明发布 PNG 错误；源图与成品哈希谱系、靶标比对 | **不支持此推断**：缺真实执行与生产谱系 |
| **H14-04** | `uint16` 注册未显式除1000就一定错；读 OpenCV 3.4 实现 | **推翻**：库对 CV_16U 内部×0.001 |
| **H14-05** | 任意 K 可不管分辨率/角色直接互换；半分辨率和不同 YAML 比较 | **推翻通用性**：版本/角色必须锁定，数值差不能当真实误差 |
| **H14-06** | 配准契约通过就能保证毫米级轨迹；独立仿体真值 | **未验证**；静态合同仅必要非充分条件 |
| **H14-07** | 数字 scribble 可代表实体墨线和人工笔顺；真实曲面双入口盲测 | **未验证且构念不同**；保留分离 |
| **H14-08** | 厂商恒力足以完成双向接触；独立载荷台架比较 | **未验证**；仅确认 SDK 存在 |
| **H14-09** | 缺少关键证据时离线门控保守拒绝；故障注入单测 | **组件级通过**；不是实时安全认证 |

## 14. 误差传播、统计解释、威胁与安全边界

本轮九个测试点是**人为固定的确定性算例**，不是随机采样；最小/中位/最大没有统计推断含义，不能给置信区间、显著性或成功率。`12 px` 是不同参数契约投影的差，不等于实际 RGB—深度误配像素，更不能未经深度/焦距/外参真值转换成实际三维轨迹误差。对 `K`、`T_CD`、`T_EC`、`T_BE(t)`、`z`、`uv`、工具边缘与力觉零点，既有随机误差也有共模系统误差，NIST 组合不确定度方法提醒必须显式建模协方差；无测量就不填“95%精度”。

**威胁 1**：示例函数可能不是已发布注册深度的生成路径；**威胁 2**：变量命名可能与实际物理角色不一致；**威胁 3**：OpenCV 3.4 源码和实际编译版本可能不同；**威胁 4**：Rt 矩阵截断与镜头畸变、像素中心定义、遮挡会改变数值；**威胁 5**：室内 RGB-D/数字标注向皮肤实体墨线存在严重域迁移；**威胁 6**：腕部六维力不提供局部压强的独立真值。所有人体试验仍不允许进入；本轮没有提出人体可用的力、速度或压力限值。

## 15. 决策记录 ADR-R14-01—ADR-R14-11（保留、修订、推翻）

| ID | 决策与证据 | 退出/反转条件 |
|---|---|---|
| **ADR-R14-01** | **新增硬门**：深度源相机、目标 RGB、K 角色、变换方向和分辨率必须成套追溯 | 靶标+生成谱系证明自动发现/修正全部角色而不误放行 |
| **ADR-R14-02** | **修订** R13 深度支持统计：仅源文件层描述，不能称注册可信三维支持 | 同源/目标独立靶标验证后升级证据等级 |
| **ADR-R14-03** | **保留** `OrderedStroke ≠ SupportGraph`；自动点位识别后置 | 真实实体笔顺可独立、稳定恢复且达预注册阈值 |
| **ADR-R14-04** | **推翻**“OpenCV uint16 没显式除1000就错”；源实现内部有尺度 | 编译运行的指定版本证明行为不同 |
| **ADR-R14-05** | **不确认**“TUM 成品深度注册错误”；只确认示例/官方契约存在条件性冲突 | 真实源图、注册输出、靶标与版本谱系复算 |
| **ADR-R14-06** | **保留** 简单直接深度+拒绝/重观测为强基线，但先过配准闸门 | 独立真值上复杂补洞在同等错误接受预算下更好 |
| **ADR-R14-07** | **保留** 真实刮板边缘/安装测量是姿态规划前提 | 实测证实点状工具近似满足预注册任务要求 |
| **ADR-R14-08** | **保留** `FT_Control` 与离面返程为非人体强基线，返程前确认卸载 | 台架证据+独立风险审查支持更优策略 |
| **ADR-R14-09** | **新增** V14 数据接口字段及离线失败即拒绝；27项单测通过 | 跨设备验证后按失效模式修订字段，不可把单测当安全认证 |
| **ADR-R14-10** | **未通过** 本地真实 PNG、激光/仿体真值、实体墨线、FR5 台架 | 原始数据/实物测量真正取得并完成盲测 |
| **ADR-R14-11** | **禁止** 写旧 `robot` 仓库；可见仓库中未确认独立新研究仓库 | 仅在新仓库身份和写权限明确时向新仓库提交 |

## 16. 第十五轮工作包 WP15-A—WP15-F、进入/退出与止损

| 工作包 | 预注册比较/输入 | 进入条件 → 退出条件 / 失败动作 |
|---|---|---|
| **WP15-A 源码—数据生成谱系** | 未注册 IR、同步 RGB、注册成品、完整 K/Rt、OpenCV 编译版本、SHA | 取得原始字节→能复现成品或解释差异；无法追溯则维持 `registration_unverified` |
| **WP15-B 独立彩深靶标/仿体** | 标定棋盘/深度阶梯/曲面，独立尺/激光，正确/交换 K、畸变与遮挡对照 | 非人体安全场景与测量协议→盲测二维靶标重投影和三维误差、按会话分组置信区间；失败 HOLD |
| **WP15-C 本地真实 PNG** | R13 六 PNG 的 blob SHA 与两种解码器 | 合法取得原文件→6/6 SHA、Python/JS逐像素及 R13 4/8 图指标一致；否则不得声称本地复现 |
| **WP15-D 人工点选/实体墨线双入口** | 同一非人体曲面、人工笔顺真值、反光/遮挡/交叉，B14-0/重观测/补洞 | 先过 WP15-B→按完整有序路径 P95/最大 3D 误差、拒绝率、重拍次数盲测；失败回退人工确认 |
| **WP15-E 实测刮板 + FR5 非人体力控** | 轮廓/TCP重复装夹、六维力零点/重力、独立测力、官方 `FT_Control` 与返程离面 | 风险评估+急停+日志→比较边缘覆盖、工作力误差、卸载换向峰值、轻力/离面返程；失败停止 |
| **WP15-F 博士创新止损** | 深度/注册/接触估计近邻全文、独立盲留出数据 | 强基线与真值齐备→有证据的新增研究问题；若基线足够则停止扩张复杂网络 |

进入/退出阈值应由**非人体仿体测量分辨率、实际任务需求和独立风险评估在实验前确定**，不从本轮 12px 反事实数值倒推。统计单位以**独立采集会话/安装/曲面**为主，禁止把同一图像邻接像素当独立样本。报告同时给出接受率、已接受样本误差、完整路径资格率和失败原因；缺任何一项都不得只用漂亮均值宣布胜利。

## 17. 本地复现、内容验收、限制与最终判断

**实际执行**：从 GitHub 只读连接器核对注册相关源文件及 Git blob SHA；从官方 OpenCV 文档和 3.4 源码核对函数角色与 `CV_16U` 单位；本地 NumPy 对9个人为点作两种投影，保存 `audit_result.json`；本地 `provenance_gate.py` 做离线 V14 证据门；`python -m unittest -v test_round14` 实际 **27/27 通过**。程序**没有调用 OpenCV 二进制配准、没有真实 PNG 像素复算、没有调用 FR5 SDK**。GitHub 可见仓库列表未出现明确的独立新研究仓库，故无远端提交；旧 `robot` 仓库未修改。

**文件**：`ROUND_14_RESEARCH_REPORT.md`、`ROUND_14_TEACHING_GUIDE.md`、`registration_contract_audit.py`、`provenance_gate.py`、`test_round14.py`、`audit_result.json`、`test_results.txt`、`QA_R14.json` 和复现 README。第三方源码不随包复制，保留官方链接和 SHA；本项目代码是独立实现的数值审计。结构 QA 检查报告 0—17 章、公式 R14-1—9、来源 L14-01—15、假设 H14-01—09、决策 ADR-R14-01—11、工作包 WP15-A—F，并检查教学逐章解释与所有表格逐表镜像。**结构覆盖与篇幅均衡不等于零基础学生实测理解。**

**本轮最终判断**：这是一次**实质性的证据资格收敛**，不是“发现某公开数据集有 12px 错误”。它把第一部分最先要验证的对象从“有深度/图上连通”上移到“相机源—目标投影、尺度和数据谱系”，并明确了可以用真实靶标证伪的条件。几何规划与力控模块继续保留接口和强基线，但在无独立三维真值、刮板实测和非人体 FR5 台架前，不能宣布精准三维轨迹、恒力控制或人体安全已完成。博士原创性仍未建立。