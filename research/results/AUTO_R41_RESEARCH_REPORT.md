# AUTO-R41｜FAIRINO FR5刮痧机器人博士级完整研究报告

**2026-10-21｜内容验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY。** 主题：有限宽刮板的边缘载荷是否能从腕六维wrench唯一反演？本轮没有任何FR5、D435、KWR75D、人体或非人体实体数据；所有代码与消融均为合成数学研究，机器人运动许可永远HOLD。主论文赢家待定。

**实质工作量**：实读并哈希核验R40完整ZIP；本轮新访问去重外部来源11项，其中2篇公开PDF全文核读方法/实验/限制；完成三箱载荷零空间与纯力偶混淆推导、四变量LP、73项新测试、430+60项历史/上轮回归、240组同条件合成消融、40组错误证书对抗反例。R40继承不当成新研究发现。


## 01｜上一轮真实归档核验与R40→R41差分

本轮直接读取R40 ZIP，逐个校验46项成员中的45项内部SHA256，并核对外部ZIP摘要与CRC；还实际打开R40研究报告、零基础讲义、来源登记、决策、动态核LP源码和测试。旧结论是“瞬时力峰值受动态核与错误证书影响”，R41没有推翻它，而是将其固定为前置条件，研究另一种独立风险：**即使瞬时六维wrench已可信，有限宽刮板的空间载荷也可能不可辨识**。本轮新增的不是又一套时间滤波算法，而是空间零空间证明、纯力偶边界、三箱LP与独立真值设计。先核验旧件再讨论新结果，避免把先前合成数据或本次触发当作新实验。

**FORMULA-R41-01（本章数学/决策锚点）**

$$
H_{40}=SHA256(archive_{40});\quad N_{verified}=45
$$

**TABLE-R41-01（本章证据/比较表）**

|审计对象|实际结果|
|---|---|
|R40 ZIP|46成员，45内部SHA与CRC通过|
|R40正文/讲义|已实读|
|R40源码|已实读动态核与测试|
|R41新内容|空间LP/73项新增测试|


## 02｜三项决策问题、可证伪假设与退出条件

决策Q1：总法向力与一阶力矩是否足以唯一恢复左右边载荷？本轮给出否定反例。Q2：独立纯力偶界C是否能在保持覆盖的前提下缩窄可辨识区间？本轮仅在人工合成证书下支持，物理证书缺失。Q3：2026真实刮削论文是否应替换主学术Baseline？暂不替换，因500Hz底层关节扭矩控制与FR5接口未经证实。H1可证伪：在独立分段力台、未参与调参的仿体上，预注册区间应覆盖至少约定比例的左右载荷；本轮不预设通过阈值或把人工覆盖率当作安全阈值。退出：C不可认证、盲检漏真值、强Baseline不劣于创新。

**FORMULA-R41-02（本章数学/决策锚点）**

$$
H_1:\ P(r\in I_{cert})\ge 1-\epsilon\quad (\text{独立仿体盲检})
$$

**TABLE-R41-02（本章证据/比较表）**

|决策问题|本轮证据与状态|
|---|---|
|Q1唯一性|严格反例：否|
|Q2证书收益|人工条件成立，物理HOLD|
|Q3论文替换|证据不足，待定|


## 03｜文献检索去重与证据层级

本轮对接触wrench锥、软指纯扭矩、分布式压力、真实刮削自适应控制及接触定位检索，核对11项新访问的去重外部来源。Xu2020和Cetin2026实际打开公开PDF并核读方法、实验和限制，标A。Caron2015作者公式页虽包含完整16行约束，但其HAL PDF遭访问拒绝，仅B；6DLS的17MB arXiv PDF超出浏览上限，只核正式摘要。R40已核的Li工具力论文只列H，不冒充本轮再读。论文全文开放不等于源码、权重和数据许可开放；本轮没有复制任何第三方代码。完整逐项元数据在SOURCE_REGISTER。

**FORMULA-R41-03（本章数学/决策锚点）**

$$
A\neq B\neq H;\quad N_{A,full}=2
$$

**TABLE-R41-03（本章证据/比较表）**

|层级|实际含义|
|---|---|
|A|本轮公开PDF全文方法/实验已核|
|B|作者正文/正式摘要/README片段|
|H|R40继承，未重新核原文|
|U|尚未取得|


## 04｜Xu2020六维面积接触全文：方法与重要负结果

S41-01全文第III节假设准静态、已知刚性物体与线弹性夹爪、常摩擦系数；第IV节通过接触三角网格采样摩擦wrench，拟合六维椭球并线性化为抓取约束。第V节1500次ABB YuMi抓取的分类实验中，6DFC召回0.67、点接触0.50；**但另875次规划抓取实验中点模型成功339次、6DFC成功272次**，作者指出大面积接触的高估与假阳性。不能只摘“召回提高17%”作为普适优势。该论文优化抓取稳定性，不反演刮板压力线，几何/材料/机器人均不同。对本课题的直接影响是把“六维面积接触是创新”淘汰为已有技术。

**FORMULA-R41-04（本章数学/决策锚点）**

$$
w\in\mathbb{R}^{6},\quad (w-f_c f_N)^TA(w-f_cf_N)\le f_c^2
$$

**TABLE-R41-04（本章证据/比较表）**

|Xu2020实验|论文原文结果|
|---|---|
|1500抓取分类|6DFC recall .67 vs point .50|
|875规划抓取|point成功339 vs 6DFC成功272|
|设备|ABB YuMi+软夹爪|
|迁移|不能推断FR5刮痧收益|


