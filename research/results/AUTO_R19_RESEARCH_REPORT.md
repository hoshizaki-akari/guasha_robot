# AUTO-R19｜FAIRINO FR5刮痧机器人博士级完整研究报告
**题目：厂家FT_Control参数传递的可辨识性缺口、六维力读数新鲜度审计，以及2026遮挡接触/摩擦融合创新性反证**  
**日期：2026-10-20｜证据状态：离线内容验收通过，实体E0–E5未通过。**
> 本轮没有连接机器人、没有发送运动/力控指令、没有真实接触/人体试验。所有人工构造参数与测试均不代表FR5实测。Issue只代表用户报告，论文结果只代表作者设备。

## 01｜上一轮文件复核、回归与可审计差分

本轮不是从聊天摘要推断R18已完成，而是读取挂载的`AUTO_R18_RESEARCH_REPORT.md`、`AUTO_R18_TEACHING_GUIDE.md`、`BASELINE_AND_INNOVATION_REGISTER.md`、`DECISION_RECORD.md`、`QA_R18.json`，并从R18 ZIP提取`src/evaluation_guard.py`、`tests/test_evaluation_guard.py`、执行说明与协议。逐文件路径、大小和SHA256写入`provenance/PRIOR_FILES_CHECKED.json`。在本次运行环境重跑R18的50项测试，50/50通过，原始输出在`results/R18_REGRESSION.txt`。这不是R17或更早全部历史测试的重跑；不能把此前回复累计数充当本轮执行数。R18已经淘汰运动方向投影RMSE作为接触安全主指标，规定EgoPressure UV压力与Sensel原网格同源，增加独立episode统计与FT已知加载响应Gate。

R19的新旧差分：①固定厂家Python SDK V4.0.0提交，发现`FT_Control`传给控制器的参数列表里`B[1]`未出现；②同一SDK的`FT_GetForceTorqueOrigin/RCS`在代码路径中直接返回`0`及内存状态，不足以证明状态新鲜；③公开的2026年FT_Control失效/FR5 ros2_control启动故障报告要求把厂家强Baseline从“可直接使用”改为“必须先完成独立安全验收”；④核读两篇2026全文，普通遮挡下接触定位和静摩擦残差融合的原创性门槛提高；⑤EgoPressure工具源码和数据字典完成版本锁定，但真实Parquet仍未取得。新发现有可复现的离线AST检查和失败对照，故达到本轮离线内容验收；物理Gate没有提升。

**F19-01（本章判据）**

$$
G_{19}=G_{R18\_read}\land G_{SDK\_source}\land G_{AST\_counterexample}\land G_{tests}\land G_{falsifiable},\quad G_{physical}=0.
$$

**T19-01（证据/决策表）**

|维度|R18结论|R19可证实增量|仍缺|
|---|---|---|---|
|SDK|全零个例与时间戳疑点|B第二分量在源码打包中丢失；返回0不证明新鲜|本机SDK/固件、真实接口调用日志|
|论文|视觉力估计已覆盖|2026遮挡iFEA、触觉电流摩擦融合反证|FR5工具接触独立真值|
|代码|R18 50项|R19 103项新增，R18 50项回归|实体系统集成|


## 02｜本轮三项决策问题、假设和退出条件

Q19-A：官方`FT_Control`的`M/B`打包能否区分两个独立阻尼分量？独立判据是固定提交源码AST的参数依赖关系，而非“调用返回0”。Q19-B：F/T软件读数与厂家力控何时有资格进入工程强Baseline？判据是版本匹配、独立采集时间、已知非零外载、原始/RCS坐标一致性、异常停机链路与无接触阶段的系统验证。Q19-C：2026最新论文是否已覆盖视觉遮挡下接触定位和摩擦状态融合，迫使博士唯一创新进一步收窄？判据是全文数学假设、独立真值和迁移接口。

可证伪H19-A：若Python调用接口应传入两个可独立设置的阻尼参数，则当前SDK的两组输入`B=[0.5,1]`和`B=[0.5,9]`会生成相同的控制器参数列表；反例一旦成立，直接禁止用该接口做双参数调节消融，直到厂家确认语义/修复。H19-B：仅凭`FT_GetForceTorqueRCS`返回0及有限次相同值不能通过读数Gate；需独立参考载荷和采集时刻。H19-C：如果2026论文已经实现相似的遮挡定位或静摩擦补偿，本项目不把这些通用模块列为主原创。所有假设允许被厂家更新版本、独立测量或新论文证据推翻。

**F19-02（本章判据）**

$$
H_{19A}:\ B^{(a)}_0=B^{(b)}_0,\ B^{(a)}_1\ne B^{(b)}_1\Longrightarrow P_{SDK}(B^{(a)})=P_{SDK}(B^{(b)}).
$$

**T19-02（证据/决策表）**

|问题|独立判据|入门Gate|退出或回退|
|---|---|---|---|
|Q-A传参|源码参数依赖+厂家确认|源码版本固定|不做双B参数调节，保持HOLD|
|Q-B读数/控制|独立参考+时钟+安全回路|E0–E3通过|离面无接触，不启用FT_Control|
|Q-C新颖性|全文方法/实验/限制|独立工具接触真值|I-A退回待定或淘汰|


## 03｜来源级别、16项去重来源和获取限制

