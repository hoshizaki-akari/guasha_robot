# AUTO-R45｜FR5刮痧机器人博士级算法研究完整报告

**主题：ImplicitRDP官方离线复现安全入口审计 + 虚拟目标是否增加有限宽刮板局部压力信息。**

**状态：PASS_OFFLINE_RESEARCH_CONTENT_ONLY；主学术Baseline待定；所有实体/人体/运动Gate HOLD。**

2026-10-21。全部数值实验均为人工数学数据；论文作者实验单独标注。

## 研究执行摘要

本轮以固定commit只读审计证实作者官方eval入口并非无硬件纯forward；以数据处理不等式、零空间反例和独立LP证明VRR不增加局部压力信息。由此将ImplicitRDP主论文审计优先级2→3，FEATS3→2，保留VPEC1与赢家待定。R44归档真实核验；R39–R44历史369项与新增66项合计435项通过。

## 01｜上一轮真实归档与新旧差分

本轮在实际挂载目录读取R44完整研究报告、零基础讲义、候选矩阵、12项决策、失败清单、源码及测试。核对外部ZIP SHA、45个ZIP成员的CRC与44条内部SHA256，无不一致。R44的主结果是三箱模型下k≥2对右侧比例不增益、组条件统计预算不能保证逐episode支持正确；R45没有重复做另一次假阴性概率调参，而转向**主论文可复现性与虚拟目标的信息量**。R44将ImplicitRDP升至第2，但当时尚未固定作者源码commit、未读论文v2全文、未检查真实eval入口。本轮补上这三个可审计缺口。历史R39–R43测试40+60+73+23+49=245；R44顶层124，合计369。第一次错误地对R44归档递归收集，遇到2个重名模块错误；改为仅R44本轮两个测试文件，124/124通过。R45新增66项，总435项，不能将失败首次执行写成成功。

**对应公式 F45-01**：

$$
SHA256(archive_R44) = d7ff40fd62b1b3c656a2ea1751f78a1c9d545b8879364fbdc6cd046f4ad625ad
$$

**对应表 T45-01**：

|核验对象|R45实际证据|限制|
|---|---|---|
|R44 ZIP|45成员/44 SHA+CRC通过|只证完整性|
|历史回归|369项通过|首次收集错误已记录|

**决策索引**：见`DECISION_RECORD.md`中D45-01；来源见`SOURCE_REGISTER.md`。

## 02｜本轮两大决策问题与可证伪假设

问题Q1：ImplicitRDP作者公开的“推理”脚本是否真是与硬件隔离的纯张量forward？独立判据不是README中的Inference标题，而是从脚本、Hydra配置、runner到实际命令sink的可达链。问题Q2：论文的虚拟目标VRR能否为有限宽刮板提供腕部wrench以外的局部压力信息？独立判据是三箱载荷零空间、条件信息与LP可行集合。假设H1：当姿态X和刚度K均由已知量/全局wrench计算时，虚拟目标V不提供独立局部压力观测；反证条件是K或V确实由额外空间压力传感器测得。假设H2：官方eval入口含实机副作用且含不可信pickle加载审查点；反证条件是公开新版本具有经隔离测试的无硬件纯forward。所有研究结果不得触发机器人动作，判定状态固定HOLD_NO_MOTION。

**对应公式 F45-02**：

$$
H_1: I(P;V\mid W,X,K)=0;\quad H_2: safe\_offline(eval.sh)=False
$$

**对应表 T45-02**：

|决策问题|独立验收判据|失败回退|
|---|---|---|
|Q1源码可达性|入口→runner→硬件sink|禁止运行|
|Q2信息量|同观测不同局部p|保留部分辨识|

**决策索引**：见`DECISION_RECORD.md`中D45-02；来源见`SOURCE_REGISTER.md`。

## 03｜来源级别、检索范围与反证标准

本轮实际核对12个去重来源组、19项具体页面/源码对象；完整核读ImplicitRDP arXiv v2八页与Marra 2026公开HTML全文的方法、实验和局限。RSS RDP仅官方摘要，RCT仅摘要，VPEC/ViPER/FEATS仅作者仓库或项目，不能把这些写成全文。作者GitHub同一仓库的eval.sh、control.py、runner、配置等虽分别读到具体行号，但严格合并为一个“论文代码来源组”，避免以多文件凑十篇论文。每个结论区分A全文、B官方源码/数据、P摘要、H历史。最重要的新文献反证来自Marra的独立标定+CoP路线，以及ImplicitRDP的VRR数学：压力估计/力觉融合已高度拥挤，博士创新必须限定为独立空间压力真值与失败拒绝。

**对应公式 F45-03**：

$$
N_{source\ groups}=12,\quad N_{fulltext}=2
$$

**对应表 T45-03**：

|来源层级|本轮实例|不能声称|
|---|---|---|
|A全文|ImplicitRDP v2、Marra v2|未复现作者实验|
|B官方|作者代码/HF/SDK|未运行代码|
|P摘要|RCT|未核全文|

**决策索引**：见`DECISION_RECORD.md`中D45-03；来源见`SOURCE_REGISTER.md`。

## 04｜ImplicitRDP全文：因果慢快结构

论文III-B将低频视觉/位姿与动作频率力觉编码为不同token，使用GRU编码力序列、因果注意力屏蔽未来力信息。为了让同一动作块的逐步预测一致，算法缓存慢观测表示和初始扩散噪声，并使用η=0的DDIM，随着新的力token到来扩展序列、重复去噪。它解决的是动作块内反应性，不是局部压力线反演。作者论文指出视觉慢环每0.6s刷新、快环10Hz；“高频”在作者系统中指相对视觉更新更快，不可偷换成FR5已有500Hz或1kHz底层控制。论文给出v-prediction、Euler角等稳定性消融；这些是作者实验选择，不是本轮重新训练。

**对应公式 F45-04**：

$$
a_{t+s}\leftarrow \pi_\theta(I_{\le t},F_{\le t+s},A^K_{\le t+s})
$$

**对应表 T45-04**：

|结构|作者方法|FR5迁移疑点|
|---|---|---|
|慢环|视觉每0.6s|D435帧时序未核|
|快环|10Hz力反馈动作|FT_Control外环未核|

