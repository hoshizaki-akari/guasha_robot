# AUTO-R15｜FR5刮痧机器人博士级研究报告：三区载荷传递随几何变化的不可辨识性与校准覆盖

> 日期：2026-10-20（北京时间）。内容验收基于可访问证据、严格反例、可执行离线算法与可证伪实验；**无FR5连接、无真实压力/相机数据、无人体实验、无运动指令。**

> 与R14差分：三区+独立总力仍可能无法唯一测得边缘风险；新增几何条件传力包络、独立留出反证与明确退出Gate。


## 01. 上一轮真实文件、回归和新旧差分

本轮实际打开R14研究报告、教学讲义、Baseline登记表、非人体台架协议，且从R14 ZIP解出决策记录、两份源码/测试与QA_R14.json；文件SHA256和大小在`prior/`可查。直接重新执行R14全部26项、R13全部26项测试，均通过；更早R12/R11/R10的结果仅见R14包内历史日志，**本轮未重新执行，不计入本轮回归数**。R14证明连续刚性板两支座不能提供边缘真值，提出机械隔离三区台架。本轮新问题是：**即便已经物理分成三区，若接触几何改变三区间传力矩阵，三区输出及独立总力是否足以给出边缘风险真值？** 本轮给出同读数不同边缘载荷的严格反例，并完成随几何变化的校准包络LP及留出反证。R14原双区域LP没有三区中心状态、几何条件或留出条件覆盖证据。D15-01：将R14“隔离三区即可作真值”的表述修订为“隔离三区是必要工程尝试，只有跨几何、材料、温度及加载模式校准合格后才可能作条件性真值”。本轮没有FR5、相机或实体台架数据。

**E15-01**：$G_{15}=G_{\rm prior}\land G_{\rm sources}\land G_{\rm derivation}\land G_{\rm executable}\land G_{\rm falsifiable}$。此处$G$为内容验收布尔量，非触发计数。

**T15-01（研究证据/决策表）**

|证据|本轮实际动作|不允许推断|
|---|---|---|
|R14报告/讲义/登记/台架协议|读取并保存`prior/`|不等于核验其全部历史引文|
|R14/R13测试|本地重跑26+26=52项通过|不等于FR5验证|
|R15新内容|三区几何反例、45项新测试、600组合成|不等于物理真值|


## 02. 唯一科学问题、备选与证伪框架

首选科学问题Q15-A：在有限宽柔性刮板接触中，能否用**已验证的几何条件传力包络**与独立总力，对边缘载荷份额$e$给出非空、足够窄且跨条件有效的可信上界，并证明匹配RGB-D提供了腕部wrench无法替代的额外信息？H15-A（可证伪）：忽略几何依赖的三区读数会错误放行边缘载荷风险；独立几何条件与有效误差包络能消除该类模型内错误放行，但会牺牲放行率。H15-B：若真实RGB-D在遮挡、曲率与材料留出条件下不显著改善边缘上界宽度或错误放行率，则退出视觉融合主创新。唯一备选Q15-B：工作—平滑卸载—离面返程的厂家力控+简单限速状态机，只有独立力台证明现有强基线不足时才研究延迟鲁棒改进。D15-02：维持首选I-A与备选I-B，但把**真实几何条件标定**提升为I-A入门Gate，不新增“曲率神经网络”主创新。

**E15-02**：$e=(F_L+F_R)/(F_L+F_C+F_R),\quad H_{15}:\Pr(\widehat e_{\rm upper}<\tau\mid e>\tau)\leq\alpha$；$\tau,\alpha$仅为未来预注册的实验判据，本轮没有物理阈值。

**T15-02（研究证据/决策表）**

|方向|独立真值|入门Gate|退出Gate|
|---|---|---|---|
|I-A首选|隔离三区+独立总力+位置参考|跨几何留出、时序/误差界合格|RGB-D匹配无增益或无法校准|
|I-B备选|力台+离面轨迹+独立时间|厂家FT_Control时序合格|厂家简单方案已足够|


## 03. 本轮去重检索、访问层级与来源审计

本轮按**一篇论文只计一份研究来源**去重；作者仓库单列为复现资格来源，不把README当全文。实际访问12项来源：S15-01 Zhong2025、S15-02 Chelly2025、S15-03 Egger2025、S15-04 Kohlbrenner2024、S15-07 ViPER2023与S15-08 TacGraph2026为可访问HTML全文（核读程度详见SOURCE_AUDIT.json）；S15-05 Bae2025只读摘要/出版信息，S15-06 Costanzo2019页面反复验证码，只核摘要和检索片段；S15-09 VPEC、S15-10厂家ROS2、S15-11 Egger代码、S15-12 ViPER为仓库README级。对前三篇实读方法、实验、对照、限制；对Kohlbrenner与ViPER读相关章节；TacGraph本轮只重核相关段落。完整URL、作者、年份、DOI/arXiv及访问层级均在`SOURCE_AUDIT.json`。历史Jiang/Peng/Sferrazza/Foster/Arefin仅保留竞争矩阵，不算本轮新核读。D15-03：曲率、材料与传感器空间定位的外推风险已被既有工作覆盖，不能把普通几何校准包装成创新。

