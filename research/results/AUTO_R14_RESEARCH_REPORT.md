# AUTO-R14｜FAIRINO FR5刮痧机器人博士级研究报告：独立边缘真值的力学陷阱与校准包络

> 日期：2026-10-20（北京时间）。**真实新增：**连续刚板两支座无法识别边缘载荷的五格反例；双区域校准矩阵不确定性LP；26项新测试、160项历史回归、600组合成对照。**无FR5连接、无真实传感器、无运动指令、无人体实验。**


## 01. 上一轮可访问证据、回归与差分

实际打开R13五份顶层文件（研究、教学、登记、决策、ZIP），解包43个文件；本轮复跑R13 26项、R12 41项、R11/R10 93项，分别全部通过。上一轮关注独立事件锚点、仿射时钟包络；本轮避免重复时间模型，转向更根本的“实验真值是否真能辨别边缘风险”。R13报告、讲义、决策与测试可追溯；本轮尚未重新检查其每条历史外部引文原始PDF。差分：新核读2026 TacGraph全文、2021线接触全文和2019 FEM真值全文；新增刚性板反例；新增静态双通道有界混合LP、物理分区测量退出Gate。

**E14-01（公式与前提）**

$$
G_{14}=G_{\mathrm{files}}\land G_{\mathrm{literature}}\land G_{\mathrm{algorithm}}\land G_{\mathrm{falsifiable}}
$$

**适用条件：**轮次由证据验收而非定时触发计数。

**T14-01（本章决策/证据表）**

|对象|实际状态|与R13差分|
|---|---|---|
|R13报告/讲义/登记/决策/ZIP|五件本地存在且已读取/解包|有可核对历史输入|
|历史测试|26+41+93=160/160重新运行|无新增FR5证据|
|R14新增|26/26新测试、600合成样本|连续板两支座反例与标定LP|

**本章证据边界与决策：**本地文件真实存在、解包与测试日志在results/；无法由上一轮“完成”措辞替代这轮重跑。


## 02. 唯一首选科学问题及退出判据

Q14-A：能否建立对有限宽刮板“左右边缘载荷份额”真正独立、可校准、可跨材料复验的非人体真值？若无此真值，任何RGB-D+腕部F/T估计器都无法证伪。H14-A：相同总力与力矩下，连续板双支座读数不能区分中心载荷与双边缘载荷；严格静力学反例即可推翻“两个称重传感器一定足够”的设计。H14-B：只有独立隔离载荷路径且校准误差包络有效，双区域模型才能给出有意义的边缘比例区间。H14-C：匹配视觉必须显著优于打乱时序/视角的视觉，且胜过仅wrench与简单厂家力控的同条件强对照。备选Q14-B：若可见性/真值/FR5外环长期不可得，优先非人体工作—卸载—离面返程简单鲁棒状态机，不强行创新。

**E14-02（公式与前提）**

$$
H_A:\ \mathrm{Benefit}(\mathrm{matched\ RGBD},\mathrm{wrench})>\mathrm{Benefit}(\mathrm{shuffled\ RGBD},\mathrm{wrench})
$$

**适用条件：**增益必须在同一实体样本、独立边缘真值上比较。

**T14-02（本章决策/证据表）**

|问题|可证伪真值|退出条件|
|---|---|---|
|I-A边缘风险|隔离三分区+独立总力|视觉匹配无增益或不可标定→退出|
|I-B状态机|独立力台与离面轨迹|厂家简单方案已达目标→不做博士创新|

**本章证据边界与决策：**首选只保留一个：边缘风险可辨识与可信拒绝；备选只保留一个：厂家工作—卸载—离面返程。


## 03. 去重来源、访问层级与新颖性筛查

本轮实际去重核查S14-01～S14-12共12项学术/作者/出版商来源，另继承S14-13～S14-16四项历史官方/论文材料。五篇开放全文相关方法及实验重点核读：Ma2021、Sferrazza2019、ViPER2023、Peng2024、TacGraph2026；VPEC仅作者仓库README/许可入口；Son1996、Grinberg2025、IROS2025论文只读摘要，TacO与柔性工具项目只读公开介绍。出版社与作者网站的论文指标不是FR5实测。2026 TacGraph正式DOI从MERL/DBLP核实，避免把arXiv DOI误当出版社DOI。来源S14-14的官方页面本轮遇到429，明确继承历史记录。

**E14-03（公式与前提）**

$$
\mathrm{Level}(\mathrm{full\ text})\ne\mathrm{Level}(\mathrm{code})\ne\mathrm{Level}(\mathrm{physical\ truth})
$$

**适用条件：**访问层级不可互相替代。

**T14-03（本章决策/证据表）**

|来源组|实际读取级别|可用于什么|
|---|---|---|
|S14-01～05|5篇开放全文方法/实验|方法和迁移限制比较|
|S14-06～12|仓库/摘要/项目页|代码资格或研究范围|
|S14-13～16|历史来源继承|保持竞争表，不冒充本轮核读|

**本章证据边界与决策：**本轮有12项新核查独立来源与4项历史继承；全文方法/实验实读5篇，不把项目摘要算全文。


## 04. 全文核读A：Ma2021分布触觉与线接触退化

