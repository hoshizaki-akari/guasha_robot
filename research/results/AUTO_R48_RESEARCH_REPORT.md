# AUTO-R48｜FAIRINO FR5刮痧机器人博士级算法研究完整报告

**日期：2026-10-21；状态：PASS_OFFLINE_RESEARCH_CONTENT_ONLY；主学术Baseline待定；所有实体/运动Gate HOLD。**

**本轮唯一实质创新候选的新增部分**：在R47有限宽taxel/公共注册偏移的保守边际区间上，加入同原点、同时间、独立F/T力矩与有界纯力偶的**精确公共平移可行投影**。不是新传感器，也不是FR5实体性能。历史R39–R47实跑713项+R48新增23项=736项通过；3篇全文、13项去重外部来源；512组固定种子人工消融，R48与R47均512/512覆盖，宽度从16.002111N缩至7.398026N。

**读者提醒**：报告的“精确”只修饰公共平移的数学可行投影；三箱输出仍为保守边际外包络，未知摩擦/多点压力/纯力偶不能由腕F/T唯一反演。所有代码始终HOLD_NO_MOTION。


## 01｜真实归档核验与研究差分

本轮不是把触发次数当研究成果。先从真实挂载路径读取R47研究报告51451字节、讲义74568字节、候选矩阵、决策日志、来源表、数学源码和测试脚本。R47 ZIP SHA256为ee6ff08fbbbde5aa590d585730297b7004cd6f11546d489f33f19f129340d2d5，30个成员CRC完好，29条内部SHA逐项匹配；重跑R39–R46历史688项与R47原25项均通过。本轮新增23项pytest测试，其中300个独立HiGHS投影LP、2000个隐藏接触点实例、250个单调性实例，不把内部循环次数伪报为pytest测试数。R47证明原生Sensel Force Array以grams报告、有限footprint+公共平移的不确定性必须保守传播；R48只新增独立wrench对同一平移的可行约束。R47边际区间作为被复用的强工程核，未改写旧源码。

**公式 F48-01**

$$
\mathrm{SHA256}(ZIP_{47})=\texttt{ee6ff08f\ldots a340d2d5}
$$

**表 T48-01**

|核验对象|实际结果|不能推论|
|---|---|---|
|R47归档|30成员、29 SHA、CRC通过|不等于实验真实性|
|历史测试|688+25=713通过|不含R30–R38|

**本章证据与可撤销决定**：S48-05；D48-01。这些标识可在附录的完整来源/决策登记中逐条核验。


## 02｜唯一科学问题与可证伪假设

本轮唯一首选问题：在有限宽刮板压力taxel载荷与公共一维注册平移已给出区间时，来自独立腕F/T的法向力与一阶力矩，能否在明确纯力偶与误差上界后，缩小“可信注册平移集合”，进而缩小左中右载荷的保守区间？这不是从wrench唯一反演压力，也不是允许机器人在线纠偏。预注册H48-1：若真实接触位置、共同平移、独立力矩及纯力偶满足模型，则真实平移必须落在投影区间；H48-2：将该区间传给R47事件包络不会漏掉真实三箱载荷；H48-3：在同信息合成盲测中，较R47一致性强基线可获得更窄的平均边际区间。失败标准包括隐藏真值漏覆盖、独立LP不一致、单位/坐标/时间不明、纯力偶上界不可证明；任一项触发拒绝或路线退出。

**公式 F48-02**

$$
H_{48}:\quad \delta^*\in D_{\mathrm{wrench}},\quad p_j^*\in[L_j,U_j]
$$

**表 T48-02**

|假设|独立判据|不成立时|
|---|---|---|
|平移投影|LP+隐藏δ|撤销数学声明|
|三箱覆盖|隐藏每taxel位置|拒绝可信输出|
|算法收益|同输入强基线|修订创新|

**本章证据与可撤销决定**：S48-03；D48-02, D48-04。这些标识可在附录的完整来源/决策登记中逐条核验。


## 03｜来源分级、文献核读与复现证据

本轮去重打开13项外部对象，A全文3篇：Zuo/Yang/Chua 2026、Min等2026、ViPER 2023，均核读方法、数据/实验、数学和限制；VPEC只打开正文片段，沿用R47全文审计而不虚报本轮A。Sensel API、NIST计量、FAIRINO 4.0.0手册、SOFA inverse许可证等按B级记录。三篇全文的任务和真值不同：Zuo在Fin-ray软夹爪用ATI Nano17验证**总抓取力**；Min在带气动皮肤的机械臂验证**电流残差与外部力**；ViPER在fully压力阵列数据验证**二维压力图**，weak仅F/T。不能把不同指标、不同传感器或作者阈值放进同一个“FR5性能排行榜”。所有DOI、版本、是否有可复用源码与数据都在SOURCE_REGISTER中列明。

**公式 F48-03**

$$
N_{\mathrm{screened}}=13,\qquad N_{\mathrm{fulltext}}=3
$$

**表 T48-03**

|全文|方法|真值/局限|
|---|---|---|
|Zuo2026|RGB-D+逆FEM|ATI总力，不是空间图|
|Min2026|接触事件+TCN|电流mA，不是局部压力|
|ViPER2023|RGB→压力图|fully有物理阵列，weak没有|

**本章证据与可撤销决定**：S48-01, S48-02, S48-03, S48-04, S48-13；D48-07, D48-08。这些标识可在附录的完整来源/决策登记中逐条核验。


## 04｜2026视觉接触定位新颖性反证

Zuo等2026全文给出DLC从腕RGB-D抽取柔性Fin-ray夹爪关键点，FoundationPose与点云重建估计物体位姿，SOFA inverse QP以14个候选接触位置迭代切换力作用点。作者在台架上用ATI Nano17获得独立总力，三直径×三位置×五加载位，并在Kinova Gen3上测试；报告加载阶段平均RMSE 0.23N、全抓取0.48N。方法受TPU线性弹性假设、关键点遮挡、内部应变、FoundationPose接触前10Hz和0.25秒力起始延迟影响。论文已覆盖“视觉几何定位+逆模型估力”的一般方向，因此本项目不得宣称该组合是首创。其总抓取力参考没有提供三箱压力分布独立真值；与本轮的注册区间可行证书仍有明确不同。

**公式 F48-04**

$$
J(\lambda_a)=\tfrac12\lambda_a^T(W_{ea}^TW_{ea}+\epsilon W_{aa})\lambda_a+\lambda_a^TW_{ea}\delta_e^{free}
$$

**表 T48-04**