**E15-03**：$\mathrm{source\ level}\in\{\mathrm{full},\mathrm{abstract},\mathrm{repo},\mathrm{historical}\}$；四类不能互相替代。

**T15-03（研究证据/决策表）**

|ID|论文/官方来源|实际获取层级|本轮决策用途|
|---|---|---|---|
|S15-01|Zhong2025曲率校准|全文方法/实验|III–VIII|
|S15-02|Chelly2025触觉力控|全文方法/实验|III–VI|
|S15-03|Egger2025电容阵列|全文方法/实验|2–4|
|S15-04|Kohlbrenner2024阵列定位|全文方法/实验|III–V|
|S15-05|Bae2025无串扰结构|摘要/元数据|abstract and publication details|
|S15-06|Costanzo2019软触觉|摘要/检索片段|full page intermittently blocked by captcha|
|S15-07|ViPER2023|全文方法/实验|III–IV, 47k fully/275k weak images, pressure array, |
|S15-08|TacGraph2026|开放全文相关章节|III–VI contents available|
|S15-09|VPEC作者代码|作者README/许可|MIT license, Python3.10/PyTorch1.12.1, evaluator ent|
|S15-10|FR5官方ROS2|官方README|driver families and ROS2 SDK existence|
|S15-11|Egger作者代码|作者README/许可|MIT, src/tests/poetry, serial touchpad UI|
|S15-12|ViPER作者代码|作者README|Ubuntu20.04, Sensel API, weights link, model train/i|


## 04. 全文深读A：Zhong2025曲率校准的收益与边界

S15-01（arXiv:2510.25965v2，2025-10-31）实际阅读III–VIII：4×4 Velostat电阻阵列、行列扫描、无载基线特征的残差MLP预测曲率、三次多项式力标定。三块传感器在10种柱面曲率采基线，训练/验证/测试为60/20/20；预测范围0–80 m⁻¹，作者报告$R^2=0.91$；静态加载用ATI Nano17作为**总法向力**参考，五件日用品测试2/4/6/8 N。作者指出高曲率响应变弱、100 m⁻¹处不稳定，并承认只研究一维曲率与静态条件。数学核读发现一个需要复核的**记号约定**：文中先定义平均曲率$H=(k_1+k_2)/2$，随后称圆柱$H=1/R$；若$k_1=1/R,k_2=0$，标准平均曲率应为$1/(2R)$。可能作者后文实际使用圆柱主曲率$1/R$而非平均曲率；本轮只标记“变量定义不一致风险”，不推断实验失效。其代码链接在HTML为匿名占位，不能宣称已取得可运行源码。D15-04：三区标定必须明确曲率定义、量纲、几何域，不得用平面校准外推。

**E15-04**：$H=(k_1+k_2)/2,\quad (k_1,k_2)=(1/R,0)\Rightarrow H=1/(2R)\ne 1/R$（标准几何约定）。

**T15-04（研究证据/决策表）**

|维度|论文可核实事实|FR5迁移限制|
|---|---|---|
|传感器/参考|4×4柔性电阻阵列+ATI Nano17|测总力而非边缘分布|
|实验|静态2–8 N、五种物体|不覆盖刮板换向动态|
|模型|MLP曲率+多项式力校准|无真实刮板曲率数据与源码|
|新颖性|已研究几何条件校准|普通曲率补偿不再算原创|


## 05. 全文深读B：Chelly2025触觉力估计与控制

S15-02（Chelly等，arXiv:2411.13335v2，2025-03-10）核读III–VI。作者在Allegro Hand的18组uSkin阵列上，用已知taxel局部坐标旋转、原始3n维触觉、线性/二次回归、岭回归与小MLP比较3D合力估计；ATI Mini45作为独立总力参考。100Hz采集，10Hz低通，ROS Noetic时间对齐，静态校准中使用多种3D打印压头；球面压头留作在线测试，YCB四种材料留作闭环泛化。报告MLP在线平均绝对误差0.21±0.10 N、正则化线性0.31±0.14 N（特定指尖实验），但换材料时线性模型更稳；力控使用手指关节力矩/雅可比和100Hz回路，**不能直接假定FR5开放相同权限**。论文明确忽略触觉变形动力学与滞回。这里可借鉴的是“简单线性模型对照、跨材料留出、独立力参考”，不是其手指力控参数。D15-05：把**跨材料留出**列为三区台架及RGB-D融合的必做消融。

