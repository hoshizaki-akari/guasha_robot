# FAIRINO FR5刮痧机器人｜AUTO-R73 博士研究报告

**日期：2026-10-22｜候选版本：AUTO-R73｜验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY（须以实际JUnit/归档QA为准）**

**研究定位**：独立AUTO博士链路、真实整机可验证能力优先。本轮仅研究/证据验收，DEV-R45只读；不添加LP/EKF/MPC/ESO，不连接机器人。

**硬约束**：唯一条件首选I-A=有限宽柔性刮板局部载荷部分可辨识与可信拒绝（PAUSE_FOR_EVIDENCE）；唯一备选I-B=WORK—UNLOAD—OFFSURFACE_RETURN延迟鲁棒柔顺（HOLD_FOR_FR5_TIMING）。本项目已核独立非人体同面三区原始N样本0；FR5实体运动/FT_Control/ROS2发布/真人试验全部HOLD。

## 全部来源与证据等级（本文件自包含）

|ID|来源与层级|可访问性/边界|
|---|---|---|
|P1|AUTO-R72完整研究/测试ZIP|本地CRC+内部SHA及定向复跑|
|P2|DEV-R45完整工程/测试ZIP|本地CRC+内部SHA及定向复跑；只读|
|S1|Huang et al., DEFT-VLA, 2026-10-10, arXiv:2610.14854v1|https://arxiv.org/html/2610.14854；全文方法/实验；原始trial未取得|
|S2|Chen et al., Robot Learning with Visual Predicted Force, 2026-10-03, arXiv:2610.04741v1|https://arxiv.org/html/2610.04741；全文方法/实验/局限；原始数据未下载|
|S3|VisualForce作者项目与Figure4(b) JSON|https://visual-force.github.io/static/data/force-trajectory.json；网页可读、本地原始字节0|
|S4|He et al., RCT, 2026, arXiv:2606.31694|https://faerber-lab.github.io/RCT/；CC BY4.0项目数据，未下载|
|S5|Khalifeh et al., IntelliMan Zenodo 2026, 10.5281/zenodo.18759556|https://zenodo.org/records/18759556；人手指采集/许可未核|
|S6|Engelbracht et al., Hoi! 2026, 10.60507/FK2/QODWTV|https://bonndata.uni-bonn.de/catalog/datasets/10.60507/FK2/QODWTV/；当前410 Gone|
|S7|FAIRINO 4.0.0 Python力控手册|https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html；官方网页非固定SDK源码|
|S8|FAIRINO 4.0.0周边适配手册|https://manual.fairino.support/latest/CobotsManual/robot_peripherals.html；列KWL-SFTE75B|
|S9|FAIRINO SDK v2.3.0 Robot.py固定源码|R72归档静态证据；commit 9541beaab0662dc8c267c0b9ba925aaf14858a77；本轮未重下载|

本轮新核S1/S2全文的方法、实验、局限；S7/S8官方文档；S3/S4/S5/S6项目/数据卡。S9固定SDK静态审计来自经SHA校验的R72归档，不冒称本轮重下载。外部数据无原始字节下载。

## 十二章目录

01. 继承归档、真实差分与内容Gate
02. 有限宽刮板的科学问题与不可唯一反演
03. DEFT-VLA全文方法：接近与接触分阶段
04. 真实论文表I的分母、五种Baseline与独立复算
05. 阶段证据合同的弱Baseline、强Gate和故障反例
06. VisualForce全文：有独立整体力真值，但不是三区
07. 合法真实数据筛选、原生单位与许可证
08. 非人体最小实验：曲率、ABBA、三区力与时钟
09. FR5 SDK、KWR75D与厂家FT_Control强Baseline边界
10. M1/M2/M3统一3D路径、有限宽扫掠与离线状态机
11. 唯一备选：换向延迟鲁棒柔顺的退出Gate
12. 五篇主Baseline、2026竞争、验收与下一轮退出

## 第01章｜继承归档、真实差分与内容Gate

### 科学问题、方法与证据层级

