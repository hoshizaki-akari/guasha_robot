# AUTO-R46｜FAIRINO FR5刮痧机器人博士级算法研究完整报告

**主题：空间压力“真值”来源核验 + 同一腕部wrench下局部载荷不可辨识的最坏情形量化。**

**验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY；主论文Baseline待定；所有实体/人体/运动Gate HOLD。** 日期：2026-10-21。全部新数字为离线人工数学测试。

## 研究执行摘要

本轮实际核验R45研究归档，核读VPEC/ViPER/FEATS三篇论文方法、数据与实验，严格区分VPEC/ViPER物理压力阵列、ViPER弱F/T标签和FEATS有限元伪标签。建立已知纯力偶下的精确三箱载荷可行线段、逐箱上下界及最坏空间误差，独立LP核验100组；512组同信息人工消融证明三种点估计聚合wrench残差都近零，但空间MAE明显不同。R39–R45回归435项+新增253项=**688/688**通过。ViPER研究审计4→2，FEATS2→3，ImplicitRDP3→4，主学术Baseline继续待定。

## 01｜上一轮归档、测试与真正的新旧差分

R45的核心结论是ImplicitRDP官方eval会进入Flexiv runner、dill/pickle加载需要安全审查，VRR由全局wrench确定性变换不能增加局部压力信息。本轮不重复VRR数值实验，而是检查真正影响主论文选择的空间压力标签来源：VPEC、ViPER、FEATS究竟使用独立物理压力真值还是模型生成伪标签。实际读取R45报告、零基础讲义、五候选矩阵、12项决策、源码、测试和失败清单，核对外部ZIP SHA与37条内部SHA256。

R45本轮可访问的测试R39–R45共435项重新运行，另新增253项数学/来源Gate测试，合计688项有效通过。上一轮的435是R45当时执行数，本轮的688是实际本轮执行数，不能把两者相加当新增研究。差分是：从策略模型安全入口审计，推进到物理空间真值与同wrench零空间的证据级判定；新增固定版本仓库核验、独立LP oracle、元数据Gate和512例公平消融。

**公式 F46-01**

$$
\mathrm{SHA256}(ZIP_{45})=\texttt{c8369b3f74e1029a668f8673cc5d16e0b71c2b12fd9fba1bad9cec0eab212b4e}
$$

**表 T46-01**

|审计对象|R46实际读取|边界|
|---|---|---|
|R45归档|38成员、37内部SHA、CRC和外部SHA一致|只证文件完整|
|历史回归|R39–R45共435项本轮重新执行|不声称R30–R38执行|


**可撤销决策与来源**：D46-01; S46-01/02/03。

## 02｜两个核心决策问题与预注册反证条件

Q1：VPEC、ViPER和FEATS公开的“pressure ground truth”是否具有相同证据地位？判据是标签是否由独立物理空间传感器直接产生、是否保留校准/面积/时钟/坐标、是否与训练/预测过程独立。Q2：若网络预测满足腕部合力与力矩，是否足以支持刮板左右偏载纠偏？判据是给出同一wrench但不同局部支持的构造性反例和独立LP极值。

预注册H1：三箱非负法向模型即使F/q/c无误，局部压力仍非唯一；若有真实额外分段载荷传感器或可证明的接触支持约束，结论需重审。H2：FEA监督标签不能直接替代独立物理压力真值；若作者公开独立逐格物理测量与盲检协议，候选可重新提升。Gate入口为真实原始压力/许可/标定，退出条件为长期无法取得非人体独立真值或同信息强基线不改善。

**公式 F46-02**

$$
H_1:\;\mathrm{MAE}_{local}>0\;\text{can coexist with}\;\|\Delta W\|=0
$$

**表 T46-02**

|问题|独立判据|可推翻条件|
|---|---|---|
|Q1：标签可信|原始物理空间传感器及校准|取得FEATS独立逐格物理盲检|
|Q2：聚合指标|相同F/q不同p的解析反例|存在额外独立局部观测|


**可撤销决策与来源**：D46-02/03/06; S46-01/02/03。

## 03｜检索范围、阅读层级与许可审计

本轮筛查12个去重外部来源组，其中VPEC2022、ViPER2023、FEATS2025三篇论文打开全文并核读方法、数据采集、训练、评估与局限。官方GitHub只读固定VPEC commit 371b9ec、ViPER commit 08c7d2f、FEATS commit 7d1ae43；读取README及VPEC downloader具体blob，确认FEATS/VPEC MIT，ViPER公开根目录没有LICENSE且GitHub许可证字段未返回SPDX。公开仓库能访问不等于取得复制/再分发许可。

额外访问FEATS Hugging Face数据卡3.98GB、作者项目、PressureVision作者仓库、Zenodo IntelliMan数据、FAIRINO官方SDK和RSS RDP题录。PressureVision是人手压力数据，Zenodo为人体手指触觉事件与腕部F/T参考，均不满足非人体刮板独立空间真值。全部原始模型/大数据未下载，绝不声称重训或复现。来源详情见SOURCE_REGISTER，每项明确A/B及事实缺口。

**公式 F46-03**

$$
N_{source}=12,\quad N_{fulltext}=3
$$

**表 T46-03**

|层级|本轮对象|不能宣称|
|---|---|---|
|A|VPEC、ViPER、FEATS论文方法/实验|没有运行原论文模型|
|B|9个项目/仓库/数据/题录组|不能当论文全文|


**可撤销决策与来源**：D46-02/10; S46-01..12。

## 04｜VPEC全文：真正的空间压力阵列和实验局限

VPEC通过四个RGB视角观察柔性夹爪接触平面，将Sensel Morph 23×13cm、185×105压力阵列测量投影到图像，训练单帧压力图网络。论文记载每种夹爪约一小时、96个动作/光照序列，按序列留出20%测试而不是纯随机帧；这是比随意混帧更合理的划分，但尚未证明对材料、曲率和遮挡留出泛化。论文Table I报告腱驱动/气动夹爪接触IoU 73.8%/63.3%、压力MAE 5.3/9.7Pa，不能套用FR5。

VPEC作者Stretch RE1用压力图积分成总力，以简单bang-bang和视觉伺服控制接触、轨迹与抓取；这些是作者实验，非本项目控制接口。Sensel是物理空间压力参考，优于FEA伪标签作为验证目标，但当前只有README称170GB数据和下载脚本，尚未核原始校准、时间同步、实际可下载分片、压力单元面积和逐episode盲检。VPEC保留审计第一，绝不声称已在FR5复现。