## 05｜Cetin2026真实刮削全文：强挑战者与接口鸿沟

S41-02全文第III节采用视觉定位、PPO高层wrench策略和低层笛卡尔阻抗；第IV节明确Franka Research 3、D405、玻璃瓶刮削、MuJoCo 1kHz步长、500Hz关节力矩环与10Hz策略。真实实验五种材料各5次，固定4N策略为同论文优化后的强Baseline，成功率由称重材料去除衡量；作者报告平均10.9%提升。该数值不是FR5力误差或人体安全改善；4N不能转为人体设定。由于FR5厂家外环时序、力矩接口、授权模型均未核，本轮仅将其列为**主论文挑战者**而非替换五候选。它强烈反证“刮削自适应wrench首次提出”。

**FORMULA-R41-05（本章数学/决策锚点）**

$$
a_t=[F_x^{cmd},\tau_y^{cmd},z^{des}],\quad f_{high}=10Hz,\ f_{low}=500Hz
$$

**TABLE-R41-05（本章证据/比较表）**

|Cetin2026维度|全文实际记载|
|---|---|
|策略/底层|10Hz/500Hz关节力矩|
|视觉|D405 Eye-in-Hand|
|实体材料|5种、每种5次|
|指标|称重去除率；非力安全|
|FR5适配|底层力矩接口未知|


## 06｜Caron2015、6DLS与接触模型边界

S41-03作者网页公开完整矩形接触wrench约束，包含切向摩擦、CoP在支撑域内与绕法向扭转力矩界。其对象是刚性矩形足底稳定性，不能直接给出柔性刮板左右压力分布。S41-04 6DLS正式摘要显示六维非平面接触与KUKA实体抓取也已有先例。S41-05对触觉阵列和F/T定位的老论文摘要提示F/T受标定、瞬态和低力影响。三个来源共同反证“只需腕F/T就可唯一反演所有局部接触”，但不能据摘要臆造其完整算法。Caron仓库GPL-3.0，依赖旧OpenRAVE且HRP4模型授权不明，未移植代码。

**FORMULA-R41-06（本章数学/决策锚点）**

$$
|f_t|\le \mu f_n,\quad x_{CoP}\in[-a,a],\quad |\tau_{yaw}|\le B(w)
$$

**TABLE-R41-06（本章证据/比较表）**

|来源|关键边界|
|---|---|
|Caron2015|矩形刚性支撑的CoP/扭转约束|
|6DLS2021|非平面六维面接触已研究|
|Son1996|F/T接触定位受校准与瞬态影响|
|许可证|Caron GPL-3.0；不引入|


## 07｜法兰、TCP与真实边缘的坐标链

将传感器原点T的wrench变换到真实接触边缘坐标C，必须先用已标定的刚体旋转与力臂平移，并考虑柔性刮板真实接触中心与名义TCP不同。对于工具边缘x轴横向、z轴为局部法向的三箱模型，正常压力在x位置产生y轴力矩，约定q=-tau_C,y。这个符号约定不可与原法兰力矩混用；任何未知力臂、变形、摩擦切向力造成的附加项应并入有界扰动，若无法上界则拒绝。相机外参、手眼、工具边缘标定不完成，不能从D435点云直接得出接触中心。

**FORMULA-R41-07（本章数学/决策锚点）**

$$
\tau_C=R_{CT}(\tau_T-r_{TC}\times F_T),\quad q=-\tau_{C,y}
$$

**TABLE-R41-07（本章证据/比较表）**

|坐标对象|含义|
|---|---|
|FLANGE|机器人法兰刚体位姿|
|TCP|标定工具参考点|
|EDGE_CONTACT_FRAME|真实接触线局部坐标|
|q|在接触坐标中负tau_y|


## 08｜有限宽三箱法向载荷模型与零空间

以刮板宽w=2a，横向离散x=(-a,0,a)，三箱法向合力p_L,p_M,p_R非负。可观测的两个量是总力F与关于接触中心的一阶矩q，另有未知纯力偶c。即使c=0、F和q完全准确，测量矩阵[[1,1,1],[-a,0,a]]秩仅2，未知量3个，零空间向量(1,-2,1)。例如p=(1,4,5)与p=(3,0,7)都给F=10N、q=.08N·m，却给右边5N和7N。这里的“压力箱”只是离散法向载荷，不是连续皮肤压力分布。

**FORMULA-R41-08（本章数学/决策锚点）**

$$
F=p_L+p_M+p_R,\quad q=-a p_L+a p_R+c
$$

**TABLE-R41-08（本章证据/比较表）**

|人工剖面|F与q、右侧比例|
|---|---|
|p=(1,4,5)|10N, .08N·m, 0.50|
|p=(3,0,7)|10N, .08N·m, 0.70|
|差向量|(2,-4,2)位于零空间|


## 09｜未知纯力偶、摩擦与工具弯曲的耦合

柔性接触中的未知纯接触力偶、摩擦切向力经力臂产生的附加转矩、工具弯曲/重力残差等，均可能污染q。把它们强行设为零会造成虚假的偏载精度。若经过独立动态与几何计量能够给出C上界，则可作保守区间；如果没有独立证书，软件拒绝输出“已认证”偏载。例：同一q=.08N·m可由真实一阶矩.08且c=0产生，也可由真实一阶矩.04且c=.04产生。腕部六维wrench并不意味着知道内部纯力偶。

