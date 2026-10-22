# AUTO-R36｜博士级完整研究报告

**2026-10-21｜离线研究内容验收；实体E0–E5 HOLD；主学术Baseline待定。** 本轮科学问题：异步采样是否将无迟滞的分段参考阵列伪装成LOAD/UNLOAD迟滞？

研究包包含实际执行的`src/temporal_calibration_gate.py`、30项新增pytest、R30–R35逐套回归原始记录、固定种子消融和上一轮SHA审计。本文引用文献的访问层级逐条披露；全部实验为人工数学数据，零次FR5、D435、KWR75D、分段力台实测。

## 01｜实际归档核查与R35→R36差分

本轮首先逐成员打开上一轮真实归档，而不是根据对话摘要声称“已读”。外部SHA256与发布文件一致，41个ZIP成员CRC检查无错，40条内部SHA256逐条一致。实际阅读报告、教学、候选矩阵、决策、测试、来源、标定算法及其测试，保留原ZIP供复核。R35证明P满秩、条件数与双支路盲检是必要条件，但没有为分段标准器通道与加载标准器提供独立时钟映射和载荷变化率证据。R36不修改R35历史记录，而是建立新的可审计时间误差门。

**公式 F36-01**

$$
\mathrm{SHA256}(ZIP_{35})=\texttt{da0fd245\ldots600b7d1},\quad N_{CRC}=41,\ N_{SHA}=40
$$

**表 T36-01**

|项目|R35已证|R36新证|
|---|---|---|
|矩阵标定|FIT满秩、LOAD/UNLOAD盲检|时间错位误差传播|
|资料审计|41文件、40哈希|实际重读八类核心文件|

**关联证据**：S36-01, S36-02；**关联决策**：D36-01, D36-02。

## 02｜三个决策问题、可证伪假设与预注册

Q1：加载与卸载不同输出究竟是材料迟滞，还是采样滞后在相反斜率上制造的伪差异？Q2：若相对采样时间和各单元变化率有可信上界，能否得到不漏掉真矩阵的保守误差盒？Q3：时间Gate是否改变边缘载荷创新的可执行性和退出条件？预注册H0为真实矩阵不随支路变化，全部差异由固定延迟产生；用外部同步脉冲/动态标准器可证伪。不能拿模型自己推断的时间差当独立真值。

**公式 F36-02**

$$
H_0:\ y_b(t)=C\,p_b(t-\delta)+e,\quad C_{LOAD}=C_{UNLOAD}
$$

**表 T36-02**

|问题|独立判据|失败动作|
|---|---|---|
|Q1伪迟滞|同一C+已知δ能解释差异|暂停材料迟滞结论|
|Q2误差界|有证据的L与δ|拒绝时序资格|
|Q3创新收益|同覆盖/拒绝率下宽度净减|降级I-A|

**关联证据**：S36-01, S36-02, S36-03；**关联决策**：D36-02, D36-03。

## 03｜全文核读：Bae等2016多速率视觉力觉

实际阅读公开全文Preliminaries、Estimator design、Experimental results、Conclusions。Bae、Jeon、Huissoon 2016用多速率Kalman框架、力作为过程输入、延迟视觉作为观测，并采用回溯修正。作者BHand快速周期2.5ms，视觉总延迟约25ms；测试包括平面气浮台物体和重力条件下接触运动。重要限制：作者的离线“监控真值”由原视觉序列去延迟与插值得到，非独立接触压力真值；接触力与接触点精度是估计前提，且系统依赖BHand专用接口。故本轮不能宣称首次视觉力觉时间补偿，也不移植作者周期为FR5参数。

**公式 F36-03**

$$
x_{k+1}=f(x_k,u_k),\qquad z^v_k=h(x_{k-d})+\eta_k
$$

**表 T36-03**

|维度|论文实际条件|FR5迁移缺口|
|---|---|---|
|快速输入|BHand关节/应变CAN 2.5ms|FR5反馈时序未核|
|视觉|约25ms视觉延迟|D435固件/时间域未核|
|评价真值|原视觉离线去延迟插值|非独立分段载荷真值|

**关联证据**：S36-02, S36-03, S36-04；**关联决策**：D36-03, D36-04。

## 04｜Koike2019迟滞历史与访问层级