**公式 F46-04**

$$
F_z=\sum_{i,j}P_{ij}A_{ij}
$$

**表 T46-04**

|项目|作者报告|FR5迁移限制|
|---|---|---|
|真值|Sensel Morph 185×105，约100Hz|需新刮板几何/校准|
|视觉|四RGB 30Hz，约650k帧|非D435 Eye-in-Hand|


**可撤销决策与来源**：D46-04; S46-01/04。

## 05｜ViPER全文：强/弱标签分离与测试误读

ViPER采用Eye-in-Hand RGB，ResNeXt-50特征、压力9离散箱、F/T辅助回归与对抗域适应；论文训练200k次，批次14强标签+14弱标签，λ1=5e-3、λ2=1e-3。作者明确指出压力到wrench是多对一映射，F/T弱标签可帮助训练，但不能唯一恢复压力图。训练有47k同时含Sensel压力+ATI F/T的fully-labeled图像，另有275k仅F/T的weakly-labeled图像。

论文在真实家庭测试上报告fully-labeled的ViPER volumetric IoU 40.3% vs 同数据重训VPEC 28.9%；weakly-labeled的接触准确率92.9%、F/T RMSE 1.57N/0.19Nm。但weak接触真值是以3N腕部阈值定义，不是逐像素压力真值；不能把weak set指标转述成空间偏载精度。论文还明确承认遮挡、同色背景、未见非指尖受力等失败。ViPER Eye-in-Hand视角更接近D435，因而升至第二审计位；然而源码许可证未证，不能复制代码。

**公式 F46-05**

$$
\mathcal L=\mathcal L_p+\lambda_1\mathcal L_{FT}+\lambda_2\mathcal L_{domain}
$$

**表 T46-05**

|数据层|有无独立空间压力|允许指标|
|---|---|---|
|fully labeled 47k|Sensel+ATI+RGB|空间IoU/接触|
|weak 275k|只有ATI+RGB|wrench/弱接触|


**可撤销决策与来源**：D46-05/09/10; S46-02/05。

## 06｜FEATS全文：FEA标签与独立物理真值的差别

FEATS在CNC设备上让GelSight Mini接触24种3D打印压头，14279个样本中19种压头训练、5种测试。其Neo-Hookean材料模型通过CalculiX有限元计算接触力场，再将网格投影、分箱为24×32×3每格力标签，U-Net从GelSight内部RGB预测这些标签。论文III-C明确把空间标签称为FEA计算出的近似ground truth；物理RESENSE HEX-21用于材料参数拟合与总力对照，不能替代每个网格的独立力台真值。

论文已见压头1068测试、未见压头2789测试；z总力MAE分别0.7994N、1.1862N；跨另一GelSight传感器初始z总力误差2.99N，加入4400多传感器样本后降到1.79N。模型在作者Quadro RTX5000报告平均4.1746ms推理，不能当双3090或FR5外环实测。FEATS MIT代码和HF MIT 3.98GB数据是有价值的FEA先验学习资源，但在本项目独立物理压力Gate中不能取代VPEC/ViPER的Sensel参考；因此由第二降至第三。

**公式 F46-06**

$$
U=C_{10}(\bar I_1-3)+D_1^{-1}(J-1)^2
$$

**表 T46-06**

|数据/实验|作者报告|证据性质|
|---|---|---|
|空间标签|CalculiX FEA→24×32×3|模型派生|
|F/T验证|RESENSE HEX-21总力/材料拟合|非逐格真值|


**可撤销决策与来源**：D46-03/09; S46-03/06/07/08。

## 07｜三论文方法、实验、标签、迁移的公平比较

三篇都属于“从图像预测空间压力”，但图像位置与标签来源不同：VPEC外部相机+平面物理阵列；ViPER Eye-in-Hand相机+一部分物理阵列、一部分腕部弱标签；FEATS是光学触觉传感器内部成像+有限元空间标签。它们不能用不同数据集上的IoU、Pa MAE、网格N MAE、F/T总力误差做横向胜负表。特别是FEATS每网格N与VPEC图像Pa单位不同，不能直接相减。

针对FR5，真正可迁移的不是作者控制器，而是评估设计：独立空间参考、按完整episode/材料/曲率/遮挡留出、保留原始校准与像素面积、同步外部wrench、区分法兰/刚性TCP/柔性实际接触线。ViPER视角较近但授权未知；VPEC真值好但数据大；FEATS许可清晰但标签是模型先验。新颖性边界已被三篇覆盖泛视觉压力估计，博士创新应聚焦有限宽接触可辨识与可信拒绝。

**公式 F46-07**

$$
\mathrm{rank}(H_{FT})<\dim(p)\quad\text{for distributed contact}
$$

**表 T46-07**

|论文|监督标签|FR5最小迁移证据|
|---|---|---|
|VPEC|真实平面压力阵列|合法小episode+标定|
|ViPER|真实压力+F/T弱标签|许可+fully-test|
|FEATS|FEA空间伪标签|独立局部物理真值|


**可撤销决策与来源**：D46-03/04/05/09; S46-01/02/03。

## 08｜坐标、时间、单位和真实接触线合同

压力Pa=N/m²，压力阵列第i格转换力需记录有效接触面积Ai；FEATS给出的网格值是力N，VPEC/ViPER图像压力是Pa，不能混用。wrench中的力N与力矩Nm必须记录坐标、法向正负号、作用点；六维变换会混合平移引起的力矩，但不会凭空增加空间观测秩。时间戳使用明确时钟ID与int64ns，RGB-D、KWR75D、法兰位姿、压力阵列的动态核和同步延迟均须独立核查。

FR5法兰是腕端刚体坐标；标定TCP是几何工具参考点；刮板受力后真实接触中心/线、局部偏载可能相对TCP移动。三者不等价。真实边缘标定缺失时必须把contact_line记UNKNOWN并拒绝姿态纠偏，不能将压力中心从单一腕部合力矩无条件反演出来。

**公式 F46-08**

$$
F_i[\mathrm N]=P_i[\mathrm{Pa}]\,A_i[\mathrm{m}^2]
$$

**表 T46-08**