本轮实际核查16项去重来源。S19-01为厂家源码（固定2026-09-22提交`9541bea...`，Linux/Windows Robot.py blob SHA同为`f831a649...`）；S19-02为厂家3.9.5文档；S19-03/04/05/06/15为公开用户Issue，仅能证明有人报告、不能证明FR5普遍故障；S19-07/08为本轮实际打开并核读方法、实验和局限的两篇**完整HTML论文**；S19-09为R18已经全文核读、本轮重核官方数据定义的EgoPressure；S19-10为作者工具源码六个模块及数据字典，实际检查了`hub/parquet/registration/senselpad/torch_dataset`等；S19-11/12为VPEC/ViPER作者代码和历史全文资格；S19-13为Jiang 2024历史摘要/片段，本轮没有重新核读全文；S19-14为厂家ROS2公开仓库；S19-16为ICRA2026 IEEE摘要，不能冒充全文。

对论文应分别记载“方法描述是否可读”“数据/模型是否可获得”“许可证是否明确”“能否在FR5接口运行”。有网页、仓库、Git SHA并不等于模型已复现；有实验RMSE也不等于刮板边缘真值。两篇2026全文的具体数学与实验见第8、9章，其他来源不伪称精读。官方数据集HuggingFace目录确认发布版本`cea37de`、约1.96TB，但运行环境无法直连下载，原始压力Parquet数量为0。

**F19-03（本章判据）**

$$
E=\{F,C,D,I,A,H\},\qquad F\ne C\ne I\ne \text{FR5实测}.
$$

**T19-03（证据/决策表）**

|来源ID|作者/组织、题名及年份|本轮证据层级|实际核查入口|
|---|---|---|---|
|S19-01|FAIRINO official Python SDK Robot.py|C|https://github.com/FAIR-INNOVATION/fairino-python-sdk/blob/9541beaab0662dc8c267c0b9ba925aaf14858a77/linux/fairino/Robot.py|
|S19-02|FAIRINO official Python Robot Force Control 3.9.5|D|https://documentation.fairino.co.in/SDKManual/PythonRobotForceControl.html|
|S19-03|FT_Control does not work, issue 25, 2026-09-07|I|https://github.com/FAIR-INNOVATION/fairino-python-sdk/issues/25|
|S19-04|Raw F/T Mz duplicate on FR10 controller v3.9.6, issue 23, 2026-08-30|I|https://github.com/FAIR-INNOVATION/fairino-python-sdk/issues/23|
|S19-05|FR5 ROS2 hardware activation crash report, issue 35, 2026-07-17|I|https://github.com/FAIR-INNOVATION/frcobot_ros2/issues/35|
|S19-06|Historical checksum issue 15 and current code comparison|I+C|https://github.com/FAIR-INNOVATION/fairino-python-sdk/issues/15|
|S19-07|Zuo, Yang, Chua (2026) A Model-based Visual Contact Localization and Force Sensing System for Compliant Robotic Grippers|F|https://arxiv.org/html/2605.00307|
|S19-08|Min et al. (2026) Tactile-Proprioceptive Sensor Fusion for Contact Wrench Estimation in Whole-Body Physical Human-Robot Interaction|F|https://arxiv.org/html/2605.28412|
|S19-09|Zhao et al. EgoPressure CVPR 2025|D+H|https://arxiv.org/html/2409.02224|
|S19-10|ETH-SIPLAB EgoPressure toolkit and DATA.md|C+D|https://github.com/eth-siplab/EgoPressure|
|S19-11|Grady et al. VPEC IROS 2022|H+C|https://github.com/Healthcare-Robotics/VPEC|
|S19-12|Collins et al. ViPER IROS 2023|H+C|https://github.com/Healthcare-Robotics/ViPER|
|S19-13|Jiang et al. Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment, 2024|H+A|https://doi.org/10.3390/act13020062|
|S19-14|FAIRINO official ROS2 repository|D+C|https://github.com/FAIR-INNOVATION/frcobot_ros2|
|S19-15|FT zero reading user issue 20|I|https://github.com/FAIR-INNOVATION/fairino-python-sdk/issues/20|
|S19-16|TacTip-based Dynamic Contact Force Estimation with Sequential Tactile Images and Its Applications to Robotic Force Tracking, ICRA 2026|A|https://doi.org/10.1109/ICRA57385.2026.11695909|


## 04｜厂家FT_Control参数打包：可重复的源码审计

对`FAIR-INNOVATION/fairino-python-sdk`固定提交`9541beaab0662dc8c267c0b9ba925aaf14858a77`的`linux/fairino/Robot.py`逐行只读核验，`FT_Control`约8437–8473行接受`M`与`B`两元素列表，并将`[M[0], M[1], B[0], B[0], threshold[0], threshold[1], adjustCoeff[0], adjustCoeff[1]]`传给XML-RPC。`B[1]`未进入这8个参数。Windows版本该文件blob SHA与Linux相同。厂家3.9.5文档12.21/12.22将B描述为阻尼参数且示例使用`B=[15,15]`，因此示例恰好无法暴露第二分量被忽略的问题。

本轮没有修改厂家仓库、没有调用`FT_Control`。我们手工转录**最小相关表达式**为`src/pinned_sdk_excerpt.py`，独立编写`inspect_ft_marshalling`，通过Python AST定位远程调用与八元素列表；若源码不完整、函数或调用多义，直接抛出EvidenceHold。输出在`results/R19_OFFLINE_RESULTS.json`。**代码事实是B[1]未被引用；“控制器实际第二阻尼无法设置”仍需厂家确认底层参数语义**，故把它标为强工程疑点而非已证实实体故障。