Koike、Sakaino、Tsuji的Sensors 2019 DOI 10.3390/s19194259研究六轴F/T迟滞补偿，检索到摘要和可见结论：时间序列输入可改善某些线性回归标定，神经网络/混合模型的历史输入不一定改善，数据规模影响比较。尝试打开PMC正文遇到验证码，MDPI打开遇429，因此本轮不把该文计为全文核读。与Bae相比，Koike关注传感器历史依赖而非视觉延迟；与R36相比，它没有提供FR5刮板参考仪器的独立时间证据。

**公式 F36-04**

$$
y_t=g(p_t,p_{t-1},\ldots)+e_t
$$

**表 T36-04**

|证据|已访问|不可声称|
|---|---|---|
|2019论文|PubMed摘要及索引结论|完整实验参数已核|
|方法|时间序列线性回归/NN|作者代码复现|
|迁移|六轴F/T历史效应|本FR5迟滞量值|

**关联证据**：S36-03, S36-04, S36-05；**关联决策**：D36-04, D36-05。

## 05｜NIST动态校准与标准器独立性

NIST Vlajic与Chijioke 2016官方摘要说明用已知质量及激光干涉测得加速度建立动态力标准，谐波标定考虑动态摇摆等不确定度；官方报告的频率和不确定度属于其装置。我们只取得NIST摘要，没有读完整Metrologia论文，不复制其设备性能。关键迁移条件是独立动态力、相位和时钟证据必须与阵列原始读出链一同保存。单轴动态标准不能直接认证多单元曲面阵列。

**公式 F36-05**

$$
F(t)=m\,a(t),\quad a(t)=\frac{d^2x(t)}{dt^2}
$$

**表 T36-05**

|参考来源|独立测量|迁移限制|
|---|---|---|
|NIST2016|质量+激光干涉加速度|非本项目阵列|
|R35已知加载|合成p_j及矩阵C|未校动态相位|
|R36要求|独立时差/速率上界|仍待实物证书|

**关联证据**：S36-04, S36-05, S36-06；**关联决策**：D36-05, D36-06。

## 06｜ROS2与D435时间域源码审计

实际检查ROS2 message_filters官方rolling Python源码：ApproximateTimeSynchronizer以header时间戳、slop及队列选择匹配，部分选项在rolling可见；未证明用户ROS2 Humble构建具有同样接口。检查librealsense官方master rs_frame.h，确认HARDWARE_CLOCK、SYSTEM_TIME、GLOBAL_TIME三种域，global为设备时间转换到OS时钟。2024年D435 issue报告过未来时间戳，但只是用户故障报告，不可泛化。R36契约不接受“两个数差不多”代替独立时钟映射，禁止以ROS到达时间当真实采样时刻。

**公式 F36-06**

$$
t_{common}=a\,t_{device}+b\pm\epsilon_{clock}
$$

**表 T36-06**

|组件|本轮已核|仍未核|
|---|---|---|
|ROS message_filters|rolling源码按header与slop配对|Humble版本二进制/运行时|
|RealSense rs_frame.h|三种timestamp domain枚举|D435固件/SDK实测|
|D435 issue|用户报告future timestamp|本项目是否发生|

**关联证据**：S36-05, S36-06, S36-07；**关联决策**：D36-06, D36-07。

## 07｜2025–2026新颖性反证与边界

本轮进一步实际打开Min等2026与UniForce 2026两篇arXiv v1全文，核读方法、实验、局限。Min以电机电流/关节模型+气动皮肤+TCN处理静动摩擦转变，实验为自制4DoF皮肤机器人，作者明确动态转静态仍不可靠；依赖电机电流与关节模型，不具备已核的FR5迁移接口。UniForce用GelSight/TacTip/uSkin的双指准静态力平衡训练共享潜力表示，在白板擦拭任务做视觉与触觉对照；作者§VI明确快速运动、滑移、柔顺、未对准会破坏力平衡，恰好反证把准静态监督直接搬到动态刮痧。两篇都不是FR5/D435实测，也未运行其作者代码。加上Bae 2016的延迟补偿和R35已核的双阶段/曲率标定，进一步收紧创新：不做“首次融合/首次迟滞网络/首次同步”的宽泛主张；首选只保留有限宽边缘可辨识、独立参考及拒绝的组合科学问题。

**公式 F36-07**

$$
\mathcal{N}_{novel}\subseteq\mathcal{N}_{edge\;identifiability}\setminus\mathcal{N}_{generic\;fusion}
$$

**表 T36-07**