|字段|正确含义|错误用法|
|---|---|---|
|flange_SE3|机器人法兰位姿|冒充TCP|
|contact_line|柔性工具实际接触线|用刚性TCP替代|


**可撤销决策与来源**：D46-06/08/12; S46-01/02/03。

## 09｜三箱空间不可辨识：最有利的已知纯力偶条件

令pL,pM,pR≥0为刮板左、中、右三个法向载荷（N），a>0为半宽（m），q为经坐标核准的一阶力矩（Nm），c为**独立已知**的纯接触力偶（Nm）。则F=pL+pM+pR，d=(q-c)/a=pR-pL。观测矩阵H=[[1,1,1],[-a,0,a]]，秩2，零空间向量(1,-2,1)；只要载荷非负域允许，沿此向量移动就改变真实空间压力而不改变F/q。

R46刻意采用比现实更乐观的c已知条件，证明连理想情况都不唯一。现实c往往未知，摩擦、变形、压力多点分布使问题更难而非更容易。若引入真正独立的中心箱物理读数并已知c，秩可能达到3；但从相同wrench派生的VRR或刚体姿态变换不增加秩。

**公式 F46-09**

$$
F=p_L+p_M+p_R,\quad d=(q-c)/a=p_R-p_L
$$

**表 T46-09**

|已知量|物理含义|未知量|
|---|---|---|
|F,q,a,c|总力/力矩/半宽/已知纯力偶|pL,pM,pR|
|观测秩|最多2|一维零空间|


**可撤销决策与来源**：D46-06; S46-01/02/03。

## 10｜精确可行族、逐箱区间和最大局部差

引入s=pL+pR。非负性要求|d|≤s≤F，故可行集是一条线段；各箱极值无需黑箱滤波即可闭式给出：pL∈[(|d|-d)/2,(F-d)/2]、pM∈[0,F-|d|]、pR∈[(|d|+d)/2,(F+d)/2]。任意两个可行点的三箱平均绝对误差为2|s1-s2|/3，最大为2(F-|d|)/3 N/箱。该式是不可辨识上限而非算法精度。

例子F=10N、a=0.02m、q=0.04Nm、c=0→d=2N：p=(0,8,2)N与(4,0,6)N同wrench，但平均逐箱差16/3≈5.333N。一个中箱承载8N，另一个中箱为零，真实支持都变了。任何仅检查合力/力矩一致性的模型都无法分辨两者。

**公式 F46-10**

$$
s=p_L+p_R\in[|d|,F],\quad(p_L,p_M,p_R)=((s-d)/2,F-s,(s+d)/2)
$$

**表 T46-10**

|量|精确可行范围|证书前提|
|---|---|---|
|中箱|[0,F-|d|] N|已知c、非负|
|右箱|[(|d|+d)/2,(F+d)/2] N|同上|


**可撤销决策与来源**：D46-06/07; S46-01/02/03。

## 11｜minimax同信息强Baseline与点估计局限

所有可行压力沿s区间线性变化。若必须输出单个三箱点，选择s*=(|d|+F)/2是最大逐箱L1误差意义下的minimax点估计：它将到两个端点的最大距离对半，最坏每箱MAE=(F-|d|)/3。任何其他固定点都使到较远端点的最坏误差更大。这是强Baseline而不是新传感器，不能从数学minimax保证推出它就是现实局部真值。

本轮将端点中心重、端点边缘重、minimax中点三者在相同F/q/a/c条件下对照，三者的聚合wrench残差都近零；独立生成的人工p真值才能揭示空间MAE差异。首选创新不是让中点网络看起来“精准”，而是报告完整可行区间、物理证书与失败拒绝。

**公式 F46-11**

$$
s_*=(F+|d|)/2,\quad\max_s\mathrm{MAE}_{bin}(p(s_*),p(s))=(F-|d|)/3
$$

**表 T46-11**

|方法|使用信息|数学保证|
|---|---|---|
|端点点估计|F,q,a,c|可能偏到最大差|
|minimax中点|完全同信息|最坏逐箱MAE减半|


**可撤销决策与来源**：D46-07/11; S46-01/02/03。

## 12｜独立LP oracle、严格类型和可执行代码

新增纯离线`spatial_truth_gate.py`。Wrench2显式要求force_N、moment_Nm、half_width_m、**known_pure_couple_Nm**，拒绝布尔伪数、NaN、Inf、负力/非正半宽与不可行wrench。`bounds_from_wrench`返回三箱上下界、右箱比例及最大两分布逐箱MAE；`minimax_point`给同信息点强基线。没有未知c时偷偷设零的代码路径：调用者必须提供独立已知c这一强假设。

独立oracle使用SciPy HiGHS对每箱分别min/max，在100个随机载荷与随机已知c样本上比较闭式端点，测试通过；另外100个随机wrench检验端点同wrench和最大差公式。共253个pytest参数化测试通过。所有输出`HOLD_NO_MOTION`，无ROS2、SDK、网络、硬件命令；算法仅作数学和数据资格审计。

**公式 F46-12**

$$
\min/\max\; e_i^Tp\;\mathrm{s.t.}\;Hp=(F,q-c),\;p\ge0
$$

**表 T46-12**

|实现|作用|执行状态|
|---|---|---|
|spatial_truth_gate.py|闭式区间、来源Gate、Pa→N|HOLD_NO_MOTION|
|test_spatial_truth_gate.py|SciPy HiGHS独立端点oracle|253/253通过|


**可撤销决策与来源**：D46-07/08; S46-01/02/03。

## 13｜空间真值来源Gate与证书失败拒绝

新增`spatial_truth_provenance`只审计输入字典的精确字段：来源枚举、episode ID、heldout_test、episode分组、训练标签独立性、传感器序列号、校准/配准/原始数据SHA256、时钟、int64ns、压力Pa、单元面积m²、来源许可。FEA派生直接REJECT_NOT_PHYSICAL_TRUTH，wrench派生REJECT_AGGREGATE_ONLY，缺校准哈希/时间/单位/episode则拒绝；即使全部字段形式上齐备，也只返回METADATA_PREQUALIFIED_ONLY和HOLD_NO_MOTION。

这套Gate不会从纸面SHA证明真实校准，亦不会自动读取论文数据。它的用途是防止将FEATS FEA标签、ViPER weak set或同一wrench积分错误地注册为独立物理压力真值。测试中的“通过元数据预资格”样本是人工fixture，其证书哈希只是合成字符串，不是任何真实力台的校准证书。