**F19-04（本章判据）**

$$
P_{SDK}(M,B)=[M_0,M_1,B_0,B_0,\theta_0,\theta_1,a_0,a_1].
$$

**T19-04（证据/决策表）**

|源码检查|固定提交所见|可证伪点|当前结论|
|---|---|---|---|
|Linux Robot.py|B0、B0|厂家修复后B0、B1|参数二不独立|
|Windows Robot.py|相同blob SHA|检查新分支|跨平台同表达式|
|厂家示例|B=[15,15]|改成不等值并查协议|相等示例掩盖问题|
|本轮AST|能识别B1缺失与修订对照|真正全文件AST未在本地跑|离线最小重现通过|


## 05｜FT读数返回码与新鲜度不是同一证据

固定版本`Robot.py`的`FT_GetForceTorqueOrigin`和`FT_GetForceTorqueRCS`分别直接返回`0`及`self.robot_state_pkg.ft_sensor_raw_data`/`ft_sensor_data`六个内存字段；`GetRobotRealTimeState`同样返回`0,self.robot_state_pkg`。这条代码路径并没有对每次调用附带独立采集时间或新鲜度证书。`RPC.__init__`建立CNDE状态客户端、连接XML-RPC，CNDE解析函数将收到的字段写入同一个状态结构；因此“函数返回0”只能说明**这个Python包装路径给出成功码**，不能推出传感器已更新、坐标正确、已校准或具有独立物理激励。

R12/R13已经指出软件到达时间不能唯一恢复采集时间。本轮新`Feedback`门控明确分离`acquired_s`与`received_s`，要求独立采集时间和独立参考时间对齐、连续帧计数模256、六维数值有限、已知非零载荷激励以及独立参考误差界。满足这些条件最多返回`OFFLINE_DIAGNOSTIC_ELIGIBLE_ONLY`，仍永远不授权机器人接触。未知采集时间时拒绝；仅软件到达时间对齐不算同步成功。

**F19-05（本章判据）**

$$
\mathrm{API\_ret}=0\;\not\Rightarrow\; \mathrm{fresh}(F/T)\land\mathrm{calibrated}(F/T)\land\mathrm{safe}.
$$

**T19-05（证据/决策表）**

|观测|可说明|不能说明|需要的独立证据|
|---|---|---|---|
|API返回0|包装层给出成功码|采集新鲜度/传感器健康|原始帧与采集时钟|
|连续全零|读到六个零|真实无力或传感器失效|已知外载与独立力台|
|frame_cnt递增|部分帧连续性|无跨256圈遗漏|时钟与物理事件|
|RCS与raw不同|坐标/补偿可能不同|某一路必然正确|坐标链/工具载荷标定|


## 06｜历史checksum问题：新版本复核纠正旧报告

2025-09-23公开Issue #15声称旧Python状态接收代码在校验失败时将前三关节位置清零。本轮核对2026-09-22官方V4.0.0源码：相应`checksum == checkdata`分支和`jt_cur_pos[0:3]=0`在`Robot.py`约2790–2890行**全部处于注释块**；新初始化路径明确称CNDE端口20005替代20004，状态解析由`FRCNDEClient`负责。因而不能声称“当前V4.0.0仍在执行旧清零逻辑”。

但这不等于新CNDE链路已经获得安全证明：`_parse_cnde_state_data`按配置字段逐一解码，网络断连可触发重连，未看到调用者每次取力时得到独立的采集时间/有效位。正确结论是**旧Issue作为历史风险，已由新源码部分反证；当前链路仍需独立新鲜度与异常帧实验**。这项纠错避免把历史Bug机械复制为当前事实，也表明研究应随版本更新。

**F19-06（本章判据）**

$$
\mathrm{historical\ issue}\not\Rightarrow\mathrm{active\ code\ in\ commit\ 9541bea}.
$$

**T19-06（证据/决策表）**

|证据|时间/版本|本轮核验|不能外推|
|---|---|---|---|
|Issue #15|2025-09-23|报告旧清零分支|不是当前必然执行|
|V4.0.0源码|2026-09-22|旧分支已注释|不能证明CNDE无问题|
|CNDE新路径|本轮源码|20005端口/字段解析|采集时间未证实|


## 07｜三条公开硬件问题报告的严格风险分级

2026-09-07 Python SDK Issue #25报告某用户使用`FT_Control`未获得预期恒力控制，并称碰撞检测被关闭且与桌面发生严重接触；其中约350N是**用户自述，不是本轮测量或厂家确认**。2026-08-30 Issue #23报告FR10、控制器v3.9.6的`ft_sensor_raw_data[5]`与`[4]`在两次20点读取中完全相等，RCS的Mz却不同；报告明确机器人是FR10，**不是FR5**。2026-07-17 ROS2 Issue #35报告FR5 V6.0、v3.9.7/Humble的ros2_control硬件激活时崩溃；作者描述命令服务能连接，硬件插件初始化失败。三者都没有在本机复现，也没有独立安全调查。