研究起点不能用历史聊天中的“最近报告”替代。实际可读的最新归档是AUTO-R72与DEV-R45，故下一轮候选为R73；逐个读取其报告、零基础讲义、register、决策、CHANGELOG、FAILURES、源码、测试并按清单验SHA/CRC。R72时序Gate已被R45离线工程吸纳，本轮不能重复包装成创新。新差分应聚焦阶段切换的实验资格与独立力真值获取。内部SHA说明文件字节未变，但不说明厂家固件、真实接触、ROS2或伦理资格已验证。

### 数学定义、量纲与适用边界

$$
\mathrm{ArchiveOK}=\bigwedge_i[\operatorname{SHA256}(b_i)=h_i]\land\mathrm{CRC\_OK}
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|研究继承|AUTO-R72研究38/38、测试11/11|完整ZIP内部校验，不是外部侧车全覆盖|
|工程继承|DEV-R45工程2121/2121、测试25/25|原Python工程只读，不做DEV版本|
|定向重跑|R72 45/45、DEV-R45 31/31|非DEV全历史回归|

**来源**：P1、P2；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

DEV保持R45三入口和时间面板；AUTO只给出下一张只读证据卡的数据合同。

### 本章决定与退出条件

D73-01：以实测可访问归档为唯一继承基线；若字节或测试不符，禁止升号。

### 练习题与完整答案

**题：**为什么研究ZIP全部SHA通过仍不能批准实体运动？

**答：**SHA仅检文件完整性；没有独立物理真值、FR5兼容、校准、时钟、安全评估和实机反馈，实体运动仍HOLD。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第02章｜有限宽刮板的科学问题与不可唯一反演

### 科学问题、方法与证据层级

首选I-A仍是有限宽柔性刮板边缘/接触线局部载荷的部分可辨识与可信拒绝，不是把腕部单个wrench反演成任意压力图。三个区域力至少三个未知量；即便理想情况下腕部能提供一个总法向力和一个横向力矩，也只给两个独立约束，仍有自由度。法兰坐标、TCP与真实受压柔性边缘不是同一点。视觉观测能帮助约束形状，却不能在缺独立真值时证明三区载荷精度。研发优先建立测量链而非叠加LP/EKF/MPC/ESO。

### 数学定义、量纲与适用边界

$$
F_z=F_L+F_C+F_R,\quad M_x=-aF_L+aF_R
$$

**解释**：$F_L,F_C,F_R$均为N；$a$是边缘横向距离m，$M_x$为N·m。仅总力与力矩时仍不能唯一求三区。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|腕部F/T|整体合力与力矩|没有逐区压力真值|
|视觉|曲面/刮板形变线索|受遮挡、标定和柔性变化影响|
|独立三区|同面左中右N参考|本项目已核原始样本仍为0|

**来源**：P1、S2、S4；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

统一接口保留flange_frame、tcp_frame、actual_contact_line_status和有限宽扫掠资格。

### 本章决定与退出条件

D73-07：I-A保持PAUSE_FOR_EVIDENCE；I-B仅为换向延迟鲁棒柔顺的备选。

### 练习题与完整答案

**题：**两组三区力同为3N且左右对称，能否据此判定中间区域压力相同？

**答：**不能。总力和左右平衡不足以确定中间力，须独立三区参考或额外已验证约束。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第03章｜DEFT-VLA全文方法：接近与接触分阶段

### 科学问题、方法与证据层级

2026年10月10日的DEFT-VLA是本轮最直接的真实擦拭竞争论文。其方法先以完整演示训练视觉主导的base expert，再冻结主干并用接触片段训练力感知contact expert；推理阶段以视觉、机器人状态和因果wrench历史估计阶段，使用滞回和连续决策避免抖动。关键实验细节：pipette与aspiration使用学得的自动路由，而whiteboard wiping的双专家切换由人工触发。论文有真实擦拭成功率，但不能据此说自动换向已在擦拭上实证。

### 数学定义、量纲与适用边界

$$
c_t=\mathbf{1}\left[\max_{j\in\mathcal W_t}\lVert f_j\rVert_2>\delta_{\rm task}\right]
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|作者硬件|Mercury B2，15Hz；3×D435；双腕F/T|不是FR5控制率|
|接触标签|按力阈值和邻域扩展|5/2/4N是作者任务阈值|
|白板擦拭路由|双专家手动切换|不能当自动换向验证|

**来源**：S1；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

