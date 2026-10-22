# 刮痧机器人算法研究｜第一轮调研与路线迭代报告

> **版本**：R1.0（独立研究记录）  
> **研究日期**：2026-10-15  
> **研究设备约束**：FAIRINO FR5、眼在手上 RGB-D 相机、六维力/力矩传感器；Ubuntu 22.04、ROS 2、GPU。相机型号、工具尺寸、采样率尚未核验。  
> **状态**：第一轮**文献与算法架构研究完成**；实验假设待验证；未执行真实机器人实验；未同步 GitHub。  
> **主目标**：先由操作员指定点位或直接在背部画线，再通过三维曲面感知生成接触轨迹，实现工作程规定法向力、返程较轻法向力的往返曲面操作。自动点位识别作为后续扩展。  
> **原则**：目标任务稳定；科学假设可被推翻；不因算法“先进”而引入无必要的复杂度；所有论文创新均视为待证实。

---

## 0. 本轮摘要：最重要的结论

本轮的重心不是把某个视觉网络选定为最终答案，而是检查 **“目标在哪里—曲面是什么—真实接触点与工具如何对应—控制器需要哪些几何量”** 能否形成一条数学上闭合、工程上可测的链路。结论如下：

1. **必须下调“RGB-D 背部三维重建+人工规划三维轨迹”作为单独创新的预期**。Xu 等（2024）已经将操作员在 *RGB 图像上* 绘制的二维曲线映射为三维按摩路径，并通过刚性点云配准更新轨迹、结合力位控制执行按摩 [L01]。另有 2026 年学位论文摘要报告了眼在手上 RGB-D、机械臂位姿初始化、多视角彩色 ICP、背部穴位三维定位与法向估计的完整链路 [L16]。这些既有工作使“基础模块集成”不足以构成博士级贡献。
2. **第一阶段不应强求完整背部数字人体模型**。先围绕目标线形成局部、可追溯且含质量信息的三维曲面 `S_local`。整背重建只在视野不够、路线跨区域或多次操作定位需要时启用；否则额外配准可能引入新的误差。
3. **人工选点与皮肤实体画线，应在前端分流、在三维轨迹表示处合流**。二者不能被混称：前者可以是屏幕点击/辅助标记，后者是相机实际观测皮肤墨迹；前端识别和遮挡处理不同，后端共享空间曲线、法向、切向、置信度和目标接触属性。
4. **应把误差从第一部分传给第二、第三部分**，尤其包括 3D 点的协方差/质量等级、法向角度不确定性、与标记的关联可信度和时间有效性。只传一组 XYZ 点会让轨迹规划与法向力分解误把不可靠几何当作真值。
5. **眼在手上不是意味着接触时始终看得见工作点**。末端刮痧板和手臂可能遮挡 ROI；第一版采取“接触前扫描—离线规划—执行中使用力反馈—可见时再观测更新”的分时机制；连续在线视觉更新是可验证的扩展，而非未经测试的前提。
6. **最有潜力、但尚不能宣称原创的候选研究点**：*面向刮痧板接触任务的质量感知三维目标轨迹生成*。即以最终工具接触几何和力控方向的误差要求反向优化视角、重建与曲面法向估计，而不是单纯追求点云更密。后续需与已有不确定性标定、主动视觉、曲面工具规划工作比较 [L08][L09][L12][L14]。

**本轮路线建议**：`相机-机械臂标定/同步 → 操作员点选或实体画线 → ROI 局部 RGB-D 质量评价 → 目标约束的局部曲面拟合（必要时多视角） → 曲线回投影/曲面约束平滑 → 接触位置/切向/法向/置信度 → 工具几何规划 → 双向力控`。

> **不等同于实验证明**：上面的 1—6 项是文献与机理分析形成的研究决策，尚未在 FR5 数据上验证效果。

---

## 1. 本轮目标、边界与工作量验收

### 1.1 任务的最终可观察行为（冻结功能，不冻结算法）

- **模式 A：人工指定点位**。操作员在背部实际位置或其配准图像上指定起点、终点、若干控制点；系统把指定转换为背部曲面上的三维轨迹；后续可插入自动解剖关键点识别，但不使其成为当前基础链路阻塞项。
- **模式 B：背部实体画线**。操作员用可识别的安全皮肤标记在背部画出连续线条；机器人扫描、提取线中心并提升为三维曲线。墨迹的宽度、反光、深度缺失、断线与遮挡都属于算法输入的不确定性。
- **动作语义**：沿指定空间曲线执行去程接触力 `F_work`，在换向区平滑转移到较小的返程接触力 `F_return`（两者均不在本轮指定人体使用的绝对牛顿数值）；工具有限宽度、倾角和接触边缘由后续规划器处理。
- **目标输出**：连续、带曲面约束和置信度的路径，而不是点云可视化图片。

### 1.2 本轮内容验收规则（不以任意工作时长充数）

| 验收项 | 预先规定的内容标准 | 完成情况 |
|---|---|---|
| 功能问题 | 两种人工输入、统一路径输出、向控制接口贯通 | **完成设计** |
| 研究证据 | 至少 12 项可追索正式论文/学位成果，覆盖视觉、标定、曲面、工具几何、控制关联；区分证据级别 | **完成，见 §2；尚非系统综述** |
| 候选方案 | 至少三种几何路线、两种目标提取路线、显式选型理由 | **完成，见 §4–6** |
| 数学与接口 | 定义坐标系、误差传播、轨迹契约、失效标志 | **完成，见 §3、§7** |
| 可证伪性 | 至少 4 个明确假设 + 对照组 + 指标 + 退出规则 | **完成设计，未运行实验** |
| 迭代记录 | 把保留/修订/否决和下一轮触发条件写入决策日志 | **完成，见 §10–11** |

**完成定义**是“形成可证伪的研究选择与实验协议”，**不是**“算法已经实现”。下一轮必须产生可复验的数据/代码结果，才能把相关判断提升为实验结论。

### 1.3 暂不纳入的内容

当前不开展全自动背部穴位语义识别、治疗效果验证、真实人体接触力推荐、完整设备驱动适配、全身非刚性数字孪生、端到端强化学习训练。必要的数学约束和测试闸门仍应保留；真正人体接触须另行满足实验伦理及机械安全前提。

---

## 2. 文献调研策略、证据分级与核心发现

### 2.1 这是什么类型的检索？

本轮是**目的驱动的首轮范围调研（scoping / seed review）**：优先识别会改变算法框架的关键现有工作，不能冒充已完整覆盖 IEEE Xplore、Scopus、Web of Science 的系统综述，也不能臆造检索命中数、PRISMA 流程或精读深度。