Ma、Dong、Rodriguez 2021（ICRA，DOI 10.1109/ICRA48506.2021.9561781）通过GelSlim-3.0分布触觉标记跟踪刚体相对运动，再用固定点与固定方向/非穿透约束优化外部接触点或线。全文II节要求刚体、刚性环境、稳定无滑抓取与接触持续；III节包括点接触实物、线接触仿真和实物。最有价值的负结果：作者明确报告实际线接触常退化成点接触，导致假设失效。迁移到柔性刮板时，接触形成会随形变和换向变化，不能在每帧假定持续线接触。对我们而言“识别接触模式并拒绝错误模式”必须有独立几何真值，且分布触觉传感器并非现有Eye-in-Hand RGB-D。

**E14-04（公式与前提）**

$$
(I-R_k)P_b=t_k
$$

**适用条件：**Ma等的刚体固定点接触约束，不能直接用于柔性刮板分布压力。

**T14-04（本章决策/证据表）**

|Ma2021项目|论文真实条件|FR5迁移判断|
|---|---|---|
|固定点/线约束|刚体、稳定抓取、持续接触|柔性刮板不可默认|
|实物线接触|作者报告常退化点接触|必须识别模式并拒绝|
|真值|Vicon/实验几何|不是刮板压力阵列|

**本章证据边界与决策：**重点反证是作者自己的真实线接触失败，说明接触模式变化本身是必须记录的变量。


## 05. 全文核读B：Sferrazza2019的FEM“真值”边界

Sferrazza等2019（IEEE Access，DOI 10.1109/ACCESS.2019.2956882）以Ogden超弹性模型拟合独立材料试验，在Abaqus中模拟13,448次压入并以FEM逐元力作为学习标签；实物F/T只验证求和后的总力。全文III节涉及材料试验，IV节涉及FEM网格、摩擦假设，V节给出压力场相对FEM标签的误差与总力相对F/T误差。其论文清楚说明：F/T不能测量逐元分布。即使总力高度吻合，也可能有两种不同分布产生相同总力；FEM标签是“模型派生参考”，不是独立逐元传感器真值。对FR5博士实验的影响是决定性：不得用自己训练或标定的FEM输出同时充当压力真值与被测算法依据。

**E14-05（公式与前提）**

$$
F_{\mathrm{FT}}\approx\sum_{j=1}^{n}F^{\mathrm{FEM}}_j
$$

**适用条件：**FEM总力与F/T吻合只是积分约束，不保证逐元压力标签真实。

**T14-05（本章决策/证据表）**

|Sferrazza2019|证据|本项目判断|
|---|---|---|
|逐元力标签|材料标定后FEM计算|模型派生标签|
|真实F/T|验证总力求和|不能验证逐元压力|
|模型参数|材料、摩擦、老化|须跨材料与留出|

**本章证据边界与决策：**关键不等价是“逐元FEM标签”与“逐元独立实测”，总力正确不能消除压力分布零空间。


## 06. 全文核读C：ViPER2023与Peng2024的直接竞争

ViPER（Collins等2023）用Eye-in-Hand RGB图像预测软夹爪压力，训练结合Sensel Morph平面压力阵列的强标签与ATI Mini45 F/T弱标签，作者在III–IV节明确F/T只用于训练，推理时用图像。作者比较重新训练的VPEC和VFTS，并按独立测试环境评估；全文IV节坦言指尖遮挡、相似颜色背景和训练外载荷会失败。Peng等ICRA2024将气泡传感器内部RGB-D、气压与薄膜FEM/CVXPY结合，170条作者机器人压入轨迹对比；力预测改善而接触区域分辨存在反例。两者已覆盖“视觉+力弱监督”“物理模型+凸优化”，不能作为我们的原创。ViPER需要可见软夹爪形变；FR5刮板可能遮挡接触线。Peng需要内置相机/气压，不是外部Eye-in-Hand。

**E14-06（公式与前提）**

$$
F_z\approx\int_{\mathcal C}p(x,y)\,\mathrm dA
$$

**适用条件：**ViPER弱标签约束总力，不会唯一确定压力场。

**T14-06（本章决策/证据表）**

|方法|强标签/物理假设|本项目限制|
|---|---|---|
|ViPER2023|Sensel Morph压力+ATI F/T弱标签|软指可见、遮挡失败|
|Peng2024|内部RGB-D+气压+FEM|FR5无内置气泡传感器|
|共同反证|视觉+力+物理约束已有|不能直接称创新|

**本章证据边界与决策：**ViPER文中遮挡失败属于直接迁移风险；Peng内部气泡传感器结构与FR5不兼容。


## 07. 全文核读D：TacGraph2026与2025–2026邻近工作

TacGraph（Van der Merwe等，RA-L 2026，DOI 10.1109/LRA.2026.3653324）使用两枚GelSight Mini、KUKA、已知物体/环境刚性几何、外部视觉可选，以几何一致性、非穿透、接触运动学和力平衡构造因子图。全文III节明确假设单一汇总接触点、无纯接触偶矩；V–VI节用环境中ATI Gamma F/T作训练标签与评价，而非在线算法输入。它直接反证“几何+力平衡+因子图/在线接触点”作为单独原创；但其环境F/T仍只提供合力，不是有限宽压力分布真值。2025柔性工具建模项目与2026 TacO传感器任务基准继续抬高新颖性门槛：有学习和优化并不等于对本任务有可测收益。

**E14-07（公式与前提）**

$$
\hat w=J(c)f,\quad \tau_{\mathrm{contact}}=0\ \mathrm{(TacGraph\ assumption)}
$$