离线状态日志增加router_mode、router_trigger_source、transition_capture_ns/decision_ns。

### 本章决定与退出条件

D73-02：新增HOLD_STAGE_ROUTER_UNVERIFIED_FOR_WIPING；不抹去作者真实成功率。

### 练习题与完整答案

**题：**为什么白板擦拭90%整体成功不能证明自动阶段切换成功？

**答：**论文该任务的双专家阶段切换是MANUAL；成功率是人工切换条件下的任务结果。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第04章｜真实论文表I的分母、五种Baseline与独立复算

### 科学问题、方法与证据层级

实际将DEFT论文Table I的五种方法×三任务共15行作者汇总值转成整数成功次数，以Python Fraction逐行独立复算，全部与论文报告的四舍五入百分比一致。相同任务各方法均是20次真实试验，简单对照为原π0.5，较强论文对照还包括力token、全局FAC和无接触专用FAC双专家。pipette DEFT接近成功15/20，接触条件成功14/15；把93.33%当整机成功会夸大23.33个百分点。全任务整体最好非DEFT分别30%、30%、70%；DEFT分别70%、50%、90%。这些是作者公开汇总，不是我们获取的原始trial或独立复现实验。

### 数学定义、量纲与适用边界

$$
P(\mathrm{complete})=\frac{n_{\rm complete}}{n_{\rm total}},\quad P(\mathrm{contact}\mid\mathrm{approach})=\frac{n_{\rm complete}}{n_{\rm approach}}
$$

**解释**：两个分母分别是全部试验数和已经成功到达接触阶段的试验数。使用相同20次试验、相同任务才能公平比较。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|DEFT pipette|14/15=93.33%接触条件成功|14/20=70%整机|
|DEFT aspiration|10/20=50%|10/20=50%整机|
|DEFT wiping|18/20=90%|MANUAL路由；20次|

**来源**：S1、P1；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

增加n_total、n_approach、n_complete、split_id、router_mode并显示两个分母。

### 本章决定与退出条件

D73-03：同数据同20试验分母报告整机与条件阶段成功，不混同MANUAL与AUTO。

### 练习题与完整答案

**题：**接近成功15次、最终成功14次、总共20次，接触条件率与整机率分别多少？

**答：**14/15=93.33%，14/20=70%，相差23.33个百分点；不能互相替代。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第05章｜阶段证据合同的弱Baseline、强Gate和故障反例

### 科学问题、方法与证据层级

新离线程序r73_stage_audit.py只接受论文汇总声明，不推断硬件资格。设计12个可审计异常，包括把擦拭人工路由伪报自动、把接触条件分母冒充全体、伪造三区真值、伪造已获得原始trial、允许运动和关键字段缺失。弱Baseline只看成功数是否非负，11/12错误放行；强Gate逐项拒绝，0/12误放；有效论文汇总声明仅获得PASS_PUBLISHED_AGGREGATE_AUDIT_ONLY。首次弱Baseline遇到None产生TypeError，已修复并保留失败记录。这个测试证明证据合同在人工注入条件下的逻辑，不证明真实FR5安全。

### 数学定义、量纲与适用边界

$$
\mathrm{PASS}_{\rm audit}=\mathrm{CountsOK}\land\mathrm{DenominatorOK}\land\mathrm{RouterOK}\land\neg\mathrm{MotionPermit}
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|同一作者汇总|15行×20试验|全部独立算术核查|
|弱Baseline|只检查成功数为非负|12故障误放11个|
|强证据Gate|分母/路由/原始数据/物理真值/运动标志|12故障误放0个|

**来源**：P1、S1；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

增加HOLD_STAGE_DENOMINATOR_MISMATCH与HOLD_ROUTER_MODE_NOT_VALIDATED只读拒绝码。

### 本章决定与退出条件

D73-05：将路由与分母合同作为DEV可视化审核，不加控制器或新数学状态。

### 练习题与完整答案

**题：**强Gate对有效论文汇总给PASS，是否等于物理性能验收？

**答：**不等于。PASS仅说明公开汇总和字段一致；未取得作者原始trial和本项目三区独立物理数据。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第06章｜VisualForce全文：有独立整体力真值，但不是三区

### 科学问题、方法与证据层级