**FORMULA-R41-09（本章数学/决策锚点）**

$$
q_{obs}=\sum_i x_i p_i+c+\eta_M,\quad |c|\le C
$$

**TABLE-R41-09（本章证据/比较表）**

|扰动项|需要的独立证据|
|---|---|
|纯接触力偶|非人体力台/姿态扫描|
|切向摩擦力矩|独立剪切/几何测量|
|工具弯曲|工具刚度与边缘变形标定|
|动态残差|R40动态核与时间Gate|


## 10｜精确法向力、零偶矩时的解析部分界

若F>0、c=0、q精确且d=q/(aF)非负，三箱非负约束给p_R-p_L=q/a，故p_R至少为q/a；而p_M≥0推出p_L+p_R≤F，联立得p_R≤(F+q/a)/2。于是右边比例在[d,(1+d)/2]内；q为负时需采用相应分段表达而非硬套。以F=10N、a=.02m、q=.08N·m为例，d=.4，右侧比例**只能确定在[.4,.7]**，绝非“右侧60%”。即使不存在纯力偶，也有宽区间。

**FORMULA-R41-10（本章数学/决策锚点）**

$$
\rho_R\in[\max(0,d),\ (1+d)/2],\quad d=q/(aF),\quad 0\le d\le1
$$

**TABLE-R41-10（本章证据/比较表）**

|解析人工条件|右边比例界|
|---|---|
|F=10,a=.02,q=.08,C=0|[.4,.7]|
|额外C=.04|[.2,.8]|
|无C证书|不作为可信纠偏证据|


## 11｜有力误差时的分式规划变换

当真实总力F*仅在[F_obs-eF,F_obs+eF]时，直接优化p_R/Σp是分式规划。使用Charnes–Cooper变量s_i=p_i/F*、u=1/F*，可将比例目标变为线性s_R；非负载荷和总和约束变为Σs=1；总力误差化为对u的两个线性不等式。力矩区间约束也可乘u后线性化。前提是F_obs-eF>0，否则分母可能接近零，程序拒绝。这个变换是经典数学技术，不宣称原创。

**FORMULA-R41-11（本章数学/决策锚点）**

$$
s_i=p_i/F^*,\ u=1/F^*,\ \sum s_i=1,\ (F-e_F)u\le1\le(F+e_F)u
$$

**TABLE-R41-11（本章证据/比较表）**

|变量|单位与用途|
|---|---|
|p_i|N，真实分箱载荷|
|s_i|无量纲，归一化载荷比例|
|u|1/N，真实总力倒数|
|eF|N，独立力误差界|


## 12｜四变量LP与独立证书约束

R41实现的四变量为(sL,sM,sR,u)，三箱非负且Σs=1；x=(-a,0,a)。在总力与力矩误差盒内，用SciPy HiGHS分别最小化/最大化右侧比例、中间比例与CoP，得到**条件性外包区间**。由于约束是线性的，若真实压力分布符合模型、误差与纯力偶上界证书确实成立，真实三箱比例一定在可行集合中。算法不估计连续接触压力、不分离摩擦系数、不发ROS2指令。优化无解时不输出“修正结果”，而抛出EvidenceRejected并HOLD。

**FORMULA-R41-12（本章数学/决策锚点）**

$$
(q-e_M-C)u\le\sum_i x_i s_i\le(q+e_M+C)u
$$

**TABLE-R41-12（本章证据/比较表）**

|LP量|数学定义|
|---|---|
|决策变量|sL,sM,sR,u|
|目标|min/max sR,sM,Σx s|
|约束|非负、总力盒、力矩盒、证书C|
|状态|HOLD_NO_MOTION|


## 13｜数据契约、拒绝行为与安全隔离

输入类型要求宽度m、力N、力矩N·m、时间int64 ns、接触坐标EDGE_CONTACT_FRAME、工具/FT标定版本、时钟域、独立证书ID与原始数据SHA256。动态核资格、重力补偿、坐标变换资格及证书独立性缺一拒绝；力接近零、误差负值、矩不一致也拒绝。三个路径模式WORK/UNLOAD/OFFSURFACE_RETURN可作为只读标签，但本轮代码无硬件适配器或运动输出。样例把资格布尔置True仅是**合成证据测试**，不能用于FR5准入。

**FORMULA-R41-13（本章数学/决策锚点）**

$$
Gate=G_{frame}\land G_{clock}\land G_{FT}\land G_{kernel}\land G_C
$$

**TABLE-R41-13（本章证据/比较表）**

|失败输入|软件行为|
|---|---|
|单位mN/N*mm混用|拒绝|
|未证C或证书ID空|拒绝|
|负载荷不可能的矩|拒绝|
|时间浮点或int64溢出|拒绝|
|全部合成条件满足|仅HOLD_NO_MOTION|


## 14｜新测试、历史回归与真实执行范围

本轮新增73项pytest覆盖解析区间、同wrench不同分布、同观测不同纯力偶、非零误差、单位/坐标/时间证书拒绝、不可行wrench、三模式HOLD、单调性、符号反射、尺度不变性及20组独立解析人工真值。R40新增60项实际重跑通过。R39–R31九套历史共385项实际逐套重跑通过，外层总脚本在R30附近被超时终止，随后单独重跑R30的45项通过；合计**563/563**，但超时本身未记成功。测试文件和JUnit逐套归档。