**公式 F46-13**

$$
\mathrm{TruthGate}(label)=\begin{cases}\mathrm{REJECT},&FEA/wrench\\\mathrm{PREQUALIFY},&physical+metadata\end{cases}
$$

**表 T46-13**

|标签来源|本轮判定|是否运动|
|---|---|---|
|FEA/wrench|拒绝独立空间真值|否|
|物理压力阵列+元数据|仅元数据预资格|否|


**可撤销决策与来源**：D46-08/11; S46-01/02/03/07。

## 14｜512组合成公平消融：总力一致不代表局部一致

固定种子460046先人工生成512组F∈[3,25]N、d/F∈[-0.85,0.85]、半宽a、已知c和真实s∈[|d|,F]，再根据同一F/q/a/c构造三个点估计。真值由生成器隐藏的s定义，不由待评算法反推，属于**独立数学oracle**，绝不是独立物理力台。三种方法使用完全相同的观测；所有预测的合力残差最大约3.55e-15N，力矩残差另按Nm独立报告（不能混合量纲），没有空间真值时看起来都“完美”。

实际512例区间覆盖512/512；中心重平均逐箱MAE 2.688435N，边缘重2.777669N，minimax1.355473N；平均两个极端分布之间的逐箱MAE 5.466105N。minimax改善是同信息最坏误差选择，不是测量精度提升。不能把这些合成数字迁移到FR5或FEATS/VPEC作者数据。

**公式 F46-14**

$$
\mathrm{MAE}_{bin}=\frac{|p_L-\hat p_L|+|p_M-\hat p_M|+|p_R-\hat p_R|}{3}
$$

**表 T46-14**

|同信息方法|平均逐箱MAE N|聚合wrench|
|---|---|---|
|中心重端点|2.688435|约零残差|
|边缘重端点|2.777669|约零残差|
|minimax中点|1.355473|约零残差|


**可撤销决策与来源**：D46-06/07; S46-01/02/03。

## 15｜失败反例、支持集合和未知纯力偶边界

两组明确反例使用半宽0.02m、c=0，得到相同10N总力和0.04Nm一阶力矩，但中心箱从8N变0N，右箱从2N变6N。此时任何以F/q误差小为唯一指标的空间压力网络都可完全通过聚合验收却在接触支持上犯严重错误。若视觉漏检中心区域而硬置零，可能产生内部可行但漏真值的区间，R42–R44已给出相关反例。

本轮算法没有尝试由未知c反推压力：真实纯力偶、多点摩擦与工具弯曲未分离时必须扩展不确定性或HOLD。真实刮板不是三个理想点，三箱只是说明观测边界的最低维模型；高分辨率压力线的自由度更高。仅有机器人法兰/TCP位姿不能说明柔性真实接触中心。

**公式 F46-15**

$$
p^{(1)}=(0,8,2),\quad p^{(2)}=(4,0,6),\quad F=10,\ q=0.04
$$

**表 T46-15**

|失败类型|数学表现|工程拒绝|
|---|---|---|
|空间错位|相同F/q、不同支持|不得纠偏|
|c未知|观测误差集合扩大|要求独立上界|


**可撤销决策与来源**：D46-06/11; S46-01/02/03。

## 16｜主论文候选排序、许可证与路线收敛

本轮不是按论文作者性能排名，而是按能否取得合法、独立非人体空间压力参考并构建离线可复现对照排序。VPEC仍第一：Sensel独立空间真值与MIT代码，但170GB数据过大、视角与刮板不同。ViPER升第二：同时有Eye-in-Hand和真实Sensel强标签，虽其代码许可缺失仍不得复制；FEATS降第三：MIT/HF数据公开、FEA先验有价值，但空间标签非独立物理真值。ImplicitRDP降第四是R45的Flexiv runner/dill与无空间真值阻塞；RDP第五因合法离线版本/接口未证。

这一排序只决定“下一步先审计哪一个”，没有宣称复现成功或FEATS方法无效。主学术Baseline最终赢家待定；厂家/简单强Baseline仍是审核路径+真实厂家FT_Control经固件时序核验后单向低能量恒力/平滑卸载/离面返程，当前仅设计方案。若ViPER许可证无法补齐，不能将其升为可复用代码；若FEATS公开独立逐格力台数据，排序可再次修订。

**公式 F46-16**

$$
\mathrm{priority}_{46}:VPEC>ViPER>FEATS>ImplicitRDP>RDP
$$

**表 T46-16**

|候选|R45→R46|依据|
|---|---|---|
|ViPER|4→2|真实Sensel+Eye-in-Hand，许可未证|
|FEATS|2→3|FEA空间标签非独立真值|
|ImplicitRDP|3→4|实机runner与dill风险|


**可撤销决策与来源**：D46-09/10; S46-01..07。

## 17｜唯一首选创新、备选与失败退出

唯一条件性首选I-A仍是有限宽刮板真实接触线/局部偏载的**部分可辨识区间、独立物理真值与可信拒绝**。本轮收紧原创性：VPEC/ViPER已做视觉压力图，FEATS已做FEA分布估计，不能以“融合RGB-D与F/T”或“再加EKF/ESO/MPC”自称创新。真正待研究的是法兰/TCP/柔性接触线区分、未知纯力偶下的可证区间、跨材料/曲率/遮挡的独立物理覆盖及错误证书的拒绝。

备选I-B是WORK—UNLOAD—OFFSURFACE_RETURN接触换向与延迟鲁棒柔顺控制，但FR5 FT_Control真实SDK/固件/反馈时序未核、无独立卸载风险实验，不能现在抢主创新份额。I-A退出Gate：拿不到非人体独立分段压力真值、同信息强基线没有可复现收益、或失效时无法拒绝/卸载；出现这些证据时必须允许“待定”或转向I-B，而不是硬造论文。

**公式 F46-17**

$$
I_A=\mathrm{partial\ identification}+\mathrm{independent\ truth}+\mathrm{fail\ closed}
$$

**表 T46-17**

|创新|保留条件|退出条件|
|---|---|---|
|I-A有限宽局部载荷|独立物理真值+同信息强基线|长期无真值/无增益|
|I-B延迟鲁棒卸载|真实FR5外环时序|接口不支持则暂缓|