**决策索引**：见`DECISION_RECORD.md`中D45-04；来源见`SOURCE_REGISTER.md`。

## 05｜ImplicitRDP全文：VRR公式与物理含义

论文III-C从M ẍvt+D ẋvt+K(xvt−xreal)=fext出发，在准静态假设下定义虚拟目标xvt=xreal+K^{-1}fext。作者按力大小自适应选刚度，在低力时用高刚度压低噪声影响，在接触时用低刚度强调力反馈。训练把动作、虚拟目标与刚度拼成增强目标，推理时丢弃辅助维度，只执行动作。论文明确承认给定xreal与K时，虚拟目标预测和力预测在数学上等价；其声称优势来自与动作空间对齐、训练加权，而非获得新物理测量。需区分位置xreal的世界坐标、TCP wrench的坐标变换与柔性刮板真实接触线。

**对应公式 F45-05**：

$$
x_{vt}=x_{real}+K^{-1}f_{ext}\quad(\ddot x,\dot x\approx0)
$$

**对应表 T45-05**：

|物理量|单位|误用风险|
|---|---|---|
|f_ext|N/Nm|混合坐标未补偿|
|K|N/m（平移）|不等于厂家真实刚度|
|x_vt|m|非实际接触中心|

**决策索引**：见`DECISION_RECORD.md`中D45-05；来源见`SOURCE_REGISTER.md`。

## 06｜ImplicitRDP全文：实验设计、强基线与限制

论文IV使用Flexiv Rizon4s、内置关节力矩用于示教、六维F/T、柔性指尖和腕部相机；采集相机及力流均10Hz，作者报告其系统F/T延迟<20ms、ROS2多传感同步延迟<120ms、RTX4090快环推理<30ms。三项任务每项40次示教、20次测试；表I：DP箱翻转0/20、开关8/20、插孔0/20；RDP分别16/20、10/20、5/20；ImplicitRDP分别18/20、18/20、8/20。表II、III、IV有SSL/VRR/v-prediction消融，比较不是简单挑弱基线。作者箱翻转14N判失败、约0.1mm跟踪精度只属于作者设备，不得迁移FR5。限制：示教机器人依赖、非人体局部压力真值缺失、20次样本不支持宣称人体安全。

**对应公式 F45-06**：

$$
p_{success}=n_{success}/20\quad(\text{author trials only})
$$

**对应表 T45-06**：

|作者任务|DP/RDP/ImplicitRDP|FR5意义|
|---|---|---|
|箱翻转|0/20;16/20;18/20|不代表恒力安全|
|开关|8/20;10/20;18/20|不代表接触换向|
|插孔|0/20;5/20;8/20|不代表曲面贴合|

**决策索引**：见`DECISION_RECORD.md`中D45-06；来源见`SOURCE_REGISTER.md`。

## 07｜Marra 2026全文：独立标定与CoP反证

Marra等2026使用Xela磁性触觉指尖、独立ATI Nano17测量压头三维力，专门采集约200k触觉-力对、每指整理约20k；考虑传感器差异、姿态重力和滞后，用各指独立MLP回归，再以多指法向力估计CoP，QP控制指尖与平面，另外“Keep in Touch”补偿未建模误差。实验单指模型归一化MAE约9%、共用模型13%；真实托盘平衡成功率受摩擦、滚动冲击和传感器差异影响。其局限明确指出未传感掌部接触会破坏全接触已观测假设。该论文不是刮板论文，不能把其阈值、100Hz控制、成功率当作FR5结果；但它对“多通道独立力校准+CoP控制”原创性构成反证。

**对应公式 F45-07**：

$$
CoP=\frac{\sum_i F_{n,i}r_i}{\sum_i F_{n,i}}
$$

**对应表 T45-07**：

|Marra环节|证据|对本项目启示|
|---|---|---|
|标定|ATI Nano17+Xela|必须独立物理参考|
|控制|QP+CoP|依赖多局部通道|
|局限|未传感掌部|漏检区域不可设零|

**决策索引**：见`DECISION_RECORD.md`中D45-07；来源见`SOURCE_REGISTER.md`。

## 08｜跨论文比较：数据、数学、实验与迁移

三类方法解决的问题不同：VPEC/ViPER用图像估计压力分布，FEATS用GelSight形变+FEA预测局部法/切力，ImplicitRDP/RDP学习接触策略，Marra依赖多指局部传感器和QP控制。不能因某篇策略论文有“force”一词就让它替代压力分布真值论文。VPEC官方仓库确认约170GB数据、旧PyTorch1.12环境；FEATS标签来自FEA而非独立空间物理压力；ViPER官方README明确强监督train/test有压力真值，但根许可证仍缺；ImplicitRDP有mini数据却含pickle与实机runner。独立FR5适配还要核对Eye-in-Hand D435视角、KWR75D外置时钟、Ubuntu22.04/ROS2 Humble、厂家控制接口及真正有限宽边缘变形。

**对应公式 F45-08**：

$$
transfer\_gate=math\land data\land license\land interface\land truth
$$

**对应表 T45-08**：

|方法族|主要可观测量|主要缺口|
|---|---|---|
|VPEC/ViPER|RGB→压力图|合法数据/几何域迁移|
|FEATS|GelSight→FEA分布|独立物理真值|
|ImplicitRDP|RGB+全局wrench→动作|实机耦合/无局部压力|

**决策索引**：见`DECISION_RECORD.md`中D45-08；来源见`SOURCE_REGISTER.md`。

## 09｜官方仓库固定版本与源码调用链

本轮经公开GitHub只读接口取得作者commit `112d6857d914c5dee4c4c172790088adc6bdc3a5`（2026-08-02），逐项记录Git blob SHA。`eval.sh`第39行启用`eval_real_robot_flexiv.py`；后者第86行`env_runner.run(policy)`。配置把runner指定为`RealStableReactiveRunner`，其构造阶段创建`RealRobotEnvironment`并在第137行调用`send_gripper_command_direct`。`control.py`第59行构造`BimanualFlexivServer`，第63行启动线程；服务器源码初始化可调用`SwitchMode`。所以作者README中“inference”流程属于实机评估入口，不能在本项目离线环境执行。只读源码审计不等于已下载并测试官方运行路径。

