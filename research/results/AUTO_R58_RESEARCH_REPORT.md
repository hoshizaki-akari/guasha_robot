# AUTO-R58 博士研究内容验收报告（独立完整版）


**2026-10-21；PASS_OFFLINE_RESEARCH_CONTENT_ONLY；研究与DEV工程独立；I-A PAUSE_FOR_EVIDENCE，I-B HOLD_FOR_FR5_TIMING；非人体独立三区真实原始数据=0。**


**本轮新增决定性证据：**2026-10-15 Tekscan I-Scan10/TekDAQ220官方发布；500g与100g单区容量反例；Zvyagin2022 16页全文方法/实验核读；三区相同wrench反例；36项新测试+75项R57定向回归。没有实机连接、运动、ROS2运动消息、物理仿真或人体试验。


## 目录
01. 归档验收与真正继承
02. 唯一科学问题与证据层级
03. 2026年I-Scan10/TekDAQ220更新
04. 压力阵列量程与接触条带
05. 独立三区力台的单区过载反例
06. 相同腕力矩的不同三区真值
07. Zvyagin全文方法：原生单位与校准
08. Zvyagin实验结果与独立真值缺口
09. 采样率、时钟与许可不许推断
10. 可执行合同、消融和失败反例
11. DEV三入口移交与安全边界
12. 停止/退出Gate与下轮最小实验


## 01. 归档验收与真正继承

本轮实际从挂载路径读取AUTO-R57研究ZIP及外部报告、讲义、register、决策和源代码；对ZIP执行CRC和35个内部SHA256，对12个独立挂载文件逐一比较R57外部清单，均一致。DEV-R30源码ZIP的1417个内部SHA256亦一致，并只读审查合成RGB-D反投影模块及其测试。没有触碰DEV写入路径，也没有把R57已报告的152项历史结果当作本轮新测试。R57新增75项本轮实跑；R58新增36项实跑。上一轮已读的固定源码commit/论文候选被明确标为“继承未重核”。归档真实性是科研复现的前提，不是机械臂安全证明。


### 01章公式（与另一份文档一致）

$$
Q_1 = N_{R57,\,SHA}=35,\quad N_{DEV30,\,SHA}=1417
$$


### 01章证据表

|归档对象|校验层级|本轮可作结论|
|---|---|---|
|R57研究包|CRC+35内部SHA+12外部SHA|可作为真实已读继承|
|DEV-R30|CRC+1417内部SHA|只读代码审计，不是DEV回归|


**对应来源**：S58-01,L58-01,L58-02；**对应决策**：D58-01。证据等级详见文末完整来源表。


**可证伪检查点 01：**为什么报告中不能把R57旧152项全部写成“本轮实测”？ **当前判断：**因为只有75项R57新增测试在R58实际重新执行，旧152包含R56与DEV定向回归，不能重复计数。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 02. 唯一科学问题与证据层级

唯一条件性首选I-A是柔性有限宽刮板接触线局部载荷在视觉、腕F/T、姿态、时钟和配准误差下的部分可辨识与可信拒绝。唯一备选I-B是WORK—UNLOAD—OFFSURFACE_RETURN换向的延迟鲁棒柔顺控制。现阶段I-A保持PAUSE_FOR_EVIDENCE，I-B保持HOLD_FOR_FR5_TIMING。证据必须区分：厂商产品规格、作者发表实验、人工数学例、独立非人体实测。腕部六轴力矩只描述总合力/合矩，不天然揭示局部压力场。三入口路径生成成功也不等于压力证据可信。


### 02章公式（与另一份文档一致）

$$
F_{\mathrm{wrist}}=\int_{\Gamma} f(s)\,ds,\quad \tau_{\mathrm{wrist}}=\int_{\Gamma} r(s)\times f(s)\,ds
$$


### 02章证据表

|证据|能证明|不能证明|
|---|---|---|
|厂家规格|名义节距/量程|独立真实局部压力|
|人工解析|模型逻辑|实体安全|
|独立非人体三区台架|有条件提供分区参考|自动证明每个细格压力|


**对应来源**：S58-05,S58-08；**对应决策**：D58-11。证据等级详见文末完整来源表。


**可证伪检查点 02：**为什么一个总力传感器不够？ **当前判断：**不同空间载荷可以产生相同合力与合矩，必须另有分区参考或可信拒绝。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 03. 2026年I-Scan10/TekDAQ220更新

Tekscan官方2026-10-15宣布I-Scan10及TekDAQ220，列出对既有矩阵压力传感器兼容、USB-C、改进抗ESD/噪声及可替换线缆。官方I-Scan10产品页明确ASCII导出、二维/三维显示和力/面积分析。但并未提供本项目5027或5051与TekDAQ220组合的逐帧原生counts、时间戳、同步触发、完整帧率和科研再发布许可。因此R58改变采购询证接口：必须把传感器型号、电子读出硬件、软件版本、固件、数据导出格式、授权和时间证据写成一套固定配置，而非只问“有无压力传感器”。


### 03章公式（与另一份文档一致）