- **检索时间**：2026-10-15；核对原始期刊/会议页、CVF 开放论文页、作者/机构学术库、PubMed/PMC、论文 DOI 元数据。
- **检索簇**：① `robotic gua sha scraping force trajectory`；② `vision based massage interactive trajectory RGB-D`；③ `eye-in-hand hand-eye calibration uncertainty`；④ `colored point cloud registration back surface normal`；⑤ `next best view active robot scanning`；⑥ `surface tool contact footprint trajectory`；⑦ `normal force control uncertain surface orientation`；⑧ `human back acupoint RGB-D`。
- **优先纳入**：机器人实际接触相关；三维重建与标定有可直接采用的数学方法；与“标记/画线→三维接触轨迹”近似的系统；工具有限接触面积对规划产生影响的工作。
- **不以“是否用了深度学习”为纳入依据**。近两年的工作优先用于新颖性碰撞检查，经典方法优先用于可靠基线。

**证据标签**：`F` = 本轮已查看开放 HTML 正文中的方法/实验段落；`A` = 核对出版方/机构摘要及元数据，未逐式审阅全文；`T` = 学位论文摘要或二级索引，只作警示线索不作充分创新性否证。`A` 不意味着阅读了论文完整实验表。

### 2.2 与本任务最直接冲突的现有成果（必读）

| ID / 证据 | 文献与已验证信息 | 对本研究的影响 | 不能贸然下的结论 |
|---|---|---|---|
| **L01 / F** | Xu et al., *Toward automatic robotic massage based on interactive trajectory planning and control*, *Complex & Intelligent Systems*, 2024, DOI: `10.1007/s40747-024-01384-5`。人工在**RGB 图像上画 2D 线**，像素+深度转 3D，ICP 刚性更新，混合运动/力控制；正文有标记点定位实验。 | “屏幕画线→3D→按摩力控”已有直接先例；我们必须专门评价皮肤**实体**画线、局部曲率、工具接触边缘、路径不确定性。 | 不能说该文完全未处理遮挡/动态；它已有在线更新，但其文本中主要采用刚性 ICP，并不等同于非刚性皮肤面变形处理。 |
| **L02 / A** | *Research on Chinese Scraping Automation Based on Sensor-Fusion Method*, *IEEE Sensors Journal* 24(2), 2024, pp.1751–1759, DOI: `10.1109/JSEN.2023.3336220`。刮痧轨迹与力信息融合。 | 任何“刮痧运动规划+力融合”创新声明都必须直接对照。 | 未审阅全文，不推断其是否解决眼在手上物理画线。 |
| **L03 / A** | Zhao et al., *Human Back Acupuncture Points Location Using RGB-D Image for TCM Massage Robots*, *International Journal of Robotics and Automation*, 2023, DOI: `10.2316/J.2023.206-0790`。RGB-D + 姿态关键点 + GBDT；摘要报告平均点位误差 10.57 mm。 | 自主穴位检测已有技术；此数值是该论文定义下的结果，不是本项目的合格误差。 | 不以平均误差直接判断单个解剖点“医学准确”。 |
| **L04 / A** | *Robot Pose Estimation and Normal Trajectory Generation on Curved Surface Using an Enhanced Non-Contact Approach*, *Sensors*, 2023, DOI: `10.3390/s23083816`。末端 RGB-D 估计曲面法向和姿态修正。 | 曲面法向/位姿估计也非新方法；需要以任务需求定义真实误差传播。 | 论文的工业曲面结论不能直接外推到柔性人体背部。 |
| **L05 / A** | *Physical therapy massage robot system with human motion tracking and finger-pressing force control*, *Advanced Robotics*, 2025, DOI: `10.1080/01691864.2025.2469695`。RGB-D 估计骨架点并映射按摩区域。 | 为后续自动点位扩展提供对照。 | 当前阶段不必复现完整人体语义识别。 |
| **L16 / T** | 《基于机器视觉的人体背部穴位识别及三维重建》，2026 年收录的硕士论文摘要；报告 Eye-in-Hand RGB-D、多视角基于机器人位姿初值的彩色 ICP、背部法向与三维穴位。公开摘要链接见文末。 | 与我们“完整背部建模”高度重叠，因此将该模块降为基线/支撑技术。 | 学位论文内容需获取全文才能判断具体精度、鲁棒性和创新范围，摘要不是充分审稿证据。 |

### 2.3 方法基础、几何与可观测性证据

| ID / 证据 | 方法与来源 | 本轮使用方式 |
|---|---|---|
| **L06 / A** | Tsai & Lenz, *A New Technique for Fully Autonomous and Efficient 3D Robotics Hand/Eye Calibration*, *IEEE TRA*, 1989, DOI `10.1109/70.34770` | Eye-in-Hand 刚性变换标定基础；作为标定基线之一。 |
| **L07 / A** | Park & Martin, *Robot Sensor Calibration: Solving AX=XB on the Euclidean Group*, *IEEE TRA*, 1994, DOI `10.1109/70.326576` | Lie 群中的手眼标定/最小二乘基础。 |
| **L08 / A** | Ulrich & Hillemann, *Uncertainty-Aware Hand–Eye Calibration*, *IEEE T-RO* 40, 573–591, 2024, DOI `10.1109/TRO.2023.3330609` | 标定不确定性不应在“机器人重复精度很好”的假设下被忽略。 |
| **L09 / A** | Park, Zhou & Koltun, *Colored Point Cloud Registration Revisited*, *ICCV*, 2017, DOI `10.1109/ICCV.2017.25` | RGB+几何联合配准基线；光滑曲面存在纯几何配准滑移风险。 |
| **L10 / A** | Newcombe et al., *KinectFusion: Real-time dense surface mapping and tracking*, *ISMAR*, 2011, DOI `10.1109/ISMAR.2011.6092378` | TSDF/稠密融合经典基线；不必默认套用到局部扫描。 |
| **L11 / A** | Newcombe, Fox & Seitz, *DynamicFusion: Reconstruction and Tracking of Non-Rigid Scenes in Real-Time*, *CVPR*, 2015, DOI `10.1109/CVPR.2015.7298631` | 非刚性曲面变化的候选上限；较重，先不选为 MVP。 |
| **L12 / A** | Sanchez et al., *Robust normal vector estimation in 3D point clouds through iterative principal component analysis*, *ISPRS JPRS* 163, 18–35, 2020, DOI `10.1016/j.isprsjprs.2020.02.018` | 常规 PCA 与鲁棒法向估计对照；法向角误差比单纯重建误差更切近后续力控。 |
| **L13 / A** | Khoshelham & Oude Elberink, *Accuracy and Resolution of Kinect Depth Data for Indoor Mapping Applications*, *Sensors*, 2012, DOI `10.3390/s120201437` | 提醒相机深度误差依距离/结构变化；参数必须针对实际相机重新测得。 |
| **L14 / A** | Border & Gammell, *The Surface Edge Explorer (SEE): A Measurement-Direct Approach to Next Best View Planning*, *IJRR*, 2024, DOI `10.1177/02783649241230098` | 主动复测/下一最佳视角候选，而非仅依赖单帧可见性。 |
| **L15 / A** | *A structured review and taxonomy of next-best-view strategies for 3D reconstruction*, *ISPRS Open JPRS*, 2025, DOI `10.1016/j.ophoto.2025.100098` | 将视角选择分为规则、不确定性、采样、学习、预测等，避免直接上学习式 NBV。 |
| **L17 / A** | *Uniform Coverage Tool Path Generation for Robotic Surface Finishing of Curved Surfaces*, *IEEE RA-L*, 2022, DOI `10.1109/LRA.2022.3152695` | 接触足迹随曲率和力变化；第一部分输出必须支撑第二部分工具边缘/足迹规划。 |
| **L18 / A** | Karayiannidis & Doulgeri, *Robot contact tasks in the presence of control target distortions*, *Robotics and Autonomous Systems*, 2010, DOI `10.1016/j.robot.2009.12.004` | 曲面法向不准会同时扭曲位置/力的目标与控制子空间；给“几何误差→力控”建立理论动机。 |
| **L19 / A** | *Robotic Contact on Complex Curved Surfaces Using Adaptive Trajectory Planning Through Precise Force Control*, *Machines*, 2025, DOI `10.3390/machines13090794` | 曲线平滑、姿态插值和接触力已有整合方案；不能把贝塞尔+阻抗并列就声称新颖。 |
| **L20 / A** | *Recognition of Acupoints on Human Back Based on Machine Vision and Deep Learning*, *Big Data and Cognitive Computing*, 2026, DOI `10.3390/bdcc10070204` | 自动点位识别方向已有较新成果；当前阶段后置，保留统一接口。 |
| **L21 / A** | *Quality-Aware Active Re-Observation for Constrained Eye-in-Hand Robotic Manipulators*, *Applied Sciences*, 2026, DOI `10.3390/app16189389` | **任务区域深度质量**比“看见目标”重要；任务驱动重观测不能仅作为新颖点，需明确区别。 |
| **L22 / A** | *Improving 3D Reconstruction Through RGB-D Sensor Noise Modeling*, 2025（PubMed 可查摘要） | 测距和入射角等影响深度质量；可作为质量模型候选，待全文核验。 |