2026年10月3日VisualForce完整方法与实验明确提供了值得借鉴的独立整体力标定：用3D打印接触夹具与Mitsumi/ATI参考传感器采117段标定数据；评估另用Nextech力计与外部Micro-SBeam负载元件，3个未参与训练的episode合计442帧，作者报告视觉估计0.91N MAE、电机电流线性Baseline 4.20N。视觉估计来自柔性Fin Ray夹爪形变，推理用32个动作-力候选选最接近目标的动作。作者局限明确只估Fz，且部署没有在线力反馈闭环；不能当刮板局部载荷、FT_Control或FR5闭环性能。

### 数学定义、量纲与适用边界

$$
\mathrm{MAE}_{e}=\frac1{n_e}\sum_{t\in e}|\hat F_z(t)-F_z^{\rm ref}(t)|
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|训练标定|117 episode/47.5min；30Hz图像、约20Hz力|作者报告|
|独立评估|3 episode/442帧；外部负载参考|本轮未下载trial原始数据|
|作者MAE|视觉0.91N、电流4.20N|等权episode均值，非三区|

**来源**：S2、S3；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

在数据证据卡区分GLOBAL_FZ、WRIST_WRENCH、INDEPENDENT_THREE_ZONE_N。

### 本章决定与退出条件

D73-04：VisualForce列为独立整体Fz标定的邻近证据，不升为主学术baseline或三区真值。

### 练习题与完整答案

**题：**论文有外部负载传感器，为何本项目三区真值仍记0？

**答：**其参考量是整体轴向Fz，不是同面左中右三区独立N；且本轮未取得原始字节。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第07章｜合法真实数据筛选、原生单位与许可证

### 科学问题、方法与证据层级

四个数据候选分别按合法获取、原始SHA、接触对象、独立参考维度、原生单位、面积、曲率、标定、时钟和episode筛查。VisualForce公开Figure4(b) JSON网页能看到时间及测量/预测列，但网络DNS受限，本轮本地0字节，不得据此算自己的MAE；RCT真实机器人压触且项目页明确CC BY4.0，却没有同面三区独立力；IntelliMan含人手指操作和整体6D F/T，Zenodo Rights未核到具体许可；Hoi当前URL 410。全部标HOLD_DATA_ELIGIBILITY。本轮确有更清晰的真实整体力来源，但仍没有本项目可用三区原始数据。

### 数学定义、量纲与适用边界

$$
F_i=\sum_{j\in\mathrm{zone}_i}p_jA_j\quad(\mathrm{Pa}\cdot\mathrm{m}^2=\mathrm{N})
$$

**解释**：Pa是N/m²，乘每个sensel有效面积m²才能得到N；必须有独立标定，不得把热图颜色直接当牛顿。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|VisualForce|网页JSON含time/measured/predicted/current|本地原始字节0、许可/时钟未锁、仅整体力|
|RCT|CC BY4.0；1,832机器人压触|每帧合力、非三区独立|
|IntelliMan|76.9MB；两人手指触觉|许可证未核、人体操作|
|Hoi!|历史卡CC BY4.0|当前官方链接410 Gone|

**来源**：S3、S4、S5、S6；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

只读数据卡必填source_kind、license_id、raw_sha256、episode_id、native_unit、zone_area_m2。

### 本章决定与退出条件

D73-04：原始合法三区数据继续0条；优先采购/采集三路独立参考。

### 练习题与完整答案

**题：**一个压力格读2000Pa，面积为2e-4m²，对应力多少N？

**答：**F=pA=2000×0.0002=0.4N；还需独立标定与串扰评估。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第08章｜非人体最小实验：曲率、ABBA、三区力与时钟

### 科学问题、方法与证据层级

物理验证从非人体软垫开始，不能以人体、仿真或腕部合力替代。第一步在已知曲率夹具上用独立砝码与三路力参考做零点、线性、漂移、串扰和饱和校准；同时记录D435原生Z16深度尺度、手眼、TCP和真实柔性接触线。第二步预注册ABBA插膜对照，验证压力薄膜是否改变接触面积和力分配。第三步同步采集原生纳秒时戳、帧序号和独立高速参考，按episode/材料/曲率留出。只有拿到可追溯原始N、单位、面积和许可证，才可开始I-A可证伪的物理比较。

