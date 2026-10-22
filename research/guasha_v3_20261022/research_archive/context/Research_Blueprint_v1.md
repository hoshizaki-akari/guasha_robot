# Research Blueprint v1.0 — 从零启动（2026-10-15）

## 0. Project charter

- 研究领域：机器人连续柔性接触操作；刮痧作为代表性滑动工具实验任务。
- 实验系统：FAIRINO FR5, eye-in-hand depth camera, six-axis F/T sensor, Ubuntu 22.04, ROS 2 Humble + GPU。
- 总目标：经可信实验验证的多模态在线接触状态估计与带安全约束的自适应运动/力控制方法；不预设一定发表顶会/顶刊。
- 科研成果单位：可复现实验报告 + 一项明确理论/方法贡献 + 真实 FR5 柔性仿体验证，逐步延伸为多论文体系。
- 优先原则：可证伪、可复现、可测量、安全性先行；不照搬以前的算法设计。

## 1. 核心科学问题与可证伪假设

**Q1. 状态可观测性：** 深度测量在接触处因末端工具遮挡而缺失，且深度传感器估计的是受形变的可见表面。给定机器人状态、F/T、历史 RGB-D，何时能同时辨识局部法向、接触起始点及等效刚度？何时不能？

**H1**：融合历史几何先验与连续接触 F/T、控制输入的估计器，在未知曲率、未知刚度的仿体上，比单纯几何投影/力觉反馈具有更小的局部法向误差与接触状态估计误差（需使用独立参考测量验证）。

**Q2. 受限控制：** 接触力受法向渗入量、速度、刚度、黏弹阻尼与摩擦影响。如何在模型误差与通信延迟下保持轨迹覆盖质量同时控制法向力超调及剪切风险？

**H2**：带有在线参数置信区间、预测安全过滤和可行性降级逻辑的控制器，在相同任务速度/覆盖率约束下，相比经典位置+法向导纳和标称 MPC 更低的超限频率/峰值力（统计检验）。

**Q3. 迁移泛化：** 学到的柔性接触模型能否跨工具形状、材料刚度、润滑程度与曲率变化复用？

**H3**：在线低维物理参数估计与不确定性门控策略比纯离线拟合模型在 OOD 仿体上更稳定，且所需校准探测次数更少。

> 这些是**待验证的科研假设**，不是现有方法尚未解决的已证事实。2023 TRO 和 2025 BIR 已研究软接触 MPC 和参数估计，必须严肃比较。

## 2. 状态、接触物理与测量模型（起始假设而非最终模型）

机器人状态 q, dq；手眼已标定的点云提供局部接触面模型 S(p), 估计法向 n(p) 及几何协方差 Sigma_g；传感器提供工具/基坐标统一后的净力矩 w = [f;tau]（包括偏置/工具重力补偿与延迟）。

沿局部法向定义接触压入量 delta = max(0, -phi(q,S)), 则一个**待辨识的简化局部模型**为：

  f_n = k_n delta + c_n max(0, d(delta)/dt) + epsilon_f,
  ||f_t|| <= mu f_n  (静摩擦上界；滑动摩擦另行识别).

这里 k_n, c_n, mu 是区域/速度相关的等效参数，接触面积和工具姿态可影响它们；并不声称它是皮肤的完整本构模型。真实模型可比较 Kelvin–Voigt、Hunt–Crossley、Hertz 和数据驱动残差。需要通过可观测性/可辨识性分析决定哪些参数能同时估计。

视觉观测 z_v = h_v(S, T_base_camera, q) + noise; 力觉观测 z_w = h_w(delta,d_delta,theta,n, tool geometry) + noise. 设 belief b_t = (xhat_t, thetahat_t, uncertainty_t, contact_mode_t)。

重点注意：F/T 在工具坐标系测的是**合力/合矩**，与局部接触压力峰值并非同一量；接触可能不止一处；传感器带宽、延迟和标定误差必须记录。不可据此宣称“皮肤压力安全”。

## 3. 候选算法与选择闸门

A. **基线**：RGB-D 曲面重建 + 常规轨迹生成 + 正交位置/力混合控制或导纳控制。优点易落地，是必备控制组。

B. **候选主线 P1**：时序接触状态估计（EKF/UKF/因子图/递推最小二乘，结合 RGB-D 几何先验、F/T、接触模式） + 不确定性区间 + 约束预测控制或安全过滤。选择条件：证明可辨识性与实时性；限定已验证的安全性质，不能随意宣称 CBF 形式保证真实人身安全。

C. **候选探索线 P2**：基于模型残差学习或置信度门控的模型更新。优先用离线数据和安全仿体实验；只有 B 的模型误差成为主导瓶颈后才投入 RL 等复杂学习。