组合后的工程决策不是断言厂家产品不安全，而是把“厂家的FT_Control+简单限速”保留为**必须独立验收的工程强Baseline候选**，当前不具备接触运动资格。软件力控不得替代独立硬件限位、急停和外部力参考；尤其不得用Issue中的350N当成FR5允许力阈值。我们还将Mz重复只作为“独立Mz激励下的疑似异常”检测，未激励时即使两列相等也不作故障定论。

**F19-07（本章判据）**

$$
G_{FT\_engineering}=G_{version}\land G_{wrench}\land G_{time}\land G_{safety}\land G_{independent\_bench}.
$$

**T19-07（证据/决策表）**

|公开报告|设备/版本|证据等级|本轮工程动作|
|---|---|---|---|
|Issue #25 FT_Control|未独立核实的现场自述|I|暂停接触力控授权；审查碰撞保护|
|Issue #23 Mz复制|FR10 v3.9.6|I|本机逐轴独立激励核验，不移植结论|
|Issue #35 ROS2激活|FR5 V6.0 v3.9.7|I|先离线版本匹配/模拟启动|
|Issue #20 全零|其他Fair5用户|I|独立已知载荷非零响应Gate|


## 08｜2026全文Zuo：遮挡接触定位已有强先例

本轮打开并阅读Zuo、Yang、Chua的2026预印本`arXiv:2605.00307`完整HTML的II方法、III实验与IV结论。其EIH RGB-D观测柔性fin-ray夹爪关键点，DeepLabCut提取结构点，FoundationPose估计物体姿态，SOFA逆有限元/QP以位置约束求外力；522节点/夹爪、14个离散候选接触位置。FoundationPose使接触前约10Hz，稳定接触后约30Hz，iFEA子模块报告可到40Hz。它已解决部分遮挡下的接触位置迭代推断，因此“EIH RGB-D+逆有限元+遮挡定位”不能作为本项目单独创新。

作者在台架以ATI Nano17获得独立合力参考，在Kinova Gen3抓取中以物体内嵌F/T与外部光学跟踪建立参考坐标链，做上/中/下接触位置、不同尺寸与力等级对照。论文也明确：遮挡接触点位置**难以直接量化真值**，接触估计器主要通过力估计RMSE改善间接评价；操作力真值不充分，因此只评价抓取力。论文报告整个抓取过程存在约0.25s力起始延迟，卸载期误差大于加载期、受遮挡和内部应变影响。这对刮板换向/卸载尤其重要，且论文的误差与速度不能作为FR5实测。

**F19-08（本章判据）**

$$
\hat f=\arg\min_f\|x_{FEA}(f,c)-x_{RGBD}\|^2\quad\text{subject to assumed contact candidate }c.
$$

**T19-08（证据/决策表）**

|方法项|Zuo论文实证|本项目迁移|限制|
|---|---|---|---|
|视觉|EIH RGB-D关键点/物体重建|可以参考|柔性夹爪可见性≠刮板|
|力|SOFA逆FEA、QP|需材料与工具模型|无现成FR5刮板FEA|
|真值|ATI Nano17、嵌入F/T|独立合力思路可用|没有直接接触线真值|
|时间|10/30Hz，0.25s起始滞后|动态卸载要单测|不可移植带宽|


## 09｜2026全文Min：静摩擦分离和触觉融合不是空白

本轮核读Min等2026预印本`arXiv:2605.28412`完整HTML的II–VI节。作者将气动皮肤接触事件与关节电流残差融合：静止接触前后残差差分近似外力矩；运动开始后静摩擦不再恒定，加入TCN建模静转动摩擦迟滞。核心前提是电机电流/关节状态可读、接触事件由独立皮肤传感器提供，且无外力训练轨迹可获得。作者以4关节电流残差（mA）报告静态与静转动过渡误差改善，并承认无官方电机转矩常数，因此mA不能自动换算成Nm；70ms窗口约30个点，停机瞬间仍存在模型误差。

该论文已经覆盖“用接触状态区分摩擦残差+时序网络补偿换向/起动”这一组合，进一步削弱本项目把ESO/TCN直接堆进刮板的创新理由。其接触皮肤在机器人表面、数据来自关节电流；现有FR5为腕部六维F/T+EIH RGB-D，未取得经标定的低层电流—力矩通道与同样的皮肤传感器。论文的mA RMSE和阈值不能作为FR5接触控制参数，真人交互演示也不能替代本项目独立伦理审批。

**F19-09（本章判据）**

$$
\Delta\tau_{res}=\tau_{res}(t)-\tau_{res}(t_{on}^{-})\approx\tau_{ext}\quad(\dot q=0,\ \Delta\tau_{fric}\approx0).
$$

**T19-09（证据/决策表）**

|比较维度|Min 2026|Zuo 2026|FR5迁移结论|
|---|---|---|---|
|传感器|气动皮肤+电流|EIH RGB-D|本项目仅EIH+腕F/T|
|核心数学|接触前后残差+TCN|逆FEA/QP|均有强先例|
|独立真值|无外力电流残差/演示载荷|ATI/内嵌F/T总力|需分区独立载荷|
|失败条件|动态→静态估计不稳|遮挡/迟滞/起始滞后|卸载必须单独Gate|


## 10｜EgoPressure官方源码与数据可获得性再审计