$$
\mathrm{ReproducibleDataset}=\mathrm{sensor}\land\mathrm{electronics}\land\mathrm{raw}\land\mathrm{clock}\land\mathrm{license}
$$


### 03章证据表

|配置项|官网已确认|必须再索取|
|---|---|---|
|TekDAQ220/I-Scan10|2026-10-15发布、兼容矩阵|5027/5051具体组合证明|
|导出|ASCII|原生counts、帧时间与许可|


**对应来源**：S58-01,S58-02；**对应决策**：D58-02。证据等级详见文末完整来源表。


**可证伪检查点 03：**ASCII导出为何还不够？ **当前判断：**ASCII不保证原生counts、逐帧完整矩阵、纳秒时钟、同步机制及公开数据许可。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 04. 压力阵列量程与接触条带

5027官方列44×44、0.635mm节距、50/500psi量程；5051为44×44、1.27mm节距、7psi起多档。以3N、20mm宽、2mm名义接触条带为纯假设，平均压力75kPa；若条带变0.5mm则为300kPa。5051-7psi名义上限约48.3kPa，在2mm假设下平均值已超；5027-50psi约344.7kPa虽大于300kPa平均，但非均匀峰值可能更高，绝不能据此宣布安全。3N只是离线算术值，不是FR5目标力或医疗建议。


### 04章公式（与另一份文档一致）

$$
p_{\mathrm{mean}}=\frac{F}{w\,b};\quad p(3\mathrm N,0.02\mathrm m,0.002\mathrm m)=75\,000\mathrm{Pa}
$$


### 04章证据表

|假设条带|平均压力|量程结论|
|---|---|---|
|20mm×2mm|75kPa|5051-7psi均值超量程|
|20mm×0.5mm|300kPa|5027-50psi平均未超，但峰值未知|


**对应来源**：S58-03,S58-04；**对应决策**：D58-04。证据等级详见文末完整来源表。


**可证伪检查点 04：**为何不能只根据75kPa选50psi档？ **当前判断：**75kPa是平均值，真实局部峰值可能显著更高且曲面/动态标定未验证。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 05. 独立三区力台的单区过载反例

与压力膜相互独立的三区单轴力台是下一步可证伪设计，而非本轮已建硬件。假设总法向载荷3N，全部集中左区时，不能按1N/区选传感器。100g额定载荷约0.980665N，在该反例下超载；SparkFun 500g TAL221名义约4.903325N，在3N×1.2=3.6N纯假设裕量下通过额定预筛。额定通过不意味着偏载、夹具串扰、机械隔离、ADC精度和计量证书已合格，必须盲测并保留原生信号。


### 05章公式（与另一份文档一致）

$$
F_{\mathrm{rated}}=m_{\mathrm{rated}}g_0;\quad F_{\mathrm{zone,required}}\ge 1.2F_{\mathrm{total,max}}
$$


### 05章证据表

|单区候选|名义额定N|对3N×1.2预筛|
|---|---|---|
|100g TAL221|0.980665|FAIL|
|500g TAL221|4.903325|仅容量预筛PASS，物理HOLD|


**对应来源**：S58-06,S58-07；**对应决策**：D58-05。证据等级详见文末完整来源表。


**可证伪检查点 05：**为什么不是3N/3来选每只秤？ **当前判断：**刮板接触分布可能极端不均匀，必须考虑3N全部集中某一区的最坏情况。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 06. 相同腕力矩的不同三区真值

以三区中心位置x=(-0.01,0,+0.01)m，纯法向接触，载荷A=(1,1,1)N与B=(1.5,0,1.5)N均产生总Fz=3N、My=0Nm，但中区相差1N。这是无需引入新滤波器的解析反例，严格说明仅靠一个腕部wrench不能唯一重建三区分布。它也给出最小台架要求：至少要有独立测量三区的参考通道，且力台本身要验证机械隔离；压力膜内部划三区不等于独立测量。


### 06章公式（与另一份文档一致）

$$
\begin{bmatrix}1&1&1\\-0.01&0&0.01\end{bmatrix}\begin{bmatrix}1\\-2\\1\end{bmatrix}=\begin{bmatrix}0\\0\end{bmatrix}
$$


### 06章证据表

|三区载荷N|Fz N|My Nm|
|---|---|---|
|(1,1,1)|3|0|
|(1.5,0,1.5)|3|0|


**对应来源**：S58-05；**对应决策**：D58-06。证据等级详见文末完整来源表。


**可证伪检查点 06：**为什么力矩相同也不代表中间压力相同？ **当前判断：**腕部只得到两个独立的纯法向合成约束，三区载荷存在非零零空间。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 07. Zvyagin全文方法：原生单位与校准

本轮实际打开Zvyagin 2022的16页PDF，核读实验§2、方法§3、结果§5及结论§6。作者使用两张Tekscan 5051-350、2/5/10kg砝码、多种接触垫与多位置patch，旧VersaTek/I-Scan 7.70-16I实验采集**设置为200Hz**；单sensel是8-bit原生数值。校准以f(X)=A X^b把原始X映射到N，聚合多个patch寻找参数。论文设备旧系统最高730Hz的描述不能直接用作2026 TekDAQ220的帧率。