> 检索证据等级提示：本轮详细核读了 L01 的出版方开放 HTML 方法/实验内容；其余多数完成标题、出版年份、DOI、摘要与适用范围核验，**未完成完整方法式、代码与实验表的逐项对照**。这也是第二轮必须补齐的限制。

### 2.4 本轮文献引起的方向修正

- **修正 R1**：以前把“背部整体 3D 重建”视作第一模块默认产物；现改为目标 ROI 的高质量局部曲面，必要时扩展全背。原因：目标任务要求局部准确切向/法向，整背点数不是直接性能指标；多视角融合已有成熟先例 [L09–L11][L16]。
- **修正 R2**：把“人工选点/实际画线转三维”视为功能与基线，不直接宣称创新。原因：图像上绘线已有 [L01]；皮肤实际标线需要从任务差异中建立实质改进证据。
- **修正 R3**：不把视角复测或不确定性传播单独当作创新标签。原因：有不确定性手眼标定、NBV 和质量感知重观测的现有研究 [L08][L14][L21]。可能的贡献在于**终端接触误差驱动的联合决策**，仍需严格区分。
- **修正 R4**：图像/深度误差必须沿接口进入后端，而不是在感知阶段平均处理掉。原因：曲面法向误差会影响接触运动和力/位子空间 [L18]。

---

## 3. 数学问题重建：从“像素点”到“可用于接触的空间路径”

### 3.1 坐标定义（规范）

- `B`：FR5 机器人基座；`E`：末端法兰/受控末端刚体；`C`：深度相机对齐后的光学坐标系；`I`：RGB 像素坐标；`S`：背部表面局部任务坐标；`T`：刮痧工具本体坐标；`Q`：工具**实际接触参考点**（不同于 TCP 的默认位置）。
- 采用符号 `^A T_B` 表示将 B 坐标的齐次点转换到 A 坐标。
- **Eye-in-Hand**：`^B T_C(t)= ^B T_E(t) · ^E T_C`；只要相机固定于手臂，`^E T_C` 为待标定的固定刚体变换。RGB/Depth 是否已经硬件对齐需要由具体设备核验。
- 像素 `(u,v)` 和已对齐深度 `d` 的回投影：

\[
{}^C\mathbf p = d\,K^{-1}[u,v,1]^\top,\qquad
{}^B\mathbf p = {}^B R_C {}^C\mathbf p + {}^B\mathbf t_C.
\]

以上忽略畸变，前提是像素已校正；无效深度不得默认为 `z=0` 或最近有效邻点。

### 3.2 输入与任务输出

**输入** `O={RGB_t,Depth_t,K,T_BE(t),timestamps, operator_annotation}`。第一轮只用视觉与位姿；后续数据融合添加 `wrench_t`。

**核心输出** 是定义于背部表面上的曲线：

\[
\Gamma(s)=\left\{\mathbf p(s),\mathbf t(s),\mathbf n(s),
\kappa(s),\Sigma_p(s),\Sigma_n(s),q(s),t_{obs}(s)\right\},\quad s\in[0,L].
\]

- `p`：曲面目标点（米）；`t=dp/ds /||dp/ds||`：单位切向；`n`：一致定向的单位法向；`κ`：局部曲率/曲线弯曲信息；`Σp`：3×3 点位置协方差或保守边界；`Σn`：在法向切平面表示的两维角度不确定性；`q`：质量/可执行性状态；`t_obs`：最近有效观测时间。
- 此输出**不是 FR5 目标法兰轨迹**；法兰位姿必须由第二部分根据工具几何、接触点和倾角计算。

### 3.3 为什么法向误差比“点云看起来平滑”更关键？

假设真实表面法向 `n`，估计法向 `n_hat`，夹角 `θ=acos(n·n_hat)`。真实接触力可分为 `F=F_n n + F_t`（`F_t·n=0`）；按错误法向估计得到

\[
\hat F_n=\mathbf n_{hat}^\top\mathbf F,
\quad \left|\hat F_n-F_n\right|
\leq |F_n|(1-\cos\theta)+\|\mathbf F_t\|\sin\theta,
\]

此上界从向量投影和 Cauchy–Schwarz 推得（方向符号需统一），说明 **摩擦切向力与法向偏差耦合**：即使腕部合力测量完全准确，法向估计错误也会导致所谓“恒力法向控制”偏移。[L18] 对接触子空间误差的分析提供了相关理论背景。此推导是本项目的建模说明，不是论文原创主张。

### 3.4 三维位置不确定性初步传播

设校正后的像素与深度联合测量 `y=[u,v,d]^T`，协方差 `Σ_y`。局部一阶近似：

\[
\Sigma_{p_B}\approx
J_y\Sigma_yJ_y^\top + J_{HE}\Sigma_{HE}J_{HE}^\top
+ J_{robot}\Sigma_{robot}J_{robot}^\top
+\Sigma_{fit}.
\]