**E15-05**：$\hat{\boldsymbol f}_{\rm lin}=A\boldsymbol x+b,\quad \hat{\boldsymbol f}_{\rm MLP}=\mathcal N_\theta(\boldsymbol x)$；复杂模型必须同条件对比。

**T15-05（研究证据/决策表）**

|对照|作者实验条件|对FR5的意义|
|---|---|---|
|M3λ线性|跨不同接触材料较稳|工程简单强对照|
|M4小MLP|已知球面压头误差较低|不能保证材料外推|
|控制|Allegro手指关节力矩、100Hz|FR5底层接口未核，原样暂缓|


## 06. 全文深读C：Egger2025的采样与处理延迟区分

S15-03（Egger等，Sensors 2025，DOI 10.3390/s25103011）实际阅读方法、结果与局限：6×6电容柔性阵列、MUCA/Arduino采样、Otsu阈值+连通域+Jaccard跟踪，使用UR3e与Schunk F/T进行1–3N范围加载。作者给出传输约16ms/帧的例子，同时对含接触簇样本报告Python处理平均627.2µs；**处理耗时<1ms不等于物理采样1kHz**。回归部分70/30拆分、100次重复加载，决策树报告$R^2\approx0.9996$，但未来工作才计划详细滞回和动态加载验证；逐元真实压力并非由Schunk F/T直接提供。S15-11作者MIT仓库确有`src/tests`和串口配置，未下载/运行原程序。D15-06：非人体动态真值必须单独测机械频响、逐元采样和时间戳，不能引用图像处理延迟充当传感器带宽。

**E15-06**：$T_{\rm end-to-end}=T_{\rm sensing}+T_{\rm transport}+T_{\rm processing}+T_{\rm actuation}$，各项必须独立测量。

**T15-06（研究证据/决策表）**

|指标|论文报告|不可推断|
|---|---|---|
|示例数据发送|约16ms/帧|不能声称逐元1kHz|
|含接触簇处理|平均627.2µs|不能代表物理响应|
|回归R²|约0.9996，有限加载范围|不等于刮板边缘真值|
|作者代码|MIT、串口UI|未在FR5或真实台架运行|


## 07. 其他文献反证：阵列位置、机械串扰和许可

S15-04 Kohlbrenner等2024全文III–V：柔性非均匀电容阵列用100个已知探针位置生成空间响应图，插值和阈值中心定位；两块人工皮肤报告约±2mm位置误差，误差源含探针定位。它测的是**taxel位置**，不是压力分布真值。S15-05 Bae等2025只读摘要：通过独立感测单元和弹性网格减少横向拉伸串扰，提示“机械隔离”必须实证，未核全文。S15-06 Costanzo等2019仅摘要/检索片段：软垫分布触觉标定复杂，不能假装核读全文。S15-07 ViPER全文明确外部压力阵列全标签+腕部F/T弱标签，25Hz Eye-in-Hand图像；S15-08 TacGraph开放全文说明几何和力平衡融合已有先例。S15-09 VPEC MIT可见，S15-12 ViPER仓库根目录许可本轮未见，不能默许商用/改造。D15-07：新颖性再压缩——**阵列位置校准、抗串扰结构、几何力融合、视觉压力弱监督都不是新颖性来源**。

**E15-07**：$\mathrm{GT}_{\rm taxel\ position}\neq\mathrm{GT}_{\rm pressure}\neq\mathrm{GT}_{\rm total\ wrench}$。

**T15-07（研究证据/决策表）**

|文献|已解决的对象|仍缺的刮板证据|
|---|---|---|
|Kohlbrenner2024|taxel空间位置|逐区载荷与柔性桥接|
|Bae2025摘要|机械/电气串扰设计|真实刮板跨材料验证|
|ViPER2023|软夹爪视觉压力弱监督|遮挡边缘与独立刮板真值|
|TacGraph2026|单点接触因子图|有限宽分布/纯偶矩|


## 08. 核心新反例：三区+独立总力仍不能保证边缘真值

构造静态、无量纲、对称三区载荷$x=(L,C,R)$，总力$F=1$，两侧一阶力矩$M=R-L=0$。设几何条件$q\in[0,1]$改变载荷从左右区泄漏到中央区的比例$a=0.35q$，测量矩阵$A(q)$每列和为1，因此独立总力闭合**始终完美**。状态A：$q=1,x_A=(0.4,0.2,0.4)$，三区读数$y=(0.26,0.48,0.26)$，真实边缘份额0.80；状态B：$q=0,x_B=(0.26,0.48,0.26)$，读数**完全相同**，真实边缘份额0.52。两者总力1、对称一阶力矩0、三区读数相同，独立总力闭合也相同。这是对R14“隔离三区+独立总力就足够”直觉的进一步反证。前提是**几何改变传力矩阵**，并非声称已在真实刮板观察到35%泄漏。源码`three_zone_witness()`独立断言上述等式。