### 数学定义、量纲与适用边界

$$
\Delta F_i(t)=F_{i,\rm sensor}(t)-F_{i,\rm ref}(t)
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|独立参考|三块独立N测量或校准Pa阵列|不可只用腕wrench|
|ABBA|无膜A→有膜B→有膜B→无膜A|估计插膜扰动|
|留出|按材料/episode/曲率/工具分组|不能随机分同次接触帧|

**来源**：S2、S4、P1；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

最小实验数据合同见R73_EXPERIMENT_MINIMUM_PROTOCOL.md，DEV仅规划离线导入。

### 本章决定与退出条件

D73-07：拿不到三路真实原始N数据则冻结首选实证，不增加数学模块。

### 练习题与完整答案

**题：**为什么有膜与无膜要交替ABBA，而不能只测一次A再一次B？

**答：**交替可减弱时间漂移和加载顺序影响，估计薄膜本身对接触的扰动；仍需随机化和重复。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第09章｜FR5 SDK、KWR75D与厂家FT_Control强Baseline边界

### 科学问题、方法与证据层级

FAIRINO 4.0.0 Python力控文档FT_SetConfig列Kunwei company=17、device=0对应KWR75B，周边章节另列KWL-SFTE75B。用户KWR75D与[1,17,3,0]的实际返回结构、型号和总线映射均不能从这些网页推定。官方FT_Control原型有isNoBlock，但其“非阻塞”语义不等于网络错误下有界返回。R72已核固定SDK源码的持续socket.error重试路径，当前仍HOLD。厂家FT_Control作为未来工程强baseline必须先确认固件、接口、独立看门狗、时间戳和只读反馈，不能现在导入SDK、连接FR5或执行文档示例。

### 数学定义、量纲与适用边界

$$
T_{\rm observed}=T_{\rm capture}+T_{\rm transport}+T_{\rm process}\quad(\mathrm{s})
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|官方4.0.0 Python手册|company17 device0 KWR75B|不证明KWR75D device3|
|官方周边章节|KWL-SFTE75B|与KWR75D非同一型号声明|
|固定SDK静态审计|socket.error循环无显式总期限|不是实机延迟测量|

**来源**：S7、S8、S9；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

只读字段sdk_commit、robot_version、ft_config_raw、ft_timestamp_ns、timeout_bound_verified。

### 本章决定与退出条件

D73-06：FT_Control未来强baseline资格不变，KWR75D/时钟/超时全HOLD。

### 练习题与完整答案

**题：**官方列出KWR75B device0，能否把KWR75D [1,17,3,0]认定兼容？

**答：**不能。型号、配置字段及版本未核实，必须取得厂家适配矩阵与固件证据。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第10章｜M1/M2/M3统一3D路径、有限宽扫掠与离线状态机

### 科学问题、方法与证据层级

实际可读DEV-R45原工程已保留三入口人工/合成3D路线、静态可视化及每入口46帧APPROACH—WORK—UNLOAD—OFFSURFACE_RETURN离线演示。本轮研究不重建、不写DEV代码。统一输出应包含SI位置、切向、曲面法向、曲率与不确定性、姿态、int64ns时钟、置信度、工具有限宽边缘扫掠资格及各阶段路由来源。中心线无碰撞不能推出整片柔性刮板连续扫掠无碰撞；TCP也不是真实受压边缘。论文白板人工路由的发现应显示在DEV离线状态图旁，而不是暗中升级实体资格。

### 数学定义、量纲与适用边界

$$
\mathbf p_{\rm edge}(s,u)=\mathbf p_{\rm center}(s)+u\,\mathbf b(s),\quad |u|\leq w/2
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|M1|人工有序3D点/人工路径|必须人工批准|
|M2|实体画线输入契约/起终方向/反投影|目前仅人工合成示例|
|M3|可编辑3D候选/专家审核模板|不得宣称临床穴位识别|

**来源**：P2、P1；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

离线UI显示三入口同一证据合同、MANUAL/AUTO标签、拒绝理由和真实接触线HOLD。

### 本章决定与退出条件

D73-08：保持离线三入口，新增阶段路由与分母只读证据，不提升运动权限。

### 练习题与完整答案