这里 `J_HE` 表示对手眼外参（在 SE(3) 切空间中）的雅可比，`Σ_fit` 表示表面拟合剩余误差。此表达式**依赖线性化与近似独立性**；多帧共享同一手眼外参存在相关误差，不能把共享系统偏差当作多帧独立噪声平均消失。实际协方差需靠标定板/人工靶点重复实验校准；无法建模时必须用可观测的误差界与质量标志替代伪精确协方差。

### 3.5 三维曲线/实际画线的曲面约束

为实体线提取候选像素中心线 `q_i=(u_i,v_i)`，提升并投影至曲面 `S` 得到 `p_i`。可以以 B 样条或离散点为变量求：

\[
\min_{\{\mathbf x_i\}} \sum_i w_i \|\mathbf x_i-\mathbf p_i\|_{\Sigma_i^{-1}}^2
+\lambda\sum_i\|\mathbf x_{i+1}-2\mathbf x_i+\mathbf x_{i-1}\|^2,
\]
\[
\text{s.t.}\quad d_S(\mathbf x_i)=0,\quad
\text{preserve endpoints/marked corners},\quad
\text{reject if quality below threshold}.
\]

- `d_S` 为所拟合局部曲面的有符号距离近似；`w_i` 由分割、深度和曲面质量确定。
- **不能一味平滑**：如果操作人员画出有意转角，平滑可能改写意图，应保留拐点/端点并报告几何偏差。
- **不是每条手画的短直线都要计算测地线**：实体线本身提供了表面路径约束；仅给定端点而没有指定形状时，才需解释“最短测地线、投影直线还是基于解剖规则的曲线”。

---

## 4. 第一部分算法框架：感知—重建—目标轨迹

### 4.1 两种输入前端，统一输出后端

```text
                        ┌─ 模式 A：屏幕选点/人工标记 ── 像素或 3D 锚点 ─┐
RGB-D + EIH 位姿  ────┤                                         ├─ ROI 确定
                        └─ 模式 B：皮肤实体画线 ───────── 分割与骨架 ──┘
                                                                      │
                                      深度质量筛查 / 目标可见性检查  ────┘
                                                  │
                                     局部点云 + 可选多视角配准
                                                  │
                                   曲面拟合 + 法向/曲率与质量估计
                                                  │
                            目标曲线沿曲面提升、平滑、保形与弧长重采样
                                                  │
                                 Γ(s) + 不确定性 + 有效区域掩码
                                                  │
                                     交给刮痧板接触姿态规划
```

#### A：人工点选/点位定位

1. 在 RGB 图像（或现场被标记的位置）获取人工指定 `start / end / via-points`；屏幕坐标点击和实体标志点须在数据中区分输入来源。
2. 深度有效时：将点击锚点回投影为 3D；深度空洞时：在局部可信曲面上用相机射线求交，而非盲目插值。
3. 依用户意图选择曲线类型：单点不自动产生行程；两端点需要明确曲线路由规则；多点以保形样条拟合，并投影回表面。
4. 对投影唯一性、曲线跨越空洞及后续工具可接近性给出有效性标记。

#### B：皮肤表面实体画线

1. 在 RGB 空间中先用**可控颜色与光照**构造传统阈值 / 背景差分基线；再尝试轻量语义分割，不能无数据直接假定 SAM 解决了细线中心与纹理混淆。
2. 对掩码执行连通分量、去噪、线宽估计、骨架化、分支识别和中心线排序；出现 Y 形分叉须要求人工确认路径顺序，不凭程序随意选。
3. 依据彩色-深度映射将线上的像素转到 3D，记录深度有效比例；对于少量缺失，通过已拟合曲面与相机射线求交；对于长距离缺失或遮挡，标为 `REOBSERVE`，而非“猜”完整曲线。
4. 沿曲面进行保端点、保有意拐点的弧长重采样；保持墨迹原始曲线的像素与三维双向对应，便于误差解释。
5. **材料变化/动作中的标线模糊**：首版只保证扫描时看得见标线；刮擦中被遮挡或墨迹变化时，使用已建 3D 路径、机器人状态及可用传感反馈；要在线更新必须额外验证重新配准是否可信。

### 4.2 三条几何路线竞争，不提前绑死

| 路线 | 算法 | 优点 | 限制 | 本轮决策 |
|---|---|---|---|---|
| **G0：单视角局部法向** | 点云滤波 + KD-tree 邻域 PCA / 二次曲面 + 像素深度回投影 | 最短可实现路径、速度快、易定位误差来源 | 遮挡、切线视角、稀疏区域、深度孔洞敏感 | **第一基线，必须实现** |
| **G1：目标 ROI 多视角融合** | 手眼/机器人位姿作初值 + point-to-plane ICP 或 colored ICP + 局部加权曲面拟合/TSDF | 可提高 ROI 覆盖和冗余观测，降低随机深度噪声 | 配准漂移、共享标定偏差、被试移动后伪融合 | **优先候选**，仅在质量有收益时启用 |
| **G2：全背稠密/非刚性重建** | TSDF/KinectFusion、DynamicFusion 或变形图 | 有利于大范围多点规划和明显形变跟踪 | 成本高、数据缺口多、算法创新已拥挤 | **暂缓**，由实验失败触发 |

**最关键的判据**：比较 `G0` 与 `G1` 的**末端目标点误差、法向误差、线条 3D 偏差、无效输出率和耗时**，而非仅用点云 RMSE 或视觉观感。

### 4.3 有效曲面表示如何选？

对短范围、光滑背部区域，可采用带局部参考系的二次曲面 `z=a x²+b xy+c y²+d x+e y+f`；优先做稳健加权拟合，输出法向与曲率解析值。复杂区域改用局部 MLS、稀疏 mesh 或 TSDF。法向方向必须跨路径一致（统一指向相机/表面外侧，仅作为约定，还需校验力控符号），邻域尺度须针对曲率与噪声调参。

**防止因皮肤标线导致偏差**：局部曲面拟合以**深度几何**为主、RGB 只为匹配/目标识别赋权，避免把暗色笔迹当作凹槽；深度和颜色配准误差单独测量。

### 4.4 为什么“主动复看”先作为门控策略而非复杂学习策略？

对每个轨迹区域生成质量向量：`coverage, depth_valid_ratio, incidence_angle, fit_residual, multi_view_consistency, line_confidence, hand_eye_residual`。

- 若质量满足阈值，则直接输出；
- 若 ROI 局部出现深度缺口/过斜视角，则建议“换一视角再看一次”；
- 若复看也无法改善，输出不可执行区段并明确停机/人工修订需要。

视角选择第一版用少量离散候选位姿和质量增益减运动代价的启发式评分：

\[
\mathbf v^*=\arg\max_{\mathbf v\in\mathcal V_{valid}}
\big(\Delta Q_{ROI}(\mathbf v)-\lambda c_{move}(\mathbf v)\big).
\]

预测质量 `ΔQ` 没有神奇预知能力：先使用可计算的入射角、覆盖率、遮挡几何与已有重复观测统计做代理，执行后再核验是否实际改善。主动视觉已有成熟工作 [L14][L15][L21]，当前它是保证链路可靠的功能，不自动构成论文创新。