**FORMULA-R41-14（本章数学/决策锚点）**

$$
N_{R41}=73,\quad N_{R40}=60,\quad N_{R30:R39}=430
$$

**TABLE-R41-14（本章证据/比较表）**

|测试套件|本轮实际通过|
|---|---|
|R30–R39|430|
|R40|60|
|R41新增|73|
|合计|563|


## 15｜同条件消融：零偶矩、宽证书与窄证书

固定种子410041生成240组三箱非负压力、真实F在4–16N、真实|c|≤.01N·m，并加入相同的力与力矩误差；解析pR/F为独立数学真值。三法共享**同一组**观测：零偶矩弱Baseline覆盖234/240，平均区间宽.3809；宽C=.04的强Baseline覆盖240/240、宽.5694；人工独立窄C=.012的条件算法覆盖240/240、宽.4447，较强Baseline缩窄约21.89%。这不是物理证书、也不是FR5性能；窄证书的获取成本未计入。优先看真值覆盖，再看区间宽度，不可只报收窄。

**FORMULA-R41-15（本章数学/决策锚点）**

$$
W_{cert}=\rho_{R,max}-\rho_{R,min}
$$

**TABLE-R41-15（本章证据/比较表）**

|同条件方法|覆盖/240；平均比例区间宽|
|---|---|
|C=0弱Baseline|234；0.380904|
|C=.04强Baseline|240；0.569394|
|C=.012条件算法|240；0.444726|
|相对强Baseline|宽度降低21.89%，人工条件|


## 16｜错误证书对抗反例：可行但不可信

在240组随机人工样本上，错误C=.0001仍全部LP可行，其中6组漏真值；为了独立验证风险，另预设40组对抗样例：左箱0，右箱比例从.05逐步变化，真实c=.04N·m，而伪证书仅.0001N·m，测量误差.0005N·m。结果**40/40内部可行、40/40真实右边比例未覆盖**。这不是算法“检出错误”，而是算法被假证书欺骗的明确失败。只有独立物理计量、不同负载/方向/姿态与盲检才能给C证书可信度。

**FORMULA-R41-16（本章数学/决策锚点）**

$$
C_{claimed}=10^{-4}<C_{true}=0.04\quad(N\cdot m)
$$

**TABLE-R41-16（本章证据/比较表）**

|错误证书案例|实际合成结果|
|---|---|
|随机240|内部可行240，漏真值6|
|对抗40|内部可行40，漏真值40|
|结论|内部LP可行不代表物理证书真|


## 17｜五篇主论文候选竞争与新挑战者

保持VPEC、ViPER、FEATS、ImplicitRDP、RDP五篇候选，逐项核作者、DOI、来源层级、代码/数据许可、参数、硬件模态与FR5高频控制依赖，详见BASELINE_AND_INNOVATION_REGISTER。R41未新核五篇原始LICENSE/权重，故不声称已复现。新增Cetin2026作为挑战者：其真实刮削任务相关性强，但需要500Hz关节力矩阻抗环、D405和专用玻璃瓶工况。它对I-B的“自适应刮削wrench创新”构成反证，却尚不能替换五篇主候选。主学术赢家仍待定。

**FORMULA-R41-17（本章数学/决策锚点）**

$$
B^*=\arg\max_{B\in\mathcal B}\{reproducibility,FR5\ fit,independent\ truth\}
$$

**TABLE-R41-17（本章证据/比较表）**

|候选|当前复现判断|
|---|---|
|VPEC|有条件优先审计|
|ViPER|有条件保留|
|FEATS|暂缓FR5移植|
|ImplicitRDP|有条件保留，接口HOLD|
|RDP|暂缓|
|Cetin2026|挑战者，暂缓接口复现|


## 18｜唯一核心创新I-A、备选I-B与新颖性收敛

唯一条件性首选I-A定义为：有限宽柔性刮板左右边载荷的**部分可辨识区间**，利用独立视觉接触支持、F/T、动态核与外部纯力偶证书，在盲检中保证覆盖并比宽证书强Baseline更紧；不宣称通用工具尖端力估计、六维接触锥或自适应刮削wrench是首创。唯一备选I-B为延迟鲁棒WORK→UNLOAD→OFFSURFACE_RETURN；Cetin2026使I-B必须强调换向/安全退出与FR5可执行性，而非一般自适应wrench。若独立C无法证明或物理真值不能收紧区间，I-A退出。

**FORMULA-R41-18（本章数学/决策锚点）**

$$
I_A=\text{partial ID}+\text{independent truth}+\text{fail-closed}
$$

**TABLE-R41-18（本章证据/比较表）**

|创新路线|本轮判断|
|---|---|
|I-A|条件性首选；新增C证书硬Gate|
|I-B|唯一备选；受Cetin2026新颖性约束|
|通用6D面积接触|已有Xu2020/Caron2015，非首创|
|一般工具力估计|已有Li2025，非首创|


## 19｜M1/M2/M3、有限宽扫掠与三模式继承

完整系统合同未缩减：M1人工有序选点及指定路线；M2背部实体画线识别、人工确认起终方向、重建有序3D曲线；M3解剖标志与穴位候选经专家审核，生成可编辑关键点图并允许两点路径或刮痧方式模板。三入口统一输出位置、切向、法向、置信度/不确定性、工具姿态、真实有限宽连续扫掠资格与WORK/UNLOAD/OFFSURFACE_RETURN。R41没有实现这三入口的新代码，只继承规格；本轮LP也不决定工具姿态或自动修正人体接触。任何接触前必须用户审核。