**可撤销决策与来源**：D46-09/11; S46-01/02/03。

## 18｜M1/M2/M3、有限宽路径与非运动安全Gate

M1人工有序3D点与指定路线；M2实体画线的检测、起终方向确认及有序三维曲线；M3相机看背部、识别解剖候选、专家审核的穴位关键点图，可选两点图搜索或审核模板。三个入口必须在接触前由用户审核，统一输出位置m、切向单位向量、曲面法向、置信度/协方差、法兰位姿、标定TCP、柔性实际接触中心/线、工具姿态、连续有限宽扫掠资格及WORK/UNLOAD/OFFSURFACE_RETURN。

离线验证顺序为相机/手眼/边缘标定、KWR75D偏置/重力/动态核、厂家FT_Control SDK v2.3.0与robot v4.0.0真实时序、独立非人体力台、离面轨迹、低能量单向WORK、平滑UNLOAD、离面返程。当前只有软件，没有FR5/D435/KWR75D、授权仿真模型或真实动态参考；所有硬件Gate和人体伦理Gate为HOLD。源码无ROS2发布或厂家SDK导入，结果恒HOLD_NO_MOTION。

**公式 F46-18**

$$
\gamma(s)=(x(s),t(s),n(s),\Sigma(s),R_{tool}(s),mode(s))
$$

**表 T46-18**

|入口|必须保留的人审步骤|统一输出|
|---|---|---|
|M1/M2/M3|选点/画线方向/解剖候选专家审核|有序3D路径|
|WORK/UNLOAD/RETURN|接触前审核+安全状态机|不输出实机运动|


**可撤销决策与来源**：D46-12; S46-11。

## 19｜本轮验收、失败清单与下轮自适应实验

本轮实际完成三篇论文方法/实验核读、12个来源组去重审计、三官方仓库固定commit与许可检查、一个完整零空间解析证明、独立LP oracle 100组、253项新增pytest、512组同信息人工消融，以及R39–R45 435项历史回归。原始数据下载0、作者网络复现0、FR5实体接触0、物理仿真0、人体测试0、GitHub写入0。前轮代码/报告/讲义都实际可访问，相关哈希与结果写入测试报告。

下轮优先级不再是增加合成神经网络，而是寻找合法、可核验的一条VPEC或ViPER fully-labeled非人体完整episode：原始压力阵列Pa、有效面积、相机注册、校准曲线、独立F/T、时钟/episode、材料与遮挡标签、许可及SHA。拿不到则设计非人体三分段力台计量BOM与独立盲检协议；在任何情况下不触发硬件运动。若某轮缺新反证/代码/可证测试，应标记未验收而不递增编号。

**公式 F46-19**

$$
N_{pass}=435+253=688,\quad N_{physical}=0
$$

**表 T46-19**

|交付|实际状态|不得宣称|
|---|---|---|
|双Markdown|逐章对应并QA|非论文投稿完成|
|代码/消融|688离线测试、512人工例|非FR5/人体实测|


**可撤销决策与来源**：D46-01..12; S46-01..12。


## 附录A｜本轮12组来源与实际阅读层级

# AUTO-R46｜来源审计登记（2026-10-21）

阅读层级：**A**＝本轮实际打开论文全文并核读方法、数据、实验与局限；**B**＝本轮访问官方项目、仓库、数据卡或题录，仅核其公开对象；**H**＝历史结论，本轮没有重新读取论文。`repo@SHA`来自本轮公开GitHub只读查询；未运行作者代码、下载数据集或模型。

|ID|独立来源组 / 精确定位|层级|本轮实际检查的可证事实与不能推导的结论|
|---|---|---|---|
|S46-01|Grady等，*Visual Pressure Estimation and Control for Soft Robotic Grippers*，IROS 2022，DOI `10.1109/IROS47612.2022.9982073`，https://arxiv.org/pdf/2204.07268|**A全文8页，方法/实验**|Sensel Morph 185×105、约100Hz物理压力阵列；四RGB相机、约30Hz、650k帧、96序列/夹爪、按序列留20%测试；Table I接触IoU 73.8%/63.3%，MAE 5.3/9.7Pa；机器人Stretch RE1控制，不是FR5。公开170GB数据未下载，压力原始校准记录未核。|
|S46-02|Collins等，*Visual Contact Pressure Estimation for Grippers in the Wild*，IROS 2023，DOI `10.1109/IROS55552.2023.10342124`，https://arxiv.org/pdf/2303.07344|**A全文8页，方法/实验/局限**|47k真实压力+F/T强标签、275k仅F/T弱标签；Sensel Morph + ATI Mini45、Eye-in-Hand 25Hz；独立物理压力仅存在于fully-labeled集；Table I fully-labeled IoU 40.3% vs VPEC重训28.9%，weak set报告接触和wrench，不能称独立压力真值；遮挡/背景/未见载荷失败。|
|S46-03|Helmut等，*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*，IROS 2025，DOI `10.1109/IROS60139.2025.11246486`，arXiv:2411.03315v3（2026-01-13），https://arxiv.org/html/2411.03315|**A全文方法/实验**|14279压痕、24种压头、FEA+Neo-Hookean、24×32×3标签由仿真生成；外部RESENSE-HEX-21用于材料参数/总力对照，**不是独立逐像素力场真值**。1068已见压头测试，2789未见压头，跨传感器初始z总力误差2.99N，增4400样本后1.79N；作者硬件/频率不能转移FR5。|
|S46-04|VPEC官方仓库，`Healthcare-Robotics/VPEC@371b9ec2b9a2f627e381f31d3fb25833c6247504`，https://github.com/Healthcare-Robotics/VPEC|**B官方源码/许可证**|MIT；Python3.10、PyTorch1.12；README约170GB；本轮只读固定commit及`recording/downloader.py` blob `32accbef6d880bf41f06ea8e4a1aebf1e478603d`；下载脚本外部Dropbox权重，不代表已核验下载可用性/数据许可细项。|
|S46-05|ViPER官方仓库，`Healthcare-Robotics/ViPER@08c7d2fc4f8da34a113a76973e12a99dda7597cf`，https://github.com/Healthcare-Robotics/ViPER|**B官方源码/README**|README blob `e4150f92a270b7b5d91ec5502d5ef04890ffa626`；OneDrive数据/权重未下载；GitHub仓库许可证字段未返回SPDX，根目录无LICENSE可见，**不得认定可合法复用代码**；README含实体Stretch控制入口，禁止运行。|
|S46-06|FEATS官方仓库，`feats-ai/feats@7d1ae43daf8a86d389a6e48fead6478f7c1bd4e6`，https://github.com/feats-ai/feats|**B官方源码/许可**|MIT、README blob `de94368242eaa518c0f2c69c66db4174e76b7a5b`，新模型文件名`unet_09042025_124903_80.pt`，归一化`.npy`；未运行、未验证模型格式安全；仓库含CNC/传感器采集代码，不应执行。|
|S46-07|FEATS官方Hugging Face数据卡，https://huggingface.co/datasets/erikhelmut/FEATS/tree/main|**B官方数据清单**|3.98GB、HF标记MIT、公开短revision `7fd3027`；一个`v24_labels_24_32.zip`。说明标签来自FEA，不能作为独立物理空间压力真值；未下载、未逐样本审计。|
|S46-08|FEATS作者项目，https://feats-ai.github.io/|**B作者实验说明**|25Hz GelSight Mini模型与100Hz RESENSE合力对比，作者称demo总力误差<1N；只证明所述演示的**合力**，不是刮板压力线物理验证。|
|S46-09|PressureVision作者仓库，https://github.com/facebookresearch/PressureVision|**B官方README**|MIT、140GB“小数据集”、960GB全量，2023-10-31归档；数据为**人手**压力，不能替代要求的非人体刮板真值，未下载。|
|S46-10|IntelliMan 2026 Zenodo `10.5281/zenodo.18759556`，https://zenodo.org/records/18759556|**B公开数据描述**|8路PVDF阵列、腕部F/T事件真值、press/slide/release、76.9MB；两名受试者的人体手指触觉事件，不是逐箱独立空间压力真值；未下载。|
|S46-11|FAIRINO官方Python SDK，https://github.com/FAIR-INNOVATION/fairino-python-sdk|**B官方仓库**|Apache-2.0；本轮只读项目首页；未核验FR5 SDK v2.3.0、robot v4.0.0真实`FT_Control`时序/固件/反馈，硬件HOLD。|
|S46-12|RDP RSS2025官方题录，DOI `10.15607/RSS.2025.XXI.052`，https://roboticsproceedings.org/rss21/p052.html|**B官方题录/摘要**|慢快视觉触觉策略、TactAR；未本轮核读论文全文，不能当本项目复现/FR5接口证明。|