---

## 5. 从第一部分反推第二部分的必要接口（工具接触规划）

第一部分输出必须**比“若干 XYZ 路点”更丰富，但也不能越权直接决定接触力**。

### 5.1 刮痧板接触几何关系

给定沿曲线的局部 Frenet-like 参考（避免零曲率处不稳定）：

\[
\mathbf t=\frac{\partial_s\mathbf p}{\|\partial_s\mathbf p\|},\quad
\mathbf b=\frac{\mathbf n\times\mathbf t}{\|\mathbf n\times\mathbf t\|},\quad
R_{surface}=[\mathbf t,\mathbf b,\mathbf n].
\]

规划器从 `R_surface` 再施加工具特定的接触角 `α(s)` 与绕法向的工具方位，得到期望工具姿态 `R_BT(s)`，并根据工具边缘局部接触点 `^T p_Q` 求工具原点：

\[
{}^B\mathbf p_T(s)={}^B\mathbf p_Q(s)-{}^B R_T(s){}^T\mathbf p_Q.
\]

因此，工具倾角变化会使 TCP / 法兰路径发生偏移。对弧形/宽刮板，`p_Q` 还可能是受曲率、接触模式和工具形变影响的**有效接触点**，不能假定固定偏置永久有效。[L17] 提供了接触足迹随曲面和载荷变化的既有参照。

### 5.2 第二部分具体需要的字段

- `pose_seed`: 沿路径的接触参考点 `p_B`、单位切线 `t_B`、单位法向 `n_B`，曲率/曲面邻域。
- `uncertainty`: `Sigma_p`, `Sigma_n` 或保守误差界，测量时间和是否需要复观测。
- `topology`: 线条方向、端点、允许换向区、路径分叉信息、允许处理的局部表面区域。
- `tool_context`（由第二部分创建，不由视觉臆造）：工具边缘几何、刮痧板厚度/宽度、装夹变换 `^E T_T`、接触模式与倾角范围。
- `feasibility`: IK 可达、工具与环境几何间隙、可执行轨迹片段；不可执行时反馈第一部分请求局部路线修正。

### 5.3 不应该提前锁死的选择

- 视觉阶段不直接规定工具恒定倾角；倾角应由工具接触几何与接触任务联合选。
- 视觉阶段不规定工作程绝对牛顿值；临床适宜力度与压力分布需要独立研究/证据，不可从几何计算臆造。
- 视觉曲面法向不是力传感器的“真实法向”；两者可作为后续互相验证的不同观测。

---

## 6. 与第三部分的接口（力控与在线纠偏）

### 6.1 分层频率与状态估计思想

建议先解耦：视觉低频或分时输出 `Γ`，规划器输出可执行的接触参考 `T_BQ(s)`，内层由实际支持的机器人/厂商控制能力实现力位协调；六维力传感器反馈法向/切向合力与力矩。**不同环路的最终频率不得事先虚构**，必须等真实时间戳和控制接口测量后再定。

### 6.2 力量投影与摩擦耦合

力传感器读数必须首先转换到 `B` 或一致的任务坐标，进行传感器零偏、工具重力及杠杆臂力矩补偿，才能称为接触合力估计。以曲面估计法向 `n_hat` 定义：

\[
\hat F_n=\mathbf n_{hat}^{\top}\mathbf F_c,\qquad
\mathbf F_{t,hat}=(I-\mathbf n_{hat}\mathbf n_{hat}^{\top})\mathbf F_c.
\]

这不是对局部皮肤压力的测量；**同一个腕部合力/合矩可能对应多个不同的接触压力分布**，刮板有宽度时尤其如此。刮痧板倾角/边缘接触信息须与视觉曲面、工具模型结合，必要时另增局部接触传感才能建立压力模型。

### 6.3 工作程与返程统一的轨迹契约

```text
APPROACH → CONTACT_ESTABLISH → WORK_STROKE(F_work)
         → UNLOAD_AND_REVERSE → LIGHT_RETURN(F_return)
         → NEXT_CYCLE / RETRACT
```

第二部分对每个 `s` 输出 `force_profile(s,phase)`、`speed_profile(s,phase)`、`tool_orientation(s,phase)`；第三部分仅在可用力控能力与传感质量满足约束时执行。两个方向可共享同一条中心线，但工具接触边缘/攻角和速度不一定对称，换向区尤其要重新规划，而不能直接把位置数组倒序回放。

### 6.4 反馈给第一部分的纠偏信号

| 从控制端回来的观测 | 可能原因 | 感知端应如何响应 |
|---|---|---|
| 力误差与曲率/法向变化同步增大 | 几何法向不准；接触刚度变化；控制延迟 | 标记对应段 `geometry_or_contact_ambiguous`，触发复测或比较多法向候选，不能直接断言原因 |
| 端点附近力矩突变 | 刮板角度变化、接触边缘转移、换向动作 | 反馈第二部分重规划换向与姿态；必要时重新获取局部几何 |
| 视觉 ROI 被工具遮挡 | Eye-in-Hand 自遮挡 | 暂停视觉更新；使用最后可信几何并标注时效；在非接触可观察窗口重观测 |
| 轨迹目标因身体运动偏移 | 刚体位姿变化或局部非刚性形变 | 先做可观测刚体配准检查；残差呈空间结构时考虑局部形变；不能盲目 ICP 更新整条曲线 |

---

## 7. ROS 2 模块数据契约（首版建议）

> 以下是**接口设计草案**，并非已经创建或编译成功的 `.msg` 文件；尺寸、序列化方式和 ROS 2 QoS 需要第二轮原型检验。

```yaml
SurfaceStrokePlan:
  header:
    frame_id: robot_base        # 所有三维向量统一在 B 系
    stamp: <observation_time>
  plan_id: <uuid>
  input_mode: SCREEN_POINTS | PHYSICAL_MARKER | IMAGE_DRAWN | FUTURE_AUTO_POINT
  surface_model_id: <version>
  source_observation_ids: [<frame_id>, ...]
  path_direction_defined: true
  branches_resolved: true
  segments:
    - s_m: 0.0
      point_m: [x, y, z]
      tangent: [tx, ty, tz]
      normal: [nx, ny, nz]
      local_curvature_1_per_m: [k1, k2]
      covariance_position_m2: [9 numbers]
      covariance_normal_rad2: [4 numbers]  # 法向局部切平面 2x2
      confidence: <calibrated_score_or_null>
      quality: VALID | REOBSERVE | INVALID
      source_pixel: [u, v]
      obs_age_s: 0.0
  stroke:
    path_type: OPEN | CLOSED
    semantic: START_TO_END | MARKED_DIRECTION | USER_DEFINED
    endpoints_pinned: true
  validity:
    calibration_id: <id>
    rgb_depth_alignment_id: <id>
    region_of_interest_id: <id>
    max_gap_m: <measured_requirement>
```