**FORMULA-R41-19（本章数学/决策锚点）**

$$
\gamma(s)=[p(s),t(s),n(s),\Sigma(s),R_{tool}(s),mode(s)]
$$

**TABLE-R41-19（本章证据/比较表）**

|模块|保留的职责|
|---|---|
|M1|人工有序3D点与路线|
|M2|实体画线识别+方向确认|
|M3|专家审核关键点图/模板|
|统一路径|3D位置/切法/不确定性/姿态/扫掠/模式|


## 20｜非人体真值、厂家强Baseline与物理Gate

物理路线应先手眼/相机/TCP/边缘标定，核D435曝光、KWR75D六维F/T偏置/重力/动态核、真实时间戳和FR5厂家FT_Control SDK/固件/外环反馈；再在非人体仿体上用独立分段力台测左右边载荷与真实接触中心。先离面无接触、再低能量单向恒力、平滑卸载、离面返程；贴面返程后审。强工程Baseline是厂家授权恒力+人工审核+离面返程，但**尚未实测**。独立盲检需冻结算法与证书，比较右边比例覆盖、区间宽度、拒绝率、模式切换失败；真人必须风险与伦理独立批准。

**FORMULA-R41-20（本章数学/决策锚点）**

$$
G_{physical}=G_{calib}\land G_{wrench}\land G_C\land G_{truth}\land G_{FR5}
$$

**TABLE-R41-20（本章证据/比较表）**

|Gate|当前状态|
|---|---|
|标定/时间/动态核|实体HOLD|
|纯力偶C独立证书|HOLD|
|分段载荷独立真值|HOLD|
|FR5真实FT_Control时序|HOLD|
|人体风险伦理|HOLD|


## 21｜十轮审查继承、局限、下轮自适应

R40十轮里程碑审查独立存在，R41不是十轮审查，不擅自生成六份开题文档V2/V3。当前可审计成果是两篇全文核读、11项新访问去重来源、73项新增测试、430+60项实际回归、240组公平合成消融及40组错误证书反例。局限包括三箱模型、准静态、法向接触、人工C证书、无真实FR5/相机/F/T/分段力台、未取得主论文代码许可与模型。下一轮优先寻找可合法获取的独立分段压力传感器/数据与纯力偶计量方法；若无证据则记录阻塞而非再制造精度幻觉。

**FORMULA-R41-21（本章数学/决策锚点）**

$$
Decision_{41}=\text{HOLD}_{physical}\land\text{KEEP}_{I_A,conditional}
$$

**TABLE-R41-21（本章证据/比较表）**

|下轮优先项|可验收最小证据|
|---|---|
|P1|分段力台型号、许可证、校准证书/原始波形|
|P2|纯力偶C的独立姿态/摩擦扫频证据|
|P3|Cetin2026代码/数据/底层接口合法性|
|P4|VPEC与ViPER标签真值审计|


## 附录A｜完整逐项来源核读审计

# AUTO-R41｜去重来源审计｜2026-10-21

等级A=本轮实际打开公开论文**全文PDF**并核读方法、实验与局限；B=本轮访问作者/出版机构的正文页、摘要或README，**未**核读PDF全文；H=只从本地AUTO-R40实读继承，本轮未重开原文；U=未获取。没有下载/执行作者代码、数据或模型。作者的机器人实验不能归为FR5实验。