**对应公式 F45-09**：

$$
eval.sh\to eval\_real\_robot\_flexiv.py\to env\_runner.run(policy)
$$

**对应表 T45-09**：

|源码节点|审计行|结论|
|---|---|---|
|eval.sh|39|调用实机eval|
|eval_real_robot_flexiv.py|86|runner.run|
|runner|135/137|构造真实环境/发夹爪|

**决策索引**：见`DECISION_RECORD.md`中D45-09；来源见`SOURCE_REGISTER.md`。

## 10｜反序列化、许可证与数据可信边界

作者`eval_real_robot_flexiv.py`第49行使用`torch.load(open(ckpt_path,rb),pickle_module=dill)`；Python官方明确警告pickle可在反序列化时执行代码。HF mini数据页面列出pickle引用作者自定义`SensorMessage`/`SensorMessageList`等类。**这只是安全风险审计，绝不指控作者文件恶意**。作者根LICENSE同时有MIT和CreativeML Open RAIL-M，HF数据/模型卡标MIT不自动覆盖第三方依赖。哈希只能证明拿到的文件与指定版本一致，不会让pickle变成无害格式。最低可行措施是固定完整repo revision、逐文件许可、检查文件格式、在隔离且无设备/无网络/无密钥环境完成数据转换后才考虑加载；当前不下载、不执行。

**对应公式 F45-10**：

$$
SHA256(file)=h\not\Rightarrow safe\_to\_unpickle(file)
$$

**对应表 T45-10**：

|证据|可以证明|不能证明|
|---|---|---|
|SHA|文件一致性|反序列化安全|
|HF MIT标签|卡片许可声明|全依赖权利|
|pickle类扫描|文件含对象引用|文件恶意|

**决策索引**：见`DECISION_RECORD.md`中D45-10；来源见`SOURCE_REGISTER.md`。

## 11｜模型、数据大小与离线forward最低门槛

HF作者数据总目录约3.77GB，mini约182MB（flip 126MB、toggle 56.2MB）；作者模型目录约27.7GB、包含多种策略/消融。页面可见数据完整revision `3dced28f0d789245730255c4ba3a9f4767c784be`，模型页面短revision `24e410d`，但本轮未核完整模型revision与单checkpoint SHA。训练配置明确RGB [3,360,640]、观测TCP pose [9]、wrench [6]、数据动作[19]，模型动作[13]，`control_fps=10`与`latency_step=2`；不能只靠“GPU有双3090”保证可以运行。真正的离线forward必须只接收已验证的张量，不实例化ROS2节点、Flexiv server或runner，不执行动作。

**对应公式 F45-11**：

$$
offline\_eligible=revision\land no\_robot\_imports\land safe\_load\land shape\_contract
$$

**对应表 T45-11**：

|公开对象|规模/形状|状态|
|---|---|---|
|HF mini|182MB|仅网页审计|
|HF模型目录|27.7GB|未下载|
|动作|19原始/13模型|转换链未复现|

**决策索引**：见`DECISION_RECORD.md`中D45-11；来源见`SOURCE_REGISTER.md`。

## 12｜坐标、单位、法兰与真实柔性接触线

本项目必须区分法兰坐标F、标定刚性TCP坐标、受力变形的真实接触线。腕部F/T读数需先做偏置、重力、动态核和正确的wrench伴随变换；平移会改变力矩，旋转会改变力与力矩分量。RGB-D曲面法向与F/T坐标必须在同一时刻、同一坐标系比较。论文VRR把TCP力变换到世界坐标构造虚拟目标，但这个操作不能自动确定柔性刮板左边缘、右边缘、局部压力峰。单位固定m、N、N·m、N/m、ns；KWR75D采样与D435帧同步没有实测，所有未校准字段必须UNKNOWN。

**对应公式 F45-12**：

$$
{}^{B}T_{contact}={}^BT_F\,{}^FT_{TCP}\,{}^{TCP}T_{contact}(f,\kappa)
$$

**对应表 T45-12**：

|对象|所需标定|当前状态|
|---|---|---|
|法兰→TCP|刚性SE(3)|未实测|
|TCP→接触线|随力弯曲|UNKNOWN|
|F/T→世界|wrench变换+时间|未实测|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 13｜信息论证明：VRR不增加局部压力观测

设真实三箱压力P=(pL,pM,pR)，腕部观测W=(F,q)，已知位姿X和按全局力计算的刚度K。论文VRR是V=g(W,X)=X+K(W)^{-1}f(W)。因此给定W和X时，V没有额外随机性，满足Markov链P→W→V（条件于X），由数据处理不等式有I(P;V|X)≤I(P;W|X)。若V只使用合力而丢掉力矩，信息可能更少；若给定X、K可逆，V与力一一对应，信息相同但不会更多。这个证明不否认VRR作为训练正则有收益，只否定“VRR是独立空间压力传感器”。若K来自真正独立的局部压力测量，条件关系改变，必须重新建模。

**对应公式 F45-13**：

$$
I(P;V\mid X)\le I(P;W\mid X),\quad V=g(W,X)
$$

**对应表 T45-13**：

|量|来自哪里|独立空间信息？|
|---|---|---|
|W|腕部六维F/T|只有合力/一阶矩|
|V|W和X确定性变换|否|
|pM真值|独立分段力台|是（条件性）|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 14｜零空间反例：同一wrench、不同右边缘压力

取a=0.02m、c=0、p0=(1,4,5)N，p(2)=(3,0,7)N。两者F=10N，q=a(pR−pL)=0.08N·m完全相同，但右侧压力从5N变成7N。H矩阵为[[1,1,1],[-a,0,a]]，向量(1,-2,1)在其零空间：总力增量1−2+1=0，一阶矩增量−a+a=0。VRR即使再接一个任意正定K(W)，对两种压力的输入仍相同。右侧比例区间不是单点，而是[0.4,0.7]；若纯力偶c未知、界C>0，区间一般更宽。该反例是本轮新数学判据，并不是作者论文的实验复现。