|作者实验|可用证据|不可迁移|
|---|---|---|
|ATI Nano17台架|独立总力|无逐taxel真值|
|SOFA逆FEM|可优化力点|特定TPU模型/AGPL插件|

**本章证据与可撤销决定**：S48-01, S48-09, S48-10；D48-07, D48-10。这些标识可在附录的完整来源/决策登记中逐条核验。


## 05｜2026摩擦/接触状态分离反证

Min等2026在带气动机器人皮肤的机械臂上，用皮肤接触事件确定静止接触前的电机残差基线；在静止且摩擦近似不变的条件下，以接触后的残差增量估计外部力矩。运动开始后静摩擦假设失效，作者引入TCN利用状态历史处理Stribeck/滞回过渡。作者表I的平均电流残差RMSE静态280.17→31.96mA、静转动270.95→124.81mA；作者明确电机力矩常数未获，报告单位mA。该结果既不是F/T压力真值，也不能被移植为FR5阈值或带宽。备选创新I-B不应变成“泛化TCN摩擦补偿”，而要聚焦FR5可执行的WORK/UNLOAD/OFFSURFACE_RETURN换向时序与失效拒绝。

**公式 F48-05**

$$
\Delta\tau_{res}(t)=\tau_{res}(t)-\tau_{res}(t_{on}^{-})
$$

**表 T48-05**

|状态|Min方法假设|FR5缺口|
|---|---|---|
|静止接触|基线摩擦不变|无气动皮肤事件|
|动态换向|TCN历史补偿|无实测时序/电流接口|

**本章证据与可撤销决定**：S48-02；D48-08。这些标识可在附录的完整来源/决策登记中逐条核验。


## 06｜ViPER与VPEC主Baseline竞争

ViPER全文将训练集分为fully（RGB+真实阵列压力+F/T）与weak（RGB+F/T），以ResNeXt-50编码、压力解码、弱标签头和域对抗模块训练。作者在held-out家庭场景的fully集报告ViPER volumetric IoU 40.3%、重训VPEC 28.9%，并在weak集报告F/T RMSE；只有fully能独立计算压力IoU。VPEC用单帧RGB映射压力图，作者平面Sensel与ChArUco定位销提供注册参考。两篇论文均与FR5工具几何、D435深度、背部曲率不同，ViPER仓库根目录许可证尚未确认，VPEC原始数据与单位转换链未取得。R48没有因一项合成算法收益就改动论文候选排名，仍优先审计VPEC与ViPER的合法fully-test数据。

**公式 F48-06**

$$
IoU_{vol}=\frac{\sum_{i,j}\min(P_{ij},\hat P_{ij})}{\sum_{i,j}\max(P_{ij},\hat P_{ij})}
$$

**表 T48-06**

|候选|独立空间标签|复现阻塞|
|---|---|---|
|VPEC|作者平面Sensel|约170GB数据/单位/注册|
|ViPER|fully有，weak无|许可证/权重/原始episode|

**本章证据与可撤销决定**：S48-03, S48-04, S48-05；D48-11。这些标识可在附录的完整来源/决策登记中逐条核验。


## 07｜坐标、力矩符号与三种工具位姿

明确区分法兰位姿T_base_flange、标定TCP位姿T_base_TCP，以及柔性刮板真实接触线中心T_base_contact（当前UNKNOWN）。本轮仅定义工具接触线局部一维x横跨刮板，z为法向，y构成右手系。每taxel法向力f_i沿+z作用于x_i时，关于工具接触线原点的y轴力矩τ_y=-Σx_i f_i，故定义q=-τ_y。传感器原点与工具原点不同：若传感器原点在工具坐标x_s处，则q_tool=q_sensor+F x_s（在这个纯法向简化模型中）；一般必须用六维wrench的SE(3)伴随变换、重力/偏置/动态补偿和时间对齐。只用RCS设置值不能证明真实柔性接触线位置已标定。

**公式 F48-07**

$$
q_{tool}=-\tau_{y,tool}=q_{sensor}+F_z x_s
$$

**表 T48-07**

|对象|能提供什么|不能等同什么|
|---|---|---|
|法兰|机器人末端刚体位姿|柔性接触线|
|标定TCP|刚性工具参考|真实压载中心|
|接触线|随变形/偏载变化|传感器原点|

**本章证据与可撤销决定**：S48-06；D48-05, D48-09。这些标识可在附录的完整来源/决策登记中逐条核验。


## 08｜有限宽taxel、原生单位与几何先验

沿用R47的不可修改核心：taxel i在工具接触线上的名义中心u_i、footprint宽w_i>0、非负已知载荷f_i。输入Pa且面积m²时f_i=P_i A_i；显式`gf_equivalent`且来源为Sensel force array时f_i=0.00980665 g_i，但这仅是人工数学解释，不能代替厂家/力台计量证书。真实作用点x_i=u_i+δ+e_i，|e_i|≤w_i/2，所有taxel共享同一δ。R47会拒绝footprint在任何允许平移下离开有限工具宽度[-a,a]；本轮不把接触线投影到三维曲面，也不假定每taxel压力均匀。宽度不是噪声方差，而是几何支持集合。

**公式 F48-08**

$$
f_i=P_iA_i\quad\text{或}\quad f_i=0.00980665g_i,\qquad x_i=u_i+\delta+e_i
$$

**表 T48-08**

|输入|量纲|拒绝条件|
|---|---|---|
|Pa×m²|N|面积≤0/来源不明|
|gf等效|N|原生g未经声明|
|taxel宽度|m|超出工具扫掠边界|

**本章证据与可撤销决定**：S48-05, S48-08；D48-02。这些标识可在附录的完整来源/决策登记中逐条核验。


## 09｜独立wrench与力守恒入口Gate

本轮输入WrenchEvidence独立记录法向总力F_w、工具接触线原点的一阶力矩q、总力误差预算ε_F、力矩误差ε_q、纯接触力偶界C，以及episode、标定ID、纳秒时间、符号与来源。先验证F_taxel=Σf_i>0且|F_taxel−F_w|≤ε_F；不成立返回REJECT_FORCE_MISMATCH或REJECT_ZERO_FORCE且result=None。来源字段只允许`SYNTHETIC_INDEPENDENT_WRENCH`，明确拒绝由同一taxel积分派生的伪独立wrench；此字符串仅是人工实验契约，不是现实中两台仪器独立性的证明。episode不一致、标定相同/缺失、时间差超预算、浮点NaN/Inf或bool伪装数字均拒绝。

**公式 F48-09**