本轮只读核验EgoPressure官方仓库固定提交`517b4ffe...`：`docs/DATA.md`、`egopressure/senselpad.py`、`hub.py`、`parquet.py`、`registration.py`、`torch_dataset.py`等文件，并记录每个blob SHA。`senselpad.py`把raw counts转换为N和kPa，网格105×185、单元间距1.25mm；`registration.py`对未配准512×512深度使用k4a内参、depth→color变换、z-buffer投影；`parquet.py`对缺帧/未下载模态分别处理；`torch_dataset.py`提供按序列连续帧生成clip的函数，不能跨缺帧/序列。数据文档还确认raw depth单位mm、0表示无测量、世界+z向下、原始Sensel与UV压力同源。

我们尝试在运行容器直接访问HuggingFace目录，遇到DNS解析失败；公开网页能浏览数据卡与目录，但没有获得可校验的Parquet字节，不能把网页预览当成原始数据下载。故R18“优先合法小切片”决策继续保留，但本轮只完成源码/元数据审计和单位守恒离线测试，**没有训练或推理EgoPressure作者模型**。尤其不能把把压力投影到RGB后的图像像素当作原始独立压力传感器：透视投影会改变像素覆盖面积，正确的总力应从原始网格counts积分。

**F19-10（本章判据）**

$$
F_{total}=\sum_i\frac{c_i}{1736}\ \mathrm N=\sum_i p_i(\mathrm{kPa})\,(0.00125\mathrm m)^2\,1000.
$$

**T19-10（证据/决策表）**

|官方模块|真实源码用途|本轮验证|缺口|
|---|---|---|---|
|senselpad|counts→N/kPa，pad投影|单位守恒/重复像素反例|无真实压力帧|
|registration|depth→color重投影|代码路径/单位核对|无真实k4a标定文件|
|parquet|缺帧/模态读取|静态源码核读|Parquet未下载|
|torch_dataset|clip按连续帧生成|设计约束审查|无作者模型推理|


## 11｜坐标链、力矩平移与接触线不可辨识性

定义世界W、机器人基座B、法兰F、标定工具TCP T、腕力传感器S、相机C、真实柔性接触中心K及局部接触线L。`T_WF`描述法兰位姿，`T_FT`由工具标定获得，`T_FC`由手眼标定获得；柔性接触K/L相对T可能随加载、角度、摩擦、磨损变化，不能把TCP直接当接触中心。六维wrench从S变换到另一点时，力随旋转变换，力矩还要加力臂叉乘；若未补偿工具自重、重心、惯性和偏置，则任何边缘力矩估计都可能错。

对接触法向Fz、沿刮板宽度坐标y，观测Mx可能等于`yFz+τx`；未知纯接触偶矩τx时，y与τx不可分。多个分布压力点产生相同总力与一阶矩，R14–R16反例继续有效。即使Zuo用视觉几何缩小候选接触位置，FR5刮板接触线和边缘压力仍需要独立几何/分区载荷参考。软件读取的raw Mz如果受报告中的通道重复问题影响，还会破坏切向力矩解释；但该报告仅FR10特定版本，不能推断本机。

**F19-11（本章判据）**

$$
{}^{W}\!M_O={}^W\!R_S\,{}^S\!M_S+(p_{OS}\times {}^W\!F),\quad M_x=yF_z+\tau_x.
$$

**T19-11（证据/决策表）**

|坐标/量|来源|能否直接观测|Gate|
|---|---|---|---|
|法兰F|机器人状态|有条件|时间/标定|
|TCP T|工具标定|标定值|装夹复核|
|相机C|手眼标定|标定值|RGB-D注册|
|接触K/L|柔性接触物理量|通常不可直接唯一反演|独立边缘真值|


## 12｜独立数学证明：第二阻尼参数的零敏感度

对第4章厂家打包函数，固定M、B0、阈值与调整系数，只改变B1。由于表达式仅出现B0两次，输出向量完全不变。这是确定性的**软件参数不可辨识性**，与传感器噪声无关；数学上关于B1的偏导数为0，参数雅可比在B1方向有零列。人工离线例子`M=[1,2]`、`B^a=[0.5,1]`、`B^b=[0.5,9]`，两次打包都产生`[1,2,0.5,0.5,0.2,0.2,1,1]`。修订对照表达式用B1后两组输出不同。

这个反例与R14–R16“多点压力反演不唯一”不同：前者是**软件接口丢失一个用户自由度**，后者是**物理观测维度不足**。两者都会导致“看似调了参数/加了传感器，却没有独立信息收益”，但解决方式不同。软件问题优先版本核对、厂家确认、修复接口；物理问题需要新增独立真值和模型假设。绝不把离线B参数列表变化解释为真实机器人阻尼响应实验。

**F19-12（本章判据）**

$$
\frac{\partial P_{SDK}}{\partial B_1}=\mathbf0,\qquad \mathrm{rank}\left(\frac{\partial P_{SDK}}{\partial(B_0,B_1)}\right)\le1.
$$

**T19-12（证据/决策表）**

|离线输入|当前SDK表达式输出B槽|假设双B正确表达式|可观测差异|
|---|---|---|---|
|B=[0.5,1]|[0.5,0.5]|[0.5,1]|当前丢B1|
|B=[0.5,9]|[0.5,0.5]|[0.5,9]|当前丢B1|
|B=[15,15]|[15,15]|[15,15]|示例无法识别|


## 13｜可执行离线算法、103项测试与回归状态