**严格去重**：12个外部来源组（3篇论文全文、3个代码仓库、1个HF数据集、1个作者项目、1个人手压力项目、1个事件数据集、1个厂家SDK、1个RSS题录）。仓库内不同文件不额外计算为独立论文。另有上一轮AUTO-R45本地归档作为内部证据，非外部来源。外部代码仅只读，无用户仓库写入。

**关键反证**：S46-03明确将空间分布“ground truth”标签定义为FEA模型输出，外部真实F/T只约束总力/材料参数；S46-01/02有物理压力阵列但FR5工具几何/独立分段校准仍未取得。**这改变主论文审计顺序。**


## 附录B｜五篇主论文竞争与唯一创新

# AUTO-R46｜BASELINE_AND_INNOVATION_REGISTER（可撤销主论文竞争矩阵）

**最终主学术论文Baseline：待定。** 排序仅表示下一轮“合法、无运动、可获得独立物理压力参考”的审计优先级，不是性能排名或作者论文复现成功。R45顺序：VPEC→FEATS→ImplicitRDP→ViPER→RDP。**R46：VPEC→ViPER→FEATS→ImplicitRDP→RDP。** 本轮ViPER由4→2，FEATS由2→3，ImplicitRDP由3→4；均可因后续许可/数据/独立物理证据撤销。

|审计序位 / 变化|论文元数据、年份、DOI|本轮实际获取层级；源码/数据/许可证/版本|数学、实验、FR5匹配与2025–26反证|结论 / 事实缺口 / 最小补证|
|---|---|---|---|---|
|**1 = VPEC**|Patrick Grady, Jeremy A. Collins, Samarth Brahmbhatt, Christopher D. Twigg, Chengcheng Tang, James Hays, Charles C. Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*，IROS2022；`10.1109/IROS47612.2022.9982073`|**A论文全文**；官方MIT仓库固定`371b9ec...`；约170GB数据/权重，未下载；外部Dropbox下载入口仅静态核查，数据分片及校准文件版本U|外部RGB+Sensel Morph独立二维压力真值、按96序列/夹爪划分；软夹爪平面≠D435 Eye-in-Hand刮板/背部曲面。输出压力Pa，须乘单元面积变N；无需假定FR5底层力矩，但其Stretch闭环12Hz不能移植为FR5时序；ViPER2023/FEATS2025/2026视觉压力文献限制新颖性|**优先合法离线复现审计，非已复现**。获取不运行机器人代码的最小数据文件清单、真实压力帧/面积/标定/episode、权重固定hash；否则保持HOLD。|
|**2 ↑ ViPER**|Jeremy A. Collins, Cody Houff, Patrick Grady, Charles C. Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*，IROS2023；`10.1109/IROS55552.2023.10342124`|**A论文全文**；固定`08c7d2f...`；OneDrive数据/权重未下载；GitHub license/SPDX未证、根目录无LICENSE；训练/测试区分fully/weak|Eye-in-Hand更接近D435视角；47k物理压力+F/T fully、275k仅F/T weak；物理真值仅fully；weak test接触标签来自3N F/T阈值，不可声称空间真值；遮挡/相似背景/非指尖载荷失败；Stretch控制/ROS20.04≠FR5/ROS2 Humble|**研究优先上升、代码复用暂缓**。先获明确许可证/数据权利与最小fully-test原始压力阵列+分组，未经许可不复制作者源码。|
|**3 ↓ FEATS**|Erik Helmut, Luca Dziarski, Niklas Funk, Boris Belousov, Jan Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*，IROS2025；`10.1109/IROS60139.2025.11246486`；arXiv2411.03315v3|**A论文全文**；官方MIT仓库固定`7d1ae43...`；HF MIT 3.98GB，短revision `7fd3027`；权重`.pt`/归一化`.npy`未加载，CalculiX许可/二进制逐项U|FEA Neo-Hookean仿真产生空间标签，不是独立物理空间真值；RESENSE总力仅材料/总力验证；24×32×3每格**力N**而非Pa；GelSight Mini内视图≠D435外视图；模型25Hz/Quadro推理4.17ms是作者硬件非FR5。对“仅合力指标即局部压力准确”构成负证|**条件保留为FEA先验/辅助数据Baseline，主物理真值优先级下降**；须获取独立分段力台实测和未见材料/传感器校准才可升为主论文。|
|**4 ↓ ImplicitRDP**|Wendi Chen, Han Xue, Yi Wang, Fangyuan Zhou, Jun Lv, Yang Jin, Shirun Tang, Chuan Wen, Cewu Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*，arXiv2025，v2 2026-07-21；`10.48550/arXiv.2512.10946`；正式期刊DOI U|R45 **A全文**，本轮**H沿用上轮固定审计**；MIT/RAIL-M混合；固定`112d6857...`；模型27.7GB、mini182MB未下载；作者eval连接Flexiv runner且dill反序列化|VRR为已知wrench的确定性变换，不增局部压力可辨识秩；Flexiv 10Hz/内置刚度/关节接口不可当FR5能力；非人体空间压力真值无|**有条件保留无运动策略基线，暂缓复现**；隔离纯张量forward/权重格式与许可、无硬件sink才可审计。|
|**5 = RDP**|Han Xue, Jieji Ren, Wendi Chen, Gu Zhang, Fang Yuan, Guoying Gu, Huazhe Xu, Cewu Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*，RSS2025；`10.15607/RSS.2025.XXI.052`|本轮**B官方题录/摘要**；作者代码、权重、许可、固定版本、数据分组均U；全文本轮未核读|慢快视觉触觉/高频动作策略、TactAR与FR5可用外环/六维F/T不同；无独立刮板压力线真值；相邻2026 ImplicitRDP已有同类策略|**暂缓**；需合法代码/模型版本、无运动forward及FR5时序证据。|