**对应公式 F45-14**：

$$
p(t)=p_0+t(1,-2,1),\quad Hp(t)=Hp_0
$$

**对应表 T45-14**：

|人工压力|F (N)|q (N·m)|
|---|---|---|
|(1,4,5)|10|0.08|
|(3,0,7)|10|0.08|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 15｜需要怎样的真正独立局部测量

若纯接触力偶c独立已知，增加一个**真正独立**的中箱载荷pM测量，矩阵从2×3秩2增为3×3秩3：pL=(F−pM−(q−c)/a)/2，pR=(F−pM+(q−c)/a)/2。这是理想条件下唯一恢复三箱压力的最小附加标量观测之一。若c仍未知，则未知量是(pL,pM,pR,c)四个，新增pM后仅3条独立方程，秩3<4，不能唯一恢复；需要独立c证书或进一步空间测量。D435的视觉阴性掩码并不等于pM力台。Marra论文的每指独立触觉校准提供了工程方向，但不能视作本刮板已安装传感器。

**对应公式 F45-15**：

$$
rank(H)=2,\quad rank([H;e_M^T])=3,\quad e_M=(0,1,0)
$$

**对应表 T45-15**：

|条件|方程/未知数|能否唯一|
|---|---|---|
|F,q|2/3|不能|
|F,q,pM且c已知|3/3|理想可|
|F,q,pM但c未知|3/4|不能|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 16｜可执行算法、严格类型与独立LP oracle

新增`vrr_information_gate.py`实现严格非负三箱压力、m/N/Nm合同、零空间变换、标量VRR示例、固定F下右侧比例解析区间与独立SciPy HiGHS LP oracle。定义d=(q−c)/(aF)，当|c|≤C时d_lo=max(-1,(q−C)/(aF))，d_hi=min(1,(q+C)/(aF))，可行右比例区间如公式所示。另实现中箱独立观测+已知c的解析恢复及rank审计。非法NaN/Inf/负压力/非正宽度/不可行wrench会拒绝。`static_entrypoint_gate.py`只做Python AST解析和Git blob SHA校验，绝不import、exec、加载pickle或调用机器人。它的静态检查不是完备安全证明，动态导入/反射仍可能漏报，输出永远HOLD。

**对应公式 F45-16**：

$$
r_{min}=\max(0,d_{lo}),\quad r_{max}=(1+d_{hi})/2
$$

**对应表 T45-16**：

|实现|独立验证|安全状态|
|---|---|---|
|解析区间|HiGHS LP|HOLD|
|零空间|两组同wrench|HOLD|
|AST审计|合成恶意入口样例|HOLD|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 17｜同信息公平消融与可复现数字

固定种子450045生成256组人工非负三箱压力、半宽a、纯力偶c及其上界C，先生成真值再构造F/q。强Baseline是仅用同一F/q/a/C的wrench-only解析区间；“VRR增强”只附加由同一F和已知X/K推得的V，不允许把V当独立带噪传感器。256/256两者区间完全相同，真值均覆盖256/256，平均宽度同为0.4259059245，最大解析–独立LP端点误差2.22e-16。额外独立中箱真值+已知c在256/256人工案例精确恢复，**这是多一个传感器信息的不同实验条件，不能用来宣称算法战胜同信息baseline**。本轮新增66项pytest、历史369项，共435项通过。

**对应公式 F45-17**：

$$
\Delta width=width(W+V)-width(W)=0
$$

**对应表 T45-17**：

|人工方法|覆盖/256|平均区间宽度|
|---|---|---|
|Wrench-only|256|0.4259059245|
|Wrench+VRR|256|0.4259059245|
|独立pM且c已知|256精确恢复|额外信息，不可直接比宽度|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 18｜论文候选重新排序与唯一创新选择

R44把ImplicitRDP因有代码/数据/模型升到第2；R45在固定commit与全文核读后发现作者官方eval并非无运动forward，且VRR不提供独立空间压力真值，因此将其审计优先级从第2降到第3，FEATS从第3升第2。VPEC仍第1但170GB数据与旧PyTorch环境增加复现成本；ViPER有真实压力标签设计但许可未明；RDP仍第5。排序是下一步离线审计资源分配，不是论文性能榜。唯一首选科学问题I-A继续为“有限宽柔性刮板局部载荷部分可辨识+独立真值+可信失败拒绝”；唯一备选I-B为接触换向的延迟鲁棒WORK/UNLOAD/OFFSURFACE_RETURN。任何赢家仍待定，退出条件是找不到合法空间真值或同信息收益。

**对应公式 F45-18**：

$$
priority_{R45}=VPEC\succ FEATS\succ ImplicitRDP\succ ViPER\succ RDP
$$

**对应表 T45-18**：

|论文候选|R44→R45|主要理由|
|---|---|---|
|VPEC|1→1|空间压力真值潜力|
|FEATS|3→2|离线标签审计优先|
|ImplicitRDP|2→3|实机耦合/VRR无新局部信息|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 19｜三入口、统一路径合同与硬件安全Gate

M1人工有序3D点与指定路线，M2背部实体画线识别、起终方向确认及有序三维曲线，M3解剖候选经专家审核、可编辑关键点图、两点间路线或已审核方式模板，全部保留。三入口输出统一位置、切向、曲面法向、置信度/协方差、工具姿态、有限宽真实扫掠资格和WORK/UNLOAD/OFFSURFACE_RETURN模式；接触前用户审核。当前没有D435、KWR75D、FR5实机，故所有硬件Gate为HOLD：G0手眼/工具边缘标定；G1传感器偏置/重力/动态核和FT_Control真实时序；G2未知纯力偶/摩擦/变形界；G3独立非人体三箱力台；G4卸载/返程与强基线；G5真人风险伦理批准。离面返程是当前设计默认，贴面轻力返程另审。

**对应公式 F45-19**：

$$
path_i=(x_i,t_i,n_i,\Sigma_i,R_i,w_i,mode_i,review_i)
$$

**对应表 T45-19**：