新增`src/ft_integrity_guard.py`、`src/pinned_sdk_excerpt.py`、`src/run_r19_study.py`及`tests/test_ft_integrity_guard.py`。程序只解析源码片段和人工构造的状态结构，不导入厂家SDK、不打开机器人网络、不发送任何运动/力控命令。AST审计要求恰好一个`FT_Control`函数、恰好一个远程调用、恰好一个八元素参数列表；含糊则HOLD。反馈Gate要求六维值有限、独立已知加载、独立采集时刻与参考时刻一致、帧计数模256连续、采集/接收时刻有序，且独立参考力误差在事先标定范围内；**即使全部满足也只给离线诊断资格，不授权接触**。

本轮执行R19新增103个pytest测试案例，103/103通过；重新执行R18打包的50项，50/50通过。新增测试包括不等阻尼丢参、修订对照、歧义源码拒绝、不同力单位、无效深度、参与者划分泄漏、缺标签、时钟错配、已知载荷全零、帧计数回绕、疑似Mz重复、无运动授权等。曾因新增`acquired_s`独立时刻字段造成一项测试未传该字段而失败，修正测试输入后复跑全部通过；该失败属于本轮测试迭代，未隐藏为首次全过。`results/R19_TESTS.txt`、`R18_REGRESSION.txt`、`R19_OFFLINE_RESULTS.json`保留执行证据。

**F19-13（本章判据）**

$$
\mathrm{DIAGNOSTIC}=\bigwedge_i G_i,\qquad \mathrm{CONTACT\_AUTHORIZED}=\mathrm{False}\ \text{(offline)}.
$$

**T19-13（证据/决策表）**

|测试层|真实执行数|检查对象|不能推出|
|---|---:|---|---|
|R19新增|103/103|AST、单位、时间、参考力、拒绝|FR5安全|
|R18回归|50/50|评估指标、分组、FT准备性|历史所有轮已回归|
|SDK原始完整程序|0|只读源码而非安装运行|SDK端到端可用|
|实体力台/FR5|0|未连接|任何物理性能|


## 14｜主论文Baseline竞争矩阵与R18→R19排序

主学术论文仍未选胜者。**VPEC**（Grady等，IROS2022，DOI 10.1109/IROS47612.2022.9982073）有MIT源码、公开训练评估入口，R17发现默认验证路径含test风险；模型/大数据未取得，外部相机视角不同，列“有条件优先最小复现”。**ViPER**（Collins等，IROS2023，DOI 10.1109/IROS55552.2023.10342124）更接近EIH RGB，但许可/模型权重和测试有放回抽样仍需确认，列“有条件保留”。**Jiang IAAC**（Jinlei Jiang等，2024，DOI 10.3390/act13020062）有力控方法但2ms外环/控制接口与本机FT_Control尚不匹配，列“暂缓”。**EgoPressure**（Zhao等，CVPR2025，DOI 10.1109/CVPR52734.2025.02582）有真实Sensel标签、MIT工具、数据CC BY-NC-SA，优先数据预研而非FR5主论文。

**Zuo2026**新增为方法学强竞争：EIH RGB-D+SOFA+遮挡接触定位，独立总力真值强，但依赖柔性fin-ray几何、外部SOFA/FoundationPose、接触位置独立真值不足，源码/许可未核，暂缓直接复现。**Min2026**触觉+电流摩擦分离有2026新颖性反证，但FR5无相同皮肤与已标定关节力矩，暂缓主Baseline。**Chen2026**多接触EIT+气压独立力参考，需额外皮肤硬件且动态速率受限，方法学保留。所有候选都必须继续核对DOI/预印本、源码、许可证、数据、版本、重要参数、RGB-D/F/T频率、是否需低层力矩/高频接口、独立真值。厂家FT_Control不再默认“可直接执行”，只作为安全Gate未过的工程强候选。

**F19-14（本章判据）**

$$
\mathrm{score}_{candidate}=\mathrm{reproducibility}\times\mathrm{FR5\ interface\ feasibility}\times\mathrm{independent\ truth},\quad \text{任一为0则暂缓}.
$$

**T19-14（证据/决策表）**

|候选(年份/DOI或arXiv)|源码/数据/许可与获取层级|FR5接口/频率与独立真值|R18→R19排序/决策及最小补证|
|---|---|---|---|
|VPEC 2022 DOI 10.1109/IROS47612.2022.9982073|MIT源码；R17全文；权重/大数据未取|外置RGB→压力，EIH遮挡差异；原Sensel非FR5|保持条件第一复现资格；取合法小样本/固定划分|
|ViPER 2023 DOI 10.1109/IROS55552.2023.10342124|作者源码；R17全文；许可/权重未核|EIH RGB更近；弱F/T标签；独立刮板真值缺|条件第二；先确认许可与无泄漏评估|
|Jiang IAAC 2024 DOI 10.3390/act13020062|本轮历史片段，源码/参数待核|软超声外环，FR5低层权限/周期未知|继续暂缓；本机版本和2ms条件核验|
|EgoPressure 2025 DOI 10.1109/CVPR52734.2025.02582|R18全文，MIT工具/非商业数据；本轮源码固定SHA|30Hz平面人手；独立Sensel但非刮板|数据预研优先，非主论文；取得合法小切片|
|Zuo等2026 arXiv 2605.00307|本轮全文；源码/许可未核|10/30Hz，0.25s起始延迟；ATI总力非接触线|新直接竞争，暂缓原样复现；索要代码/工具适配|
|Min等2026 arXiv 2605.28412|本轮全文；代码/许可未核|电机电流+气动皮肤；FR5无同等接口|新新颖性反证，主论文暂缓|
|Chen等2026 DOI 10.1109/JSEN.2026.3704916|R16全文历史，源码/数据未核|EIT+气压额外硬件，动态频率不适配|方法学保留；多压头物理真值|