## 厂家/简单工程强Baseline（方案，非实测）

用户审核M1/M2/M3有序3D曲线、标定TCP与有限宽真实接触边缘、厂家`FT_Control`经SDK v2.3.0/robot v4.0.0真实接口/时序/失效核验后单向低能量WORK、平滑UNLOAD、离面OFFSURFACE_RETURN；独立参考力台与失效监测。**当前无机器人、未核接口，禁止运动输出。** 离线数学强Baseline为相同总力F、一阶力矩q、已知纯力偶c、半宽a下的精确零空间区间和minimax中点，不允许把模型生成标签当独立真值。

## 唯一创新与备选

**I-A首选（条件性）**：有限宽柔性刮板局部载荷**部分可辨识区间 + 真实独立分段物理真值 + 证书失效拒绝**。本轮新退出条件：若只有FEA伪标签、wrench积分或VRR派生信息，**不得**以“空间压力真值”验收；相同wrench可对应完全不同局部支持，点估计最多是先验选择。新颖性反证：VPEC/ViPER已实现视觉压力估计；FEATS已实现模型推断分布；因此创新必须落在有限宽工具可辨识边界、真实接触边缘/物理校准与拒绝机制，而非泛视觉力估计。

**I-B唯一备选**：接触换向与WORK→UNLOAD→OFFSURFACE_RETURN的延迟鲁棒双向柔顺控制；只有真实FR5外环时序、独立离面安全证据后才有资格上升。没有充分证据不强制赢家。

## 物理Gate / 三入口不可删

G0相机D435内外参/手眼SE(3)/法兰、标定TCP与柔性接触中心、接触线及偏载的**区分**；G1 KWR75D偏置/重力/动态核与FR5 `FT_Control`固件时序；G2未知纯力偶c、摩擦、变形的独立上界；G3非人体三箱或连续压力线的独立物理真值、分组episode盲检；G4同信息强基线、独立真值与拒绝/卸载率；G5真人风险与伦理批准。**全部HOLD**。

M1人工有序3D点；M2实体画线检测+起终方向人工确认；M3解剖候选/专家审核/可编辑点图/两点图搜索或审核模板。三入口统一`point_m,tangent_unit,normal_unit,uncertainty,flange_SE3,calibrated_TCP_SE3,actual_contact_line,swept_width_m,sweep_eligible,phase=WORK|UNLOAD|OFFSURFACE_RETURN,reviewed,timestamp_ns`；真实接触线未知则UNKNOWN，拒绝纠偏。不得将人体实验/其它机器人力阈值当FR5结果。


## 附录C｜12项跨轮可撤销决策

# AUTO-R46｜可撤销决策日志（相对R45）

|ID|本轮有证据的变化|可证伪条件 / 最小撤销证据|
|---|---|---|
|D46-01|实际核验R45 ZIP 38成员、37项内部SHA、外部SHA及CRC；重新执行R39–R45 435项与R46 253项，合计688项。|出现不一致SHA/JUnit或可重现反例即撤销PASS。|
|D46-02|论文全文核读重点由R45 ImplicitRDP/Marra转向VPEC/ViPER/FEATS三篇；3篇论文、12个去重外部来源组。|新的原文更正/数据校准信息须更新证据级别。|
|D46-03|明确FEATS 24×32×3空间标签来自FEA而非独立物理逐像素压力；RESENSE F/T用于总力/材料参数，不足以认证局部真值。|若作者提供与FEA标签独立的逐像素/分段力台原始盲检数据，可重新评价。|
|D46-04|VPEC和ViPER确有独立Sensel Morph空间压力阵列实验；但本轮未下载/核验任何原始样本、传感器校准与时间注册，仍不算本项目物理实测。|取得合法、可核校准与episode分割的原始非人体压力数据后进入离线复现。|
|D46-05|ViPER fully labeled的物理压力真值与weakly labeled仅F/T严格区分；弱测试的3N接触阈值不是独立空间压力真值。|新数据公开独立压力参考且确证属于弱集时重审。|
|D46-06|数学证明：即使F、q、纯力偶c均精确已知，三箱压力仍有1维零空间；相同合力/力矩的局部MAE最大可达`2(F-|d|)/3` N/箱。|若加入独立局部测量或可证力学先验使观测秩3，则本证明条件改变。|
|D46-07|构造精确三箱分量区间和minimax点强Baseline，100组独立HiGHS LP oracle逐分量端点吻合；512组合成消融全部区间覆盖。|任何独立LP反例、数值不稳定或物理证书不成立都须降级。|
|D46-08|建立只读空间真值来源Gate：FEA伪标签拒绝、wrench派生拒绝、独立压力阵列也只能“元数据预资格”且始终HOLD。|真实校准、注册、原始文件哈希、时钟和数据权利通过独立审计后可申请更高证据级别。|
|D46-09|主论文审计顺序R45 `VPEC→FEATS→ImplicitRDP→ViPER→RDP` 修订为R46 `VPEC→ViPER→FEATS→ImplicitRDP→RDP`；赢家仍待定。|ViPER许可证无法补齐或FEATS出现独立压力真值时重新排序。|
|D46-10|ViPER官方代码根目录未发现LICENSE且GitHub SPDX字段空；尽管论文有代码公开声明，**代码复用授权未证**。|取得作者许可/明确许可文件或权利人授权后撤销限制。|
|D46-11|唯一创新I-A进一步限定为“有限宽部分可辨识+独立物理真值+可信拒绝”，不得以总力误差小/FEA标签吻合冒充局部压力精度；I-B延迟鲁棒卸载备选不变。|若同信息强基线无局部收益、无物理真值或安全Gate失败，I-A退出。|
|D46-12|M1/M2/M3、有限宽路径合同、WORK/UNLOAD/OFFSURFACE_RETURN保留；无FR5/仿体/人体实测，SDK/ROS2/运动输出HOLD，GitHub无写入。|只有厂家固件/FT时序、独立仿体真值、风险审查与真人伦理批准可分别推进对应Gate。|