$$
F=\sum_i f_i>0,\qquad |F-F_w|\leq\epsilon_F
$$

**表 T48-09**

|Gate|成功|失败动作|
|---|---|---|
|力守恒|允许进入矩投影|拒绝无估计|
|独立来源|仅合成独立标记|拒绝伪独立|
|时钟/episode|一致|拒绝错配|

**本章证据与可撤销决定**：S48-06, S48-08；D48-05。这些标识可在附录的完整来源/决策登记中逐条核验。


## 10｜关键数学推导：力矩约束收紧注册平移

定义F=Σf_i，B=Σf_i u_i，H=Σf_i w_i/2，真实几何力矩M=Σf_i x_i=B+Fδ+Σf_i e_i。因f_i≥0且e_i∈[-w_i/2,w_i/2]，对于固定δ，M的全部可达值恰好构成[B−H+Fδ,B+H+Fδ]：每个e_i可连续变化，区间和没有间隙。测得q=M+c+η，|c|≤C、|η|≤ε_q，因此真实M∈[q−b,q+b]，b=C+ε_q。两个闭区间相交的充要条件给出精确δ投影：[ (q−b−B−H)/F, (q+b−B+H)/F ] 与R47先验[δ_min,δ_max]取交。若为空则拒绝；若不空，仅说明数学可行，不代表真实传感器已校准。

**公式 F48-10**

$$
D_{48}=[\delta_{min},\delta_{max}]\cap\left[\frac{q-b-B-H}{F},\frac{q+b-B+H}{F}\right]
$$

**表 T48-10**

|符号|含义|单位|
|---|---|---|
|F|总法向力|N|
|B|名义一阶矩|N·m|
|H|footprint矩不确定度|N·m|
|b|纯力偶+矩误差上界|N·m|

**本章证据与可撤销决定**：S48-06；D48-02。这些标识可在附录的完整来源/决策登记中逐条核验。


## 11｜精确性证明与独立线性规划Oracle

精确性仅针对**公共平移δ的可行投影**，不针对三箱同时可行集合。充分性：给定任何落在投影区间的δ，几何力矩区间与观测矩区间相交，因每taxel作用点可在连续footprint内任意选择，存在一组e_i实现交点；又因c和η的区间和覆盖[-b,b]，可以构造误差/纯力偶使q一致。必要性直接来自区间包含关系。为防止推导和代码犯同一种错误，另写独立SciPy HiGHS LP：变量δ、每taxel力矩m_i、合并扰动t，约束m_i−f_iδ∈[f_i(u_i−w_i/2),f_i(u_i+w_i/2)]、Σm_i+t=q、|t|≤b。随机300组求δ最小最大与闭式比较，最大误差在测试容限2e−8m内；这是数学Oracle，不是物理复现。

**公式 F48-11**

$$
\min/\max\;\delta\quad\mathrm{s.t.}\; f_i(u_i-w_i/2)\leq m_i-f_i\delta\leq f_i(u_i+w_i/2)
$$

**表 T48-11**

|验证方法|独立性|结论|
|---|---|---|
|闭式投影|代数式|候选区间|
|SciPy HiGHS LP|不同变量/求解器|300组合成一致|

**本章证据与可撤销决定**：S48-08；D48-06。这些标识可在附录的完整来源/决策登记中逐条核验。


## 12｜R47有限footprint事件包络的安全继承

R48不尝试从腕wrench唯一求三箱压力，而是先把R47的公共平移先验缩小为D48，再调用未修改的`spatial_registration_gate.envelope`。R47以工具边界[-a,-a/3,a/3,a]为三箱边界，枚举每个taxel左右footprint与边界相交的所有平移事件及相邻事件中点；对每个候选平移，计算每箱“必然落入”载荷下界和“可能落入”载荷上界，最后取边际包络。缩小平移集合只能使下界不减、上界不增，且真实接触点若满足全部假设必被覆盖。此区间是每箱的边际外包络，不是可同时取到所有极值的三维长方体，更不是已求解含wrench条件的精确三箱投影。

**公式 F48-12**

$$
[L_j^{48},U_j^{48}]\subseteq[L_j^{47},U_j^{47}]\quad(j=L,M,R)
$$

**表 T48-12**

|方法|可利用信息|结果含义|
|---|---|---|
|R47|taxel+注册先验|保守三箱边际|
|R48|R47+独立q|更紧或相同的保守边际|

**本章证据与可撤销决定**：S48-05；D48-03。这些标识可在附录的完整来源/决策登记中逐条核验。


## 13｜纯接触力偶与测量误差的可辨识边界

模型中的c是未知纯接触力偶、未建模切向耦合或工具柔性内部应力对目标矩的等效项；不能把它默认为零。若C增大，b=C+ε_q增大，平移区间只会放宽，可能退化为R47原区间；若C被错误低估，则算法可能排除真实δ却仍在错误模型内给出可行窄区间。R48特意实现反例：真实δ超出宣称注册界或真实力偶超出C时，输入仍可产生`CONDITIONAL_INTERVAL_ONLY`，但真实位置不在区间。故任何“覆盖保证”都必须连同假设与独立校准写出。未知摩擦系数、压力分布和纯力偶一般不能从单腕wrench唯一反演。

**公式 F48-13**

$$
|c|\leq C,\quad |\eta|\leq\epsilon_q,\quad b=C+\epsilon_q
$$

**表 T48-13**

|误差来源|本轮处理|未解决|
|---|---|---|
|纯力偶|有界C|真实C未标定|
|矩噪声|有界εq|动态漂移未知|
|摩擦/形变|等效不确定|多点非唯一|

**本章证据与可撤销决定**：S48-11；D48-05。这些标识可在附录的完整来源/决策登记中逐条核验。


## 14｜原点误用与错误证书的决定性反例

本轮新增两个可执行反例。反例A：真实δ=0.006m，F=2N，传感器原点相对接触线位移x_s=0.01m；真实工具矩q_tool=0.012Nm，传感器原点矩q_sensor=−0.008Nm。若不做杠杆变换，R48会推得约−4mm而不是+6mm的平移，模型内部仍可行，真实δ被排除；正确加上F x_s后覆盖恢复。反例B：真实接触位于声明区间外且存在未登记纯力偶，观测矩可以伪装成区间内接触。两个反例都表明“数学可行”不等于“物理可信”。真实系统必须把RCS、传感器零点、TCP、柔性边缘、工具重力/动态补偿和同步Gate设为外部必备证据。

**公式 F48-14**