## 15｜唯一首选创新、备选和新颖性退出判据

唯一**条件性首选**科学问题I-A：在已知有限宽刮板几何、真实相机遮挡、已标定六维F/T与独立物理分区真值条件下，能否对接触边缘风险给出**部分可辨识区间和拒绝机制**，并在相同分析放行覆盖率下比wrench-only及错配视觉更可靠？与普通“视觉预测力”“遮挡接触定位”“摩擦补偿”不同，主张的可能创新只在**可验证的边缘风险证书、证据不足拒绝和独立物理真值增量**，目前尚未证明。R19新增厂家SDK可信性作为I-A必要前提；如本机力数据不可信，任何视觉融合纠偏都不得在线执行。

唯一备选I-B为WORK→UNLOAD→OFFSURFACE_RETURN的延迟鲁棒简单控制，前提是厂家FT_Control/独立限速已安全可用且简单基线确实在卸载指标上失败。摩擦—法向—变形分离和普通RGB-D+F/T姿态纠偏暂列支撑模块，不再争主创新。新颖性反证包括Zuo2026遮挡定位、Min2026接触摩擦、ViPER/EgoPressure/Chen分布压力与多接触估计；退出条件为：独立分区载荷不可获得、同覆盖率B3对B1/B4无可重复优势、真实可见性/时间同步Gate失败，或2025–26新文献已完全覆盖同等科学问题。

**F19-15（本章判据）**

$$
\Delta(c)=P(\mathrm{false\ release}\mid B3,c)-P(\mathrm{false\ release}\mid B1,c),\quad \Delta(c)<0\ \text{才可能支持增益}.
$$

**T19-15（证据/决策表）**

|方向|独立真值/可观测性|R19新颖性反证|本轮决策|
|---|---|---|---|
|I-A边缘风险区间+拒绝|需多压头独立分区/工具几何|Zuo、ViPER、Chen已有接触估计|唯一条件首选；暂不宣布原创成立|
|I-B卸载离面返程|需独立力台/速度时序|Min与厂家已有柔顺控制|唯一备选；等简单基线失败|
|摩擦扰动分离|纯偶矩/摩擦未知难辨|Min2026静摩擦TCN|暂缓独立创新|
|普通姿态纠偏|真实接触中心未观测|Zuo2026遮挡几何定位|仅I-A支撑|


## 16｜M1/M2/M3三入口与统一有限宽扫掠契约

完整上层保留三入口：M1人工按序选点并指定路线；M2识别人体背部实体画线、请用户确认起终点和方向、生成有序三维曲线；M3相机识别解剖标志，输出带置信度的穴位候选，必须专家审核，建立可编辑三维关键点图；用户可选两点之间路径或审核过的刮痧模板自动生成线路。三入口统一转换为序列`(p,t,n,Σ,q_tool,edge_sweep_eligible,mode)`，其中p是世界位置、t是切向、n是曲面法向、Σ是空间/视觉/时序不确定性、q_tool是工具姿态，`edge_sweep_eligible`标明有限宽刮板连续扫掠是否有足够几何余量，mode∈{WORK,UNLOAD,OFFSURFACE_RETURN}。

曲面图搜索与成熟视觉分割只作为工程上层Baseline，不占博士唯一创新份额。每条线路接触前必须人工审核；M2起终方向不明、M3解剖标志低置信度、有限宽扫掠越界、法向不可靠、RGB-D/FT采集时间不可信时拒绝下发接触路径。R19没有新增上层神经网络、没有运行FR5运动轨迹，沿用已审查的接口契约并增加SDK状态质量字段`force_data_validity`与`controller_version_match`，两者不能由API返回0自动置真。

**F19-16（本章判据）**

$$
\Gamma=\{(p_i,t_i,n_i,\Sigma_i,q_i,e_i,m_i)\}_{i=1}^N,\quad m_i\in\{W,U,R\}.
$$

**T19-16（证据/决策表）**

|入口|输入与审核|统一输出|拒绝条件|
|---|---|---|---|
|M1|人工点/顺序/路线|3D有序曲线|点序/法向缺失|
|M2|背部实体画线+起终确认|方向明确3D曲线|线遮挡/方向不明|
|M3|解剖标志+专家审核穴位图/模板|审核后曲线|低置信度或未审核|
|三入口共同|有限宽边缘/姿态/时间Gate|WORK/UNLOAD/OFFSURFACE_RETURN|传感/扫掠不可信|


## 17｜非人体独立真值、同条件消融与分级Gate