### 07章公式（与另一份文档一致）

$$
f(X)=A\,X^b,\quad F_{\mathrm{patch}}=\sum_{k\in T} A\,X_k^b
$$


### 07章证据表

|作者实验项|核读事实|不能外推|
|---|---|---|
|传感器|5051-350两张|不是5027/5051新档位验证|
|采样|旧VersaTek设为200Hz|不是TekDAQ220实测率|
|原生信号|8-bit sensel|不能自动视为Pa|


**对应来源**：S58-05；**对应决策**：D58-07。证据等级详见文末完整来源表。


**可证伪检查点 07：**论文中X=100代表100Pa吗？ **当前判断：**不代表。X是原生8-bit计数，必须使用经过独立验证的校准模型换算。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 08. Zvyagin实验结果与独立真值缺口

Zvyagin的150个校准patch中约72%在整体施加载荷与校准计算载荷间绝对误差<5%，另有三个patch误差达10–12%。其评价主要是积分载荷而非每个小格或左中右三区独立真实受力；作者的“Data Availability: Not applicable”不能视为可下载逐帧原始数据。论文还指出传感器标定饱和压力随时间可能变化。对本项目而言，作者结果是校准方法与失败模式的实证先例，不是FR5刮板的独立物理真值。


### 08章公式（与另一份文档一致）

$$
E_{\mathrm{patch}}=\frac{|F_{\mathrm{pred,sum}}-F_{\mathrm{reference,total}}|}{F_{\mathrm{reference,total}}}
$$


### 08章证据表

|评价对象|论文已有|本项目仍缺|
|---|---|---|
|整体patch载荷|150个校准patch的误差分布|跨会话独立盲测|
|三区局部载荷|没有独立三区误差证明|三通道力台真值及串扰|


**对应来源**：S58-05；**对应决策**：D58-07。证据等级详见文末完整来源表。


**可证伪检查点 08：**为什么总载荷误差5%不等于每区5%？ **当前判断：**左右区域的高估和低估可能在求和时抵消，整体误差不能界定局部误差。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 09. 采样率、时钟与许可不许推断

Tekscan技术页通用“250000 sensels/s”若机械地除以44×44=1936，只得到约129.13完整帧/s的**条件算术值**；它既不保证每帧读取所有sensel，也不是I-Scan10/TekDAQ220的实测帧率。Zvyagin旧系统200Hz为另一套配置，网页“最高20kHz扫描”也可能对应其他通道/模式。采样、曝光、帧号、时钟域和丢帧必须对特定配置独立记录。I-Scan10 ASCII可导出，但科研再发布许可和原生counts完整性尚未核验。


### 09章公式（与另一份文档一致）

$$
f_{\mathrm{conditional}}\le\frac{250000\,\mathrm{sensels/s}}{1936\,\mathrm{sensels/frame}}\approx129.13\,\mathrm{frames/s}
$$


### 09章证据表

|速率信息|来源|可否当新设备实测|
|---|---|---|
|200Hz|Zvyagin旧设备设定|否|
|250000 sensels/s|厂家通用技术页|否|
|TekDAQ220完整帧率|本轮未知|HOLD|


**对应来源**：S58-01,S58-02,S58-05,S58-09；**对应决策**：D58-08。证据等级详见文末完整来源表。


**可证伪检查点 09：**为什么不能把250000直接当作压力图帧率？ **当前判断：**单位是sensel/s而不是frame/s，且不是指定TekDAQ220配置的实测参数。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 10. 可执行合同、消融和失败反例

新增纯标准库`bench_evidence_gate.py`，只读检查三路独立参考、每区最坏全载荷容量、原生单位、每区校准、机械隔离、串扰、纳秒时钟、会话和科研许可。256个固定人工案例：128完整设计元数据、128分别注入八类缺口。弱基线只查总力，错误接收128/128；强基线查三路与容量，错误接收96/128；R58证据Gate错误接收0/128，且128完整案例均接受为**DESIGN_PRECHECK_ONLY**。人工故障标签预先确定，绝非传感器性能真值。所有结果始终`HOLD_NO_MOTION`。


### 10章公式（与另一份文档一致）

$$
\mathrm{Gate}_{\mathrm{design}}=C_{\mathrm{capacity}}\land C_{\mathrm{independence}}\land C_{\mathrm{calibration}}\land C_{\mathrm{clock}}\land C_{\mathrm{license}}
$$


### 10章证据表

|方法|人工完整接受/128|人工缺陷误收/128|
|---|---|---|
|仅总力|128|128|
|三路+容量强基线|128|96|
|R58合同Gate|128|0|


**对应来源**：S58-05,S58-06；**对应决策**：D58-09。证据等级详见文末完整来源表。


**可证伪检查点 10：**R58的0次误收是否证明真实压力安全？ **当前判断：**不证明。仅在预先设计的人工元数据缺陷类别中逻辑正确，未测真实压力或碰撞。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 11. DEV三入口移交与安全边界