**适用条件：**单点无偶矩的接触模型不适用于未知柔性线接触。

**T14-07（本章决策/证据表）**

|TacGraph要素|全文依据|对刮板的限制|
|---|---|---|
|因子图|几何/非穿透/力平衡|常规融合已有|
|接触模型|单点且无纯偶矩|分布柔性接触不符|
|真实参考|环境ATI Gamma F/T|不是分布压力真值|

**本章证据边界与决策：**TacGraph真实环境F/T验证了合力，未验证有限宽刮板的压力场；无纯偶矩是硬假设。


## 08. 主学术Baseline竞争与厂家工程强Baseline

保留至少六篇候选，按“能否在FR5可用信号上合法、真实、同层复现”评判。VPEC有MIT仓库与评估入口，但权重/170GB数据未取、相机外置；ViPER更接近Eye-in-Hand，论文有强/弱标签设计，但源码许可、模型下载可用性与遮挡迁移未知；TacGraph2026提供接触几何方法学强基线，但依赖两GelSight和刚体单点假设；Jiang IAAC2024 2ms超声润滑、FR5外环权限未核；Arefin2026依赖B-mode/Franka力矩；Foster2026双速率固定限速是简单对照而非本机算法。Peng2024为物理融合直接竞争。FR5厂家FT_Control为独立工程强Baseline，但本机固件、API签名、F/T坐标、控制周期尚未核。此轮将ViPER与TacGraph提高到接触创新审查的优先级，VPEC仅保留离线复现资格领先，不预选主论文。

**E14-08（公式与前提）**

$$
\mathrm{Reproducible}=\mathrm{accessible\ data}\land\mathrm{licensed\ code}\land\mathrm{interfaces}\land\mathrm{independent\ truth}
$$

**适用条件：**论文可读并不等于FR5可复现。

**T14-08（本章决策/证据表）**

|候选|论文层级/代码|FR5与排序决定|
|---|---|---|
|VPEC2022|MIT代码，数据/权重未取|离线资格优先；迁移待定|
|ViPER2023|全文+作者仓库，许可待核|接触同层优先；遮挡高风险↑|
|TacGraph2026|全文/DOI，代码待核|方法学强竞争↑，原样暂缓|
|Jiang2024|历史全文，代码待核|2ms外环未知，条件保留|
|Arefin2026|历史全文，代码待核|B-mode/Franka，原样暂缓|
|Foster2026|历史全文，代码待核|固定限速强对照保留|
|Peng2024|全文，代码未核|物理融合强竞争|
|FR5 FT_Control|厂家接口，实际固件未知|工程强Baseline，未运行|

**本章证据边界与决策：**2025–2026 TacGraph、柔性工具和TacO反证泛化融合新颖性；主论文不能仅凭“可读”锁定。


## 09. 决定性反例：刚性连续板下两支座不提供边缘真值

对跨宽度x∈[-L,L]的刚性连续板，只有位于±L的两个竖直支座时，静力平衡使支座反力由总法向力F及绕中心一阶矩M唯一决定。取五个离散位置[-L,-L/2,0,L/2,L]：中心独占载荷p_A=[0,0,1,0,0]与左右端各一半p_B=[1/2,0,0,0,1/2]均有F=1、M=0，两个支座反力均为[1/2,1/2]；但边缘份额分别0和1。支持矩阵2×5秩为2，边缘风险目标向量[1,0,0,0,1]不在其行空间。**因此，两个称重元件放在同一整块刚性板下面，不是独立边缘压力真值。** 这是R14对R07–R13“左右分段载荷”物理实施条件的决定性修订。

**E14-09（公式与前提）**

$$
R_L=(F-M/L)/2,\quad R_R=(F+M/L)/2
$$

**适用条件：**两支座刚性连续板仅响应总力与一阶矩，符号约定x左右支点为±L。

**T14-09（本章决策/证据表）**

|五格载荷|总力F|一阶矩M|两支座读数|边缘份额|
|---|---:|---:|---|---:|
|中心[0,0,1,0,0]|1|0|[0.5,0.5]|0|
|两边[0.5,0,0,0,0.5]|1|0|[0.5,0.5]|1|

**本章证据边界与决策：**这是构造性反例，不是统计趋势：相同观测对应相差1的边缘风险，直接淘汰一类错误台架。


## 10. 双区域独立载荷的标定不确定性模型

作为独立的小问题，只考虑已知无中间区域载荷的两区域非人体分区台架，令f=[f_L,f_R]≥0、两个独立受力通道y=Cf+e，C是含交叉串扰的静态混合矩阵。每个C_ij的独立标定包络为C0±Δ，每个读数误差为±ε，另有**独立第三方**总力区间[F_lo,F_hi]。要求每一受力区物理隔离、校准跨位置/方向/温度留出、动态未启用、总力仪器不与y共用未经说明的处理链。对五区域全宽刮板不能直接应用该双区域模型；需要三块独立区域（左/中/右）或左右边缘独立测量加独立总力且验证中心不串扰。只在模型前提被实体核实后，LP才可能成为离线判据。

**E14-10（公式与前提）**

$$
y=Cf+e,\quad C_{ij}\in[C^0_{ij}-\Delta_{ij},C^0_{ij}+\Delta_{ij}],\quad |e_i|\le\epsilon_i
$$

**适用条件：**仅两区域、非负法向载荷、静态线性且误差界独立校准。