|入口|审核动作|统一输出|
|---|---|---|
|M1|人工点序|有序3D曲线|
|M2|画线方向确认|有序3D曲线|
|M3|专家候选/模板审核|有序3D曲线|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 20｜验收、失败清单与下一轮自适应计划

本轮完成R44真实文件校验、2篇全文方法/实验/局限核读、12个去重来源组、固定作者源码commit与9项代码/配置接口、独立VRR信息论反证、三箱零空间和秩证明、256例固定种子人工消融、66项新增测试与369项历史回归。失败有两处：R44首次递归收集重名模块，已限定范围；消融首次NumPy int64 JSON序列化失败，已转Python int后重跑。没有作者代码执行、没有模型权重加载、没有数据下载、没有机器人/仿真/人体试验。下轮按证据自适应：优先找VPEC/ViPER合法小压力真值或设计非人体分段力台；只有能安全隔离才考虑ImplicitRDP纯张量forward。当前报告/教学/矩阵/源码/日志均独立归档，任何新证据可撤销排序。

**对应公式 F45-20**：

$$
acceptance=archive\_verified\land fulltext\land proof\land tests\land paired\_docs
$$

**对应表 T45-20**：

|验收对象|实际结果|未完成|
|---|---|---|
|研究测试|435/435|实体安全|
|作者源码|固定commit只读|纯forward未运行|
|物理真值|无|G0–G5 HOLD|

**决策索引**：见`DECISION_RECORD.md`中D45-12；来源见`SOURCE_REGISTER.md`。

## 附录A｜本轮完整来源登记与实际阅读层级

# AUTO-R45 来源登记（2026-10-21，去重，实际访问与阅读级别）

**A**：本轮确实打开全文，阅读方法/实验/局限；**B**：本轮打开官方代码/配置/数据/许可或项目页，不能称论文全文；**P**：摘要/正文片段；**H**：仅历史。以下来源ID均在本轮实际核验，不把作者结果当本项目复现。GitHub为公开作者仓库**只读**；没有读取或写入用户旧仓库。

|ID|来源、日期、DOI或固定版本、入口|级别|核读内容、限制|
|---|---|---|---|
|S45-01|Chen等，*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*，arXiv:2512.10946v2，2026-07-21，DOI 10.48550/arXiv.2512.10946；https://arxiv.org/pdf/2512.10946|**A全文8页**|III SSL因果GRU/注意力/DDIM缓存、VRR公式(3–6)、IV 3任务各20次、表I–IV、Flexiv/RDK/10Hz/4090/时延。作者称RA-L 2026 accepted June，**正式期刊DOI未核**。|
|S45-02|Marra, Caddeo, Pattacini, Natale，*Multifingered force-aware control for humanoid robots*，arXiv:2603.08142v2，2026-05-28；https://arxiv.org/html/2603.08142|**A全文**|III-A ATI Nano17独立校准+每指20k训练对、III-B QP、III-C CoP、IV 15次×对象/频率消融、V 触觉滞后/未传感掌部局限；未查正式DOI。|
|S45-03|Xue等，*Reactive Diffusion Policy*，RSS 2025，DOI 10.15607/RSS.2025.XXI.052；https://roboticsproceedings.org/rss21/p052.html|**B官方题录/摘要**|两层慢快视觉触觉，TactAR；本轮未核读全文、未运行代码。|
|S45-04|He, Färber, Calandra，*RCT*，arXiv:2606.31694，2026-06-30；https://arxiv.org/abs/2606.31694|**P摘要**|完整压痕序列/材料留出反证，非刮板三箱压力真值；本轮未下载。|
|S45-05|ImplicitRDP作者GitHub根，固定commit `112d6857d914c5dee4c4c172790088adc6bdc3a5`（2026-08-02）；https://github.com/Chen-Wendi/ImplicitRDP/tree/112d6857d914c5dee4c4c172790088adc6bdc3a5|**B官方源码只读**|通过GitHub公开API读取commit、树、9个具体源码/配置/许可；没有执行作者代码或本地复制完整源码。|
|S45-06|同commit `eval_real_robot_flexiv.py`，Git blob `c3763796b55f8a20b224e5b3da63e2a64abaec9d`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/eval_real_robot_flexiv.py|**B源码逐行**|L49 `torch.load(...,pickle_module=dill)`，L86 `env_runner.run(policy)`；不等于安全离线forward。|
|S45-07|同commit `control.py`，blob `cdc8f09e2f707e024a0a780f66ffad985040b82e`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/control.py|**B源码逐行**|L59 `BimanualFlexivServer`、L63启动server thread、ROS2 publisher；控制入口绝不运行。|
|S45-08|同commit `real_stable_reactive_runner.py`，blob `8616d30253955f6d33ab616228ed1c727e064f7e`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/ImplicitRDP/env_runner/real_stable_reactive_runner.py|**B源码接口**|L134创建ROS2节点、L135创建真实环境、L137初始化即发夹爪指令；仅静态审计相关调用位置，**未逐行阅读827行全部**。|
|S45-09|同commit `bimanual_flexiv_server.py`，blob `a6a3e5c68766a7a71637719b62c3d9be7e0f4014`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/ImplicitRDP/real_world/robot/bimanual_flexiv_server.py|**B源码接口**|L49/L57调用Flexiv模式切换，不能移植为FR5默认可用接口。|
|S45-10|同commit `real_flip_image_wrench_implicitrdp_10fps.yaml`，blob `b596e5e21a61b4b54b395d2621356cf528d03af2`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/ImplicitRDP/config/task/real_flip_image_wrench_implicitrdp_10fps.yaml|**B配置全文**|RGB [3,360,640]、TCP pose 9/模型6、wrench6、动作19/模型13、10Hz、latency_step2、Flexiv真实runner；与FR5路径合同不直接相同。|
|S45-11|同commit `requirements.txt`，blob `c7d5d07a00e00e7d173d3d9ac421ced221985df0`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/requirements.txt|**B依赖全文**|numpy==1.26.4、diffusers==0.33.1、flexivrdk==1.7.0、dill等；未安装作者环境。|
|S45-12|同commit LICENSE，blob `b268080a2b0cddcf5a4ace78fb7e863f9eb0c0af`；https://github.com/Chen-Wendi/ImplicitRDP/blob/112d6857d914c5dee4c4c172790088adc6bdc3a5/LICENSE|**B许可证全文**|多段MIT并包含CreativeML Open RAIL-M；需逐文件/模型权利核验，不笼统声称整套MIT。|
|S45-13|ImplicitRDP HF mini数据固定repo revision `3dced28f0d789245730255c4ba3a9f4767c784be`；https://huggingface.co/datasets/WendiChen/ImplicitRDP_dataset/tree/main/dataset_mini|**B官方数据目录**|mini约182MB；flip 126MB SHA256 `d7f5dcc4...`、toggle 56.2MB SHA256 `861c22f4...`；HF标识pickle引用自定义类，**未下载/解包/反序列化**。|
|S45-14|ImplicitRDP HF预训练权重目录，页面revision短 `24e410d`；https://huggingface.co/WendiChen/ImplicitRDP_model/tree/main|**B官方模型目录**|整仓约27.7GB，多任务多变体；未取得完整revision、单权重hash或安全格式验证；未下载。|
|S45-15|Python官方pickle安全警告；https://docs.python.org/3/library/pickle.html|**B官方安全文档**|反序列化不可信pickle可执行任意代码；此为静态风险分类，不声称作者文件恶意。|
|S45-16|VPEC作者代码；https://github.com/Healthcare-Robotics/VPEC|**B官方仓库**|MIT、Ubuntu/Python3.10/PyTorch1.12、数据约170GB、原任务软夹爪独立压力真值；**未下载170GB**。|
|S45-17|ViPER作者代码；https://github.com/Healthcare-Robotics/ViPER|**B官方仓库**|训练`train/test`含压力真值与F/T、`weak_train`仅F/T；Ubuntu20.04；根目录仍未见LICENSE；数据OneDrive未下载。|
|S45-18|FEATS作者项目；https://feats-ai.github.io/|**B作者项目**|FEA派生法/切向分布标签，demo外部六维合力真值不等于每像素独立压力真值；未复现。|
|S45-19|FAIRINO官方Python SDK；https://github.com/FAIR-INNOVATION/fairino-python-sdk|**B官方仓库**|Apache-2.0，SDK实际FR5 v2.3.0/robot4.0.0接口时序未核；不宣称厂家FT_Control已可用。|