|ID|独立来源及元数据|本轮实际证据层级|方法、实验与FR5迁移/许可审计|
|---|---|---|---|
|S41-01|Xu, Danielczuk, Steinbach, Goldberg, **2020**, *6DFC: Efficiently Planning Soft Non-Planar Area Contact Grasps using 6D Friction Cones*, DOI **10.1109/ICRA40945.2020.9197293**；https://goldberg.berkeley.edu/pubs/2020-ICRA-md-6D-Friction-Cones.pdf|**A：7页公开全文**，第III节假设、第IV节六维椭球与采样、第V节两类实验、第VI节限制|已是6D软面积接触与扳手建模先例，不能宣称此方向空白。1500次ABB YuMi抓取上6DFC recall 0.67、point 0.50，但另875次规划抓取中point成功339、6DFC成功272；二者任务不同。数据/代码许可未核，未复现。其假设包括刚性物体、准静态、已知几何与常摩擦，不可照搬人体刮板。|
|S41-02|Cetin, Pouli, Pizzuto, **2026-03-11** arXiv v1, *Learning Adaptive Force Control for Contact-Rich Sample Scraping with Heterogeneous Materials*, DOI **10.48550/arXiv.2603.10979**；https://arxiv.org/pdf/2603.10979|**A：8页公开全文**，第III节控制/感知、第IV节MuJoCo、真实材料与比较，表I–III；非正式录用证据|Franka Research 3；策略10Hz、底层笛卡尔阻抗500Hz并输出关节力矩；D405而非D435；5种材料、每种5次真实刮削；固定力4N是该论文条件，不是FR5推荐值；声称平均提升10.9%，评价为称重去除率而非人体力安全。代码/数据许可证和版本未核，不能复制/复现。|
|S41-03|Caron, Pham, Nakamura, **2015**, *Stability of Surface Contacts for Humanoid Robots: Closed-Form Formulae of the Contact Wrench Cone for Rectangular Support Areas*, DOI **10.1109/ICRA.2015.7139910**；https://scaron.info/publications/icra-2015.html|**B：作者本人公开正文页含完整16行约束矩阵、解释及实验介绍；HAL PDF点击返回访问拒绝，故非全文PDF核读**|接触wrench锥分解为摩擦、CoP支持区域和扭转界；适用于理想刚性矩形足底，非柔性刮板分段压力逆问题；源码见S41-10。|
|S41-04|Xu, Aykut, Ma, Steinbach, **2021**, *6DLS: Modeling Nonplanar Frictional Surface Contacts for Grasping Using 6-D Limit Surfaces*, DOI **10.1109/TRO.2021.3070102**；https://ieeexplore.ieee.org/document/9432399/|**B：正式出版摘要/元数据；arXiv PDF 17MB浏览工具超限，未声称核读全文**|2932 FEA网格接触和1035 KUKA实体抓取；面接触六维力限制已有研究；材料/几何假设不等于刮板。|
|S41-05|Son, Cutkosky, Howe, **1996**, *Comparison of contact sensor localization abilities during manipulation*, DOI **10.1016/0921-8890(95)00068-2**；https://doi.org/10.1016/0921-8890(95)00068-2|**B：出版摘要**|实际对比触觉阵列和F/T定位，报告F/T受标定、瞬态和低抓持力影响；不是KWR75D测试；没有读取全文。|
|S41-06|Talvas, Marchal, Duriez, Otaduy, **2015**, *Aggregate Constraints for Virtual Manipulation with Soft Fingers*, DOI **10.1109/TVCG.2015.2391863**；https://pubmed.ncbi.nlm.nih.gov/26357095/|**B：PubMed摘要**|非均匀压力与扭转摩擦已有先例；交互虚拟仿真非物理分段真值。|
|S41-07|Jia et al., **2019**, *Grasping Torque Optimization for a Dexterous Robotic Hand Using the Linearization of Constraints*, DOI **10.1155/2019/5235109**；https://onlinelibrary.wiley.com/doi/10.1155/2019/5235109|**B：出版社方法正文片段（接触约束小节）**|明示无摩擦点、摩擦点、软指接触差异；不能把软接触纯扭矩置零。|
|S41-08|Likar, Žlajpah, **2014**, *External Joint Torque-Based Estimation of Contact Information*, DOI **10.5772/58834**；https://doi.org/10.5772/58834|**B：出版摘要**|需要关节外力矩与圆柱连杆几何，FR5底层力矩接口未确认，不是腕F/T同问题。|
|S41-09|ContactEngineering, *ContactMechanics*，官方README；https://github.com/ContactEngineering/ContactMechanics|**B：公开README与MIT许可证标识，未下载代码，固定commit/版本未核**|弹性半空间、小变形、非黏附等假设；可作为未来几何-压力仿真对照，不是独立真值；本轮原创LP未引用其源码。|
|S41-10|Stéphane Caron, *analytical-wrench-cone* 官方仓库；https://github.com/stephane-caron/analytical-wrench-cone|**B：README/许可证/依赖列表；未下载运行**|**GPL-3.0**，仓库2024-03-19归档；CVXOPT1.1.7、OpenRAVE0.9、NumPy1.8.2，HRP4模型版权不确定；不移植代码至本项目。|
|S41-11|He et al., **2026**, *Vision-Guided Robotic Scraping for Irregular Cabin Sections with Adaptive Trajectory Generation*, DOI **10.3390/aerospace13050466**；https://www.mdpi.com/2226-4310/13/5/466|**H：R40来源登记，R41未重新取得全文**|工业刮削路径已有先例；不等于人体柔顺接触。|
|S41-12|Li & Thuruthel, **2025 online/2026 issue**, *Grasp Independent Indirect Tool Force Estimation using Vision-based Tactile Sensors*, DOI **10.1109/LRA.2025.3629985**；https://discovery.ucl.ac.uk/id/eprint/10217859/1/_RAL_Grasp_Independent_Tool_Force_Estimation_with_Tactile_Sensing%20%284%29.pdf|**H：R40记录当轮全文核读；R41未重新打开PDF**|工具间接力估计先例；UR5+DIGIT+Nano17与FR5+D435+KWR75D不同。|
|S41-13|Min et al., **2026**, *Tactile-Proprioceptive Sensor Fusion for Contact Wrench Estimation in Whole-Body Physical Human-Robot Interaction*；https://arxiv.org/abs/2605.28412|**B：arXiv摘要**|摩擦迟滞TCN及融合已有研究；使用机器人皮肤和电机电流，非腕F/T独立压力线真值。|
|S41-14|AUTO-R40完整本地研究包（46项成员）|**本地A：45/45项内部SHA、CRC、外部ZIP SHA通过；R40论文报告/教学/候选/决策/代码均实读**|继承R40动态核Gate，不等于新外部来源；独立测试记录见R41_TEST_REPORT。|

**去重结果**：本轮新访问独立外部来源11项（S41-01–10与S41-13；S41-11/12为历史继承不算新核）；全文PDF核读**2篇**（S41-01、S41-02），作者正文含完整约束但PDF受阻**1项**（S41-03），正式摘要/方法片段/README **8项**。深入比较S41-01、02、03、04，至少S41-01与02有实验正文；许可证未经确认者不得引入源码。所有链接仅供阅读，不在研究包内分发论文全文。

## 附录B｜完整Baseline/创新竞争登记