**T14-10（本章决策/证据表）**

|前提|可接受证据|失效后果|
|---|---|---|
|两独立受力区|物理隔离与逐区标定|中央载荷不可见→HOLD|
|C0±Δ|跨天/位置留出校准|虚假窄区间|
|独立总力|第三方传感器/校准|比例分母不可信|
|静态线性|滞回与频响检查|动态峰值不能证|

**本章证据边界与决策：**两区域静态模型不能代表五区域分布；只有物理隔离且无中心受力的台架才可能适用。


## 11. 区间LP与可执行源码接口

由于f≥0，任一通道在系数包络内可能产生的最小与最大响应分别为(C0−Δ)f、(C0+Δ)f。与观测误差区间相交得到两组线性不等式，连同总力约束构成多面体。为了对r=f_L/(f_L+f_R)求精确极值，设F=f_L+f_R、u=f/F、t=1/F，约束1ᵀu=1及F_lo t≤1≤F_hi t；在(u,t)上两次线性规划即可求上下界。程序`src/segmented_reference.py`只接受已声明的独立校准、独立总力与静态资格；缺任何一项返回HOLD；成功状态PASS_READ_ONLY仅表示模型内区间计算可行，绝不是允许机器人接触的指令。注意独立误差界真实性无法由布尔字段自动验证，必须审查校准报告。

**E14-11（公式与前提）**

$$
(C^0-\Delta)f\le y+\epsilon,\quad (C^0+\Delta)f\ge y-\epsilon
$$

**适用条件：**非负载荷下系数区间的必要充分逐行可行条件。

**T14-11（本章决策/证据表）**

|LP状态|含义|执行许可|
|---|---|---|
|PASS_READ_ONLY|模型内比例区间求解成功|无|
|HOLD|缺标定、模型矛盾或动态条件|拒绝纠偏|
|ANALYSIS_SAFE|示例阈值下区间上界较低|仅离线分类|

**本章证据边界与决策：**LP只验证数学可行域，不能验证声明的Δ是否覆盖真实漂移、滞回与动态误差。


## 12. 600组合成消融、26项新增与160项回归

固定种子20261020、600个人工合成无量纲双区域载荷，真实左份额均匀分布0.02–0.98；标称混合矩阵[[0.93,0.07],[0.06,0.94]]、每项独立系数扰动±0.015、观测误差±0.01、独立总力区间[0.98,1.02]、示例阈值r≤0.65。四方法在完全相同样本上对照：直接把通道比值当载荷份额、已知校准包络的LP、只有总力信息、错误假定无串扰的LP。独立生成器真值有400个满足示例阈值；新LP 600/600真值覆盖、381个分析放行、0个合成错误放行、19个保守拒绝；直接比值414放行、14错误；错误无串扰LP407放行、7错误；仅总力0放行。全部为人工信号，不能外推真实传感器、FR5安全或人体阈值。新增26项测试通过，历史160项回归通过。

**E14-12（公式与前提）**

$$
u=f/F,\quad t=1/F,\quad \mathbf1^Tu=1
$$

**适用条件：**Charnes–Cooper变换将载荷比例界变为LP。

**T14-12（本章决策/证据表）**

|合成方法|分析放行/600|合成错误放行|合成安全样本保守拒绝|
|---|---:|---:|---:|
|通道直接比值|414|14|0|
|校准包络LP|381|0|19|
|仅总力|0|0|400|
|错认无串扰LP|407|7|0|

**本章证据边界与决策：**600组合成样本仅测试实现；0次合成错误放行不能推成真实零风险。


## 13. 非人体独立真值台架的可实施设计

实体台架设计先不连接FR5：台式手动/限位压头与可换非人体弹性仿体，分别测中心、左缘、右缘。第一阶段用单独隔离的三块刚性载荷垫，每块独立力传感器，明确机械间隙/桥接件，外加独立总力计/校准砝码（仅用于静态校准，不假设高频）；对照“整块连续板下两支座”作为失败基线。至少覆盖中心独载、左独载、右独载、对称双边、同合力/同矩异分布、不同角度/材料/温度；每一类跨天留出。测量前后用相机标定刮板实际边缘与可见性，记录膜片是否改变接触。动态阶段必须另用高带宽参考及统一采集时间，不能以低速压力阵列证明换向峰值。此方案为待执行采购/搭建协议，不是已完成的实验。

**E14-13（公式与前提）**

$$
\mathrm{Gate}=\mathrm{isolated\ paths}\land\mathrm{heldout\ calibration}\land\mathrm{bandwidth}\land\mathrm{independent\ total}
$$

**适用条件：**真实台架入门条件；没有物理验证就HOLD。

**T14-13（本章决策/证据表）**

|台架部件|独立证据|不得声称|
|---|---|---|
|隔离L/C/R三区垫|独立加载、跨天标定|尚未搭建|
|独立总力计|校准来源/串扰测试|不等于动态带宽|
|外部RGB-D/标尺|遮挡与边缘几何|不等于接触压力|
|统一采集器|物理同步事件|未验证的时间戳不是真值|

**本章证据边界与决策：**三分区和独立总力仍是未执行的设计，E4未过；没有原始标定数据不能给出误差界。


## 14. FR5坐标、时间、力控、安全与三入口契约