**E15-08**：$A(q)=\begin{bmatrix}1-a&0&0\\a&1&a\\0&0&1-a\end{bmatrix},\ a=0.35q;\quad A(1)x_A=A(0)x_B=(.26,.48,.26)^T$。

**T15-08（研究证据/决策表）**

|状态|真实(L,C,R)|三区输出(L,C,R)|独立总力/力矩|真实边缘份额|
|---|---|---|---|---|
|A：q=1|(.40,.20,.40)|(.26,.48,.26)|1 / 0|0.80|
|B：q=0|(.26,.48,.26)|(.26,.48,.26)|1 / 0|0.52|


## 09. 几何条件校准包络、误差和可辨识假设

为使反例转化为可测试工程方法，定义可测几何协变量$q$（例如曲率、刮板倾角、接触位置的**连续、同单位归一化**向量），在若干独立标定锚点$q_k$测得矩阵$\widehat A_k$。若经物理证据证明$|A_{ij}(q)-A_{ij}(q_k)|\le K_{ij}\|q-q_k\|$、锚点误差$E_{ij}$，则逐元素交集给出$A^-_{ij}(q)=\max(0,\max_k[\widehat A_{k,ij}-E_{ij}-K_{ij}d_k])$，$A^+_{ij}=\min_k[\widehat A_{k,ij}+E_{ij}+K_{ij}d_k]$。$q$若有半径$\delta_q$的误差，用$d_k+\delta_q$保守扩张；查询超出已标定坐标范围、锚点邻域或上下界矛盾则HOLD。**材料类别不能随意作为连续数值插值**，需独立材料族标定或保守并集。留出测试只能证伪错误界，不能单凭有限样本证明全域Lipschitz上界。R15代码中`lipschitz_physically_validated=True`仅为合成假设开关，**真实世界尚无该证据**。

**E15-09**：$A^-_{ij}(q)=\max\{0,\max_k(\widehat A_{k,ij}-E_{ij}-K_{ij}(d_k+\delta_q))\},\quad A^+_{ij}(q)=\min_k(\widehat A_{k,ij}+E_{ij}+K_{ij}(d_k+\delta_q))$。

**T15-09（研究证据/决策表）**

|误差源|数学变量|现实证据要求|
|---|---|---|
|锚点读数/参考力|E_ij|独立力计、重复加载、留出|
|几何变化速率|K_ij|跨曲率/角度密集测量并留出|
|几何观测误差|δq|相机/工具边缘标定|
|材料变化|离散族|不同材料单独验证，禁止随意插值|


## 10. 三分区边缘风险LP与坐标、单位、退化

令$x\ge0$为工具真实接触区域L/C/R的**法向载荷积分**，而非腕部力传感器在某坐标系的原始$F_z$。必须先用手眼、法兰→TCP、工具真实边缘与局部法向建立一致的坐标与载荷定义；未知切向摩擦、纯接触偶矩或多点压力不能凭wrench唯一消去。观测$y=A(q)x+\epsilon$，独立总力$F\in[F^-,F^+]$，$|\epsilon_i|\le\eta_i$。定义$u=x/F,t=1/F$，$u_L+u_C+u_R=1$，$1/F^+\le t\le1/F^-$，线性不等式$A^-u\le(y+\eta)t$与$A^+u\ge(y-\eta)t$。分别最小化/最大化$u_L+u_R$得边缘风险区间。**仅当真实A落在逐元素区间、误差界有效且三分区模型适用时**，该区间覆盖真实风险；A项误差相关会使该方法保守，不会凭空变成精确压力图。几何未知或无独立总力，HOLD；即使LP数值求解成功也不代表机器人运动许可。

**E15-10**：$\min/\max\ (u_L+u_R)\ \mathrm{s.t.}\ u\ge0,\ \mathbf1^Tu=1,\ A^-u\le(y+\eta)t,\ A^+u\ge(y-\eta)t,\ t\in[1/F^+,1/F^-]$。

**T15-10（研究证据/决策表）**

|符号|意义|不能混淆|
|---|---|---|
|x_L,x_C,x_R|真实刮板接触分区法向力|不是传感器读数y|
|y|台架三区传感器输出|不是腕部六维wrench|
|q|几何/接触配置|不是随意网络隐变量|
|F|独立总力参考|不能用sum(y)冒充独立|
|u,t|归一化LP变量|不代表实时控制指令|


## 11. 可执行实现、测试与独立合成对照