|相邻工作|公开证据层级|本轮否定的泛化创新|
|---|---|---|
|Min2026|全文§II–VI|首次摩擦迟滞TCN|
|UniForce2026|全文§III–VI|首次跨触觉力表征|
|Bae2016|全文|首次视觉力觉延迟补偿|

**关联证据**：S36-06, S36-07, S36-08；**关联决策**：D36-07, D36-08。

## 08｜时间错位如何伪装加载—卸载迟滞

设标定仪器真实线性、无材料迟滞，真实输出y(t)=kp(t-δ)，但加载参考使用p(t)。对恒定上升斜率+v，读数为k(p-vδ)；对下降斜率-v，读数为k(p+vδ)。同一名义p上两支路相差2kvδ，纯由时间滞后造成。若k=0.8、v=10N/s、δ=0.005s，则伪差为0.08N；这只是说明性数学例子，绝非KWR75D实测。独立时差测量或改变加载速率可区分部分解释，但未知真实迟滞也可能与延迟共存。

**公式 F36-08**

$$
\Delta y_{pseudo}=y_{UNLOAD}-y_{LOAD}=2k\,v\,\delta
$$

**表 T36-08**

|条件|LOAD|UNLOAD|
|---|---|---|
|同一真实增益k|k(p-vδ)|k(p+vδ)|
|相同名义载荷p|读数偏低|读数偏高|
|误判风险|伪支路差|伪迟滞回线|

**关联证据**：S36-07, S36-08, S36-09；**关联决策**：D36-08, D36-09。

## 09｜时间差与真实迟滞的不可辨识边界

当观测只有名义载荷p和读数y时，可以用“同一矩阵+延迟”或“两个不同支路映射+零延迟”拟合相同加载/卸载曲线，参数映射非单射。此处不是宣称真实材料无迟滞，而是证明**缺少独立时间信息时，迟滞大小不可唯一归因**。R35的满秩P解决空间方向的C辨识，不解决时间相位与历史状态辨识；这也是R36对上一轮结论的实质限制。

**公式 F36-09**

$$
\{h(p),\delta\}\mapsto y(t)\quad\text{may be non-injective}
$$

**表 T36-09**

|证据|能判断|不能判断|
|---|---|---|
|仅两条回线|输出不同|差异归因唯一|
|已知δ及速率|时间误差上界|材料迟滞完全为零|
|独立动态标准|更强因果判据|自动证明压力分布|

**关联证据**：S36-08, S36-09, S36-10；**关联决策**：D36-09, D36-10。

## 10｜Lipschitz变化率与误差上界推导

若各单元真实载荷满足|p_j(t)-p_j(s)|≤L_j|t-s|，且|C_ij|≤Cmax，则三角不等式直接给出各通道时间错位等效读数误差上界。Δtmax由采集时间戳差绝对值、时钟映射不确定度、滤波群延迟界和采样抖动界相加；不是简单用消息到达时间相减。对每条FIT和BLIND都检查，并取最坏值统一扩大读数误差。该界只在L_j及时间预算独立可信、信号无超界冲击/跳变时成立。

**公式 F36-10**

$$
|\Delta y_i|\le C_{max}\sum_{j=1}^{n}L_j\,\Delta t_{max}
$$

**表 T36-10**

|量|单位|如何取得|
|---|---|---|
|Cmax|等效N/N|独立矩阵物理界|
|L_j|N/s|独立动态加载上界|
|Δtmax|s|时钟映射+滤波/抖动预算|

**关联证据**：S36-09, S36-10, S36-11；**关联决策**：D36-10, D36-11。

## 11｜坐标、单位、时间、独立真值契约

R36延续R35的TOOL_CONTACT_LOCAL、ENV_ON_TOOL、五单元N与mm、力矩N·mm到N·m的1e-3换算。新TimingWitness逐episode绑定原样本SHA、load/channel采集ns、各自可用ns、时钟映射版本、群延迟、抖动、载荷变化率证书。法兰位姿、标定TCP与柔性真实接触线不等价。腕部净力/力矩不唯一决定压力分布；独立分段参考也需要真实计量证书。SHA是内容完整性而不是证书真实性。

**公式 F36-11**

$$
t_{effective}=|t_y-t_p|+\epsilon_{clock}+d_{filter}+j_{capture}
$$

**表 T36-11**