# AUTO-R41｜BASELINE_AND_INNOVATION_REGISTER

**状态：主学术论文未选定，FR5厂家工程Baseline仅方案，唯一核心创新I-A条件保留；排序与R40相同，证据强度与退出门槛发生修订。** 以下是**投入审计优先级**而非效果排名。A=本轮实际核读全文；B=本轮官方摘要/仓库；H=R40历史来源；U=未取得。任何数据和模型不得默认公开授权。

|排序|主论文候选：作者、题名、年、DOI|可获取层级/源码数据许可版本|数学与接口/数据率/硬件匹配|独立真值、2025–26新颖性、结论及最小补证|R40→R41|
|---|---|---|---|---|---|
|1|Patrick Grady, Jeremy A. Collins, Samarth Brahmbhatt, Christopher D. Twigg, **Chengcheng Tang**, James Hays, Charles C. Kemp; *Visual Pressure Estimation and Control for Soft Robotic Grippers*, **IROS 2022**, **10.1109/IROS47612.2022.9982073**|H：R40登记MIT/PyTorch1.12.1及大数据，**R41未独立验证LICENSE、commit、权重SHA、数据授权**|外部RGB软夹爪压力；D435 RGB有模态可能性，但曝光、压力标签、域偏差未知；无需假设FR5底层力矩可用；推理速率U|独立分段压力参考缺失，先核源码及标签；**有条件优先复现资格审计**，不等于已选主论文|顺序不变；R41新增压力不可辨识反证，若标签只由F/T合成则退出|
|2|Jeremy A. Collins, Cody Houff, Patrick Grady, Charles C. Kemp; *Visual Contact Pressure Estimation for Grippers in the Wild*, **IROS 2023**, **10.1109/IROS55552.2023.10342124**|H：论文历史登记；本轮未复核LICENSE、数据、commit、版本|视觉+腕F/T弱监督，腕力矩不能无条件唯一确定压力；FR5传感器可提供弱约束，但与原夹爪几何/训练标签不同；速率U|若没有独立压力真值不能声称重建；**有条件保留**，补许可证与真值生成机制|顺序不变，R41明确纯力矩C必须有外部证书|
|3|Erik Helmut, Luca Dziarski, Niklas Funk, Boris Belousov, Jan Peters; *Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*, **IROS 2025**, **10.1109/IROS60139.2025.11246486**|H：历史登记可能MIT及约3.98GB，未复核固定版本/数据许可|专用GelSight Mini+FEA；D435不能直接提供同域内部触觉；材料/网格参数、推理速率U|需额外触觉/独立分段真值，**暂缓FR5移植、保留压力数学比较**|顺序不变；2025-26专用压力网络已覆盖广义“压力分布估计”|
|4|Wendi Chen, Han Xue, Yi Wang, Fangyuan Zhou, Jun Lv, Yang Jin, Shirun Tang, Chuan Wen, Cewu Lu; *ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*, **arXiv 2025**, **10.48550/arXiv.2512.10946**|H：论文摘要历史登记；正式版本/源码许可/权重数据U|Flexiv视觉慢环+力快环，FR5厂家FT_Control、SDK v2.3.0/固件4.0.0真实频率未核；可能要求高频动作|非人体独立真值/同条件控制基线缺，**有条件保留但不得实体执行**；先核离线forward及底层依赖|顺序不变；Cetin2026新增10/500Hz真实刮削挑战，不能称慢快力控首创|
|5|Han Xue, Jieji Ren, Wendi Chen, Gu Zhang, Fang Yuan（RSS网页）/Yuan Fang（PDF差异）, Guoying Gu, Huazhe Xu, Cewu Lu; *Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*, **RSS 2025**, **10.15607/RSS.2025.XXI.052**|H：论文历史登记；许可证、代码commit、训练集/模型SHA U|专用TactAR光学触觉不能由腕F/T等价替代；FR5高频servo/低层关节力矩U|**暂缓**；除非合法可复现且硬件映射有据，不能当主Baseline|顺序不变；与Cetin2026共同构成控制创新先例|

**新挑战论文（不占五篇主候选席位，待下轮竞争审计）**：Cenk Cetin, Shreyas Pouli, Gabriella Pizzuto, 2026, *Learning Adaptive Force Control for Contact-Rich Sample Scraping with Heterogeneous Materials*, DOI 10.48550/arXiv.2603.10979。R41实际全文核读：Franka Research 3，10Hz高层+500Hz底层关节力矩，D405，玻璃瓶刮削；有五材料真实称重实验和固定wrench强Baseline，**任务高度相关但底层接口严重不匹配**。本轮判定**挑战者/暂缓FR5复现**，先查公开代码/数据授权、模型/控制频率与可否降阶为厂家FT_Control接口。不可移植论文4N到人体。

**新数学对照（不自动升级主论文）**：Xu et al. 2020 6DFC (10.1109/ICRA40945.2020.9197293) 和 Caron et al. 2015 接触扳手锥 (10.1109/ICRA.2015.7139910)。已证明6D面积接触/CoP/纯扭矩约束有先例；Caron仓库GPL-3.0且旧OpenRAVE依赖，未引入任何代码。

**厂家/简单工程强Baseline（未验证）**：FR5厂家合法`FT_Control`、固定轻力、外部监测、WORK→UNLOAD→OFFSURFACE_RETURN、M1/M2/M3人工接触前审核。没有真实SDK/固件时序/安全证据，不得运动。**算法强Baseline**：wrench-only带宽/力矩误差与**宽纯力偶上界**的三箱LP；本轮新增“有独立证书时较窄C”的条件性比较。**零偶矩是假设不成立的弱基线**。