本轮实际读取DEV-R30合成RGB-D源码与对应测试文件，并验证整包1417个内部SHA。建议三入口统一增加只读`pressure_truth_evidence`字段：源类型、原生单位、逐帧SHA、int64 ns时间、时钟域、每区容量余量、独立性、校准ID、串扰上界、许可、拒绝理由。M1用户有序3D选点，M2真实画线输入契约/方向确认，M3专家审核候选与模板均保留；统一路径位置、切向、法向、置信度、有限宽工具宽轴/边缘、WORK/UNLOAD/OFFSURFACE_RETURN语义不变。法兰、TCP和柔性真实接触线不等同。


### 11章公式（与另一份文档一致）

$$
\mathrm{Trajectory}_{\mathrm{approved}}\Rightarrow\mathrm{path\_geometry};\quad \mathrm{path\_geometry}\not\Rightarrow\mathrm{physical\_pressure\_truth}
$$


### 11章证据表

|入口|已知离线能力|压力真值状态|
|---|---|---|
|M1|人工3D路径/合成曲面|UNKNOWN|
|M2|合成RGB-D曲面/扫掠|UNKNOWN|
|M3|专家审核点图/合成曲面|UNKNOWN|


**对应来源**：S58-08,L58-02；**对应决策**：D58-10。证据等级详见文末完整来源表。


**可证伪检查点 11：**M2曲面路线生成成功为何仍显示HOLD？ **当前判断：**路径几何和局部压力真值是不同证据；FR5接口/物理参考仍未验证。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 12. 停止/退出Gate与下轮最小实验

当前I-A仍暂停扩模，I-B仍等待FR5实际反馈时序；主学术Baseline保持VPEC→FEATS→ViPER→ImplicitRDP→RDP，不强制选赢家。下一最小任务是只读询证I-Scan10/TekDAQ220与5027/5051的原始ASCII样例、counts映射、时间/许可，再设计三个独立单轴力台的非人体载荷盲测和串扰测试。若厂家不能提供合法可获取的空间原始数据或独立力台无法证明隔离，停止局部压力精度宣称；可继续DEV离线状态机与可视化，但不因此转向未经验证的实机力控。禁止连接、命令机器人、ROS2运动发布、未经授权物理模型和真人实验。


### 12章公式（与另一份文档一致）

$$
\mathrm{Resume}(I\!A)=\mathrm{raw}_{3z}\land\mathrm{calibration}\land\mathrm{clock}\land\mathrm{license}\land\mathrm{blind\_test}
$$


### 12章证据表

|研究方向|当前状态|最小退出条件|
|---|---|---|
|I-A局部载荷|PAUSE_FOR_EVIDENCE|独立非人体三区真值|
|I-B延迟换向|HOLD_FOR_FR5_TIMING|厂家兼容+真实只读时序|


**对应来源**：S58-01,S58-05,S58-08；**对应决策**：D58-11,D58-12。证据等级详见文末完整来源表。


**可证伪检查点 12：**为什么不立即选一篇主Baseline？ **当前判断：**独立物理真值、许可、原版可复现性和FR5数据率仍不足，强行排名会造成虚假确定性。。若取得新独立非人体物理证据，必须更新本章Gate与原始数据版本。


## 附录A：完整来源级别与可获取性

# AUTO-R58 来源级别与真实可获取性（2026-10-21）

级别 A=本轮实际核读论文原版全文方法/实验或原厂完整技术规格；B=本轮直接读取官方网页/手册；C=摘要/搜索片段；L=本地文件字节已读且SHA/CRC通过。所有外部数据仍未形成**本项目独立非人体三区物理原始真值**。厂家宣称兼容不是当前配置经过验证；网页宣传的采样速度不是可重现帧时钟。