**本轮去重实际访问19项，A全文2项，B16项，P1项。** S45-05至12为同一仓库的不同源码对象，按“独立来源”严格合并为**12个来源组**（2篇全文+RSS+RCT+ImplicitRDP仓库+HF数据+HF模型+Python文档+VPEC+ViPER+FEATS+FAIRINO）。不把多个GitHub文件虚算成多篇论文。S45-13/14 HF页面短revision和blob校验是可审计定位信息，**不是已下载校验通过**。


## 附录B｜五篇主论文竞争与创新登记

# AUTO-R45｜BASELINE_AND_INNOVATION_REGISTER｜跨轮可撤销竞争矩阵

**主论文唯一赢家：待定。** 顺序为“下一步合法离线可复现审计投入优先级”，不是论文性能/FR5适配排名。R44：VPEC→ImplicitRDP→FEATS→ViPER→RDP；**R45：VPEC→FEATS→ImplicitRDP→ViPER→RDP**。ImplicitRDP因官方推理入口绑定实机runner、dill反序列化和无局部压力真值由第2降至第3；FEATS升第2只表示可优先审计独立物理真值缺口，**不是已经复现**。

|顺序/变化|作者/题名/年份/DOI|全文、源码、数据、许可、版本与缺口|数学/FR5接口/2025–26反证/独立真值|判断、最小补证|
|---|---|---|---|---|
|**1 = VPEC**|Patrick Grady, Jeremy A. Collins, Samarth Brahmbhatt, Christopher Twigg, Chengcheng Tang, James Hays, Charles C. Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*，IROS2022，DOI **10.1109/IROS47612.2022.9982073**|本轮**B官方仓库**MIT、Python3.10/PyTorch1.12；作者声明约**170GB**数据及模型下载脚本；未下载/未固定commit/未跑网络；论文全文本轮**未读**|RGB单帧预测软爪压力图、作者有压力真值传感器；软爪与有限宽柔性刮板、D435 Eye-in-Hand视角不同；FR5无需底层关节力矩但控制频率、可见性、外部物理真值需核；2025 FEATS、2026 Marra对压力估计新颖性构成反证|**条件保留**；优先确认可合法获取的小规模真实压力标注与episode split、权重版本、数据最小子集，170GB全量暂缓。|
|**2 ↑ FEATS**|Erik Helmut, Luca Dziarski, Niklas Funk, Boris Belousov, Jan Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*，IROS2025，DOI **10.1109/IROS60139.2025.11246486**，arXiv2411.03315|本轮**B作者项目**；R44审计HF数据约3.98GB、MIT标签、短commit `7fd3027`，本轮未重新打开数据目录/未下载；代码完整许可证/权重固定版本**U**|U-Net用凝胶形变与FEA派生法/切向压力分布；D435不是GelSight Mini，标签不是独立逐箱力台；无需假设FR5关节力矩，但传感器几何不可直接迁移；作者demo 25Hz不是FR5性能|**离线数据审计优先上升，硬件迁移暂缓**；先核一小样本物理独立压力参考是否存在，否则FEA仅模拟监督。|
|**3 ↓ ImplicitRDP**|Wendi Chen, Han Xue, Yi Wang, Fangyuan Zhou, Jun Lv, Yang Jin, Shirun Tang, Chuan Wen, Cewu Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*，arXiv2025 v2 2026-07-21 DOI **10.48550/arXiv.2512.10946**，作者标RA-L2026 accepted；正式期刊DOI**U**|本轮**A全文8页+B固定commit `112d6857...`+源码9项**；MIT与RAIL-M混合；HF mini 182MB但pickle、模型目录27.7GB；未下载、未反序列化、未运行作者代码；完整模型revision/hash**U**|VRR `x_vt=x_real+K^-1f`为同一全局wrench确定性变换，不增加局部压力可辨识秩；训练/实验Flexiv Rizon4s、10Hz、相机10Hz、ROS2同步<120ms、4090推理<30ms，不能转移到FR5双3090；配置19/13维动作、Flexiv stiffness示教、`env_runner`初始化可发送夹爪命令；非人体逐箱真值**无**|**只保留条件性“无运动策略baseline”**；**官方eval入口不可直接运行**，须隔离无机器人依赖的纯张量forward、独立检查checkpoint、校验revision、证明所有硬件sink不可达。不得把VRR当压力分布创新。|
|**4 = ViPER**|Jeremy A. Collins, Cody Houff, Patrick Grady, Charles C. Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*，IROS2023 DOI **10.1109/IROS55552.2023.10342124**|本轮**B官方仓库**，训练/测试明确有压力+F/T标签，弱训练仅F/T；OneDrive模型/数据未下载，根目录无LICENSE、版本/数据权利**U**|Eye-in-Hand RGB与本项目视角相近但软爪形变不同；Ubuntu20.04、Sensel驱动/ROS迁移、FR5外环时序**U**；作者压力真值可能有价值但本轮无法独立核样本|**暂缓复制代码，保留学术候选**；需作者授权、最小压力标注数据、明确episode/材料拆分。|
|**5 = RDP**|Han Xue, Jieji Ren, Wendi Chen, Gu Zhang, Fang Yuan, Guoying Gu, Huazhe Xu, Cewu Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*，RSS2025 DOI **10.15607/RSS.2025.XXI.052**|本轮**B官方RSS题录**；论文全文本轮未读，作者代码/许可/版本/权重**U**|TactAR与慢快层次、训练动作和底层接口不同；不等同FR5外部六维F/T和可用伺服；无刮板空间压力独立真值；被2026 ImplicitRDP作者直接对照但作者20次/任务不可视为本项目复现|**暂缓**；需合法代码+非运动推理+可迁移的外环时序证据。|