本机未知：FR5固件、SDK实际FT_Control函数签名、ROS2驱动提交、控制器robotTime与F/T原始采样时刻、相机曝光/深度时间域。R12–R13已证明主机发布/到达时间不是采集真值；R14不覆盖此门。坐标链要分别记录基座B、法兰F、刚性标定TCP、柔性实际接触C，六维力/矩须明确参考点、旋转和平移、偏置/重力/动态补偿及误差传播。M1人工选点、M2背部实体画线识别/方向确认、M3解剖标志与专家审核穴位均保留；统一输出有序3D曲线、位置/切向/法向/不确定度/工具姿态/有限宽扫掠资格/工作卸载返程模式；接触前必须用户审核。没有物理门和伦理许可，任何自动纠偏、接触运动、真人试验均禁止。

**E14-14（公式与前提）**

$$
{}^{B}T_{C}={}^BT_F\,{}^FT_{TCP}\,{}^{TCP}T_C
$$

**适用条件：**法兰、标定TCP与真实柔性接触点三者不能混淆。

**T14-14（本章决策/证据表）**

|模块|必需记录|未通过的门|
|---|---|---|
|FR5|固件/SDK/FT_Control原型|E3|
|坐标|B→F→TCP→C及F/T参考点|E3|
|时间|原始帧/曝光/同步事件|E3|
|M1/M2/M3|审核后的有序3D曲线|接触前审批|

**本章证据边界与决策：**ROS2时间语义来自R12历史源码审计；本轮未接FR5，任何阈值/带宽都不称本机实测。


## 15. 预注册公平对照、Gate与独立误差真值

预注册对照：B0厂家FT_Control+简单固定限速/平滑卸载；B1仅腕部F/T总力/力矩；B2 VPEC/ViPER同层可合法复现版本；B3几何+力约束强方法；I-A有可信拒绝的有限宽边缘风险估计。相同仿体、刮板、速度、法向目标、材料、曲率、照明、同步、接触工况、训练/测试划分，按条件配对随机顺序。独立真值为三分区校准力、另一路总力/几何测量，不能使用估计器自身输出。主要指标边缘风险错误分析放行、区间覆盖、保守拒绝、接触线/中心偏差、峰值时间错配；必须同时报告成功率和拒绝率。Gate顺序E0文献/许可、E1数学、E2代码、E3只读本机、E4非人体独立真值、E5低能量受控运动，任一关键前提失败则HOLD并卸载。

**E14-15（公式与前提）**

$$
\mathrm{FA}=\sum_i\mathbf1\{\mathrm{PASS}_i\land q_i>q_{\max}\}
$$

**适用条件：**独立真值判定的错误分析放行数；需同时报告拒绝率。

**T14-15（本章决策/证据表）**

|对照|同条件|独立指标|
|---|---|---|
|B0厂家+简单限速|同仿体/力目标/速度|力台误差、卸载峰值|
|B1 wrench-only|同数据|边缘风险覆盖与拒绝|
|B2视觉压力|同训练/测试划分|独立三区真值|
|I-A匹配与打乱视觉|同wrench/同帧预算|是否有额外信息|

**本章证据边界与决策：**固定安全阈值0.65只属于无量纲合成演示，不可用于人体或FR5控制。


## 16. 决策日志、事实缺口、下轮自适应与验收

ADR14-01淘汰“连续刚板两支座=边缘独立真值”；ADR14-02保留“隔离三分区+独立总力”为首选非人体台架设计（尚未采购/搭建）；ADR14-03两区域LP仅作静态受限模型演示；ADR14-04 ViPER/TacGraph升为接触同层新颖性审查优先，VPEC仍为源码许可资格领先，主论文待定；ADR14-05 Sferrazza式FEM分布标签仅为模型派生参考，不得称独立压力真值；ADR14-06唯一条件性首选I-A保留但E4缺失，备选I-B工作卸载离面返程；ADR14-07时间同步/FR5外环继续HOLD；ADR14-08 M1/M2/M3功能契约不占主创新；ADR14-09不修改旧仓库、不新建仓库。下轮最小解法：获取现有台架/传感器清单及照片、三分区可行的CAD和静态校准原始数据、相机样张、FR5只读SDK/固件清单；如继续缺物理设备，停止把更多合成算法包装为路线突破，改为实物采购与台架验证阻塞报告。

**E14-16（公式与前提）**

$$
\mathrm{E4}=\mathrm{E5}=\mathrm{BLOCKED}\ \mathrm{until\ independent\ physical\ evidence}
$$

**适用条件：**离线数学证书不等于FR5安全许可。

**T14-16（本章决策/证据表）**

|决策|状态|最小反证/补证|
|---|---|---|
|ADR14-01 连续板双支座|淘汰作分布真值|分离载荷路径|
|ADR14-02 隔离三区|条件保留设计|实体校准|
|ADR14-03 双区域LP|仅离线|真实跨条件标定|
|ADR14-04 主论文|待定|代码权重/许可/物理真值|
|ADR14-05 FEM真值|模型派生|逐元独立测量|
|ADR14-06 创新I-A/I-B|条件保留/备选|E4|
|ADR14-07 FR5与同步|HOLD|本机只读核查|
|ADR14-08 三入口|保留|用户审核契约|
|ADR14-09 GitHub|不写|用户指定授权新仓库|

**本章证据边界与决策：**缺实物真值/SDK，维持待定；GitHub零写入。


## 附录A｜全部来源、访问等级与URL