|ID|来源及元数据、访问位置|本轮层级|实际证据及缺口|
|---|---|---|---|
|S58-01|Tekscan, *I-Scan 10 with TekDAQ 220*，**2026-10-15**，https://www.tekscan.com/news/tekscan-launches-i-scan-10-new-tekdaq-220-electronics-platform|B，实际读取2026-10-15原厂发布|宣布兼容既有matrix传感器、USB-C、抗ESD/噪声；未提供5027/5051配置的逐帧率、时钟/外触发、原生counts导出和授权合同。|
|S58-02|Tekscan, *I-Scan 10* 产品页，https://www.tekscan.com/i-scan-10-thin-film-pressure-mapping-surface-sensing-system|B，实际读取|支持ASCII导出、压力图/力/面积分析；**未承诺原生8-bit矩阵、时间戳、每帧完整导出**，不能据此认定实验数据可复现。|
|S58-03|Tekscan, 5027产品规格，https://www.tekscan.com/products-solutions/pressure-mapping-sensors/pressure-mapping-sensor-5027|B，实际读取|44×44，0.025in pitch，0.008in row/col width，50/500psi；实际动态峰值和特定放大器采样未测。|
|S58-04|Tekscan, 5051产品规格，https://www.tekscan.com/products-solutions/pressure-mapping-sensors/pressure-mapping-sensor-5051|B，实际读取|44×44，0.050in pitch，0.030in width，7/50/350等psi档；同型号不同档不能混淆。|
|S58-05|Petr Zvyagin, *Tactile Pressure Sensors Calibration with the Use of High Pressure Zones*, Sensors 2022, 22(19), 7290, DOI 10.3390/s22197290，https://pdfs.semanticscholar.org/2e4a/66816514e902cd09be15a4e3014b93c3f045.pdf|**A，实际打开16页PDF，核读§2/§3/§5/§6、表1/2与数据声明**|5051-350、两传感器、2/5/10kg砝码、200Hz实际设定、8bit原始sensel、150 patches中72%积分载荷误差<5%；**不能证明逐区独立真值**，原始CSV未取得。论文CC BY 4.0。|
|S58-06|SparkFun, *Mini Load Cell 500g TAL221 SEN-14728*，https://www.sparkfun.com/mini-load-cell-500g-straight-bar-tal221.html|B，官方产品页|500g nominal≈4.903N/区，只有候选规格；实际装配/校准/串扰/数据时钟均未验证。|
|S58-07|SparkFun, *TAL221* 数据表，https://cdn.sparkfun.com/assets/9/9/a/f/3/TAL221.pdf|C，PDF检索到容量100–1500g和结构，**未本轮逐页核读**|不能将同族容量或网上误差推导当单个500g设备计量证书。|
|S58-08|FAIRINO, *Collaborative Robot User Manual 4.0.0*, Python Force Control §12，https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html|B，实际读取官方接口|FT_SetConfig厂商17仅明确0-KWR75B，KWR75D/[1,17,3,0]仍缺确认；厂家FT_Control是待实测工程强baseline，不执行。|
|S58-09|Tekscan, *Technology*，https://www.tekscan.com/company/technology|B，实际读取|通用系统提到250,000 sensels/s；不等于I-Scan10 TekDAQ220在5027/5051下可达到的完整帧率。|
|S58-10|Yuan et al., *Bending and Stretching-Insensitive, Crosstalk-Free, Flexible Pressure Sensor Arrays for Human-Machine Interactions*, Adv. Mater. Technol. 2024, DOI 10.1002/admt.202301615，https://advanced.onlinelibrary.wiley.com/doi/abs/10.1002/admt.202301615|C，摘要及元数据可读，全文跳转摘要|曲率/拉伸机械耦合为真实问题，但不能将摘要的抗干扰结果移植到商业5027/5051。|
|L58-01|AUTO-R57完整研究ZIP与12项外部可挂载文件，`R58_PREVIOUS_ARCHIVE_AUDIT.json`|L，35内部SHA、CRC通过，12外部SHA匹配，实际读取报告/讲义/register/决策/源码/测试|R57新增75项本轮实际定向复跑；非历史所有测试。|
|L58-02|DEV-R30完整源码ZIP，`R58_PREVIOUS_ARCHIVE_AUDIT.json`|L，1417内部SHA、CRC通过，实际只读`rgbd_curved_surface.py`及其测试|DEV代码**未修改**，历史DEV完整回归本轮未复跑；AUTO研究保持独立。|



## 附录B：完整决策记录与退出Gate

# AUTO-R58 决策与退出Gate（相对AUTO-R57）

|ID|新证据或独立测试|决策/接口改变|最小补证或停止条件|
|---|---|---|---|
|D58-01|R57归档35内部SHA、12外部SHA，R30源码1417内部SHA通过；R57 75项定向复跑|沿已验证R57推进，R58只写独立AUTO包，不更改DEV|任何SHA/CRC失败则不得声称继承|
|D58-02|Tekscan **2026-10-15**发布I-Scan10/TekDAQ220|5027/5051采购技术问询改为“传感器+TekDAQ220+I-Scan10+导出授权”的**完整配置**|供应商书面确认具体型号、配套、可导出原始矩阵、合法科研许可|
|D58-03|I-Scan10官网仅承诺ASCII导出|ASCII不等于原生counts、逐帧纳秒时间戳或外触发|取得去标识的真实ASCII样例+列定义+原生counts映射+同步说明|
|D58-04|5027 50psi；5051 7/50psi等，3N/20mm/2mm假设=75kPa|量程必须按**峰值**而非仅平均；5051-7psi对该假设平均已超量程|曲面接触条带宽、峰值、饱和及低端噪声校准|
|D58-05|SparkFun 100g vs 500g候选，3N集中一分区反例|独立三区台架每个区须按**全载荷集中**校核容量，而非F_total/3|额定容量、偏载、装配和过载证书；不可直接购买当合格设备|
|D58-06|解析三区(1,1,1)与(1.5,0,1.5) N同总力和力矩|腕F/T闭合不能充当三区真值；新增三区独立采集契约|三独立通道、机械隔离、串扰上界、逐区计量校准|
|D58-07|Zvyagin 2022 PDF全文：5051-350、200Hz、8bit、150 patch|校准积分力<5%不能证明每格/三区误差<5%；不采用论文采样率作为TekDAQ220率|独立盲测区载荷、曲面/速率、原始数据|
|D58-08|通用250k sensels/s ÷1936=129.13fps为条件上界算术|不得把20kHz扫描宣传或200Hz旧实验写成新阵列真实帧率|实测5027/5051完整帧时间戳、丢帧、与F/T同步|
|D58-09|256人工元数据案例，弱基线误收128，强基线误收96，R58误收0|交付离线`bench_evidence_gate.py`供DEV只读展示HOLD原因|非人体真实物理数据前不得将人工Gate通过称作压力算法准确|
|D58-10|R30源码已核，三入口人工曲面/合成RGB-D仍为离线|向DEV移交统一`pressure_truth_evidence`与`HOLD_NO_MOTION`，不写DEV|DEV可自愿按独立版本引入，只读UI不得运动|
|D58-11|实际非人体独立三区原始真值仍0条|I-A保持`PAUSE_FOR_EVIDENCE`，不继续LP/EKF/MPC/ESO|取得原生单位、逐区参考、曲率、时间、许可、独立盲测才恢复|
|D58-12|FR5/KWR75D/[1,17,3,0]兼容与反馈率未知|I-B保持`HOLD_FOR_FR5_TIMING`，FT_Control为未来厂家强baseline|官方兼容矩阵+授权非人体只读日志，风险审批前不接触/运动|