**厂家/简单工程强baseline**：人工审核的M1/M2/M3三维曲线、已标定TCP/刮板边缘、厂家FT_Control经过真实SDK/固件/失效时序核验后的低能量单向恒力WORK、平滑UNLOAD、离面OFFSURFACE_RETURN；独立动态参考力台与监测。**目前仅方案、没有实机，硬件适配器禁用。** 数学强baseline：同一F、q、a、C的wrench-only解析/LP区间；VRR派生特征不算新增测量。

**I-A唯一首选（条件性）**：有限宽刮板真实接触线/局部载荷**部分可辨识区间** + 独立分段物理真值 + 可信拒绝。R45新修订：不把由全局F/T确定性变换得到的VRR或伪标签当作额外空间压力证据；必须有独立局部通道且纯接触力偶c有独立认证。假设：法兰/TCP/柔性边缘/法向/时间/动态核可信；单次视觉阴性预算k≤1有独立证据；所有失败回退HOLD。若无独立真值或同信息强baseline无增益，则退出主创新。

**I-B唯一备选**：WORK→UNLOAD→OFFSURFACE_RETURN的延迟鲁棒双向柔顺控制，待厂家FT_Control时序/接触换向证据；不堆EKF/ESO/MPC、不假定关节力矩伺服。**主创新尚未锁定**。

**FR5 Gate**：G0相机/手眼/刮板真实边缘/TCP；G1 KWR75D偏置/重力/动态核、采样时间、SDK v2.3.0/robot v4.0.0 FT_Control真实时序；G2纯力偶c与摩擦/变形上界；G3独立三箱或连续压力线非人体物理真值、episode/材料/曲率/遮挡盲检；G4同信息对照/独立停止卸载；G5真人风险伦理批准。全部HOLD。


## 附录C｜12项跨轮决策与撤销条件

# AUTO-R45｜12项可撤销研究决策（R44→R45）

|ID|本轮决定、相对R44差分|证据与撤销条件|
|---|---|---|
|D45-01|R44 ZIP真实核验45成员/44内部SHA/CRC/外部SHA；重新执行R39–R44 **369项**，R45新增**66项**；总435|R44首次递归收集出2个错误，改为顶层文件后124/124通过；失败日志留存；不冒充R30–R38测试。|
|D45-02|固定ImplicitRDP官方源码commit `112d6857...`，R44未固定→本轮固定；9处源码接口只读审计|Git blob SHA及行号见CODE_AUDIT_MANIFEST；commit或代码变化需重新审计。|
|D45-03|官方`eval.sh`实际进入`eval_real_robot_flexiv.py`，调用`env_runner.run`，**不是纯离线forward**|源码L39、L86、runner L135/137；只有完全隔离硬件依赖、证明无sink才可撤销HOLD。|
|D45-04|`torch.load(...pickle_module=dill)`及HF mini pkl触发**不可信反序列化审查**；未声称文件恶意|源码L49、Python官方pickle警告、HF公开文件扫描；独立来源签名/沙箱/安全格式转换前不反序列化。|
|D45-05|核实HF mini约182MB及模型目录27.7GB，数据与权重分开；**未下载**|HF repo revision短/完整记录见SOURCE_REGISTER；需单权重SHA/许可证和安全格式证明。|
|D45-06|ImplicitRDP论文v2全文8页核读：SSL/VRR/实验；作者3任务各20次成功率18/20、18/20、8/20，非FR5|若原论文更正或公开复现证据改变，可修订；不将作者指标视作本项目实测。|
|D45-07|新数学证明：VRR `xvt=x+K^-1 f`为已知全局F/T的确定性派生，不增加三箱压力可辨识秩|条件是K/x不含额外独立局部测量；若K由真实局部压力独立测得则结论边界需重审。|
|D45-08|独立三箱中心载荷测量+已知c使秩2→3；若c仍未知，3条方程/4未知仍不可唯一反演|解析线性代数、256组合成oracle与LP；物理设备与真值尚无。|
|D45-09|固定种子256例：wrench-only与VRR派生特征同信息强baseline区间完全一致，解析-LP误差≤2.3e-16；不宣称精度提升|R45_ABLATION.json；人工真值不是FR5/物理测量。|
|D45-10|主论文审计顺序VPEC→FEATS→ImplicitRDP→ViPER→RDP，ImplicitRDP **2→3**，FEATS **3→2**；赢家待定|原因是实机耦合/反序列化与无空间压力真值；纯离线安全forward+独立数据可重新提升。|
|D45-11|唯一I-A首选继续但收缩：独立局部空间观测才是信息增益；备选I-B不变|无物理逐箱真值或同信息强基线收益则退出；2026 Marra独立校准+CoP已构成相邻新颖性反证。|
|D45-12|M1/M2/M3、用户审核、有限宽扫掠、WORK/UNLOAD/OFFSURFACE_RETURN全保留；硬件/人体/GitHub写入全部HOLD|仅独立授权+厂家SDK时序+非人体风险/伦理证据允许重审；本轮所有源码HOLD_NO_MOTION。|