|字段|契约|拒绝条件|
|---|---|---|
|载荷p_j|局部法向积分力N|误用Pa或ADC码|
|时间|非负整数ns+clock_map_id|跨域无映射|
|F/T|力方向ENV_ON_TOOL|符号/原点未知|

**关联证据**：S36-10, S36-11, S36-12；**关联决策**：D36-11, D36-12。

## 12｜新增可执行算法与失败拒绝

新src/temporal_calibration_gate.py是独立原创离线模块，没有复制论文作者源码。它检查时间域、clock_map与rate证书版本、样本digest、可用性因果、各项ns非负整数、各单元N/s有限上界、物理声明的独立证据类别。对任何超预算、伪造digest、未来可用或缺失证据立即抛CalibrationHold。通过后将最坏时间误差加到R35读数误差预算，复用其冻结FIT/BLIND与系数盒；所有返回仍HOLD。该算法不做插值、不产生位置/力指令，不宣称真实时钟已校准。

**公式 F36-12**

$$
\epsilon_{y,eff}=\epsilon_{y,readout}+C_{max}\sum_jL_j\Delta t_{max}
$$

**表 T36-12**

|模块|输入|输出|
|---|---|---|
|temporal_calibration_gate.py|冻结契约+TimingWitness|扩大误差/拒绝|
|calibration_design_gate.py|R35 FIT/BLIND|矩阵盒/盲检HOLD|
|运动适配器|无|始终禁止|

**关联证据**：S36-11, S36-12, S36-13；**关联决策**：D36-12。

## 13｜独立数学验证与继承回归

实际独立解压嵌套R35→R30历史包，逐轮执行其测试：R30 45、R31 44、R32 20、R33 55、R34 40、R35 43；本轮新增30，合计277项，全部pytest退出码0。新增测试覆盖真实矩阵被误差盒包含、无时序预算拒绝、时间域错配、未来可用、SHA篡改、超速率、伪物理声明、加载卸载解析式及300个独立随机三角不等式检查。R24–R29没有执行，不计入277。

**公式 F36-13**

$$
\mathrm{passed}=45+44+20+55+40+43+30=277
$$

**表 T36-13**

|测试来源|本轮实际重新执行|状态|
|---|---|---|
|R30–R35继承|247项|通过|
|R36新增|30项|通过|
|合计|277项|全部离线通过|

**关联证据**：S36-12, S36-13, S36-14；**关联决策**：D36-01, D36-02。

## 14｜公平消融与漂移注入的真实输出

固定种子3607、同一C、相同FIT/BLIND人工载荷与通道数据，200组盒内δ=0.5–2.5ms、变化率6–14N/s；R35未计时间时系数盒平均半宽0.000300、200组中0组完整包含真C，194组盲检冲突；R36扩大后平均半宽0.045311、200组完整包含真C、零盲检冲突。另100组δ=6–12ms超过预注册5ms上界，R36全部拒绝；R35没有时差字段，100组均未因时间元数据拒绝，但不能称其整体“安全接受”。人工伪迟滞均值0.021986N、最大0.078198N。更宽的盒是诚实的不确定度成本，非性能优势。

**公式 F36-14**

$$
W_{timing}=0.045311\quad\text{vs}\quad W_{naive}=0.000300
$$

**表 T36-14**

|方法/指标|200组盒内|100组超预算|
|---|---|---|
|忽略时间的R35|0/200全矩阵系数盒覆盖；194次BLIND冲突|100组未按时间元数据拒绝|
|时间上界+R35|200/200系数盒覆盖；0次BLIND冲突|100/100时间Gate拒绝|
|Oracle真时间|未作为可执行算法|不可当公平部署基线|

**关联证据**：S36-13, S36-14, S36-15；**关联决策**：D36-02, D36-03。

## 15｜主论文Baseline竞争矩阵与厂家强基线

维持R35五篇候选与其作者/年份/DOI/全文层级/许可/数据/接口缺口，详见独立BASELINE_AND_INNOVATION_REGISTER。R36未下载作者权重，未核实新的固定commit，故不调整排名：VPEC(IROS2022,10.1109/IROS47612.2022.9982073)；ViPER(IROS2023,10.1109/IROS55552.2023.10342124)；FEATS(IROS2025,10.1109/IROS60139.2025.11246486)；ImplicitRDP(arXiv2025,10.48550/arXiv.2512.10946)；RDP(RSS2025,10.15607/RSS.2025.XXI.052)。FR5厂家FT_Control恒力+平滑卸载+离面返程仍只是工程强Baseline计划，SDK/固件与真实周期未核。