## 附录C：五篇主学术Baseline跨轮登记与竞争关系

# AUTO-R58 跨轮BASELINE_AND_INNOVATION_REGISTER

**审计继承说明：**下方完整保留R57已核验的五篇元数据和研究缺口；文末为R58新证据与排名变化。历史R57段落中“本轮”指R57，不代表R58重做了相应网络核查。

# AUTO-R57 跨轮BASELINE_AND_INNOVATION_REGISTER

**AUTO-R57实审R56归档后增量维护；主学术baseline仍未选定，I-A维持`PAUSE_FOR_EVIDENCE`。** 排名只代表当前复现/审计优先顺序，不代表论文性能优劣。R56原登记实际已读并核验；本轮未再次下载任何作者训练集/权重/代码，固定commit和许可仅按R55可校验归档**继承但未在线重验**。每个候选都必须对齐FR5 4.0.0、SDK2.3.0的实际反馈接口与采样时钟才能转入实体研究。

|排名 R56→R57|论文元数据（作者、题名、年份、DOI、全文级别）|原版代码/数据/许可/公式与公平复现缺口|FR5可执行性、2025–26竞争与独立真值|
|---|---|---|---|
|1→1 VPEC|Patrick Grady, Jeremy Collins, Manu Brahmbhatt, Christopher Twigg, Nathaniel Tang, James Hays, Charles C. Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*，IROS2022，`10.1109/IROS47612.2022.9982073`；R52曾核全文，本轮**未重读**|继承作者代码commit前缀`371b9ec2`、MIT；原始约170GB数据/权重SHA/episode分组/1736 counts到Pa独立校准缺；软夹爪RGB→压力图，依赖外部视觉可见性|FR5 D435外部视角及柔性刮板不同；不依赖厂家低层力矩控制，但必须校准/时钟；2026曲率/动态压力仪器可靠性为直接实验竞争。**HOLD_DATA**|
|2→2 FEATS|Erik Helmut, Luca Dziarski, Niklas Funk, Boris Belousov, Jan Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*，IROS2025，`10.1109/IROS60139.2025.11246486`；R53全文，本轮未重读|继承作者代码commit`7d1ae43…`、MIT、HF数据卡MIT；240×320 GelSight→24×32×3力分布，标签**FEA**而非独立空间力台；模型权重/数据SHA未本轮获取|传感器视角与D435不同，推理频率不能冒充FR5实际闭环频率；可复现方法学但不能作独立三区物理真值。**HOLD_PHYSICAL_TRUTH**|
|3→3 ViPER|Jeremy A. Collins, Cody Houff, Patrick Grady, Charles C. Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*，IROS2023，`10.1109/IROS55552.2023.10342124`；R52源码审，本轮未重读|继承作者代码commit`08c7d2fc…`；原始许可、权重SHA、文件名排序配对/时间偏差与压力原生单位未证明；避免不可信pickle|需D435/腕F/T同步和独立区域真值；不因文件数量一致而授予时间可信。**HOLD_LICENSE_CLOCK**|
|4→4 ImplicitRDP|Wendi Chen, Han Xue, Yi Wang, Fangyuan Zhou, Jun Lv, Yang Jin, Shirun Tang, Chuan Wen, Cewu Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*，arXiv2025/RA-L2026，`10.1109/LRA.2026.3710031`；本轮未读全文|源码/许可/权重/数据/原始训练测试划分及具体控制率未锁定|视觉-力觉策略动作频率与FR5 SDK实际接口未知；不能在禁运动环境测试；**HOLD_HARDWARE**|
|5→5 RDP|Han Xue, Jieji Ren, Wendi Chen, Gu Zhang, Yuan Fang, Guoying Gu, Huazhe Xu, Cewu Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*，RSS2025，`10.15607/RSS.2025.XXI.052`（继承、**本轮未核DOI**）；本轮未读全文|作者代码/许可/训练数据/权重/策略动作率/episode split未本轮锁定|高速触觉反馈不可由双RTX3090直接等同于FR5闭环；**HOLD_HARDWARE**|