|ID|作者/机构；题名；年份|DOI/编号|获取层级|主要迁移限制|
|---|---|---|---|---|
|S14-01|Ma, Dong, Rodriguez；Extrinsic Contact Sensing with Relative-Motion Tracking from Distributed Tactile Measurements；2021|10.1109/ICRA48506.2021.9561781|开放全文：II–IV方法、点/线接触仿真与实物实验；ICRA元数据独立核对|分布触觉运动场；要求刚体、稳定抓取、保持接触；实物线接触易退化点接触|
|S14-02|Sferrazza, Wahlsten, Trueeb, D’Andrea；Ground truth force distribution for learning-based tactile sensing: a finite element approach；2019|10.1109/ACCESS.2019.2956882|开放全文：III–V材料标定、FEM标签、力传感器验证与实验|FEM标签并非直接测得逐元压力；总力一致不证明分布唯一|
|S14-03|Collins, Houff, Grady, Kemp；Visual Contact Pressure Estimation for Grippers in the Wild (ViPER)；2023|10.1109/IROS55552.2023.10342124|开放全文：III–IV数据、模型、压力真值、消融和局限；作者GitHub README|Eye-in-Hand与F/T弱标签接近；软夹爪形变可见；明确报告指尖遮挡失效|
|S14-04|Peng, Yao, Hauser；3D Force and Contact Estimation for a Soft-Bubble Visuotactile Sensor Using FEM；2024|10.1109/ICRA57147.2024.10610233|开放全文：III–V模型、CVXPY、170条测试轨迹|内部RGB-D/气压+膜FEM；力预测提升并不等于接触区域总提升|
|S14-05|Van der Merwe, Ota, Berenson, Fazeli, Jha；Simultaneous Extrinsic Contact and In-Hand Pose Estimation via Distributed Tactile Sensing (TacGraph)；2026|10.1109/LRA.2026.3653324|开放全文：III–VI单点假设、因子图、独立环境F/T训练/验证、真实实验；DOI由MERL/DBLP核对|两GelSight、KUKA、已知刚性几何；明确单接触点且无纯偶矩，不是分布压力真值|
|S14-06|Grady et al.；Visual Pressure Estimation and Control for Soft Robotic Grippers (VPEC)；2022|10.1109/IROS47612.2022.9982073|作者README、MIT LICENSE入口；本轮未下载权重/170GB数据、未运行原网络|外置相机软夹爪；源码入口较明确但FR5可见性未知|
|S14-07|Son, Cutkosky, Howe；Comparison of contact sensor localization abilities during manipulation；1996|10.1016/0921-8890(95)00068-2|出版社摘要与元数据；非全文|阵列与F/T定位受校准、瞬态、剪切影响|
|S14-08|Yuan et al.；GelSight: High-Resolution Robot Tactile Sensors for Estimating Geometry and Force；2017|10.3390/s17122762|开放网页正文背景及方法概要；未逐节核读实验|内部形变成像与外部Eye-in-Hand不同|
|S14-09|Grinberg, Ding；Contact Sensing via Joint Torque Sensors and a Force/Torque Sensor for Legged Robots；2025|10.1109/CASE58245.2025.11164031|arXiv/IEEE摘要；非全文|需要关节扭矩感知；不能迁移性能到FR5|
|S14-10|作者待进一步核实；Tactile-based force estimation for interaction control with robot fingers；2025|10.1109/IROS60139.2025.11246923|IEEE摘要/元数据；非全文|外部F/T验证总力，不能自动充当分布压力真值|
|S14-11|Zorin et al.；TacO: Benchmarking Tactile Sensors for Object Manipulation；2026|待核实|项目摘要；未核读完整论文|传感器选择依任务/摩擦/材料，不能把更多模态视为必然获益|
|S14-12|Van der Merwe et al.；Estimating Deformable-Rigid Contact Interactions for a Deformable Tool via Learning and Model-Based Optimization；2025|待核实|作者项目方法与实物评价概要；非全文|柔性工具与环境F/T评估，构成工具接触新颖性反证|
|S14-13|FAIR-INNOVATION；frcobot_ros2 V4.0.0固定提交；2026审计|无|继承R12源码审计；本轮未连设备/未重核本机版本|消息发布时间不等于F/T采集时间；FT_Control固件权限未知|
|S14-14|Jiang et al.；Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment；2024|10.3390/act13020062|R04历史全文；本轮官方网页429，未重新核读|IAAC 2ms/润滑超声；FR5外环不可假定|
|S14-15|Arefin, Tiryaki；Contact-Aware Impedance Controller for Robot-Assisted Ultrasound Imaging；2026|arXiv:2609.34400|R06历史全文记录，本轮未重核全文|B-mode、Franka力矩权限与FR5不符|
|S14-16|Foster et al.；双速率力—图像控制研究（正式题名待本轮补证）；2026|arXiv:2610.05839|R08/R11历史全文记录，本轮未重核全文|固定限速强基线，复杂预测收益不应预设|