## 附录D｜失败、反例和事实缺口

# AUTO-R46｜失败、反例、真实阻塞

1. **可证伪负结果**：在最有利的已知纯力偶c条件下，三箱局部载荷仍不可唯一反演。构造(0,8,2)N和(4,0,6)N，两者总力10N、半宽0.02m时同一力矩0.04Nm，但三箱平均绝对差`16/3=5.333...N`。合力/力矩误差均为零，空间分布显著不同。
2. **论文来源反证**：FEATS空间标签由FEA产生，不是独立物理逐格真值；外部F/T总力/材料拟合不能填补该缺口。作者的论文结果不被否定，只是不能满足本项目独立空间真值Gate。
3. **许可缺口**：ViPER仓库可访问但本轮GitHub SPDX字段为空、根目录未显示LICENSE；论文声明公开代码不等于可自由复制。未下载或运行。
4. **原始数据阻塞**：VPEC约170GB、ViPER OneDrive未下载；FEATS 3.98GB未下载。无法独立核验原始校准、时间戳、像素面积、训练/测试物理episode隔离、模型权重和逐点标签质量。
5. **独立物理真值缺失**：目前没有非人体刮板左中右分段压力/连续压力线、外部力台或柔性接触中心实测；512组均为固定种子人工数学载荷。
6. **数学模型限制**：R46闭式算法要求已知纯接触力偶c、固定半宽a、纯法向非负三箱和静态平面等效模型；现实多点摩擦/柔性弯曲/未知c会扩大集合，不能将闭式窄区间直接用于机器人纠偏。
7. **来源Gate限制**：SHA只能验证文件内容一致，不能验证传感器真的经过校准或物理真值独立；即使全部字段存在，返回METADATA_PREQUALIFIED_ONLY与HOLD_NO_MOTION。
8. **指标限制**：同一F/q可有零聚合残差但较大空间误差。不能用FEATS demo合力误差<1N或ViPER弱标签RMSE推断真实局部偏载。
9. **硬件阻塞**：无FR5、D435、KWR75D、官方授权FR5仿真模型、真实SDK/固件/FT_Control反馈时序，不能执行实体实验/仿真或授权运动。
10. **下轮最小补证**：优先通过合法渠道取得VPEC或ViPER非人体`fully labeled`的一条完整episode原始压力帧、面积、相机注册、传感器标定、数据权利；不具备时设计三分段力台标准器的计量和数据结构，而非继续加复杂滤波网络。


## 附录E｜非人体独立物理真值与三入口数据Gate

# AUTO-R46｜非人体独立空间真值采集与可审计数据清单（尚未采集）

**一条独立episode最小文件**：`episode.json`（UUID、材料批次、曲率、刮板边缘ID、操作者审核、相机/力台时钟、开始/结束int64ns）；`camera_frames/`（RGB-D原始帧、内参、曝光）；`flange_pose.csv`（法兰SE3，不能替代TCP/接触线）；`tcp_calibration.json`；`blade_edge_calibration.json`（宽度、真实柔性变形/接触中心/接触线）；`wrist_ft.csv`（KWR75D原始6轴、偏置、重力、动态核）；`independent_pressure.csv`（至少左中右物理载荷N，或压力Pa与单元面积m²、完整校准曲线）；`reference_forceplate.csv`（独立总力）；`clock_alignment.json`；`calibration_certificates/`；`provenance_sha256.txt`；`license_and_consent.txt`（仅非人体材料）。

**最小实验顺序**：离面无接触传感器漂移→标准砝码/标准压头校准分段力台→多位置/方向的离线人工压载→非人体仿体离面轨迹→低能量单向WORK→平滑UNLOAD→OFFSURFACE_RETURN。前三步也必须独立安全审核；目前全部未执行。真人必须独立伦理/风险批准。

**独立判据**：至少三组不同位置相同总力/力矩的物理接触案例；真值来自未参与算法训练/推理的独立压力阵列，按episode+材料+曲率+遮挡分组留出。每例报告局部MAE、各箱区间覆盖、完整支持覆盖、假阴性、拒绝率、真实边缘位置误差、时间对齐残差、卸载时间及失效模式。所有压力Pa→单元力N必须记录单元有效面积，不能把Pa当N。

**当前Gate**：`HOLD_NO_MOTION`。没有传感器/固件/实体仿体，所有路径只用于离线数据合同规划。


## 附录F｜复现命令与绝对安全边界

在本目录执行`python -m pytest -q test_spatial_truth_gate.py`、`python ablation_r46.py`，旧测试使用`python run_historical.py`。本包只含全新原创离线审计/数学代码和上一轮归档，不含作者训练权重、作者原始数据、ROS2实体节点或FR5控制指令。所有估计结果仅HOLD_NO_MOTION。