$$
q_{tool}=q_{sensor}+F x_s,\qquad 0.012=(-0.008)+2(0.01)\;\mathrm{N\,m}
$$

**表 T48-14**

|反例|错误输入|结果|
|---|---|---|
|原点偏10mm|未转换q_sensor|假窄且漏真值|
|漏报纯力偶|C过小|可行但不可信|

**本章证据与可撤销决定**：S48-06；D48-05。这些标识可在附录的完整来源/决策登记中逐条核验。


## 15｜严格类型、时间、ROS2边界与拒绝状态

新`WrenchEvidence`使用冻结dataclass，要求N/Nm/m、int64纳秒时间、同episode、不同标定ID、有限数值、正确符号与工具接触线坐标。bool不得冒充int或float；NaN/Inf、负误差预算、超时钟偏差、伪独立来源直接抛ValueError。可计算但力守恒/矩不相容则返回`REJECT_FORCE_MISMATCH`或`REJECT_MOMENT_INCONSISTENT`，所有路径`state=HOLD_NO_MOTION`、`motion_permission=False`，拒绝时`marginal_bins_N=None`。ROS2 Humble仅定义未来离线消息适配边界，要求header.frame_id、同钟stamp、原始消息序号/标定版本，不发布控制命令。没有调用FAIRINO Python SDK，也没有打开机器人网络连接。

**公式 F48-15**

$$
|t_{FT}-t_{taxel}|\leq t_{max}\quad\text{且}\quad episode_{FT}=episode_{taxel}
$$

**表 T48-15**

|拒绝类|原因|输出|
|---|---|---|
|ValueError|格式/坐标/时间错误|无估计|
|REJECT_*|力矩物理不一致|None+HOLD|
|CONDITIONAL_INTERVAL_ONLY|仅数学条件成立|HOLD|

**本章证据与可撤销决定**：S48-06, S48-07；D48-05, D48-09。这些标识可在附录的完整来源/决策登记中逐条核验。


## 16｜实际算法实现、依赖与源码许可审计

本轮自编`wrench_registration_gate.py`只依赖Python标准库与R47未改动的`spatial_registration_gate.py`，没有网络、设备、ROS、SDK导入。`intersect`先检查证据契约和力一致性，计算B/H/b，求δ区间，再调用R47事件包络。独立测试Oracle使用SciPy HiGHS，但正式算法不依赖求解器；消融使用固定种子Python random。Zuo2026论文的SOFA逆模型源码未复现，官方SoftRobots.Inverse插件标AGPL-v3+Clause11专利许可，不能未经审查并入本项目。ViPER公开仓库不自动等于有可复用许可证。FAIRINO官方SDK Apache-2.0，但本轮仅查文档和仓库首页，没有安装或执行。

**公式 F48-16**

$$
\mathrm{R48}=\mathrm{R47.envelope}\circ\mathrm{intersect\_moment}
$$

**表 T48-16**

|对象|许可/版本证据|本轮是否执行|
|---|---|---|
|自编R48|原创离线代码|是|
|SOFA inverse|AGPL-v3+Clause11|否|
|FAIRINO SDK|Apache-2.0仓库|否|

**本章证据与可撤销决定**：S48-07, S48-09, S48-10；D48-10。这些标识可在附录的完整来源/决策登记中逐条核验。


## 17｜独立Oracle、回归与错误处理实测

本轮实际运行R39–R46历史688项、R47原25项、新增R48 pytest23项，合计736/736通过；新增23项包括300组独立LP、2000组隐藏位置真值、250组边际单调性、错误单位/时间/原点/纯力偶/力守恒拒绝。测试运行在本容器Python3、SciPy1.17.0、pytest9.0.2，JUnit记录保存。历史测试通过真实R47 ZIP内的嵌套R46归档重建并执行，而不是引用上一轮的通过声明；R30–R38未运行。独立LP与闭式投影比较绝对差<2e−8m；这只是数值一致性，不是空间压力物理准确率。执行前后均未连接任何FR5/D435/KWR75D设备。

**公式 F48-17**

$$
N_{test}=688+25+23=736,\qquad N_{fail}=0
$$

**表 T48-17**

|来源|本轮实际pytest项|状态|
|---|---:|---|
|R39–R46|688|通过|
|R47|25|通过|
|R48|23|通过|

**本章证据与可撤销决定**：S48-08；D48-01, D48-06。这些标识可在附录的完整来源/决策登记中逐条核验。


## 18｜公平消融、强Baseline与独立真值

固定种子480048生成512组人工非人体episode：5个taxel集中在三箱边界附近、随机footprint/载荷、真实公共平移在±9mm内、先验±12mm、纯力偶≤0.0005Nm、矩噪声≤0.0001Nm。独立数学真值来自生成器隐藏的每taxel真实位置与区域载荷，算法只见taxel名义位置/载荷、先验、独立合成wrench。公平比较：(i)名义δ=0错误强假设；(ii)R47+同一wrench总力/矩全局一致性检查，但不利用矩缩小平移的强Baseline；(iii)R48精确δ投影+相同R47包络。三者均收到相同信息，差异仅在如何使用矩约束。512例R47与R48均覆盖512，名义仅283；R47平均三箱边际区间宽度之和16.002111N，R48为7.398026N，**条件性收窄约53.77%**。R48平均平移宽4.068mm，R47先验24mm。不能称实体精度改善，且未与最优联合力矩—分箱LP比较。

**公式 F48-18**

$$
G_{width}=1-\frac{7.3980257317}{16.0021111197}\approx53.77\%
$$

**表 T48-18**

|方法|合成覆盖/512|平均三箱宽度和N|
|---|---:|---:|
|名义零偏|283|5.271919|
|R47同信息一致性强Baseline|512|16.002111|
|R48力矩投影|512|7.398026|

**本章证据与可撤销决定**：S48-03；D48-04。这些标识可在附录的完整来源/决策登记中逐条核验。


## 19｜可证伪物理最小实验与Gate

真正进入物理实验前须取得合法非人体压力阵列或分段力台、独立F/T、标定工具宽度/接触线、原生单位/标定记录、曲率/材料/安装证据和同钟时间戳。预注册完整episode/材料/曲率/遮挡留出，固定R47/R48算法与阈值；同一episode同时计算覆盖率、平均区间宽度、拒绝率、真实接触支持和F/T一致性，独立空间压力为真值而非腕wrench派生。入门Gate G0：相机/手眼/法兰/TCP/柔性边缘；G1：F/T偏置、重力、动态、RCS、SDK/固件时序；G2：纯力偶与矩噪声上界；G3：独立空间真值；G4：同信息盲检；G5：真人风险/伦理。任一未满足，继续HOLD。