**公式 F36-15**

$$
B^*\in\{VPEC,ViPER,FEATS,ImplicitRDP,RDP\}
$$

**表 T36-15**

|投入顺序|论文|R36结论|
|---|---|---|
|1/2|VPEC / ViPER|有条件保留，未复现|
|3|FEATS|有条件方法对照，传感器不匹配|
|4/5|ImplicitRDP / RDP|慢快策略待FR5接口核验|

**关联证据**：S36-14, S36-15, S36-16；**关联决策**：D36-03, D36-04。

## 16｜唯一创新、备选、入门与退出Gate

I-A唯一首选科学问题仍是有限宽刮板边缘载荷的部分可辨识区间、经独立物理校准的非冗余视觉证据和失败拒绝；R36新增G_time：时钟域、采样错位、动态变化率、滤波群延迟必须有独立上界。观测假设要求有限宽接触模型、已知坐标与力方向、可追溯分段载荷标准器；无法唯一估计时只输出区间。I-B仍是WORK→UNLOAD→OFFSURFACE_RETURN换向延迟鲁棒控制。若同覆盖率/拒绝率下D435无宽度净收益，I-A降级；不能为凑博士创新堆EKF/ESO/MPC。

**公式 F36-16**

$$
G_{I-A}=G_{ref}\land G_{time}\land G_{truth}\land G_{fair}
$$

**表 T36-16**

|路线|当前判断|退出条件|
|---|---|---|
|I-A有限宽部分可辨识|条件首选|参考盲检失败或无净收益|
|I-B换向卸载延迟鲁棒|唯一备选|厂家接口不可执行|
|泛化首次融合/时序学习|淘汰新颖性主张|已被先行工作覆盖|

**关联证据**：S36-15, S36-16；**关联决策**：D36-04, D36-05。

## 17｜M1/M2/M3统一曲线与安全Gate

完整系统设计继续保留M1人工有序三维选点、M2人体背部实体画线与起终方向确认、M3自动解剖标志候选及专家审核可编辑点图和模板。三者统一输出位置、切向、法向、置信/不确定度、工具姿态、连续有限宽扫掠资格、WORK/UNLOAD/OFFSURFACE_RETURN模式；接触前人工审核。R36时间Gate属于上游只读数据资格，不修改DEV工程也不连接ROS2运行时。先手眼/边缘/TCP/FT补偿/厂家FT_Control时序核验，再非人体仿体、离面、低能量单向、卸载与离面返程；贴面返程和真人实验仍HOLD。

**公式 F36-17**

$$
\gamma(s)=\{p,t,n,\Sigma,R_{tool},q_{sweep},mode\}
$$

**表 T36-17**

|入口|输入|统一输出|
|---|---|---|
|M1|人工有序3D点/指定路线|审核后有序曲线|
|M2|实体画线+方向确认|审核后有序曲线|
|M3|解剖候选+专家审核图/模板|审核后有序曲线|

**关联证据**：S36-16；**关联决策**：D36-05, D36-06。

## 18｜局限、失败清单与下一轮最小补证

当前最大局限是时间变化率和时钟不确定度全部由合成契约给定，未取得任何D435、KWR75D、FR5或分段参考力台原始动态记录；因此R36数学上界只在合成假设成立时可靠。Koike全文受访问限制，ROS rolling与RealSense master源码没有固定用户版本；Min和UniForce本轮已核读公开全文，但未核作者代码/权重许可。下轮优先建立跨设备时钟映射的只读证据清单，冻结参考标准器/滤波/固件/采集软件版本，做预注册独立速率与延迟盲测；若无硬件则继续离线反证，不虚增物理研究轮次。没有GitHub读写、没有新仓库。

**公式 F36-18**

$$
G_{next}=G_{clock\_mapping}\land G_{dynamic\_reference}\land G_{blind\_domain}
$$

**表 T36-18**

|阻塞|所需证据|下一轮最小动作|
|---|---|---|
|真实时间域|D435/FT/标准器原始timestamp|只读字段与版本清单|
|真实动态变化率|独立加载标准器/证书|阶跃/斜坡盲测计划|
|FR5力控|SDK固件FT_Control日志|只读接口审计|

**关联证据**：；**关联决策**：D36-06, D36-07。

## 附录A｜去重来源审计（逐条标识实际访问层级）