- **S14-01**：Extrinsic Contact Sensing with Relative-Motion Tracking from Distributed Tactile Measurements；2021；10.1109/ICRA48506.2021.9561781；来源：https://arxiv.org/html/2103.08108；读取：开放全文：II–IV方法、点/线接触仿真与实物实验；ICRA元数据独立核对；限制：分布触觉运动场；要求刚体、稳定抓取、保持接触；实物线接触易退化点接触。
- **S14-02**：Ground truth force distribution for learning-based tactile sensing: a finite element approach；2019；10.1109/ACCESS.2019.2956882；来源：https://arxiv.org/html/1909.04000；读取：开放全文：III–V材料标定、FEM标签、力传感器验证与实验；限制：FEM标签并非直接测得逐元压力；总力一致不证明分布唯一。
- **S14-03**：Visual Contact Pressure Estimation for Grippers in the Wild (ViPER)；2023；10.1109/IROS55552.2023.10342124；来源：https://arxiv.org/html/2303.07344；读取：开放全文：III–IV数据、模型、压力真值、消融和局限；作者GitHub README；限制：Eye-in-Hand与F/T弱标签接近；软夹爪形变可见；明确报告指尖遮挡失效。
- **S14-04**：3D Force and Contact Estimation for a Soft-Bubble Visuotactile Sensor Using FEM；2024；10.1109/ICRA57147.2024.10610233；来源：https://arxiv.org/html/2310.11372；读取：开放全文：III–V模型、CVXPY、170条测试轨迹；限制：内部RGB-D/气压+膜FEM；力预测提升并不等于接触区域总提升。
- **S14-05**：Simultaneous Extrinsic Contact and In-Hand Pose Estimation via Distributed Tactile Sensing (TacGraph)；2026；10.1109/LRA.2026.3653324；来源：https://arxiv.org/html/2512.23856；读取：开放全文：III–VI单点假设、因子图、独立环境F/T训练/验证、真实实验；DOI由MERL/DBLP核对；限制：两GelSight、KUKA、已知刚性几何；明确单接触点且无纯偶矩，不是分布压力真值。
- **S14-06**：Visual Pressure Estimation and Control for Soft Robotic Grippers (VPEC)；2022；10.1109/IROS47612.2022.9982073；来源：https://github.com/Healthcare-Robotics/VPEC；读取：作者README、MIT LICENSE入口；本轮未下载权重/170GB数据、未运行原网络；限制：外置相机软夹爪；源码入口较明确但FR5可见性未知。
- **S14-07**：Comparison of contact sensor localization abilities during manipulation；1996；10.1016/0921-8890(95)00068-2；来源：https://doi.org/10.1016/0921-8890(95)00068-2；读取：出版社摘要与元数据；非全文；限制：阵列与F/T定位受校准、瞬态、剪切影响。
- **S14-08**：GelSight: High-Resolution Robot Tactile Sensors for Estimating Geometry and Force；2017；10.3390/s17122762；来源：https://doi.org/10.3390/s17122762；读取：开放网页正文背景及方法概要；未逐节核读实验；限制：内部形变成像与外部Eye-in-Hand不同。
- **S14-09**：Contact Sensing via Joint Torque Sensors and a Force/Torque Sensor for Legged Robots；2025；10.1109/CASE58245.2025.11164031；来源：https://arxiv.org/abs/2510.10843；读取：arXiv/IEEE摘要；非全文；限制：需要关节扭矩感知；不能迁移性能到FR5。
- **S14-10**：Tactile-based force estimation for interaction control with robot fingers；2025；10.1109/IROS60139.2025.11246923；来源：https://ieeexplore.ieee.org/document/11246923/；读取：IEEE摘要/元数据；非全文；限制：外部F/T验证总力，不能自动充当分布压力真值。
- **S14-11**：TacO: Benchmarking Tactile Sensors for Object Manipulation；2026；待核实；来源：https://www.max-robotics.com/en/research/publications/cmptve5jb0081bjb2ysb6uik9；读取：项目摘要；未核读完整论文；限制：传感器选择依任务/摩擦/材料，不能把更多模态视为必然获益。
- **S14-12**：Estimating Deformable-Rigid Contact Interactions for a Deformable Tool via Learning and Model-Based Optimization；2025；待核实；来源：https://deform-rigid-contact.github.io/；读取：作者项目方法与实物评价概要；非全文；限制：柔性工具与环境F/T评估，构成工具接触新颖性反证。
- **S14-13**：frcobot_ros2 V4.0.0固定提交；2026审计；无；来源：https://github.com/FAIR-INNOVATION/frcobot_ros2；读取：继承R12源码审计；本轮未连设备/未重核本机版本；限制：消息发布时间不等于F/T采集时间；FT_Control固件权限未知。
- **S14-14**：Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment；2024；10.3390/act13020062；来源：https://doi.org/10.3390/act13020062；读取：R04历史全文；本轮官方网页429，未重新核读；限制：IAAC 2ms/润滑超声；FR5外环不可假定。
- **S14-15**：Contact-Aware Impedance Controller for Robot-Assisted Ultrasound Imaging；2026；arXiv:2609.34400；来源：https://arxiv.org/abs/2609.34400；读取：R06历史全文记录，本轮未重核全文；限制：B-mode、Franka力矩权限与FR5不符。
- **S14-16**：双速率力—图像控制研究（正式题名待本轮补证）；2026；arXiv:2610.05839；来源：https://arxiv.org/abs/2610.05839；读取：R08/R11历史全文记录，本轮未重核全文；限制：固定限速强基线，复杂预测收益不应预设。


## 附录B｜可执行算法的完整前提和可复现命令