源码`src/geometry_conditioned_certificate.py`实现：三区几何锚点包络、协变量不确定性、超域拒绝、静态LP、独立点载荷留出证伪；`src/run_synthetic.py`固定种子20261020执行600组**人工合成无量纲**三分区载荷，$q\sim U[0,1]$、真实传力$A(q)$已知，通道噪声±0.002，校准锚点0/0.5/1，声明误差±0.005，独立总力[0.999,1.001]，人为研究阈值0.65。结果：直接三区读数比例/错误平面标定各421次分析放行，其中165次合成错误放行；几何合格包络249次分析放行、0次合成错误放行、7次保守拒绝，600/600覆盖生成真值；未知几何一律HOLD。另5种人工留出加载模式：正确模型15/15观测单元未被证伪；故意篡改一个读数后1项违反包络而HOLD。R15新增45/45测试、R14/R13回归52/52通过。另以独立解析反演公式核查200组精确无噪声合成状态，发现并修复标定区间交集因约$10^{-16}$浮点舍入而错误HOLD的数值边界问题；修复后最大绝对误差$2.22\times10^{-16}$，详见`R15_ANALYTIC_ORACLE.json`。所有数值**不是FR5或实体台架实测，也不是未来实物漏检率**。D15-08：若不能取得实际$q$及其误差界，在线边缘估计拒绝。

**E15-11**：$\mathrm{false\ clear}=\#\{\widehat e_{\rm upper}\le\tau\ \land\ e_{\rm generator}>\tau\}$；仅用于人工合成评估。

**T15-11（研究证据/决策表）**

|人工合成对照|分析放行|合成错误放行|备注|
|---|---:|---:|---|
|直接三区比例|421|165|错误忽略几何泄漏|
|平面校准|421|165|本合成例等同直接比例|
|几何包络LP|249|0|假设K/E真实有效|
|未知几何HOLD|0|0|完全保守|


## 12. 非人体真值台架：跨几何留出与加载路径闭合

R14提出隔离三区、独立总力；R15增加**跨条件传力函数校准**：台式加载器（不连接FR5）用独立标准力计分别加载左/中/右，再加载左右对称、中心+边缘混合与倾斜刮板；记录每区原始ADC、温度、加载点、刮板姿态、曲率、仿体硬度、日期、卸载方向。使用无载零点、三基向量单区加载构造$A(q)$；若力路径随分布加载非线性变化，线性模型退出。每一类材料和日期留整组验证，禁止随机帧泄漏；静态合格后才考虑动态带宽与压力膜插入扰动。独立总力验证$\sum y$是否闭合，**只作必要诊断而非空间正确性的充分证据**。附`NONHUMAN_BENCH_PROTOCOL_R15.md`和空CSV模板，绝不把模板当采集数据。入门E0相机/工具几何，E1分区校准，E2时间，E3厂家SDK，E4非人体加载，E5受控接触；真人另需风险伦理审批。

**E15-12**：$r_{\rm closure}=|F_{\rm independent}-\mathbf1^Ty|,\quad r_{\rm closure}\approx0\ \not\Rightarrow\ \hat x=x$。

**T15-12（研究证据/决策表）**

|台架实验组|独立量测|预注册失败判据|
|---|---|---|
|逐区/双区/三区静载|标准力计+原始三区ADC|校准包络留出超界|
|曲率/倾角/材料留出|标定板、角度计、独立力计|几何外推不可信|
|同wrench异边缘|独立受力分区及几何|无法区分或桥接过大|
|动态换向（后续）|高带宽力台+硬件时钟|采样/相位不足|


## 13. 主学术Baseline竞争表、排序变化与最小补证

维持至少五篇主论文候选，工程强Baseline独立列出，不预锁定赢家。VPEC2022（Grady等，IROS，DOI 10.1109/IROS47612.2022.9982073）：MIT源码/评估入口可见，约170GB数据与权重未下载，外部相机软夹爪、FR5真实边缘不可见性未知；仍为**离线复现资格领先**，非主论文锁定。ViPER2023（Collins等，DOI 10.1109/IROS55552.2023.10342124）：全文+Eye-in-Hand/F/T弱标签，压力阵列强标签，但软夹爪可见形变和Ubuntu20.04/Sensel依赖，根目录许可未确认；**同层竞争优先**。TacGraph2026（Van der Merwe等，DOI 10.1109/LRA.2026.3653324，历史出版社核验）：几何+力约束，单点/刚体/GelSight/KUKA，原样暂缓。Jiang2024（DOI 10.3390/act13020062）：积分自适应导纳，2ms等接口条件尚不匹配，条件保留。Chelly2025（arXiv:2411.13335v2）新增候选观察位：真实触觉+独立力计+跨材料，但需Allegro力矩，**暂缓原样FR5复现**。Arefin2026（arXiv:2609.34400）B-mode/Franka，Foster2026（arXiv:2610.05839）固定限速强对照，Peng2024（DOI 10.1109/ICRA57147.2024.10610233）内部气泡传感器，均保留竞争。厂家`FT_Control`是独立工程强Baseline，本机固件/外环时序未取得。D15-09：R15不更换主论文领先者，但**提升“可取得独立刮板真值”的排序权重**；没有物理真值就不启动大模型迁移。