## 新邻近研究和新强Baseline（非五篇主论文排名）

- **Petr Zvyagin (2022)**，`10.3390/s22197290`：真实非人体砝码和Tekscan 5051校准patch；作者报告150个校准patch中72%误差<5%，并未获得独立三区盲测。全文索引可见方法/实验，直接网页429；**适合设计标定协议，不是刮板主学术baseline**。
- **Parmar, Khodasevych, Troynikov (2017)**，`10.3390/s17081923`：软腿模型+砝码，8小时漂移/动态10循环，证实时间和量程改变精度；与FR5不同；全文索引可见，未取得原始数据。
- **Thompson et al. (2026)**，`10.1016/j.ergon.2026.103969`：SSRN摘要称比较Pliance/I-Scan/XSENSOR在平面与曲面、30–90kPa、30min条件下误差/漂移；**只读摘要**，全文403，不推断谁最优。
- **Tekscan Wiper + 9920**：厂家确实提供刀片线载荷测量、1×128 sensel，间距6.4mm、最大69kPa；**R56实证否决其在20mm/3区保守两sensel/区预筛下的直接使用**。对20mm×2mm、3N的纯假设，平均75kPa>69kPa。若厂商提供更细节距或其他有效布置可重审。
- **XSENSOR**：厂商列部分系列最细约1mm；**不是确定型号、也非已取得独立数据**。需厂家确认曲面标定、空间面积、峰值量程、时钟和导出授权。

## 厂家/简单强Baseline

`FT_Control`厂家恒力 + 保守M1/M2/M3三维曲面路径、有限宽已观测格点覆盖、WORK→UNLOAD→OFFSURFACE_RETURN 是将来非人体基准，但目前**不连接**。官方Python 4.0.0文档`FT_SetConfig(company,device,softversion=0,bus=0)`只列坤维17的`device=0 KWR75B`，不能证实KWR75D `[1,17,3,0]`。实际控制时序、反馈率、F/T标定、法兰/TCP/柔性接触线和ROS2边界未验证，全部HOLD。

## 科学路线决策

**I-A（唯一首选）**：有限宽柔性刮板局部载荷的部分可辨识和可信拒绝。**维持PAUSE_FOR_EVIDENCE**，不新增复杂模型。新增可证伪实验门槛：非人体真实独立三区压力/力台原始数组、原生counts/Pa/N/单格面积、曲率/方位、相机/F/T/阵列同钟、合法科研许可、独立跨episode盲测和公平简单/学术/厂家Baseline。即使整体力与总力矩完全一致，也可能有不同三区分布，不能以总力闭合代替局部真值。

**I-B（唯一备选）**：WORK→UNLOAD→OFFSURFACE_RETURN换向延迟鲁棒柔顺控制。**维持HOLD_FOR_FR5_TIMING**，必须先拿到FR5实际接口与非人体动态力台时序证据，I-A暂停不自动让I-B胜出。

**R55→R56差分**：五篇候选不变；R55解决DEV-R27路径点之间的采样覆盖缺口，R56核实**真实商业刀片线载荷仪器的尺寸/节距/量程**，并用独立解析算术证明现成9920不能不经核查就当20mm三区压力真值；给DEV增加只读bench-evidence状态契约。主论文尚未选定。

## AUTO-R57 新证据对五候选的影响（无虚假排名变化）

|候选|训练/测试泄露与独立真值|固定代码/许可与可运行条件|最小补证、2025–2026竞争和排名变化|
|---|---|---|---|
|VPEC|必须按独立episode、工具/材料和传感器划分；原生counts→Pa厂家计量未证|R56继承commit `371b9ec2`、MIT；本轮未在线重核，禁止运行不可信权重|2025–26曲面压力校准与高分辨空间阵列提出更严格真值要求；保持第1，需一条非人体独立三区原始数据+校准+许可|
|FEATS|FEA空间标签不是独立物理压力；需真实阵列盲测和序列级分组|R56继承`7d1ae43…`、MIT、HF数据卡MIT；不执行外部pickle|与2025 Jiang等大面积触觉研究有相邻压力估计竞争，但视角不同；保持第2，需原始独立力台+区域压力|
|ViPER|按文件名排序不能代替时间同步；训练与测试须会话隔离|R56继承`08c7d2fc…`；许可证/原始文件可得性未确认|保持第3，需实际episode时间戳、原生压力标定和可合法访问数据|
|ImplicitRDP|动作策略需已知反馈率/时钟/训练测试物体隔离|代码固定版本/许可/依赖/权重未核，FR5扭矩接口不可假设|保持第4，需官方支持的离线策略接口和公开测试集；禁止机器人动作|
|RDP|视觉触觉跨接触episode泛化与高速执行率不可外推|代码固定版本/许可/依赖/权重未核，DOI按R56继承未本轮重核|保持第5，需固定代码、许可证、可复现实验与FR5采样验证|

## AUTO-R57 新的仪器竞争、强baseline和可证伪创新Gate