**时间戳约定**：数据使用对应曝光/深度帧的时间，而不是 ROS 收包时间；每个相机帧对应的 `^B T_E(t)` 必须与该时间对齐或内插，并记录同步残差。新扫描结果需保持 `surface_model_id` 可追溯，禁止悄悄把不同版本曲线拼在一起。

**质量值约定**：`confidence` 不得是“任意看起来合理的 0.9”；未经可靠性校准可置为 `null`，优先用可解释的深度有效率、拟合残差和误差界。

---

## 8. 第一轮提出的可证伪研究假设

| 假设 | 为什么值得测 | 强对照基线 | 支持条件（建议，非已达到） | 否定/转向条件 |
|---|---|---|---|---|
| **H1：目标 ROI 导向的局部融合，优于默认全背/单帧的一刀切** | 背部不同区域的深度噪声和遮挡不同，全部扫描可能浪费时间并引入漂移 | G0 单帧 PCA；G1 标准多视角 ICP/colored ICP；G2 全背 TSDF | 在相同视角预算下，目标点/法向/曲线误差显著降低且无效输出率不升高 | 多视角融合收益不超过噪声或导致系统偏差增大；退回 G0 |
| **H2：曲面约束+保形平滑能显著改善皮肤实际画线三维忠实度** | 逐像素深度提升会因缺失与抖动而产生锯齿，普通样条又可能过分抹平 | 直接按像素深度连线；独立 3D 三次样条；曲面约束拟合 | 对几何地面真值线的 3D 距离与端点偏差更低，且拐角保留率不下降 | 主要误差来自颜色分割或相机配准，先修正前端，不提升样条复杂度 |
| **H3：误差门控的有限主动复测优于盲目多视角扫描** | Eye-in-Hand 可以换角度，但频繁移动成本高 | 固定单视角；固定 3 视角；随机补拍 | 在相同或更低相机运动预算下，降低无效轨迹比例、法向角误差和重建耗时 | 没有统计收益则采用固定采样，并保留简单质量筛查 |
| **H4：显式传播法向与位置不确定性可以提升下游工具规划的可解释性** | 姿态规划中法向误差被放大为工具接触点偏移和力分解误差 | 不带质量的 XYZ + PCA 法向；带标量置信度；协方差/区间传播 | 预测误差界在保留集上具有可信覆盖率，且能识别高风险轨迹段 | 协方差校准失效/无实际预测力，则改为保守定性门控，不保留复杂概率模型 |
| **H5（备选）：非刚性跟踪比刚性 ICP 更必要** | 呼吸/姿态变化可能改变曲面 | rigid ICP vs local deformable | 只有在可重现的非刚性残差导致轨迹超差时才激活 | 动态误差对实际任务不显著；避免把 G2 作为第一阶段前置 |

**证据不足点**：尚缺 FR5 Eye-in-Hand 实采图像、相机标定、实际工具几何和用于可重复比较的曲面地面真值。因此不能在第一轮把 H1–H5 写成“算法提高精度 xx%”。

---

## 9. 最小可复现实验协议（第二轮执行规格）

### 9.1 数据集分层，不必马上进入人体接触

| 层级 | 数据与参考真值 | 用于验证 | 当前状态 |
|---|---|---|---|
| **S0 合成** | 平面、椭球面、二次曲面、带脊线/鞍形曲面；解析法向；加不同视角、遮挡、深度噪声与时间误差 | 误差传播及算法上限、失败边界 | 待生成 |
| **S1 桌面曲面仿体** | 固定几何模型（CAD/已知标志点），在表面画直线、弧线、折线和带分叉线 | 实体画线识别、手眼坐标误差、曲面贴合 | 待采集 |
| **S2 背部形状仿体** | 不同曲率/软硬局部仿体；静态与可控小幅变形 | 方法迁移和复看必要性；可后接轻载接触研究 | 待设计 |
| **S3 人体观测** | 在适用审批与知情同意框架下，只考虑非接触采集及标记识别 | 外观、肤色/光照变化与姿态差异；不推断临床精度 | 非本轮任务 |

**建议初步样本矩阵（可按资源缩放）**：至少 3 种曲面 × 3 种画线形状 × 3 个成像视角 × 5 次独立重复 = 135 个基本采集条件；另采深度缺失、明显反光和视野遮挡的压力测试。此数值是**下一轮实验计划**，不是已完成的样本量。设计时按独立重置和实验组分层统计，避免把相邻像素当成独立样本。

### 9.2 关键指标（先定义，再看结果）

1. **定位误差**：`||p_est - p_GT||` 的中位数、P95、分区域/视角误差（毫米）；单独报告点击像素误差与机器人坐标误差。
2. **三维线偏差**：曲线到真值的双向距离（Chamfer/受限 Hausdorff）、端点误差、弧长偏差、最大可追踪间隙；对应方向可能未定义时先匹配端点。
3. **法向误差**：`acos(clamp(n_est·n_GT,-1,1))` 的中位数/P95（角度）。对符号反转报告为独立严重错误，而非用绝对内积掩盖。
4. **时间与稳定性**：每帧处理延迟、扫描/重观测总耗时、失败率、延迟尾部、视角更新次数。
5. **可信度有效性**：标准化位置残差、置信区间覆盖率、预测的高风险区段与实际超差区段的召回率。
6. **下游潜在影响**：在离线固定工具模型下计算工具原点位置偏差、倾角偏差、接触边缘偏差；接触力实验是后续阶段，不在本轮夸大。

### 9.3 消融与统计

- 对照 C0：原始 RGB-D 像素深度提升 + 局部 PCA；C1：彩色 ICP + 同一法向；C2：局部稳健曲面 + 品质门控；C3：C2 + 有限重观测；C4：C3 + 保形曲面轨迹约束。
- 每次只增加一个关键因素；相同输入数据/标定/视角预算下比较；固定随机种子；按**采集 session 和仿体**分组做训练/验证/测试切分。
- 优先报告配对误差变化、95% bootstrap 区间和失败案例；不以单个漂亮案例判断成功。
- **绝对毫米阈值不预设为事实**：相机型号与真实噪声未知。可先以 **较基础方案降低至少 20% 主指标误差（探索性目标）且无效输出率不恶化** 作为是否继续优化的候选判断线，具体阈值在取得实际数据之后预注册修订，不能事后根据结果调指标。

### 9.4 最小系统测试的顺序

`T1: 坐标链单位测试 → T2: 静态靶点重投影 → T3: 合成曲面法向 → T4: 实体画线 2D 提取 → T5: 曲线 3D 提升 → T6: G0/G1 配对对照 → T7: 质量门控重观测 → T8: 输出 ROS 2 兼容的路径结构（先离线模拟订阅）`。

**本报告不包含已运行的 T1–T8 测试**；它们是下一轮的可验证开发交付，避免把文档设计说成实测成果。

---

## 10. 候选架构竞争与第一轮选型决定

### 10.1 路线候选