- 代码：`src/segmented_reference.py`、`src/reference_geometry.py`、`src/run_synthetic.py`。
- `PYTHONPATH=src python -m unittest discover -s tests -v`：新增26项测试。
- `PYTHONPATH=src python src/run_synthetic.py`：600组合成消融，固定种子20261020。
- 历史回归：R13 26、R12 41、R11/R10 93，合计160项；详见`results/`。程序不包含FR5控制指令。
- `DATA_MANIFEST.json`列出数据真值来源：只有人工合成，没有任何真实传感器CSV；`NONHUMAN_BENCH_CALIBRATION_TEMPLATE.csv`仅含表头，不代表采集数据。

**严格限制：**程序`calibrated=True`等参数仅为调用者声明，无法自己证明实体标定已经完成。真实使用必须先以独立原始数据证明标定界、机械隔离、总力独立性、静态假设与留出覆盖。两区模型只适用“中间无受力”的受限测试件，不能推广成有限宽五格分布。


## 附录C｜下一轮最小阻塞清单

1. 非人体三分区机械结构照片/CAD、实际受力隔离路径、标定器证书与原始静态数据。
2. 刮板材质、有效宽度、柔性边缘实际变形的可见RGB-D图像与标尺/独立几何真值。
3. FR5本机SDK/固件版本、厂家FT_Control真实签名、F/T原始坐标与采集时间语义。
4. 压力阵列/力台实际逐通道响应、串扰、同步、动态频响；未核之前仅静态验证。
5. 任何人体实验必须另行伦理与风险批准；当前无批准也无此计划。

## 附录D｜严格数学证明：两支座、部分可辨识与有界混合

**D1 刚性连续板。** 离散压力列向量p∈R⁵₊、节点坐标x=[−L,−L/2,0,L/2,L]。总力F=1ᵀp，绕中心一阶矩M=xᵀp。连续刚板在两端支座±L的反力R=[R_L,R_R]ᵀ满足R_L+R_R=F，L(R_R−R_L)=M，故R=Ap，A的第j列为[(1−x_j/L)/2,(1+x_j/L)/2]ᵀ。A的行空间由常数向量和x向量张成。边缘风险q=[1,0,0,0,1]不在此行空间：若q=a·1+b·x，则中心点要求a=0；两端同时要求a−bL=1与a+bL=1，矛盾。因而存在非零v∈ker(A)使qᵀv≠0。显式见证p_A=[0,0,1,0,0]、p_B=[1/2,0,0,0,1/2]，Ap_A=Ap_B=[1/2,1/2]，qᵀp_A=0而qᵀp_B=1。此证明仅用准静态、连续刚板、两端支撑、竖直载荷；真实柔性接触的更多复杂性不会让这套相同观测自动具有更高空间信息量。

**D2 两区域区间混合。** 对单次静态试验，真实f≥0，测量y=Cf+e，独立逐项C_ij∈[C⁰_ij−Δ_ij,C⁰_ij+Δ_ij]、e_i∈[−ε_i,ε_i]。对固定f，因非负性，行i可实现的最小与最大预测分别是(C⁰−Δ)_i f和(C⁰+Δ)_i f。测量误差使可解释的预测区间为[y_i−ε_i,y_i+ε_i]；两个闭区间相交，当且仅当(C⁰−Δ)_i f≤y_i+ε_i且(C⁰+Δ)_i f≥y_i−ε_i。每行不确定参数允许独立选择时，这两个不等式对“存在一个相容C,e”也是充分条件；若各通道误差存在共同参数/跨样本共享标定参数，单样本包络仍是外包，但多样本联合可行域需另外建模。

**D3 分式线性规划。** 若F=f_L+f_R∈[F_lo,F_hi]且F_lo>0，设u=f/F、t=1/F，则u≥0、u_L+u_R=1、t∈[1/F_hi,1/F_lo]。将上述线性测量约束乘t，可得(C⁰−Δ)u−(y+ε)t≤0、−(C⁰+Δ)u+(y−ε)t≤0；总力界等价于F_lo t≤1≤F_hi t。此时r=f_L/F=u_L，分别最小化/最大化u_L即为精确的受限模型比例区间。若真实F可能为0，则比例未定义，必须HOLD；若C中出现负灵敏度或静态线性模型失效，当前实现也HOLD。

**D4 独立性不是程序自动生成的。** 独立真值至少包含：与被测视觉算法训练标签相隔离的三分区力测量、独立总力链、机械加载位置/姿态真值，以及动态实验时的独立采集时基。所谓“外部力台”只验证总力，并不能替代左右中心分布测量；即使力台与腕部F/T数值一致，也不能证明边缘份额唯一。

## 附录E｜逐项源码与数据审计说明

- `src/reference_geometry.py`：两支座反力映射与边缘目标行空间检验，测试包含显式相同观测不同边缘风险。
- `src/segmented_reference.py`：2×2静态C区间、通道误差、独立总力界、Charnes–Cooper LP与HOLD门。
- `src/run_synthetic.py`：600组固定种子、四种同条件对照，输出JSON含每组方法放行/错放/拒绝数。真值仅由生成器构造。
- `tests/`：26项新增单元测试；`prior/`保留R13压缩包的解压副本及历史代码，R13/R12/R11/R10实际重新运行日志位于`results/`。
- **没有任何实物测量CSV**。校准CSV只有表头，`DATA_MANIFEST.json`明确`physical_measurements=0`。
- 代码无ROS2节点、无机器人IP、无FT_Control调用、无运动命令。即使数学上分析放行，也不可直接接入FR5控制环。