**公式 F48-19**

$$
Coverage=\frac{\#\{episode: p_j^*\in[L_j,U_j]\;\forall j\}}{N_{eligible}}
$$

**表 T48-19**

|Gate|独立证据|当前状态|
|---|---|---|
|G0–G2|几何、力矩、时序/纯力偶|HOLD|
|G3–G4|真实非人体压力/盲检|HOLD|
|G5|真人伦理与风险|HOLD|

**本章证据与可撤销决定**：S48-03, S48-08；D48-12。这些标识可在附录的完整来源/决策登记中逐条核验。


## 20｜五篇主论文候选与动态路线收敛

本轮维持五候选VPEC→ViPER→FEATS→ImplicitRDP→RDP：VPEC有独立平面Sensel物理空间标签但原始数据/单位链未取得；ViPER有fully强标签、weak仅F/T，代码许可证缺口；FEATS依赖FEA空间标签、F/T只核总力；ImplicitRDP的VRR为wrench确定性变换、Flexiv机器人入口和dill反序列化有风险；RDP依赖高频触觉与未经核验的控制接口。对每篇登记作者/题名/年/DOI、获取层级、源码/数据/许可/版本、数学/频率/底层接口、独立真值和最小补证，详见登记表。R48发现Zuo2026模型视觉定位更近邻，但因无合法代码数据与独立局部压力真值，不强行取代前五候选。

**公式 F48-20**

$$
B^*=\arg\max_{B\in\{VPEC,ViPER,FEATS,ImplicitRDP,RDP\}}\mathrm{ReproducibleEvidence}(B)
$$

**表 T48-20**

|优先级|论文|状态|
|---|---|---|
|1–2|VPEC/ViPER|条件保留|
|3|FEATS|FEA辅助|
|4–5|ImplicitRDP/RDP|暂缓|

**本章证据与可撤销决定**：S48-01, S48-03, S48-04, S48-09；D48-11。这些标识可在附录的完整来源/决策登记中逐条核验。


## 21｜M1/M2/M3完整系统与WORK—UNLOAD—RETURN

M1用户人工有序3D点/指定路线；M2实体背部画线识别、起终方向确认与有序三维曲线；M3自动识别解剖标志形成专家审核的穴位候选、可信可编辑关键点图，用户可选两点路径或审核刮痧方式模板。三入口统一输出位置、单位切向/曲面法向、置信度与不确定性、工具姿态、真实有限宽连续扫掠资格、法兰/TCP/柔性接触线区分及WORK/UNLOAD/OFFSURFACE_RETURN模式；接触前必须人工审核。上层成熟视觉网络和曲面图搜索不占博士唯一主创新。离面无接触轨迹、低能量单向恒力、平滑卸载、离面返程依序推进；贴面轻力返程须后续审查。本轮只开发局部载荷数学Gate，没有生成任何机器人运动命令。

**公式 F48-21**

$$
\Gamma(s)=\big(p(s),t(s),n(s),\Sigma(s),R_{tool}(s),mode(s)\big)
$$

**表 T48-21**

|入口|人工确认|统一输出|
|---|---|---|
|M1有序点|路线顺序|3D曲线+模式|
|M2实体画线|起终方向|3D曲线+模式|
|M3解剖候选|穴位/模板审核|3D曲线+模式|

**本章证据与可撤销决定**：S48-06；D48-12。这些标识可在附录的完整来源/决策登记中逐条核验。


## 22｜FR5厂家接口、许可证、安全与下一轮

本轮新增核查FAIRINO 4.0.0官方手册§12：`FT_SetRCS`区分工具/基座，`FT_GetForceTorqueOrigin`与`FT_GetForceTorqueRCS`区分原始/参考系力矩，payload/CoG必须处理，`FT_Control`虽有公开参数却未实机验证。尤须注意手册`FT_SetConfig`列坤维company=17、device=0为KWR75B，**不能据此证明用户KWR75D及历史[1,17,3,0]配置正确**；SDK v2.3.0/robot v4.0.0实际反馈时序、外环延迟、运动保护全部未知。R48研究只读手册，不执行任何命令。下一轮最小任务：合法取得一条完整非人体独立压力episode，连同工具接触线SE(3)、压力原生单位、F/T同步与误差预算；若不能取得则优先离线检查R48的联合分箱-力矩可行域与最坏误差，不增加复杂控制。I-A条件保留，I-B暂缓；六份开题文档不因无物理证据而假修订。GitHub仅授权现有独立研究仓库才可归档，旧仓库禁止访问修改。

**公式 F48-22**

$$
\mathrm{MotionPermission}=\mathrm{False}\quad\forall\;R48\;outputs
$$

**表 T48-22**

|待补证|最小可核查交付|状态|
|---|---|---|
|KWR75D配置/SDK|型号与固件读回+时序|HOLD|
|独立空间压力|原始episode/许可/标定|HOLD|
|物理/伦理|非人体先行、真人独立批准|HOLD|

**本章证据与可撤销决定**：S48-06, S48-07, S48-12；D48-09, D48-12。这些标识可在附录的完整来源/决策登记中逐条核验。


---

## 附录A｜来源完整审计（与教学讲义共享证据对象）

# AUTO-R48 来源审计（2026-10-21，按独立对象去重）

分级：**A**=本轮实际打开公开全文HTML并阅读方法/公式、实验和限制；**B**=实际打开官方页面、论文摘要或源码仓库首页，不是全文；**H**=上一轮可访问归档的继承结论（本轮未重新核读）。**本轮13个独立外部来源对象，其中3篇A全文、10个B级**；网页镜像、同一论文摘要与正文不重复计数。只读浏览，未克隆/下载作者源码、模型、数据，也未运行任何机器人入口。

|ID|文献/官方来源、日期、DOI、URL|本轮获取级别、认真核读内容|与本项目的可迁移性/限制|
|---|---|---|---|
|S48-01|Zuo, Yang, Chua (2026), *A Model-based Visual Contact Localization and Force Sensing System for Compliant Robotic Grippers*, arXiv:2605.00307v1，DOI `10.48550/arXiv.2605.00307`, https://arxiv.org/html/2605.00307|**A全文**，读II方法（SOFA QP逆FEM、14候选点、DLC/ICP/FoundationPose），III静态/实体实验（ATI Nano17独立总力，三种直径/位置、五加载位），IV限制；作者报告加载阶段平均RMSE 0.23N、全周期0.48N；SOFA最高40Hz、相机30Hz、定位接触前10Hz，作者还报告0.25s起力延迟。|新颖性反证：视觉RGB-D+变形+迭代接触定位已有2026先例；然而其输出主要**总抓取力**，不是刮板三箱空间压力；Fin-ray TPU/kinova/SOFA≠FR5。论文正文未找到可核验作者专用代码仓库/数据许可，不能声称复现。|
|S48-02|Min, Ma, Kwon, Bae, Kim, Park (2026), *Tactile-Proprioceptive Sensor Fusion for Contact Wrench Estimation in Whole-Body Physical Human-Robot Interaction*, arXiv:2605.28412v1, DOI `10.48550/arXiv.2605.28412`, https://arxiv.org/html/2605.28412|**A全文**，读II摩擦动力学、IV静态接触差分/TCN状态转换、V实验表；作者电流残差静态平均RMSE 280.17→31.96mA，动态过渡270.95→124.81mA，论文明确电机力矩常数不可用而用mA报告。|新颖性反证：静止接触前基线与动态摩擦分离已有方案；依赖气动机器人皮肤和电机电流/运动学，FR5当前没有这些经核验的接口，不能移植阈值/性能。|
|S48-03|Collins, Houff, Grady, Kemp (2023), *Visual Contact Pressure Estimation for Grippers in the Wild* (ViPER), DOI `10.1109/IROS55552.2023.10342124`, https://arxiv.org/html/2303.07344|**A全文**，读III训练/强弱标签、IV测试/消融、V控制；47k fully（独立阵列）+275k weak（仅F/T）；作者fully volumetric IoU 40.3% vs重训VPEC 28.9%；weak不能验证空间压力。|本轮作为前两位主论文候选的直接对照；Eye-in-Hand RGB近似，但软夹爪≠刮板，源码许可证未核准；作者阈值/速率不得视作FR5。|
|S48-04|Grady et al. (2022), *Visual Pressure Estimation and Control for Soft Robotic Grippers* (VPEC), DOI `10.1109/IROS47612.2022.9982073`, https://arxiv.org/html/2204.07268|**B正文片段**；本轮打开论文首页/方法目录，继承R47的全文方法审计（H），本轮**不计新增全文**。|作者有平面Sensel独立压力参考；RGB/夹爪/Stretch迁移与原生数据单位仍是阻塞。|
|S48-05|Sensel Morph 官方API primer, https://guide.sensel.com/api/|**B官方文档**；force array 185×105、0–8192、单位grams，另有Lost Frame Count、接触坐标mm。|每taxel原生grams不能无证据当Pa；克力等效仍是条件性假设；本轮不下载实测。|
|S48-06|FAIRINO Collaborative Robot User Manual 4.0.0, Force Control §12, https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html|**B官方手册**；FT_SetRCS(tool/base)、FT_GetForceTorqueRCS/Origin、payload/CoG及FT_Control签名；**手册FT_SetConfig列坤维company=17、device=0对应KWR75B**，未证明用户KWR75D或历史`[1,17,3,0]`可用。|决定性接口阻塞：不能假定原始腕矩直接处于工具接触线坐标；要实际读配置/标定/时间、SDK v2.3.0+firmware v4.0.0核对；**本轮未调用API**。|
|S48-07|FAIRINO 官方Python SDK仓库，Apache-2.0，https://github.com/FAIR-INNOVATION/fairino-python-sdk|**B仓库首页**；确认项目和许可证，未下载特定SDK v2.3.0 commit。|许可证不等于KWR75D型号兼容、固件时序或运动许可。|
|S48-08|NIST, *Calibration of Force Transducers*, 更新2025-03-26，https://www.nist.gov/programs-projects/calibration-force-transducers|**B官方计量说明**；标定依赖传感器与读出系统组合，已知力与响应建立关系。|本轮人工误差界`C,E`不是NIST或KWR75D校准数据，物理认证继续HOLD。|
|S48-09|SofaDefrost *SoftRobots.Inverse*官方仓库，https://github.com/SofaDefrost/SoftRobots.Inverse|**B官方代码/许可证**；SOFA inverse插件为**AGPL-v3及Clause11专利许可说明**，不是普通MIT；有QP、qpOASES-3.2.0依赖。|Zuo2026提及SOFA逆FEM不能直接认为可无条件复制进本项目；未克隆、未复现，许可证需法律与依赖审查。|
|S48-10|SOFA官方插件介绍，https://www.sofa-framework.org/applications/plugins/inverse-model-for-soft-robot-control/|**B官方文档**；逆模型使用QP并可含接触约束。|SOFA通用开源≠FR5官方URDF/碰撞惯量/柔性工具模型授权，也非实物仿真。|
|S48-11|Likar & Žlajpah (2014), *External Joint Torque-Based Estimation of Contact Information*, DOI `10.5772/58834`, https://doi.org/10.5772/58834|**B期刊摘要与正文开头**；依赖关节力矩和圆柱连杆模型的非线性优化；不是本轮全文实验审计。|不能把关节力矩接触点算法当腕F/T多点柔性压力唯一解，FR5底层力矩未核实。|
|S48-12|Zhong, Kim, Losey, Nunez (2025 arXiv / 2026 HAPTICS), *Curvature-Aware Calibration of Tactile Sensors for Accurate Force Estimation on Non-Planar Surfaces*, DOI `10.1109/HAPTICS66823.2026.11495507`, https://doi.org/10.1109/HAPTICS66823.2026.11495507|**B出版摘要**；2026正式发表与2025预印本版本/样本数不同，IEEE正文访问受限。|新核验的版本差异提醒不可混用2025/2026样本数；曲率校准仍须独立实物验证。|
|S48-13|2025 IROS *Tactile-based force estimation for interaction control with robot fingers*, DOI `10.1109/IROS60139.2025.11246923`, https://ieeexplore.ieee.org/abstract/document/11246923|**B检索摘要**；有独立ATI F/T验证阵列总力与100Hz手指导纳实验，IEEE正文抓取418，**未读全文**。|独立总力验证≠逐taxel空间真值，作者0.12±0.08N不是FR5阈值/误差。|

**三篇决策影响力全文的逐项比较**：Zuo的14个接触候选+逆FEM估计**总抓取力**，用ATI Nano17验证，接触位置错误显著增大RMSE、受遮挡/内部应变影响；Min使用独立气动皮肤**接触事件**区分电机摩擦和外力，但在动态转换需TCN，性能指标是mA电流残差；ViPER使用RGB→压力图，**fully物理空间标签**与weak腕F/T严格分离，使用体积IoU/接触准确率而非仅总力RMSE。三者没有任何一个提供“FR5刮板接触线注册+独立三箱空间压力真值”的现成可复现结果。R48只使用自编独立数学模型，不调用三者源码。



## 附录B｜主论文/唯一创新竞争登记

# AUTO-R48｜BASELINE_AND_INNOVATION_REGISTER（跨轮可追溯）

**主论文最终赢家：待定。** R47序位 `VPEC→ViPER→FEATS→ImplicitRDP→RDP`；R48 **不调整**。这是合法离线复现审计优先级，不是原论文指标横向性能排序。上一轮变化R46→R47为Sensel原生单位/空间注册/曲率证据门槛；本轮R47→R48为**额外独立腕wrench能否约束注册平移、工具坐标原点与纯力偶界是否可证**。当前没有FR5实体、真实压力episode或官方许可模型。

|序位/变化|作者、准确题名、出版年份、DOI|全文/摘要层级、源码/数据/许可/版本（实际核查状态）|公式、参数、硬件/频率与独立真值|判定/缺口/最小补证|
|---|---|---|---|---|
|1 = **VPEC**|Grady, Collins, Brahmbhatt, Twigg, Tang, Hays, Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*；IROS **2022**；`10.1109/IROS47612.2022.9982073`|R47全文A，R48只打开正文片段B；官方VPEC仓库MIT，历史固定commit `371b9ec2…`（未重新抓取）；原始数据约170GB**未下载**，数据使用许可与标定文件未核实。|RGB→压力图，作者平面Sensel Morph物理空间标签，ChArUco+定位销；作者设备Stretch/软夹爪、图像约30Hz/阵列约100Hz≠FR5+D435+KWR75D；需要原生grams→Pa/标签语义、压力阵列→工具SE(3)、曲率校准、时间戳。无需假设FR5底层力矩即可离线推理，但作者闭环控制不迁移。|**推荐优先获取合法单条fully-test数据做离线标签审计，条件保留**；最小补证：原始force array、帧时间、面积/单位、校准、独立物理三箱标签、许可与作者权重版本。|
|2 = **ViPER**|Collins, Houff, Grady, Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*；IROS **2023**；`10.1109/IROS55552.2023.10342124`|R48**全文A**；官方仓库历史commit `08c7d2f…`，README公开但根目录许可证未核实，**不能默认有代码复用许可**；数据/权重未下载，版本锁定不完整。|RGB Eye-in-Hand→压力图，47k fully（阵列+F/T）/275k weak（仅F/T）；弱标签不能作为空间压力真值；作者重训VPEC fully volumetric IoU 28.9% vs ViPER 40.3%（仅作者场景）。ROS2/Ubuntu22.04/双3090可做未来移植评估，但Ubuntu20.04/Stretch/作者阈值25Hz不能当FR5参数；未验证底层伺服。|**研究优先、源码复用暂缓**；最小补证：许可证/完整fully-test原始数组、单位/标定/注册、episode和材料/曲率留出，禁止运行robot/控制入口。|
|3 = **FEATS**|Helmut, Dziarski, Funk, Belousov, Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*；IROS **2025**；`10.1109/IROS60139.2025.11246486`|继承R46审计H，本轮未核全文；MIT仓库历史`7d1ae43…`、HF数据约3.98GB/短revision`7fd3027`，未下载，第三方依赖许可未逐个核。|GelSight Mini内视光学触觉，FEA生成24×32×3标签；作者总力F/T核验不是逐网格独立物理真值；作者25Hz不等于FR5外环；不要求FR5关节力矩，但需要不同触觉结构。|**条件保留作FEA先验/辅助，不可升为独立压力主baseline**；补独立分段力台/材料曲率盲检和真实数据许可。|
|4 = **ImplicitRDP**|Chen, Xue, Wang, Zhou, Lv, Jin, Tang, Wen, Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*；arXiv **2025**、v2 **2026**；`10.48550/arXiv.2512.10946`|继承R45固定commit`112d6857…`审计H；MIT与部分RAIL-M、mini约182MB、模型约27.7GB，均未下载；官方eval有Flexiv机器人调用及pickle/dill反序列化风险。|VRR虚拟目标为wrench确定性变换，不增加独立空间压力秩；GPU/ROS2表面匹配不等于接口兼容，Flexiv高频/示教刚度与FR5 `FT_Control`不等价；无独立三箱真值。|**暂缓**；仅可在隔离环境与合法可信模型格式下做纯张量离线forward，先补权重/许可/接口。|
|5 = **RDP**|Xue, Ren, Chen, Zhang, Yuan, Gu, Xu, Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*；RSS **2025**；`10.15607/RSS.2025.XXI.052`|继承R46题录H，本轮未读全文；代码commit/许可/数据/权重版本均U。|依赖高频触觉及慢快控制结构；D435+KWR75D+FR5外环实时性、底层力矩/高频servo接口未核验；无独立非人体空间压力真值。|**暂缓**；先提供合法纯离线模型、传感器速率/接口和空间真值对照。|

**2025–2026相邻工作核查与反证**：Zuo/Yang/Chua 2026 `10.48550/arXiv.2605.00307` 已用RGB-D+关键点+SOFA逆FEM+迭代接触定位，作者ATI Nano17仅验证总抓取力；不能把“接触位置视觉估计”作为本论文泛化首创。Min等2026 `10.48550/arXiv.2605.28412` 已用接触事件+电机残差/TCN分离静动摩擦；FR5没有经核验的气动皮肤/电机电流接口。Zhong等2025/2026 `10.1109/HAPTICS66823.2026.11495507` 证明曲率校准不可忽略。以上三者本轮**不是**主baseline直接可复现替代，Zuo逆FEM的SOFA插件AGPL-v3+Clause11需独立许可审计。

## 厂家/简单可靠工程强Baseline（非论文主Baseline）

M1人工有序3D点、M2实体背部画线识别及起终方向人工确认、M3解剖标志候选专家审核/可编辑三维点图/表面图搜索或审核模板。三入口统一输出`position_m,tangent_unit,normal_unit,confidence/uncertainty,flange_SE3,calibrated_TCP_SE3,actual_flexible_contact_line_or_UNKNOWN,tool_width_m,continuous_sweep_eligibility,phase=WORK|UNLOAD|OFFSURFACE_RETURN,reviewed,timestamp_ns`。三入口和工作/卸载/离面返程功能不以博士主创新占额。工程强Baseline是先离面无接触轨迹，再经独立风险审查和厂家确认后才能评估厂家`FT_Control`恒力+简单法向跟踪+平滑卸载+离面返程；**目前全部HOLD**。R48新核查4.0.0官方手册：`FT_SetRCS(ref=0工具/1基座)`、`FT_GetForceTorqueRCS/Origin`、payload/CoG、`FT_Control`，但厂商配置表只列坤维KWR75B device=0，**不能由此推定KWR75D的`[1,17,3,0]`配置**。SDK v2.3.0、firmware robot v4.0.0、真实FT时间戳/失帧/单位/外环带宽未核验。机器人运动接口一律禁用。

## 唯一条件性创新 I-A（本轮实质修订）

**有限宽柔性刮板的真实接触线/局部载荷部分可辨识区间，融合有界注册平移与独立工具坐标wrench的一致性证书，并以独立物理空间真值和不可信拒绝为主贡献。** R48把R47“只给注册区间”推进为“**独立力矩把注册区间作精确可行投影**”；这只在单轴法向、每taxel非负且已知力、共享平移、同原点同时间、纯力偶与力矩误差有真实上界时成立。腕部F/T仍不能唯一恢复未知多点压力/摩擦/纯力偶。新颖性不是“视觉+力融合”或“中心压力估计”本身，而是**可证伪可追溯的不确定性联合Gate**。入门：G0法兰/标定TCP/柔性真实接触线标定，G1 F/T重力/偏置/动态+RCS与SDK时序，G2独立纯力偶界，G3真实非人体空间压力真值，G4按完整episode/材料/曲率/遮挡盲检；目前G0–G4均HOLD。退出：任一独立真值不覆盖、同信息强Baseline不劣或拒绝率不可接受、或无法取得合法数据/可认证误差界。**只保留条件性首选，不强行宣称论文创新成立。**

## 唯一备选 I-B

WORK→UNLOAD→OFFSURFACE_RETURN换向的延迟鲁棒柔顺控制。需厂家`FT_Control`真实时序、延迟与保护、独立离面几何真值；2026 Min的摩擦/外力分离已占据泛化新颖性，故备选必须聚焦FR5可执行的换向延迟与无接触返程安全。**未取得硬件，暂缓**。

## 跨轮决策与验证边界

R46区分真实压力/FEA标签；R47修订原生grams与有限footprint空间注册；**R48新增wrench约束下的公共平移可行区间、同原点与纯力偶界前提，且正式发现FR5手册KWR75B与用户KWR75D型号核验缺口**。本轮3篇全文A、13个去重来源、23项新测试及713项历史测试；无任何FR5、D435、KWR75D、Sensel、SOFA实体/物理仿真、人体或机器人运动。不得访问/修改旧仓库或擅自新建仓库。



## 附录C｜可撤销决策与失败条件

# AUTO-R48 可撤销决策日志

|ID|本轮决定/证据|撤销条件、下一轮最小补证|
|---|---|---|
|D48-01|实际访问R47报告、讲义、候选矩阵、决策、源码与ZIP，30成员CRC、29 SHA全通过；R39–R47重新执行713项。|任何复核文件SHA/JUnit不一致则撤销。|
|D48-02|在非负taxel精确载荷+公共平移+一维法向+独立工具坐标wrench假设下，推导并实现**精确可行公共平移区间**。|独立LP/隐藏接触点真值发现反例即撤销。|
|D48-03|以可行平移区间重用R47有限footprint事件包络，得到**保守边际三箱区间**，不声称jointly tight或全局最优。|真实物理真值漏覆盖或前提错误则退出。|
|D48-04|同信息R47+全局wrench一致性强基线与R48比较，合成512例均覆盖，但R48区间较窄；名义零偏仅部分覆盖。|同条件物理盲检不能改善或拒绝率过高则撤销收益假设。|
|D48-05|对q原点、符号、同步、标定、纯力偶界及独立来源设严格拒绝；不将腕传感器原始τ直接等同接触线q。|提供完整法兰→传感器→工具真实接触线SE(3)与校准误差预算后逐项解锁。|
|D48-06|独立SciPy HiGHS LP投影核验300例；隐藏接触位置2000例覆盖，负例拒绝测试通过。|出现独立Oracle不一致即撤销数学Gate。|
|D48-07|Zuo2026全文显示RGB-D+逆FEM+迭代接触定位已存在，不能把“视觉估接触位置”作为本论文唯一新颖性。|严格不同任务/独立实验可支持更窄创新命题；不得泛称首创。|
|D48-08|Min2026全文显示接触事件辅助摩擦/外力分离已有，备选创新I-B须聚焦FR5可执行的换向延迟和卸载，不做泛化摩擦网络。|若有FR5时序/独立扰动真值和新颖性证据再审。|
|D48-09|官方FAIRINO 4.0.0手册列FT_SetRCS、Origin/RCS读取，坤维device=0注明KWR75B，**不证明KWR75D和历史配置可用**；厂家强基线继续HOLD。|用户设备型号/SDK版本/固件实读、反馈时序和厂家书面支持。|
|D48-10|Zuo使用的SOFA inverse插件官方仓库标AGPL-v3+Clause11专利，禁止不经许可审计把其代码并入MIT工程；本轮未复制作者代码。|明确许可证、版本、依赖、复现数据后再决定。|
|D48-11|五篇主论文审计顺序VPEC→ViPER→FEATS→ImplicitRDP→RDP保持，**最终主论文待定**；Zuo2026列相邻竞争反证而不替换主候选。|获取独立空间压力原始episode/合法许可证/可执行纯离线Baseline后动态调整。|
|D48-12|唯一条件性创新I-A修订为“**空间注册与wrench一致性联合可行证书**+局部载荷保守区间”，备选I-B不变；M1/M2/M3、WORK/UNLOAD/OFFSURFACE_RETURN与全运动HOLD。|真实独立压力真值、标定界和同信息盲检失败则退出I-A；安全/伦理另审。|



## 附录D｜运行与数学复现声明

从ZIP解压后在离线Python环境执行 `python -m pytest -q test_wrench_registration_gate.py test_independent_lp_oracle.py` 和 `python ablation_r48.py`。历史回归脚本 `historical_r47/run_historical_r47.py` 只运行R39–R46归档测试，随后运行R47原25项；所有原始JUnit与SHA列在包内。`wrench_registration_gate.py`不导入机器人SDK/ROS2，不执行外部程序；`spatial_registration_gate.py`为R47原字节。最小数学证书仅在模型条件成立时成立。**未进行Gazebo/MuJoCo物理仿真、FR5实体、人体或运动测试。**