**E15-13**：$\mathrm{Eligible}=\mathrm{Access}\land\mathrm{License}\land\mathrm{Interface}\land\mathrm{Truth}\land\mathrm{FairCompare}$；不是主观加权分数。

**T15-13（研究证据/决策表）**

|论文/方案|R14→R15|复现/许可证|FR5适配/真值缺口|决定|
|---|---|---|---|---|
|VPEC2022|保持源码资格领先|MIT；权重/170GB未取|软夹爪外部视觉，不是刮板|条件复现|
|ViPER2023|同层优先保持|仓库可见；许可待核|遮挡、Sensel/Ubuntu20.04|条件保留|
|TacGraph2026|方法竞争保持|全文；代码许可待核|单点/刚体/不同机器人|原样暂缓|
|Jiang2024 IAAC|力控候选保持|全文历史；代码待核|2ms/FT接口未核|条件保留|
|Chelly2025|新增观察位|全文；代码未取|Allegro关节力矩/分布触觉|原样暂缓|
|FR5 FT_Control|工程强基线保持|官方接口，固件待核|真实闭环时序/独立力台|优先实机核验|


## 14. 唯一创新点与备选：新颖性反证、收益和失败退出

I-A的可检验独特问题不是“用MLP预测压力”或“加曲率”，而是**有限宽工具在几何条件变化、接触载荷不可唯一反演、参考测量不完美时，能否输出经过独立验证的边缘风险区间并拒绝不可信纠偏**。与S15-01曲率校准、S15-02全阵列力估计、S15-07视觉弱标签、S15-08几何力融合的重叠必须在论文相关工作中承认；如果后续检索发现相同的边缘风险可信拒绝方法，也应修订新颖性。比较I-A相对强Baseline：仅wrench、直接三区比例、几何校准但无误差界、几何包络LP、匹配RGB-D+LP、打乱RGB-D+LP、无独立总力/时间消融。主要指标是独立真值上的错误放行率、放行覆盖率、区间宽度、校准成本和拒绝延迟。I-B备选只在厂家FT_Control+固定限速+离面返程未满足预注册目标时启动；未知底层力矩接口不加ESO/MPC。

**E15-14**：$\Delta_{\rm RGBD}=\mathrm{Metric}(\mathrm{matched})-\mathrm{Metric}(\mathrm{shuffled})$，须在**相同实体样本和独立真值**上估计置信区间。

**T15-14（研究证据/决策表）**

|消融|必须保持相同|反证目标|
|---|---|---|
|wrench-only vs LP|载荷、曲率、材料、时间|额外传感信息收益|
|匹配RGB-D vs 打乱RGB-D|样本、模型容量、标定数据|视觉是否提供独立信息|
|固定几何 vs 条件包络|传感器和训练预算|几何误差是否必要|
|厂家力控 vs 简单状态机|路径、仿体、独立力台|备选控制是否有实际收益|


## 15. FR5硬件接口、坐标与时间Gate

现有硬件：FR5、Eye-in-Hand RGB-D、腕部六维F/T、Ubuntu22.04、ROS2、GPU。S15-10官方ROS2仓库可访问，但本轮**没有连接本机FR5、没有核验SDK/固件版本、没有抓取反馈包**；R12已发现某官方非实时路径时间戳不是可靠采集时刻，R13建立独立事件锚点思路。本轮研究只读、离线。实际实施顺序：相机内参/手眼、工具TCP与真实有限宽边缘、六维力零偏/重力/动态补偿、`FT_Control`真实接口和反馈周期、外部采集时间、非人体独立力台，最后才考虑低能量接触。法兰位姿$T_{BF}$、标定TCP$T_{FT}$和柔性接触中心$C$不可混同；相机能看见的工具轮廓也不等于实际压力中心。未获厂家实际控制权限时，任何高频力矩闭环、贴面轻力返程或人体接触都不得执行。

**E15-15**：$\boldsymbol m_C=\boldsymbol m_S-(\boldsymbol p_C-\boldsymbol p_S)\times\boldsymbol F+\boldsymbol m_{\rm unknown}$；未知接触偶矩不等于零。

**T15-15（研究证据/决策表）**

|Gate|当前状态|必须取得的独立证据|
|---|---|---|
|E0几何|未过|内参、手眼、刮板边缘/TCP|
|E1力觉|未过|偏置、重力、动态补偿与独立力计|
|E2时间|未过|曝光/采集时钟和事件锚点|
|E3厂家接口|未过|本机固件/SDK/FT_Control时序|
|E4/E5非人体|未过|仿体、独立力台、低能量程序审查|