|编号|作者/来源、年份与获取层级|本轮核读内容及局限|决策影响|
|---|---|---|---|
|S36-01|AUTO-R35 本地完整归档；H/本地证据|本轮实读ZIP+报告+讲义+源码+测试；41成员、40 SHA、CRC通过|R35满秩双支路仍缺跨设备动态时钟/速率证据|
|S36-02|Bae, Jeon & Huissoon, Vision and force/torque integration for realtime estimation of fast-moving object under intermittent contacts (2016), DOI 10.1186/s40648-016-0054-2；A/全文方法实验|本轮实际阅读公开全文Preliminaries、Estimator design、Experimental results、Conclusions；BHand双速率2.5ms/25ms视觉延迟仅作者设备|时间延迟已被显式建模，不能声称首次视觉力觉时序补偿|
|S36-03|Koike, Sakaino & Tsuji, Hysteresis Compensation in Force/Torque Sensors Using Time Series Information (2019), DOI 10.3390/s19194259；B/摘要+可见结论|PubMed/MDPI检索索引摘要及结论可见；PMC/MDPI正文访问受验证/429阻断，未核读完整方法实验|历史依赖真实存在，但不可把时间错位误认物理迟滞|
|S36-04|Vlajic & Chijioke, Traceable Dynamic Calibration of Force Transducers by Primary Means (2016), DOI 10.1088/0026-1394/53/4/S136；B/官方摘要|NIST官方摘要；谐波加载+激光干涉加速度，报告2kHz以内作者装置，不是FR5|静态秩与迟滞门不替代动态频响/时序证书|
|S36-05|ROS 2 message_filters ApproximateTime 实现与文档；B/官方源码|本轮查看官方rolling分支Python实现：header时间戳/slop/queue_offset；不是已核实的Humble二进制接口|近似配对≠物理同时刻；Humble版本和DDS延迟需后验核验|
|S36-06|RealSense librealsense rs_frame.h 官方时间戳域声明；B/官方源码|本轮阅读官方仓库master头文件：HARDWARE_CLOCK、SYSTEM_TIME、GLOBAL_TIME；未固定D435固件/SDK版本|不同时间域不得仅凭数值接近视为同步|
|S36-07|RealSense D435 timestamp future issue #13122 (2024)；B/用户issue|阅读公开issue：用户报告global time下未来时间戳；未经本项目复现，不是厂家普遍故障结论|必须保存固件/SDK/时钟映射版本并做独立证据检查|
|S36-08|In Situ Calibration of Six-Axis Force–Torque Sensors for Industrial Robots With Tilting Base (2021/2022), DOI 10.1109/TRO.2021.3127391；B/官方摘要|IEEE官方摘要：单标准砝码、重力倾斜基座联合标定，ATI实体+模拟；未取得全文|坐标/重力补偿不能由静态参考自动解决|
|S36-09|Min et al., Tactile-Proprioceptive Sensor Fusion for Contact Wrench Estimation in Whole-Body Physical Human-Robot Interaction (2026), arXiv:2605.28412；A/全文方法实验|本轮实际打开arXiv v1全文并核读§II–V、VI：电机电流+气动皮肤+TCN，4DoF/RS485，作者以mA评估无外力残差，动态转静态可靠性受限；未运行代码|2026新邻近覆盖：不可宣称首次时序学习或迟滞分离|
|S36-10|Chen et al., UniForce: A Unified Latent Force Model for Robot Manipulation with Diverse Tactile Sensors (2026), arXiv:2602.01153；A/全文方法实验|本轮实际打开arXiv v1全文§III–VI：准静态双指力平衡、GelSight/TacTip/uSkin，作者白板擦拭20次/条件；§VI明确快速运动、滑移、柔顺与失准破坏力平衡；未运行代码|跨传感器通用力表示已有邻近研究，D435外部视觉迁移仍无证据|
|S36-11|Lo Preti et al., Two-Stage Characterization Pipeline and Open-Source Framework (2026), DOI 10.1002/aisy.70436；H/历史全文|R35历史已核全文，本轮仅阅读R35来源审计，未重取论文全文|双阶段安装标定并非本研究首创；动态时间证据需另补|
|S36-12|Zhong et al., Curvature-Aware Calibration (2025 arXiv/2026 IEEE)；H/历史全文|R35历史全文与版本差异记录；本轮仅本地回顾，正式出版全文未取得|曲率补偿新颖性受限，不能把五/六物体版本混用|
|S36-13|Grady et al., VPEC (IROS 2022), DOI 10.1109/IROS47612.2022.9982073；H/历史登记|R35候选登记历史证据；本轮未下载作者权重、数据、源码固定commit|主候选#1，视觉压力监督仍需独立参考|
|S36-14|Collins et al., ViPER (IROS 2023), DOI 10.1109/IROS55552.2023.10342124；H/历史登记|R35历史全文与候选登记；本轮未固定LICENSE/commit/权重|主候选#2，平面Sensel与曲面D435不能等同|
|S36-15|Helmut et al., FEATS (IROS 2025), DOI 10.1109/IROS60139.2025.11246486；H/历史登记|R35历史仓库/数据卡MIT记录，本轮未下载数据或运行作者代码|主候选#3，FEA标签不等于独立分段真值|
|S36-16|Xue et al., RDP (RSS 2025), DOI 10.15607/RSS.2025.XXI.052；Chen et al., ImplicitRDP arXiv:2512.10946；H/历史登记|两篇独立论文，仅在此一条对照中列示；历史来源，未复现模型或核官方FR5时序|慢快策略已覆盖宽泛多模态延迟处理；仍缺FR5实时接口|