D. **可选高级策略**：通过视觉/力觉信息增益选择下一次扫描或安全探测位置，解决 eye-in-hand 遮挡和局部材料未知。首先证明比简单定期扫描策略有效。

### 初版优化问题

At each control tick, choose tangential progression and normal offset u over horizon H:

  min_U Σ [ w_f ||f_n^pred - f_ref||² + w_s ||s - s_ref||² + w_u ||Δu||² + w_o * visibility_penalty ]

  s.t. robot kinematic limits, velocity/acceleration limits, workspace and collision constraints,
       upper_bound(f_n^pred | theta in Theta_t, latency in D_t) <= F_cap(t, region),
       model validity conditions, stop and retreat feasibility.

F_cap 不是本文给出的通用“安全牛顿数”；只能从已批准的仿体实验设计/独立危险分析导出。若不确定性过大、优化不收敛、传感器过期或观测冲突，退回低能量保持或受控撤离；实际停机由独立安全回路保证。

**稳定性问题**：若力控与未知环境相互作用，研究被动性/能量储罐、采样与延迟对接触能量的影响。只对严格陈述的模型、输入边界、延迟界给出理论保证。

## 4. 三篇潜在论文单元（非承诺投稿）

**Paper A — Perception / Estimation**
- 联合估计局部表面法向/接触模式/等效刚度时的不确定性与可观测性；跨曲面/材质比较视觉、力觉和融合估计。
- 贡献检验：存在不可辨识工况时主动报告置信区间而不是强行估计。

**Paper B — Control / Safety**
- 基于置信集的预测运动-力协同控制，考虑几何误差、柔性参数时变、信息延迟与控制器降级。
- 贡献检验：与标准导纳、混合力控、TRO-2023 接触 MPC 和 BIR-2025 交互模型框架对比；严谨报告性能-保守性取舍。

**Paper C — Generalization / Autonomous task**
- 跨工具与材质适配；联合几何采样/安全接触路径优化，测量信息收集代价；评估端到端覆盖质量。

如果 A 无法提供明显的独创性，就并入 B 的状态估计模块；如果 B 的严谨实验已形成单篇论文，优先尽早完成投稿级结果，不强行追求三篇。

## 5. 里程碑和不通过时的决策

| Gate | 必须交付的实物证据 | 不达标策略 |
|---|---|---|
| G0 研究界定 | 按 PRISMA 风格留痕的筛选库；近邻研究复现计划；科研创新矩阵 | 缩窄贡献，不以「融合」为创新点 |
| G1 工程基准 | 相机与 F/T 时间戳、坐标标定、可回放 rosbag、真实循环延迟测试 | 不启动接触闭环 |
| G2 可重复仿真 | >=3 类模型、固定种子、多参数扰动的 baseline 测试 | 先修复模型与测量链 |
| G3 仿体硬件 | 经审核的停止功能、受限力、不同曲率/材料的验证 | 回退离线或非接触测试 |
| G4 创新验证 | 明确统计改进、消融/置信区间、算力开销、失败案例 | 重新估计研究空白 |
| G5 论文成果 | 完整公式、理论/适用条件、可复现实验、代码/数据说明 | 降低宏大叙述，提升证据质量 |

## 6. 首个工作包 / 近期可执行任务

1. 向硬件管理员记录 FR5 是 V6 或其他版本、控制器固件、SDK/ROS 驱动版本、机械臂真实可用控制模式；留日志。已发现官方社区有控制接口兼容性反馈（不代表必然复现）。
2. 确定深度相机品牌型号、F/T 品牌型号、额定量程、驱动、采样率和末端工具质量/几何尺寸。
3. 完成相机内参、手眼外参、TCP、重力补偿/传感器零偏、时间同步和 TF frame 检查；确定深度遮挡盲区。
4. 建立只读/空载的 rosbag 和端到端延迟分布日志；在仿体/无接触状态验证急停和受控退避。
5. 建立至少三种 baseline 和独立参考测量方式，之后才研究新控制律。

## 7. 预期硬件架构

RGB-D (eye-in-hand) -> camera driver -> surface map + normal/uncertainty estimate
                             \-> calibrated TF + timestamp align
6-axis FT -> sensor driver -> bias/gravity compensation -> force/contact state estimate
robot joint states ------------------------------------------/
state belief -> planner (non-contact paths) -> safety gate -> validated FR5 driver
                                  \-> offline logs -> analysis / dataset / CI
external, independent physical E-stop -> hardware safety chain (not ROS dependent)

**No real robot command code is included in this starter.**