## 附录D｜真实失败/阻塞与最小补证

# AUTO-R45｜失败、负结果、阻塞和最小补证

1. **可复现负结果**：VRR虚拟目标从全局F/T、姿态与预设刚度计算，不增加局部三箱压力可辨识秩。256例合成强基线和VRR派生特征区间一致；不能将作者学习表现提升解读成新增压力传感。
2. **真正工程阻塞**：官方ImplicitRDP `eval.sh`直接调用Flexiv实机runner；runner初始化可发送夹爪命令。不能运行该脚本以“试试看是否离线”。
3. **数据安全阻塞**：`torch.load`使用`dill`，HF mini原始pkl含作者自定义类；本轮未下载/反序列化；SHA/许可证本身不能保证pickle安全。
4. **许可证缺口**：作者根LICENSE含MIT与RAIL-M，HF模型卡MIT不等于全部依赖与权重法律边界已审；逐文件与模型用途需确认。
5. **R44测试初次失败**：对R44归档递归运行`pytest .`把historical子目录中的同名测试模块重复收集，2项收集错误，保留`R45_HISTORICAL_RESULTS_INITIAL.json`及首次stdout（后者本轮仅保留错误摘要于报告）；改为指定R44本轮两份测试文件后124/124通过，R39–R44合计369通过。**不得把失败运行计为成功**。
6. **R45消融首次失败**：NumPy `int64`计数不能直接`json.dumps`；已明确转换Python `int`并重跑，66/66测试与256例消融成功。首次失败命令输出保留在本轮研究报告；未伪称首次成功。
7. **外部来源实际访问限制**：本轮ImpIicitRDP论文8页PDF全文与Marra论文HTML全文确实核读；RDP只官方摘要、RCT只摘要、其余仓库只相关源码/许可。没有运行任何作者网络、权重、训练或实验。
8. **独立物理真值缺失**：无非人体三箱力台/空间压力线、真实纯力偶c、柔性边缘标定、KWR75D动态核、D435标定和FR5控制器。解析秩3只代表理想已知c且独立中箱真值的数学可辨识，不等于实测可达。
9. **论文控制迁移阻塞**：作者Flexiv内置关节扭矩示教、10Hz外环、4090推理<30ms、ROS2同步<120ms、位置误差约0.1mm，均不得当成FR5/双3090实测。作者箱翻转14N失败阈值不是人体或FR5安全阈值。
10. **下一轮最小解法**：优先只读拿到VPEC/ViPER合法小规模压力真值清单或设计非人体分段力台BOM、时间对齐与盲检协议；并在无网络/无硬件沙箱中写**全新**纯张量forward接口（非官方eval），不加载pickle直到来源验证；继续不授权运动。


## 附录E｜非人体物理真值与三入口Gate

# AUTO-R45｜独立物理真值与工程Gate清单（尚未执行）

|门|需要的真实证据/字段|最小入门判据|当前状态|
|---|---|---|---|
|G0|D435 RGB/深度内外参、手眼SE(3)、法兰位姿、刚性TCP、柔性刮板接触中心/边缘随力变形|独立几何靶标、离面点云、刮板两边缘误差分布与时间戳可审计|HOLD，无设备|
|G1|KWR75D原始六维F/T、偏置/重力/温漂/动态核、控制器反馈/FT_Control SDK+固件|原始日志+独立动态参考、单位N/Nm、int64 ns、帧名与相位对齐、失败卸载|HOLD|
|G2|独立纯力偶c、切向摩擦、刮板弯曲及未知多点力矩|可覆盖未知纯力偶的实测区间，不允许腕部合力矩单独自证|HOLD|
|G3|非人体透明仿体、左中右独立三通道力台（或连续压力线）、独立总力台|盲测压力真值、完整episode ID、材料/曲率/遮挡分层，校准集与测试集物理独立|HOLD|
|G4|同一工具/轨迹/速度/采样、wrench-only、厂家FT_Control简单基线、创新估计/控制|独立空间压力MAE/覆盖/支持假阴性/拒绝率/时序/卸载，强baseline同信息公平|HOLD|
|G5|人体验证独立风险分析与伦理批准|不得从非人体结果自动推导人体许可|HOLD|

统一路径字段：`source=M1/M2/M3; point_m; tangent_unit; normal_unit; confidence; covariance_m2; flange_SE3; calibrated_TCP_SE3; actual_flexible_contact_line; swept_width_m; sweep_eligible; phase=WORK|UNLOAD|OFFSURFACE_RETURN; reviewed_by_human; clock_id; timestamp_ns`。未获得真实边缘变形证据时`actual_flexible_contact_line`必须标UNKNOWN，不能把TCP冒充接触线。接触前用户审核；任何不可信估计拒绝纠偏并进入经独立验证的卸载设计，不输出实机命令。


## 附录F｜复现与安全边界

本轮代码只依赖Python、NumPy、SciPy、pytest；运行`python -m pytest -q test_vrr_information_gate.py test_static_entrypoint_gate.py`和`python ablation_r45.py`，不会连接机器人。历史测试在`historical/`分别运行。源码`static_entrypoint_gate.py`只解析Python文本，绝不加载作者权重。任何输入与结果状态都为`HOLD_NO_MOTION`。作者源码仅通过公开接口**只读审计**，不包含在本包，不授权执行作者eval。

**禁止误读**：本包不包含FR5控制程序、真人数据、真实仿体压力数据、作者训练权重或作者代码复现。