- **架构 A：最简单端到端基线**：单帧深度→直接三维点→常规平滑→PCA 法向→工具规划。优点是快；缺点是对局部深度孔洞、角度和静态偏差不敏感；必须保留为论文基线。
- **架构 B：局部任务质量驱动（本轮推荐）**：首先统一两种人工输入，保留像素关联；评估目标 ROI 深度质量，触发局部多视角/曲面拟合，生成带误差的目标线；允许规划器反馈不满足接触精度的段。优点是针对真实任务，复杂度可控；难点是质量评分如何与接触规划误差闭合。
- **架构 C：全背非刚性数字孪生 + 网络关键点 + 学习控制**：学术空间看似大，但需要更多标注、动态数据、计算和模型假设，目前无证据证明其必要性；不是第一阶段主线。

### 10.2 质性决策矩阵

| 维度 | A 单帧 | B 局部质量驱动 | C 全背非刚性/学习 |
|---|---|---|---|
| 最小闭环可实现性 | 高 | 中高 | 低 |
| 可诊断误差与建立基线 | 高 | 高 | 中低 |
| 面向遮挡/深度质量可扩展性 | 低 | 高 | 高但复杂 |
| 与工具接触误差联动 | 弱 | **显式接口** | 可实现但未证明必要 |
| 当前原创性风险 | 高（已有成熟方法） | **需具体实验验证** | 高（已有大量大模型/非刚性方法） |
| 本轮处理 | **必须实现基线** | **推荐研究主线** | **条件触发，暂不实施** |

**选型**：采用 `B` 为研究假设，`A` 为不可删除的性能参照，`C` 为失败/需求驱动的备选。这里的“推荐”是探索优先级，不是证明 B 的性能更好或创新已成立。

### 10.3 计划如何避免研究漂移

每轮都进行 3 个问题审计：
1. 新算法是否直接降低从操作员意图到工具接触状态的可量化误差？
2. 是否已有足够强的基线、消融与失败边界，能够证明其必要性？
3. 是否能在 FR5 + Eye-in-Hand RGB-D + 六维力传感器的资源下完成最小验证？

若连续两轮找不到肯定答案，应归档该支线，回到最显著的误差源，不把它变成“看起来更高级”的模块。

---

## 11. 第一轮正式研究决策日志（ADR）

| 决策号 | 原先观点/风险 | 本轮更新 | 证据/逻辑 | 下轮复核触发器 |
|---|---|---|---|---|
| **ADR-001** | 必须先建整个背部 3D 模型 | **修改**：先目标 ROI 的局部高质量模型 | 完整背部不是沿一条线作接触的必要条件；已有整背/彩色 ICP 先例 [L16] | 轨迹跨 ROI、视角不够或刚性配准失败 |
| **ADR-002** | 三维重建与人工画线可能直接构成创新 | **否决其直接原创性**：作为基线功能 | [L01] 已实现屏幕绘线→3D→按摩；[L04] 已实现工具法向跟踪 | 与实体皮肤画线的严格对比表明仍有未解决问题 |
| **ADR-003** | 自动识别穴位作为前置目标 | **延期**：人工指定优先 | 用户明确需求；自动点位已有研究 [L03][L20] | 人工定位已经稳定，并确定解剖专家标注与数据来源 |
| **ADR-004** | 只输出 XYZ 路径 | **修改**：输出切向/法向/有效性/误差等 | 曲面误差影响工具接触和力控方向 [L17][L18] | 接口性能开销过大或质量模型不可校准 |
| **ADR-005** | 接触中持续视觉观测默认可用 | **否决**：分时接触前/非遮挡时观察 | Eye-in-Hand 自遮挡几何风险；需实测 | 若相机视场证明确实能实时看到 ROI 才增在线视觉 |
| **ADR-006** | 多视角总优于单帧 | **否决绝对化**：只在局部质量受益时启用 | ICP 配准、运动误差和光滑表面退化 [L09] | G1 在独立测试中显著优于 G0 |
| **ADR-007** | 多传感器融合本身是研究深度 | **修改**：重点研究误差如何跨模块传播与闭环消解 | [L02][L08][L18] 已覆盖多种融合与不确定性概念 | 下一轮精读显示已有几乎相同的端到端方案 |

---

## 12. 下一轮的工作包、内容闸门与终止规则

### R2 目标：用真实/合成数据检验路线 B 是否有必要

**WP-2.1 文献深读（完成定义）**
- 获取并逐项填写 L01、L02、L04、L08、L09、L12、L14、L17、L18 的 *方法假设—数学公式—数据—核心指标—适用边界—复现可得性*，至少 9 个“可对照方法卡”。
- 如 L02 出版全文无权访问，记录访问限制，至少核验 DOI/摘要，不臆造其具体实验结果；寻找合法开放作者版本。

**WP-2.2 最小可复现实验（完成定义）**
- 生成解析曲面和合成画线/点击数据，真实设备数据若可用则附加；至少实施 C0 与 C2 的完整 pipeline，对点/线和法向误差做对照。
- 明确采用的相机内参/合成噪声模型；在**无真实标定数据**时只写“模拟结果”，不冒充设备精度。
- 给出测量脚本、数据格式、参数与可复现实验报告，含主要失败案例。

**WP-2.3 轨迹契约原型（完成定义）**
- 产出独立于 ROS 2 的 Python 数据模型/单测，验证单位法向、一致方向、时间戳、质量门控和路径分段约束；再映射到 ROS 2 接口。
- 第二部分建立**虚拟工具几何**，验证改变刮板倾角会正确改变工具 TCP 与接触点的相对位姿；不需要先做实体接触。

**WP-2.4 迭代决策（完成定义）**
- 写 `R2_ADR.md`，明确：B 保留/简化/否决；是否纳入第二视角；是否值得进入摩擦与接触模型；更新 R3 范围。
- 若 H1 与 H2 都没有相对 A 的可复验收益，则以 A 为正式工程基线，把研究深度转移到“工具接触几何+法向力耦合”，而不是继续扩张视觉模型。

**不设虚假工时/截止日期**：以上是内容闸门；尚未执行的数据与代码工作不会被记作已完成。每轮允许改变结论，必须给出依据。

---

## 13. 开放问题与依赖信息（不阻断下一轮离线研究）

- Eye-in-Hand 相机具体型号、RGB 与深度分辨率和标定质量目前未知，会影响实际厘米/毫米级的误差结论；可先用合成/开源数据作算法比较。
- **“人工指定点位”**建议同时支持图像点击与真实皮肤标记；若只支持其中之一，应在数据集标注中明确，不混合计算成功率。
- 实体画线介质的颜色、宽度、照明与目标肤色分布尚未定义；先用可控仿体采集几类条件，后续选择合适的真实标记方式。
- FR5 与力传感器的控制速率、延迟和可用控制接口未测；第三部分只给设计需求，不宣称已可真实恒力工作。
- 刮痧板具体形状、厚度与工作边缘未知；第二部分先采用参数化工具模型（可替换）而非默认为点状球头。
- **人体皮肤接触力不是本轮所能确定的医疗建议**；只讨论“可控/可测的目标力轨迹”数学与算法，不指定人体试验数值。