**题：**为何中心线通过曲面检查仍不能说有限宽刮板可以接触？

**答：**左右边缘、柔性变形和连续扫掠可能碰撞或局部载荷不明；需要完整工具宽度及独立验证。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第11章｜唯一备选：换向延迟鲁棒柔顺的退出Gate

### 科学问题、方法与证据层级

I-B仅作为I-A长期缺独立三区真值时的备选，不意味着可直接上机器人。DEFT的白板擦拭人工切换恰好说明最像本项目的阶段路由并未自动实证。未来可先在同一离线人工事件真值上比较固定人工路由、简单阈值、带滞回路由的状态切换与延迟注入；同样输入、同样退出规则，报告误进入WORK、漏UNLOAD、离面返程是否真的离面、超时拒绝和整体成功率。任何实时闭环结论必须等FR5真实时钟、SDK有界性和合法非人体独立参考。

### 数学定义、量纲与适用边界

$$
\Delta t_{\rm route}=t_{\rm decision}-t_{\rm observation}\quad(\mathrm{s})
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|WORK|经审核的接触阶段|无真实反馈则不执行|
|UNLOAD|先减载并确认离面资格|未验证时序继续HOLD|
|OFFSURFACE_RETURN|离面返程|不是沿皮肤倒刮|

**来源**：S1、P2、S9；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

离线故障注入包含时钟乱序、观测延迟、手动/自动路由混标、返程仍接触等。

### 本章决定与退出条件

D73-07：I-B=HOLD_FOR_FR5_TIMING；若I-A长期无物理真值，先转向离线换向协议而非新数学。

### 练习题与完整答案

**题：**为什么论文擦拭手动切换不能作为本项目自动UNLOAD的强证据？

**答：**人工决定阶段边界绕开了自动检测延迟与错误切换问题，必须在同条件下单独评估自动路由。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 第12章｜五篇主Baseline、2026竞争、验收与下一轮退出

### 科学问题、方法与证据层级

五篇主学术baseline复现审计顺序仍VPEC→FEATS→ViPER→ImplicitRDP→RDP，不强选赢家；DEFT-VLA是阶段路由与真实擦拭的2026直接竞争，VisualForce是有独立整体轴向力参考的邻近方法，两者均不替代三区独立压力主baseline。I-A维持PAUSE_FOR_EVIDENCE，I-B维持HOLD_FOR_FR5_TIMING。R73新增的是论文真实汇总的分母/路由可审计反证与数据资格筛选；只有归档SHA、源码、测试、双文档覆盖全部通过才算内容验收。R60等十轮里程碑六份开题文档由独立任务处理，不在本轮重复计数。

### 数学定义、量纲与适用边界

$$
\mathrm{Eligible}=\mathrm{License}\land\mathrm{RawTruth}\land\mathrm{Calibration}\land\mathrm{Clock}\land\mathrm{FairSplit}
$$

**解释**：公式用于证据审计或离线推导；凡涉及N、m、s、Pa、int64 ns均需标清单位和坐标/时钟域。公式本身不是FR5实测认证。

### 同条件对照、来源和限制

|对象|本轮证据或结果|不能推断的内容|
|---|---|---|
|VPEC/FEATS/ViPER|视觉压力/触觉空间分布候选|作者数据/许可/三区独立真值仍缺|
|ImplicitRDP/RDP|视觉力/触觉慢快策略候选|FR5控制率/力矩接口未知|
|厂家FT_Control|工程强baseline（未来）|SDK有界性/型号兼容/实体安全HOLD|

**来源**：P1、S1、S2、S4、S7；具体论文元数据、DOI、固定版本及可获取性见本文件开头来源表与同包SOURCE_REGISTER.md。

### 对DEV整机可验证能力的接口贡献

DEV只接收成熟可执行接口和实验协议，不因AUTO编号变化而改变实体安全Gate。

### 本章决定与退出条件

D73-09：只在新增反证、独立复算、测试及双文档QA通过时升R73；物理资格不升。

### 练习题与完整答案

**题：**什么时候应该停止继续扩展I-A数学模型？

**答：**在长期拿不到独立同面三区真值、许可证/标定/时钟不齐时应冻结实证，转向I-B离线协议或最小数据获取，不空耗数学。