- **S36-01**：prior/AUTO_R35_REPRODUCIBLE_PACKAGE.zip
- **S36-02**：https://link.springer.com/article/10.1186/s40648-016-0054-2
- **S36-03**：https://pubmed.ncbi.nlm.nih.gov/31575044/
- **S36-04**：https://www.nist.gov/publications/traceable-dynamic-calibration-force-transducers-primary-means
- **S36-05**：https://github.com/ros2/message_filters/blob/rolling/src/message_filters/__init__.py
- **S36-06**：https://github.com/realsenseai/librealsense/blob/master/include/librealsense2/h/rs_frame.h
- **S36-07**：https://github.com/realsenseai/librealsense/issues/13122
- **S36-08**：https://ieeexplore.ieee.org/document/9635598/
- **S36-09**：https://arxiv.org/abs/2605.28412
- **S36-10**：https://arxiv.org/abs/2602.01153
- **S36-11**：https://doi.org/10.1002/aisy.70436
- **S36-12**：https://arxiv.org/html/2510.25965
- **S36-13**：https://github.com/Healthcare-Robotics/VPEC
- **S36-14**：https://github.com/Healthcare-Robotics/ViPER
- **S36-15**：https://github.com/feats-ai/feats
- **S36-16**：https://www.roboticsproceedings.org/rss21/p052.html

## 附录B｜完整决策日志镜像

|决策ID|类型|本轮判断|
|---|---|---|
|D36-01|归档|R35 ZIP/报告/教学/矩阵/决策/源码实际读取且CRC/SHA一致；不是聊天摘要替代。|
|D36-02|核心问题|优先验证“异步采样是否伪装LOAD/UNLOAD迟滞”；其他创新暂不并行争夺主线。|
|D36-03|数学结论|加载与卸载斜率相反时，纯延迟也产生2kLδ的伪迟滞；未独立辨认时间错位前不能解释为材料迟滞。|
|D36-04|新Gate|需独立时钟映射、相对时差上界、采样/滤波延迟上界、各单元载荷变化率上界。|
|D36-05|算法|仅把有界时差传播为读数等效误差并复用R35冻结FIT/BLIND，拒绝盲目插值。|
|D36-06|反证|Bae2016已做多速率延迟补偿，Koike2019已做F/T历史补偿；不宣称首次时序或迟滞学习。|
|D36-07|消融|同条件报告真值覆盖、区间宽度、盲检冲突、拒绝率；不能只比接受样本。|
|D36-08|Baseline|VPEC>ViPER>FEATS>ImplicitRDP>RDP投入顺序不变；主论文胜者待定。|
|D36-09|创新|唯一条件首选仍是有限宽边缘部分可辨识+独立参考+失败拒绝；时间证据新增必需Gate。|
|D36-10|备选|WORK→UNLOAD→OFFSURFACE_RETURN延迟鲁棒控制保留唯一备选，未获实体执行许可。|
|D36-11|安全|所有结果HOLD；没有FR5、D435、KWR75D、分段力台、ROS2节点或人体实测。|
|D36-12|下轮|优先时钟域转换与传感器群时延的只读证据契约，获取独立动态载荷参考后再评估I-A。|