**唯一条件性首选I-A**：有限宽柔性刮板左右边载荷的**部分可辨识区间**，不是“腕F/T唯一反演压力”。R41独立证据指出：总力+一阶矩仅确定两个线性组合，三箱至少存在一维零空间；未知纯力偶进一步扩宽偏载界；仅在独立动态核、空间变换、边缘几何与纯力偶上界证书齐全时给区间，否则拒绝。首要科学问题：**是否能用独立视觉接触支持与外部计量证书在非人体仿体上显著收紧左右偏载区间，而不损失独立分段真值覆盖？**

**唯一备选I-B**：接触换向的延迟鲁棒WORK–UNLOAD–OFFSURFACE_RETURN；Cetin2026已经做刮削自适应wrench，创新需限于FR5接口与安全卸载/换向可证伪条件；无实体接口前暂缓。

**Gate**：G0手眼/边缘/TCP与法兰分离；G1 F/T偏置/重力/动态核/时钟；**G2 纯力偶界C、摩擦与工具弯曲交叉项由独立非人体力台证明**；G3分段左右载荷独立真值、LOAD/UNLOAD与盲检；G4 FR5 SDK固件外环周期及失败卸载可验证；G5伦理/风险另批。当前G0–G5均未实体满足，结果HOLD。退出：若C无法物理上界认证、非人体盲检区间覆盖不足、相对强Baseline无法收紧、必须依赖未授权底层扭矩，则淘汰/降级I-A。未达到证据，不宣布主论文赢家。

**三入口与统一路径不变**：M1人工有序3D点；M2背部实体画线识别并人工确认起终方向；M3解剖候选经专家审核、可编辑关键点图、两点线路或方式模板。三入口输出位置、切向、曲面法向、置信度/不确定性、工具姿态、真实有限宽扫掠资格、WORK/UNLOAD/OFFSURFACE_RETURN；接触前审核。所有本轮代码只离线，不包含机器人运动指令。

## 附录C｜逐项决策变化与最小补证

# AUTO-R41 决策日志（均为可追溯的条件性决定）

|ID|决定|与R40差分|最小反证/补证|
|---|---|---|---|
|D41-01|R40已实读核验：46成员、45内部SHA、外部ZIP SHA、CRC均通过；继续以R40为唯一上一轮|未把本次触发视为新轮|如文件校验不一致则撤销本轮继承|
|D41-02|核心研究从动态时间核**转向空间有限宽压力不可辨识**；动态核作为前置Gate保留|新独立数学问题|如果空间分段真值不可得则退回仅测总力|
|D41-03|零纯力偶不是可默认采用的物理事实|新增模型不确定性|用独立力台、不同姿态/摩擦工况测出C|
|D41-04|即使C=0，三箱非负压力也不能从总力和一阶矩唯一恢复|新增明确零空间反例|增加独立分段参考或强物理先验后盲检|
|D41-05|采用Charnes–Cooper变换与线性规划，输出右边载荷占比/CoP区间，始终HOLD|新可运行算法|独立分段压力真值覆盖不足则拒绝|
|D41-06|用C=.04宽盒作数学强Baseline，C=.012条件证书算法，C=0弱基线，三者同数据比较|新增公平消融|证书C=.012若无法独立物理证明则不允许声称现实收益|
|D41-07|错误C证书40/40内部可行但40/40真值漏覆盖，定为必要失败反证|新增反证|独立计量外审，而非同源拟合|
|D41-08|Xu2020 6DFC已覆盖6D面积接触；且其规划抓取实验point 339胜6DFC 272，不能只报其召回提升|新增新颖性与性能反证|限定目标为带真值的偏载区间，不声称全面优于点模型|
|D41-09|Cetin2026真实刮削强挑战者，但500Hz关节扭矩控制与FR5接口未核；暂不替换五篇候选|新增对I-B新颖性反证|核作者代码许可与FR5外环适配再决定排序|
|D41-10|Caron2015矩形CWC已明确CoP/扭转界，作者代码GPL-3.0且HRP4模型版权不清；不引入源码|新增许可与已有方法边界|合法重写仅参考数学，或证明新任务不同|
|D41-11|VPEC→ViPER→FEATS→ImplicitRDP→RDP投入审计顺序不变；赢家待定|新增Cetin挑战者但无足够复现证据改榜|源码LICENSE/commit/数据真值/FR5接口四项补证|
|D41-12|实体Gate全HOLD；M1/M2/M3与WORK/UNLOAD/OFFSURFACE_RETURN继续作为完整系统合同|不变|独立物理/伦理与厂家接口审批|


## 附录D｜复现命令、工具边界与人工真值

```bash
python -m pytest -q test_edge_partial_identification.py
python ablation_r41.py
python edge_partial_identification.py
```

环境：Linux容器、Python3.13、NumPy/SciPy/pytest；不是用户WSL Ubuntu22.04/ROS2 Humble。全部代码原创最小数学原型，不调用厂家SDK、不下载第三方论文代码。源码、JUnit、消融JSON、决策与源级登记都在同一ZIP；源码许可证见README。输出HOLD_NO_MOTION。独立数学真值由随机生成的三箱p与真实c直接计算，与LP求解器相互独立，但它仍属于**合成真值**，绝不等同非人体力台。