**失败反例与停止条件**：未取得原始合法数据、真实FR5时序/接口或独立三区力时只能HOLD；不能把作者论文结果、合成和静态代码分析提升为本项目物理真值。

## 附录A｜真实论文Table I的15行独立整数复算（不是本项目原始trial）

|任务|论文方法|总试验|接近成功|最终成功|接触条件成功率|整机成功率|阶段路由|
|---|---|---:|---:|---:|---:|---:|---|
|pipette|pi05|20|18|6|33.33%|30.00%|NOT_APPLICABLE|
|pipette|force_token|20|8|2|25.00%|10.00%|NOT_APPLICABLE|
|pipette|global_fac|20|17|5|29.41%|25.00%|NOT_APPLICABLE|
|pipette|dual_no_fac|20|16|4|25.00%|20.00%|LEARNED_AUTO|
|pipette|deft_vla|20|15|14|93.33%|70.00%|LEARNED_AUTO|
|aspiration|pi05|20|19|1|5.26%|5.00%|NOT_APPLICABLE|
|aspiration|force_token|20|20|4|20.00%|20.00%|NOT_APPLICABLE|
|aspiration|global_fac|20|7|1|14.29%|5.00%|NOT_APPLICABLE|
|aspiration|dual_no_fac|20|20|6|30.00%|30.00%|LEARNED_AUTO|
|aspiration|deft_vla|20|20|10|50.00%|50.00%|LEARNED_AUTO|
|wiping|pi05|20|20|14|70.00%|70.00%|NOT_APPLICABLE|
|wiping|force_token|20|20|9|45.00%|45.00%|NOT_APPLICABLE|
|wiping|global_fac|20|10|6|60.00%|30.00%|NOT_APPLICABLE|
|wiping|dual_no_fac|20|20|11|55.00%|55.00%|MANUAL|
|wiping|deft_vla|20|20|18|90.00%|90.00%|MANUAL|

**读表注意**：每方法每任务20次试验是作者论文表I的汇总；并非取得了300条原始trial。`LEARNED_AUTO`仅适用于论文pipette/aspiration双专家；wiping双专家是`MANUAL`。表内作者阈值与成功率不可移植为FR5参数。

## 附录B｜合法真实数据的四项资格与获取状态

|数据资源|公开证据|缺失项|当前决定|
|---|---|---|---|
|VISUALFORCE_2026|3 heldout episodes/442 frames; 0.91N vs 4.20N episode-mean MAE|Figure 4(b) JSON viewable; full raw data not downloaded; 1D Fz, not spatial 3-zone|HOLD_DATA_ELIGIBILITY|
|RCT_2026|29279 tactile frames; 1832 robot presses; 122 materials; 3 DIGIT|Sequence-aware split; not 3-zone independent reference|HOLD_DATA_ELIGIBILITY|
|INTELLIMAN_2026|8 PVDF sensors; 2 people; 150 trials each texture per person|Not nonhuman-contact-surface sample; human fingertip; 76.9 MB|HOLD_DATA_ELIGIBILITY|
|HOI_2026|3048 sequences across 381 articulated objects; CC BY 4.0 data card|Full raw bytes inaccessible; mixed human/robot embodiment|HOLD_DATA_ELIGIBILITY|

## 附录C｜完整复现、风险边界与最小补证

在本文件同目录运行：`python r73_stage_audit.py`、`python r73_evidence_gate.py`、`python -m pytest -q test_r73.py test_r73_document_qa.py`。两份文件各自含全部12章、各章公式/来源/表格/决策/练习答案。定向R72/R45回归、实际JUnit、SHA差分和失败详见本包TEST_REPORT及FAILURES。

最小实证：独立非人体三区N或经校准Pa×面积、两种曲率夹具、原生时戳与相机/手眼/力零偏、ABBA插膜、原始文件SHA/许可、episode留出；厂家书面KWR75D与FT_Control有界/可取消接口。未完成前所有实体/ROS2运动、真人实验和未经授权仿真HOLD。

**结论**：本轮“可复现”的是论文公开汇总数值的独立审计和证据拒绝合同，绝不是DEFT/VisualForce作者原版模型复现，也不是FR5实机性能或三区压力实测。