**Tekscan 5027**原厂PDF为44×44、0.635mm pitch、0.2032mm名义单格宽、345/3448kPa量程档；**5051**为44×44、1.27mm pitch、0.762mm宽、48kPa起多个量程档。对20mm三区，两者按R56“每区至少2中心”预筛均通过（保守下界分别10、5），但两者均**不具有已证实的非人体空间压力真值资格**。电子串扰、实际接触传播、传感器变形、峰值量程、曲率、原生counts、时钟、许可证、独立盲测全部HOLD。5027名义活跃条带更窄，不能把“更密”当“已证明更可靠”。

**2024 Sizer等全文**明确软泡沫与硬基底对比时加载速率为20 vs 0.5mm/min（40倍），且没有测滞回，原始数据商业敏感不可下载。其结果对动态软表面校准提供一个**实验混杂反例**，不是直接竞争主学术算法的性能证据。2026 PRISM只说明RGB-D/F/T/触觉同步，不提供刮板三区独立空间真值证明。

**厂家FT_Control/简单可靠强baseline**仍为将来非人体测试候选，官方4.0.0 Python文档有明确签名与N/Nm单位，但`17`厂家仅列`0-KWR75B`，不证明`KWR75D [1,17,3,0]`；真实数据率、运动权限、控制稳定性未知。`FT_GetForceTorqueOrigin`“raw”是力矩输出，不等于原生ADC counts。当前只读分析，不运行厂家示例。

**I-A唯一首选**仍为有限宽柔性刮板局部载荷部分可辨识与可信拒绝，维持`PAUSE_FOR_EVIDENCE`。**I-B唯一备选**为WORK→UNLOAD→OFFSURFACE_RETURN换向延迟鲁棒柔顺控制，维持`HOLD_FOR_FR5_TIMING`。新Gate：厂家几何预筛通过只可进入**台架设计候选**，不能解除缺少独立非人体物理真值的暂停；若厂家不能提供可获取的原生空间数据/校准/许可，下一次应优先考虑独立三分区力台设计而不是新增模型。

---

## AUTO-R58增量（已核R57源码ZIP，排名与唯一科学问题不变）

**R57→R58五篇排序：VPEC 1→1，FEATS 2→2，ViPER 3→3，ImplicitRDP 4→4，RDP 5→5。** 上表作者/题名/年份/DOI、源码commit、许可与训练/测试缺口属于**R57已核档案继承，本轮没有重新在线核实五篇原版代码/权重**，绝不声称它们已在FR5运行。R58增加的是非人体**独立三区力台+压力阵列双仪器**的可证伪条件：空间压力膜自身的三块积分不是独立真值；只有经隔离与串扰盲测的三路参考可构成三区力的独立比较。

- **VPEC（第1）**：视觉预测可用独立三区载荷比较，必须同时固定图像、腕F/T、时钟和工具几何；1736 counts/N的物理校准仍缺；保留主候选但不得只用总力验证。
- **FEATS（第2）**：有限元生成空间标签不是实测真值；新的三区参考尤其适合作为模拟到真实反例，原版MIT代码版本按R57继承而非本轮重验。
- **ViPER（第3）**：时间错配/文件名配对风险持续；新I-Scan10必须确认帧级原生时间戳，否则不适合公平对照。
- **ImplicitRDP（第4）**与**RDP（第5）**：策略/控制实际执行率与FR5可用力矩接口仍未知；不以Tekscan宣传采样速度推定控制率。

**新增仪器/校准强对照**：2026-10-15 Tekscan I-Scan10/TekDAQ220宣布兼容既有矩阵传感器，ASCII导出在产品页可见；**原生counts、帧率、同步与科研再发布许可均未确认**。2022 Zvyagin论文PDF本轮完整核读：5051-350在作者旧VersaTek/I-Scan配置**设为200Hz**，采用8bit感测单元、2/5/10kg砝码和多patch拟合；150个校准patch约72%积分载荷误差<5%，**不是三区局部力误差<5%**。100g TAL221单区额定约0.981N在纯假设3N集中单区时超载；500g TAL221约4.903N名义容量通过3N×1.2的预筛，仍需实际装配/串扰/校准。FT_Control依然厂家未来强baseline，KWR75D配置和真实时序继续HOLD。

**创新Gate不变**：I-A `PAUSE_FOR_EVIDENCE`，I-B `HOLD_FOR_FR5_TIMING`；主论文不强选。独立真实非人体三区原始数据仍为**0条**。R58新可审计源码和合成256案例只检查证据合同，绝不当成论文算法压力精度、真实碰撞安全或硬件认证。



## 附录D：可复现命令、局限与安全声明

解压完整研究ZIP后执行：`python -m pytest -q test_r58.py`、`python -m pytest -q test_r57.py`、`python run_r58_ablation.py --out R58_ABLATION_REBUILT.json`、`python verify_r58_documents.py`。所有脚本仅用Python标准库（测试需pytest），没有机器人连接或运动功能。新增36项+继承R57新增75项在本轮实际执行；DEV-R30仅归档/源码只读审核，未执行其完整工程回归。独立压力真值=0；实验均是厂家资料/作者论文/人工数学/人工元数据四种不同层级。