## 16. M1/M2/M3完整功能接口与模式安全约束

三入口仍完整保留：M1人工有序选点并指定路线；M2实体画线识别、起终方向确认、反投影有序三维曲线；M3自动识别背部解剖标志并给专家审核穴位候选，建立可编辑三维关键点图，再由用户选两点路线或已审核刮痧模板生成路线。三者**接触前都由用户审核**，上层采用成熟检测/曲面图搜索，不占博士核心创新份额。统一曲线记录$\{p_i,t_i,n_i,\Sigma_i,R_i,\mathrm{sweep\_eligible},\mathrm{mode}_i\}$，其中sweep_eligible检查有限宽工具在曲面上的连续扫掠是否可行；模式仅允许WORK、UNLOAD、OFFSURFACE_RETURN，贴面返程单独风险审查。任何几何、法向、视觉、时间或力觉证据不足时禁止自动纠偏，转入HOLD→受控卸载；未验证卸载执行权限前只输出建议不发运动命令。R15没有新增三入口真实网络实现，不虚报。

**E15-16**：$\mathcal C=\{(p_i,t_i,n_i,\Sigma_i,R_i,g_i,m_i)\}_{i=1}^N,\quad g_i\in\{0,1\},\ m_i\in\{\mathrm{WORK,UNLOAD,OFFSURFACE}\}$。

**T15-16（研究证据/决策表）**

|入口|人工审核要点|统一输出|
|---|---|---|
|M1人工选点|顺序、路径方向、禁区|有序3D曲线|
|M2实体画线|起终点、方向、曲面投影|有序3D曲线|
|M3穴位/模板|专家标志审核、用户模板选择|有序3D曲线|
|共用模式|工具宽度扫掠资格、卸载/返程|WORK→UNLOAD→OFFSURFACE|


## 17. 本轮内容验收、局限与下一轮自适应决策

本轮可审计实绩：实际读取R14五类材料及ZIP中的源码/决策/QA；重跑R14+R13=52项通过；核查12项去重论文/官方/作者来源，其中Zhong、Chelly、Egger三篇全文方法与实验深入比较，Kohlbrenner和ViPER等补充核读；新增三区同读数异载荷严格反例、几何条件误差包络LP、45/45新测试、600组合成消融、15单元留出校准反证及200组独立解析反演核验，修复一项浮点数值边界问题。两份Markdown按17章对应，公式E15-01…17、表T15-01…17、来源S15-01…12、决策D15-01…09在讲义逐项覆盖；QA实际由脚本检查，见`QA_R15.json`。**实体试验数=0，论文模型复现数=0，FR5指令数=0**。R15内容验收合格依据是新严格反例与可执行验证，而不是触发时长。下一轮优先：取得真实非人体几何/三分区传力数据；若没有，先审计可采购设备的真实传感路径和相机可见性，停止无限叠加合成证书。无授权GitHub写入。

**E15-17**：$G_{\rm physical}=0\ \not\Rightarrow\ G_{\rm research}=0$；但$G_{\rm physical}=0$意味着**不得宣称实体性能或安全性**。

**T15-17（研究证据/决策表）**

|验收项|实测结果|证据边界|
|---|---|---|
|新数学反例|精确同读数异边缘|限定合成传力模型|
|新增/回归测试|45/45 + 52/52通过|离线Python|
|人工合成/留出|600组、5加载模式|不是FR5数据|
|全文核读|3篇深入+其他章节|论文性能非FR5性能|
|实体/真人|0 / 0|E4/E5未过|


## 附录A｜12项来源URL、作者与访问层级

- **S15-01** Zhong, Kim, Losey, Nunez（2025），Curvature-Aware Calibration of Tactile Sensors for Accurate Force Estimation on Non-Planar Surfaces；标识：arXiv:2510.25965v2；本轮层级：FULL_HTML_METHOD_EXPERIMENT；URL：https://arxiv.org/html/2510.25965；访问限制：III–VIII; geometry calibration, MLP, five objects, 2–8N; note κ convention discrepancy。

- **S15-02** Chelly, Cherubini, Fraisse, Ben Amar, Khoramshahi（2025），Tactile-based force estimation for interaction control with robot fingers；标识：arXiv:2411.13335v2；本轮层级：FULL_HTML_METHOD_EXPERIMENT；URL：https://arxiv.org/html/2411.13335；访问限制：III–VI; M1–M5, ATI Mini45, 100Hz, 10Hz low pass, open/closed loop, out-of-distribution objects。