非人体实验先做离线源码版本/参数协议审核E0，再做相机内外参/手眼/刮板边缘与TCP几何标定E1，腕F/T偏置、重力、重心、动态补偿、已知独立载荷与时钟E2，本机`FT_Control`固件/SDK/ROS2接口、独立硬件保护与状态反馈E3。E0–E3都必须留原始日志、标定证书、时间戳、校验和和失败回退记录。其后才允许在隔离工装和安全评审下离面无接触轨迹E4，再逐步进行低能量单向非人体仿体恒力、平滑卸载和离面返程E5；贴面轻力返程需另审，真人必须独立风险和伦理批准。本轮E0的**公开源码静态审计完成，但本机版本匹配未完成，E0实体资格仍未通过**。

公平对照预注册：B0厂家FT_Control/简单限速/状态机，B1仅六维wrench，B2仅RGB-D几何，B3匹配RGB-D+F/T区间风险，B4时间错配视觉，B5固定工具TCP代替真实接触边缘。对同一非人体材料、几何、加载组合、随机化次序与同一外部真值，报告固定覆盖率下错误放行、拒绝率、独立边缘载荷、卸载峰值、返程最小离面间隙、时序错配、计算延迟与故障回退；按独立episode/材料批次配对统计。若无法独立校准多压头串扰和同时加载，边缘载荷不算独立真值。

**F19-17（本章判据）**

$$
G_{entry}=G_{calib}\land G_{time}\land G_{FT}\land G_{independent\_truth}\land G_{safety};\quad\neg G_{entry}\Rightarrow HOLD.
$$

**T19-17（证据/决策表）**

|Gate|需提交的独立证据|当前状态|失败回退|
|---|---|---|---|
|E0源码/版本|本机SDK/固件匹配、静态参数审计|公开代码已审，本机未核|禁止接触|
|E1几何|相机/手眼/边缘/TCP标定|未完成|仅离线|
|E2力/时间|已知载荷、采集时间、补偿|未完成|HOLD|
|E3安全接口|FT_Control与独立保护/急停|未完成|不启用力控|
|E4/E5仿体|独立力台/分区真值与安全审查|0次|禁止人体|


## 18｜本轮决策日志、验收结论和下轮最小补证

本轮有**决定性工程路线修订**：D19-01厂家Python SDK当前打包表达式丢B1，禁止用它直接验证双阻尼参数；D19-02`FT_GetForceTorqueOrigin/RCS`返回0不等于采集新鲜，独立参考与时间是强制Gate；D19-03 Issue #25为未经独立确认的严重风险信号，厂家FT_Control工程强Baseline候选降为“安全验收前不可执行”；D19-04 FR10 Mz重复仅作本机逐轴检查线索；D19-05旧checksum清零问题在V4.0.0源码为注释，不再作为当前执行事实；D19-06 Zuo2026遮挡接触定位已覆盖，普通iFEA/EIH不主张原创；D19-07 Min2026触觉摩擦残差已覆盖，普通TCN/ESO不主张原创；D19-08 EgoPressure合法小切片仍未取得，只完成源码和单位审计；D19-09 I-A继续唯一条件首选、I-B唯一备选，主论文待定；D19-10三入口和分级非人体Gate不变，严禁未经许可写旧GitHub仓库。

内容验收：16项去重来源、2篇本轮公开全文方法/实验核读、厂家完整文件只读关键词审计及关键行核验、EgoPressure六模块源码核读；R19新增103/103、R18回归50/50；18章对应、18公式/18表格、16来源/10决策在双文档均覆盖。**物理证据仍为0：FR5连接0、运动命令0、FT_Control调用0、非人体力台0、真实EgoPressure Parquet 0、原论文网络推理0、人体实验0。**下一轮优先取得本机SDK/固件及独立已知载荷只读日志，要求厂家澄清B1打包/FT_Control碰撞保护语义；若无设备，继续合法数据最小切片或源代码协议补证，不以触发次数虚增研究轮。

**F19-18（本章判据）**

$$
\mathrm{AUTO\!\!\!-R19\ accepted}_{offline}=1,\quad\mathrm{E0\!:\!E5\ physical\ pass}=0.
$$

**T19-18（证据/决策表）**

|决策ID|R18→R19变更|证据|下轮最小补证|
|---|---|---|---|
|D19-01|B1参数丢失疑点，禁止双B消融|S19-01/02、AST|厂家协议/本机版本|
|D19-02|API成功不等于新鲜力|S19-01、离线Gate|独立采集时钟+力台|
|D19-03|FT_Control强Baseline降为安全条件候选|S19-03(I)|独立安全回路/非人体验证|
|D19-04|Mz复制仅FR10个例|S19-04(I)|FR5逐轴激励|
|D19-05|旧checksum问题被新源码部分反证|S19-06、S19-01|CNDE时序日志|
|D19-06|普通遮挡接触定位非原创|S19-07(F)|独立接触线真值|
|D19-07|普通摩擦TCN非原创|S19-08(F)|FR5电流接口/摩擦真值|
|D19-08|EgoPressure未取得原始数据|S19-09/10|合法小Parquet|
|D19-09|I-A首选/I-B备选继续待定|候选矩阵|固定覆盖率物理对照|
|D19-10|三入口及非人体Gate保留|用户系统契约|设备/伦理批准|


---
**复现入口**：`python -m pytest -q tests/test_ft_integrity_guard.py`；`cd prior_R18 && python -m pytest -q tests/test_evaluation_guard.py`；`PYTHONPATH=src python src/run_r19_study.py`。本包不包含厂家SDK、真实机器人IP、真实压力Parquet或控制指令。