---

## 14. 参考文献与核验链接（按文中 ID）

以下链接都用于追溯文献本身；`F/A/T` 的含义见 §2。凡 DOI 不明者不臆造。

- **[L01] [F]** Xu, Q. et al. (2024). *Toward automatic robotic massage based on interactive trajectory planning and control*. *Complex & Intelligent Systems* 10, 4397–4407. https://doi.org/10.1007/s40747-024-01384-5 ；全文：https://link.springer.com/article/10.1007/s40747-024-01384-5
- **[L02] [A]** *Research on Chinese Scraping Automation Based on Sensor-Fusion Method*. *IEEE Sensors Journal* 24(2), 1751–1759 (2024). https://doi.org/10.1109/JSEN.2023.3336220
- **[L03] [A]** Zhao, W. et al. (2023). *Human Back Acupuncture Points Location Using RGB-D Image for TCM Massage Robots*. *International Journal of Robotics and Automation*. https://doi.org/10.2316/J.2023.206-0790
- **[L04] [A]** *Robot Pose Estimation and Normal Trajectory Generation on Curved Surface Using an Enhanced Non-Contact Approach*. *Sensors* 23(8), 3816 (2023). https://doi.org/10.3390/s23083816
- **[L05] [A]** *Physical therapy massage robot system with human motion tracking and finger-pressing force control*. *Advanced Robotics* (2025). https://doi.org/10.1080/01691864.2025.2469695
- **[L06] [A]** Tsai, R. Y. & Lenz, R. K. (1989). *A New Technique for Fully Autonomous and Efficient 3D Robotics Hand/Eye Calibration*. *IEEE Transactions on Robotics and Automation*. https://doi.org/10.1109/70.34770
- **[L07] [A]** Park, F. C. & Martin, B. J. (1994). *Robot Sensor Calibration: Solving AX=XB on the Euclidean Group*. *IEEE Transactions on Robotics and Automation*. https://doi.org/10.1109/70.326576
- **[L08] [A]** Ulrich, M. & Hillemann, M. (2024). *Uncertainty-Aware Hand–Eye Calibration*. *IEEE Transactions on Robotics* 40, 573–591. https://doi.org/10.1109/TRO.2023.3330609
- **[L09] [A]** Park, J., Zhou, Q.-Y. & Koltun, V. (2017). *Colored Point Cloud Registration Revisited*. *ICCV*. https://doi.org/10.1109/ICCV.2017.25 ；CVF：https://openaccess.thecvf.com/content_iccv_2017/html/Park_Colored_Point_Cloud_ICCV_2017_paper.html
- **[L10] [A]** Newcombe, R. A. et al. (2011). *KinectFusion: Real-time dense surface mapping and tracking*. *ISMAR*. https://doi.org/10.1109/ISMAR.2011.6092378
- **[L11] [A]** Newcombe, R. A., Fox, D. & Seitz, S. M. (2015). *DynamicFusion: Reconstruction and Tracking of Non-Rigid Scenes in Real-Time*. *CVPR*. https://doi.org/10.1109/CVPR.2015.7298631 ；CVF：https://openaccess.thecvf.com/content_cvpr_2015/html/Newcombe_DynamicFusion_Reconstruction_and_2015_CVPR_paper.html
- **[L12] [A]** Sanchez, J. et al. (2020). *Robust normal vector estimation in 3D point clouds through iterative principal component analysis*. *ISPRS Journal of Photogrammetry and Remote Sensing* 163, 18–35. https://doi.org/10.1016/j.isprsjprs.2020.02.018
- **[L13] [A]** Khoshelham, K. & Oude Elberink, S. (2012). *Accuracy and Resolution of Kinect Depth Data for Indoor Mapping Applications*. *Sensors* 12, 1437–1454. https://doi.org/10.3390/s120201437
- **[L14] [A]** Border, R. & Gammell, J. D. (2024). *The Surface Edge Explorer (SEE): A Measurement-Direct Approach to Next Best View Planning*. *International Journal of Robotics Research*. https://doi.org/10.1177/02783649241230098
- **[L15] [A]** *A structured review and taxonomy of next-best-view strategies for 3D reconstruction*. *ISPRS Open Journal of Photogrammetry and Remote Sensing* (2025). https://doi.org/10.1016/j.ophoto.2025.100098
- **[L16] [T]** 《基于机器视觉的人体背部穴位识别及三维重建》（2026，学位论文摘要）。https://cnki.istiz.org.cn/kcms/detail/detail.aspx?dbcode=CMFD&dbname=CMFD2026&filename=1025797544.nh
- **[L17] [A]** *Uniform Coverage Tool Path Generation for Robotic Surface Finishing of Curved Surfaces*. *IEEE Robotics and Automation Letters* (2022). https://doi.org/10.1109/LRA.2022.3152695
- **[L18] [A]** Karayiannidis, Y. & Doulgeri, Z. (2010). *Robot contact tasks in the presence of control target distortions*. *Robotics and Autonomous Systems* 58, 596–606. https://doi.org/10.1016/j.robot.2009.12.004
- **[L19] [A]** *Robotic Contact on Complex Curved Surfaces Using Adaptive Trajectory Planning Through Precise Force Control*. *Machines* 13(9), 794 (2025). https://doi.org/10.3390/machines13090794
- **[L20] [A]** *Recognition of Acupoints on Human Back Based on Machine Vision and Deep Learning*. *Big Data and Cognitive Computing* 10(7), 204 (2026). https://doi.org/10.3390/bdcc10070204
- **[L21] [A]** *Quality-Aware Active Re-Observation for Constrained Eye-in-Hand Robotic Manipulators*. *Applied Sciences* 16(18), 9389 (2026). https://doi.org/10.3390/app16189389
- **[L22] [A]** *Improving 3D Reconstruction Through RGB-D Sensor Noise Modeling* (2025). PubMed 摘要：https://pubmed.ncbi.nlm.nih.gov/39943589/

---

## 15. 结论及本轮是否达标

**本轮达成：** 明确人工指定点与实体画线的差异及合流接口；重构可检查的坐标链与误差模型；在至少 20 项可追溯研究线索中识别了直接先例；比较 3 条曲面路线；提出 5 项可证伪假设、4 组主要基线及数据集和消融方案；完成 7 条正式研究决策。

**本轮没有达成，也不宣称达成：** 全文覆盖所有核心文献、设备原始数据、任何误差改善数值、ROS 2 编译通过、实际机械臂闭环控制、论文创新性确认或 GitHub 提交。

**下一轮唯一优先问题**：在同样的数据与视角预算下，*质量驱动的局部曲面/画线 3D 轨迹生成*是否确实比单帧点云+PCA 和常规融合更准确、更稳定，并且产生的法向和工具接触位置误差有可预测的界？

如果答案是否定的，应**主动把算法研究重心转向刮痧板真实接触几何与在线力控**，不要为了维护本轮设想继续增加复杂视觉模块。