- **S15-03** Egger, Srinivas, Brandstötter（2025），Real-Time Detection and Localization of Force on a Capacitive Elastomeric Sensor Array Using Image Processing and Machine Learning；标识：DOI:10.3390/s25103011；本轮层级：FULL_HTML_METHOD_EXPERIMENT；URL：https://pmc.ncbi.nlm.nih.gov/articles/PMC12115265/；访问限制：2–4; 6x6, 16ms transmission, clustering processing latency, 70/30 regression split, hysteresis future。

- **S15-04** Kohlbrenner, Escobedo, Nechyporenko, Roncone（2024），A Sensor Position Localization Method for Flexible, Non-Uniform Capacitive Tactile Sensor Arrays；标识：arXiv:2412.00672v1；本轮层级：FULL_HTML_METHOD_EXPERIMENT；URL：https://arxiv.org/html/2412.00672；访问限制：III–V; 100 point logs, probe-position dependence, 2 skin patches; not force truth。

- **S15-05** Bae, Kim, Sim, Kang, Kim（2025），Dome-Structure Array from Pre-Strained Extendable Mesh for Tactile Sensing Without Crosstalk and Lateral Strain Interference；标识：DOI:10.1002/smtd.202401730；本轮层级：ABSTRACT_AND_METADATA；URL：https://onlinelibrary.wiley.com/doi/10.1002/smtd.202401730；访问限制：abstract and publication details; no full method/experiments claimed。

- **S15-06** Costanzo, De Maria, Natale, Pirozzi（2019），Design and Calibration of a Force/Tactile Sensor for Dexterous Manipulation；标识：DOI:10.3390/s19040966；本轮层级：ABSTRACT_AND_SEARCH_SNIPPETS；URL：https://pmc.ncbi.nlm.nih.gov/articles/PMC6412380/；访问限制：full page intermittently blocked by captcha; no full reading claimed。

- **S15-07** Collins, Houff, Grady, Kemp（2023），Visual Contact Pressure Estimation for Grippers in the Wild (ViPER)；标识：DOI:10.1109/IROS55552.2023.10342124；本轮层级：FULL_HTML_METHOD_EXPERIMENT；URL：https://arxiv.org/html/2303.07344；访问限制：III–IV, 47k fully/275k weak images, pressure array, occlusion and source baseline。

- **S15-08** Van der Merwe, Ota, Berenson, Fazeli, Jha（2026），Simultaneous Extrinsic Contact and In-Hand Pose Estimation via Distributed Tactile Sensing (TacGraph)；标识：DOI:10.1109/LRA.2026.3653324 (historical publisher check)；本轮层级：FULL_HTML_SECTIONS_AND_ABSTRACT；URL：https://arxiv.org/html/2512.23856；访问限制：III–VI contents available; current focus on contact assumptions; not executed。

- **S15-09** Healthcare-Robotics（2022），VPEC author repository；标识：GitHub project (paper DOI:10.1109/IROS47612.2022.9982073)；本轮层级：AUTHOR_README_AND_LICENSE；URL：https://github.com/Healthcare-Robotics/VPEC；访问限制：MIT license, Python3.10/PyTorch1.12.1, evaluator entry, 170GB dataset; no weights downloaded。

- **S15-10** FAIR-INNOVATION（2026），FAIRINO official ROS2 driver；标识：official GitHub repository; version varies；本轮层级：OFFICIAL_REPOSITORY_README；URL：https://github.com/FAIR-INNOVATION/frcobot_ros2；访问限制：driver families and ROS2 SDK existence; local FR5 firmware not verified。

- **S15-11** Peter Egger（2025），CapSense monitor author code；标识：GitHub MIT license；本轮层级：AUTHOR_README_AND_LICENSE；URL：https://github.com/peterpf/capsensor-monitor；访问限制：MIT, src/tests/poetry, serial touchpad UI; no physical pressure model reproduction。

- **S15-12** Healthcare-Robotics（2023），ViPER author repository；标识：GitHub; license unconfirmed；本轮层级：AUTHOR_README；URL：https://github.com/Healthcare-Robotics/ViPER；访问限制：Ubuntu20.04, Sensel API, weights link, model train/inference; no weights downloaded。

\n## 附录B｜坐标与数据/许可证/工程证据缺口\n坐标约定：$B$基座、$F$法兰、$T$标定TCP、$C$真实柔性接触中心、$S$六维力传感器。$x$为真实局部法向载荷积分，$y$为台架输出，$q$为几何协变量；三者不可互换。实际SDK固件、FT_Control外环周期、相机曝光时间、F/T采集时钟、工具标定、压力阵列逐元采样、台架非线性、材料外推、所有论文权重/许可证，均存在未闭合证据缺口。\n\n源码和合成输出位于`src/`、`tests/`、`results/`；只读，绝无机器人动作接口。研究假设阈值0.65不是人体安全阈值。仓库不写入、不开新仓库，十轮里程碑任务独立保留。\n