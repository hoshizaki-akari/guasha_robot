# AUTO-R59 FAIRINO FR5刮痧机器人博士研究报告（独立完整）

**日期**：2026-10-22；**验收对象**：离线研究内容，不是机器人或人体安全验收。**状态**：PASS_OFFLINE_RESEARCH_CONTENT_ONLY（166项定向测试及双文档QA完成，非物理验收）。

**核心新发现**：固定3DCal作者代码公开helper存在分组后图像/标签索引错配路径；Tekscan官方帧率资料适用范围未消歧；接触区MAE远高于全图MAE。研究首选继续PAUSE_FOR_EVIDENCE。

## 目录
- 01. 归档、继承与真实差分
- 02. 唯一首选科学问题与不可辨识边界
- 03. 真实非人体数据资格与许可
- 04. Tekscan 5051与新采集平台的帧率证据冲突
- 05. 短时事件的独立采样反例
- 06. 3D Cal全文方法与可迁移的标定协议
- 07. 3D Cal全文实验：接触区误差与评价偏差
- 08. 固定作者源码的分组后标签错配反例
- 09. 作者代码许可、依赖与原始数据可复现Gate
- 10. FR5 SDK力控与真实反馈有效性边界
- 11. 本轮源码/消融/异常拒绝与DEV接口
- 12. 科学决策、退出Gate与里程碑边界


## 研究边界与术语约定

本轮研究对象是**非人体、离线**的20mm名义宽度柔性刮板验证链。M1为用户有序3D点，M2为实体画线及起终方向确认的输入合同（当前只有合成演示），M3为专家审核且可编辑的解剖候选3D点图（非临床识别）。人工审批后统一输出3D位置、切向、曲面法向、置信度、有限宽工具边缘与扫掠资格、WORK/UNLOAD/OFFSURFACE_RETURN。法兰、TCP和真实柔性接触线不是同一个几何实体；单腕wrench不能唯一反演局部压力。相机、手眼、原点、力偏置、重力与动态响应校准必须在非人体实体实验前完成。所有ROS2/FR5运动、FT_Control、物理仿真、真人试验保持HOLD。


## 01. 归档、继承与真实差分

### 本章决策问题

本轮究竟从哪个经过字节校验的状态出发？

### 实际核验的新旧证据

本地AUTO-R58研究ZIP SHA256=683dfe84f80298e3381898df2a186553939fab75beafcc762026e6cae41976f4；37个成员、36项内部SHA均匹配，CRC无损。DEV-R31 ZIP SHA256=fcc59e33bc763596c2bac0e083f3275938dd8537c7d89cd07401fb2126ab3b2c；1433个成员CRC通过。已实读R58研究报告、零基础讲义、register、决策、失败、源码与测试，以及R31离线动作回放源码。

### 方法、公式与可审计路径

使用本地真实文件字节计算SHA256，先校验R58包内SHA256SUMS，再校验可访问的外部清单文件，最后检查DEV-R31 ZIP CRC并提取离线回放源文件SHA。此步骤独立于任何历史自述；R58以前的状态只能作为经R58继承记录，而不能冒充本轮逐字节重新核查。R31只是只读接口参考，研究链路不修改DEV文件。

**本章公式F01**：

$$
\operatorname{ArchivePASS}=\bigwedge_{f\in\mathcal M}[\operatorname{SHA256}(f)=h_f]\land\operatorname{CRC}_{ZIP}
$$

### 本章证据表T01

|对象|实审证据/数值|结论或限制|
|---|---|---|
|R58研究ZIP|36/36内部SHA+CRC|可继承|
|DEV-R31 ZIP|1433项CRC，离线回放只读|不计DEV测试|
|R59差分|新代码语义反证/帧率证据/接触区指标|独立验收|


### 同数据Baseline、独立真值与反例

R58已有三区力台证据合同和腕部wrench不可辨识反例；R59新发现是固定作者源码分组后标签行号错配、厂家帧率资料适用范围未消歧、全文接触区误差显著高于全图误差。这三项直接影响复现、仪器选型与验收指标。

**失败/不可外推边界**：若ZIP路径不存在、SHA不匹配或只能看到旧回复而读不到文件，则本轮不得宣称继承，也不得递增AUTO编号。

### 决策与来源

决策ID：`R59-D01、R59-D02`。来源：`L01、L02`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：为什么仅看到上一轮“111项通过”的文字不够？

**答案**：因为它不是当前运行时的字节与退出码证据；需要读到实际ZIP、SHA、测试日志并核验。


## 02. 唯一首选科学问题与不可辨识边界

### 本章决策问题

为什么腕部六维力不能唯一确定20mm柔性刮板的三区局部载荷？

### 实际核验的新旧证据

R58已经有独立解析反例：对称三分区(1,1,1)N和(1.5,0,1.5)N具有相同总法向力3N和相同关于中心的力矩0；真实独立非人体三区真值仍为0条。R59没有伪造任何物理记录。

### 方法、公式与可审计路径

将柔性接触线沿工具宽轴划成左中右三个区域。若只观察法向总力与关于中心的一个力矩，三项未知量只受两个独立约束，存在非零零空间方向。视觉、工具姿态、配准和时间信息可能提供附加约束，但不能因为模型输出了三区数字就宣称唯一可辨识。应报告“在给定观测和误差条件下的部分可辨识集合”及拒绝条件，且只在有独立三路物理参考时评价误差。

**本章公式F02**：

$$
F_\Sigma=F_L+F_C+F_R,\quad M=a(F_R-F_L),\quad (1,1,1)\sim(1.5,0,1.5)
$$

### 本章证据表T02

|对象|实审证据/数值|结论或限制|
|---|---|---|
|单腕部wrench|整体力/矩|不唯一|
|三独立参考|三区N与校准/串扰|未取得|
|首选创新|部分可辨识+可信拒绝|PAUSE_FOR_EVIDENCE|


### 同数据Baseline、独立真值与反例

简单baseline可将3N均分为每区1N，但它对(1.5,0,1.5)N的真实分布会给出错误的中心载荷。强工程baseline将来是厂家FT_Control加保守路径，但FT_Control只调整体力，并不直接提供三区真值。

**失败/不可外推边界**：不能把整体力闭合或合成三分区真值当成真实压力空间分布；法兰、TCP和柔性实际接触线必须分别建模。

### 决策与来源

决策ID：`R59-D10`。来源：`L01、S08`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：两组总力都为3N，为什么中间区可能差1N？

**答案**：因为左右对称变化相互抵消了总力和中心力矩，整体测量缺少分辨左右/中间重分配的独立信息。


## 03. 真实非人体数据资格与许可

### 本章决策问题

哪些“可下载数据”仍然不能充当博士主实验的局部压力真值？

### 实际核验的新旧证据

3D Cal Zenodo v2.0.0页面列出DIGIT 36,630张、GelSight Mini 36,270张和ReSkin 3,780条真实采集记录；标签目标为探针位置/几何深度而非独立三区压力。Zenodo权利栏仅显示Copyright，未看到明确再利用许可；小型ReSkin tar.gz下载因DNS失败，未取得原始字节。

### 方法、公式与可审计路径

把“网页可访问”“许可明确”“字节实际读取”“原生单位”“独立物理真值”分成五个不同Gate。3D Cal的球探针坐标和CAD形状是真实非人体标定的有用先例，但与压力膜每个格子的独立Pa/N测量不同。未来若取得压力矩阵，必须同时取得逐帧counts、每格面积、曲率、标定版本、传感器/电子设备序列号、合法科研使用条款和盲测会话分组。预训练`.pth`可能经Python反序列化加载，不在本轮执行。

**本章公式F03**：

$$
F_{zone}=\sum_{i\in zone}p_iA_i\quad\text{仅在 }p_i,A_i\text{已独立标定时有物理意义}
$$

### 本章证据表T03

|对象|实审证据/数值|结论或限制|
|---|---|---|
|3D Cal真实探针图像|可见数据目录、未实读字节|几何标定参考|
|独立三区N/Pa|0条|核心阻塞|
|权利/权重|Copyright/不执行pth|HOLD_REUSE|


### 同数据Baseline、独立真值与反例

相比“有数据集就能训练”的弱判断，R59采用可获取性与用途双审查：即使几何真值可靠，也不自动成为局部载荷真值；即使网页标为Open，也不能从Copyright字段推出无限制重发布许可。

**失败/不可外推边界**：不下载不可信pickle/dill并运行；不把作者训练标签或模拟FEA标签冒充独立压力台架真值。

### 决策与来源

决策ID：`R59-D05、R59-D07`。来源：`S05、S07、S10`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：3D Cal有真实图像，为什么不能直接评价三区载荷？

**答案**：真实图像的存在只证明图像采集；其标签是深度/位置，不是独立计量的左中右压力或力。


## 04. Tekscan 5051与新采集平台的帧率证据冲突

### 本章决策问题

2026年新TekDAQ220是否已证明能以200Hz采集5051的1936个格子？

### 实际核验的新旧证据

Tekscan官方Electronics页写5051在all electronics最大100Hz；官方FAQ又写USB最高100Hz、VersaTek 250Hz、高速接口20kHz；2026-10-15发布的I-Scan10/TekDAQ220公告确认兼容旧矩阵但未给5051组合的实际完整帧率。2022旧论文设定200Hz不能推定2026新硬件。

### 方法、公式与可审计路径

先按传感器型号、量程档、矩阵规模、电子采集器、固件、连接方式与软件版本组成完整配置键，再索取逐帧真实时间戳和原生导出样例。100与250可能对应不同硬件世代或适用范围，不能凭网页矛盾断言厂家错误；也不能任取对研究有利的较高值。1936格和“每秒百万格”是通道吞吐指标，非必然的完整帧率，因为扫描、传输、滤波、缓存均有开销。

**本章公式F04**：

$$
f_{frame}\leq\frac{S_{sensels/s}}{1936}\quad\text{只是理想吞吐上界，非实测帧率}
$$

### 本章证据表T04

|对象|实审证据/数值|结论或限制|
|---|---|---|
|官方Electronics|5051 all electronics 100Hz|适用年代待核|
|官方FAQ|USB100/VersaTek250/高速20k|接口依赖|
|TekDAQ220发布|2026-10-15，兼容旧矩阵|5051实测率UNKNOWN|


### 同数据Baseline、独立真值与反例

弱baseline只读产品宣传“高达20kHz”；强baseline至少按传感器格数和采集器区分；R59证据Gate还要求实际帧时间与短事件响应。当前5051×TekDAQ220标记UNKNOWN而非100、200或250。

**失败/不可外推边界**：将旧VersaTek论文200Hz、宣传20kHz或机器人SDK周期直接写入新压力阵列时间戳字段，会制造虚假的时间可辨识能力。

### 决策与来源

决策ID：`R59-D03`。来源：`S01、S02、S03、S04`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：为什么1936格不能直接用1,000,000÷1936得出厂家实测帧率？

**答案**：这个商是理想格点吞吐上界；实际传输、扫描顺序、滤波、协议和具体电子接口都可能降低完整帧率。


## 05. 短时事件的独立采样反例

### 本章决策问题

采样率标称足够时，为什么仍可能漏掉短时卸载峰值？

### 实际核验的新旧证据

本轮对理想矩形脉冲做解析相位扫描：2ms脉冲在100Hz理想点采样中相位捕获概率20%，250Hz为50%，500Hz为100%；5ms脉冲在100Hz为50%、250Hz为100%。这只是人工理想模型，不是Tekscan实测漏检率。

### 方法、公式与可审计路径

假设等间隔采样周期T=1/f，矩形事件宽度τ，相位在一个采样周期内均匀分布，且样本是瞬时点值。若τ<T，只有τ/T比例的相位能落入事件；若τ≥T，理想情况下至少有一个点采到。真实压力膜是有响应时间、滤波、非同步扫描和丢帧的物理系统，必须用独立已知输入脉冲和同钟参考测量，不能把理想概率当作实际安全率。

**本章公式F05**：

$$
P_{detect}^{ideal}=\min(1,f_s\tau),\qquad T=1/f_s
$$

### 本章证据表T05

|对象|实审证据/数值|结论或限制|
|---|---|---|
|2ms/100Hz|20%理想相位捕获|非硬件率|
|2ms/250Hz|50%理想相位捕获|仍可漏|
|5ms/250Hz|100%理想点采样|仍需动态标定|


### 同数据Baseline、独立真值与反例

在同样事件输入和相位条件下，弱baseline仅看名义采样率；R59先把短事件检测作为必要条件，再强制逐帧ns时间戳、丢帧证据和参考时钟。反例不引入EKF/MPC等模型，只修改实验Gate。

**失败/不可外推边界**：真实低通传感器即使在数学上至少采到一点，也可能因幅值响应不足而漏报；时钟不一致会破坏上述假设。

### 决策与来源

决策ID：`R59-D04`。来源：`S01、S02、T02`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：2ms事件、250Hz采样的周期是多少，理想相位捕获多少？

**答案**：周期4ms，事件占半个周期，因此在均匀相位的理想点采样模型中为50%。


## 06. 3D Cal全文方法与可迁移的标定协议

### 本章决策问题

哪一部分3D Cal方法可以帮助非人体刮板实验，哪一部分不能直接迁移？

### 实际核验的新旧证据

本轮实际核读Kota等8页论文§II/III：Ender3打印机作为自动探针平台，半径2mm球头，0.5mm网格，DIGIT 1221位置、GelSight Mini 1209位置，每点30张图；TouchNet输入RGB+XY嵌入共5通道，输出梯度并通过Poisson积分得到深度。

### 方法、公式与可审计路径

真正可迁移的是“把传感器固定在已知坐标系、重复探针位置、记录探针半径/位置/深度/批次、按空间位置做训练验证划分”的实验方法。不可直接迁移的是其RGB触觉相机网络、梯度到深度模型和已发表深度误差：FR5外部D435、柔性刮板实际接触线、三区法向力与GelSight/DIGIT传感器的光学几何完全不同。3D打印机本身也属于运动设备，本轮仅核读方法，没有运行作者控制打印机的程序。

**本章公式F06**：

$$
(R,G,B,x,y)\xrightarrow{\mathrm{TouchNet}}(G_x,G_y)\xrightarrow{\mathrm{Poisson}}\hat z
$$

### 本章证据表T06

|对象|实审证据/数值|结论或限制|
|---|---|---|
|标定工具|3D打印机+2mm球头|方法可借鉴|
|训练目标|图像→深度梯度|不是Pa/N|
|FR5迁移|只读协议和校准清单|不运行设备|


### 同数据Baseline、独立真值与反例

与手工随机按压相比，已知探针轨迹可提高几何可重复性；但“重复按压”不等于有可溯源压力真值。未来三区参考台架需要独立力传感器、曲面、时间与串扰校准。

**失败/不可外推边界**：不能把TouchNet输出的深度直接称为局部压力，也不能在没有工具/曲面标定时拿其网络作为FR5压力主baseline。

### 决策与来源

决策ID：`R59-D05`。来源：`S05、S10`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：TouchNet输出梯度后为何还要积分？

**答案**：梯度描述表面每个位置的斜率，Poisson积分把局部斜率整合为相对高度/深度图；它没有生成力单位。


## 07. 3D Cal全文实验：接触区误差与评价偏差

### 本章决策问题

全图平均误差为什么可能掩盖接触区域的真实偏差？

### 实际核验的新旧证据

论文Table II六组作者聚合结果：DIGIT半球全图16.984µm/接触区107.127µm；DIGIT pawn 52.211/296.381µm；GelSight Mini pill 23.641/152.846µm等。R59独立复核6组接触区Type2/全图MAE比值，均>1，平均约6.01。

### 方法、公式与可审计路径

论文先按空间坐标分组训练/验证，未见物体为半球、药丸状和棋子状CAD物体；将CAD STL转深度图作为几何真值，手工压入后用2D互相关对齐XY，并调整CAD深度以最小化预测与真值的MSE。这是合理的几何对齐研究做法，但意味着所报误差是对齐后的深度重建误差，不是独立力台误差。Type1为空白区、Type2为真实接触区；大面积空白像素可拉低全图平均。

**本章公式F07**：

$$
\rho=\frac{\mathrm{MAE}_{contact}}{\mathrm{MAE}_{all}}\quad(\text{六组均}>1,\ \bar\rho\approx6.01)
$$

### 本章证据表T07

|对象|实审证据/数值|结论或限制|
|---|---|---|
|DIGIT hemispheres|107.127/16.984≈6.31|接触区更差|
|DIGIT pawn|296.381/52.211≈5.68|几何复杂|
|6组总体|比值均>1，均值≈6.01|不代表压力精度|


### 同数据Baseline、独立真值与反例

弱baseline仅报告全图MAE；R59强制同时报告接触区MAE、三区分位误差、最坏区和拒绝率。作者论文Table II只是重新算比值，不是下载原始图像后重训。

**失败/不可外推边界**：不能将对齐后CAD深度误差作为无偏的原始压力预测误差；更不能只展示总体平均来证明接触线局部可靠。

### 决策与来源

决策ID：`R59-D08`。来源：`S05、S06、T02`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：若空白区域占多数，全图平均误差会发生什么？

**答案**：低误差空白像素稀释少量高误差接触像素，因此全图MAE可能显著小于接触区MAE。


## 08. 固定作者源码的分组后标签错配反例

### 本章决策问题

3D Cal按坐标分组避免泄露，却为什么可能在公开helper中错配图像和标签？

### 实际核验的新旧证据

只读核查作者GitHub固定commit fd04a700...：`split_dataset`先对唯一(x_mm,y_mm)做train_test_split，再pd.merge返回过滤后的self.data；`TactileSensorDataset.__getitem__`图像从self.data.iloc[idx]取，但`get_gradient_map(idx,annotation_path)`重新读取原始全量CSV的第idx行x_px/y_px。

### 方法、公式与可审计路径

如果原CSV顺序是A、B、C，验证集只包含C，验证集self.data.iloc[0]对应C图像，而get_gradient_map(0)取原CSV第0行A坐标，目标就错配。R59独立用pandas/sklearn重构这种语义，覆盖8种坐标数×4种每坐标帧数×4随机种子=128个人工案例；128/128出现训练错配、128/128出现验证错配；改用原始行ID/过滤后坐标索引的独立Oracle则128/128零错配。坐标训练验证集合仍不重叠，故“分组不泄露”和“图像标签错配”可以同时成立。

**本章公式F08**：

$$
y_{wrong}(i)=g(D_{original}[i]),\quad y_{correct}(i)=g(D_{subset}[i])
$$

### 本章证据表T08

|对象|实审证据/数值|结论或限制|
|---|---|---|
|固定代码版本|commit fd04a700... / MIT|源码已读|
|128组合成划分|训练与验证均128/128有错配|可复现反例|
|原行ID修正|128/128零错配|仅语义修复|


### 同数据Baseline、独立真值与反例

同样人工数据下，作者风格行号baseline总训练错配11115条、验证错配3000条；正确索引baseline零错配。**这不是对作者真实数据或已发表实验的复跑**；不能证明作者论文训练使用了这个版本，需作者澄清训练脚本/版本。

**失败/不可外推边界**：如果论文发表训练用不同代码或手动修复过索引，不能由当前公开仓库缺陷推断论文结论失效。禁止运行作者训练权重或G-code控制。

### 决策与来源

决策ID：`R59-D06`。来源：`S06、S05、T01`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：坐标分组无泄露，是否必然图像标签正确？

**答案**：不必然。分组只控制哪些坐标进入训练/验证，目标索引仍可能错误地指向原始CSV的另一行。


## 09. 作者代码许可、依赖与原始数据可复现Gate

### 本章决策问题

如何在不运行外部未知程序的前提下审计学术baseline？

### 实际核验的新旧证据

固定commit 2026-03-16，setup.py声明py3dcal 1.1.0、MIT；源码依赖torch>=2、pandas>=2、scikit-learn>=1等范围版本，不是固定环境。Zenodo v2.0.0约6.9GB，`.pth`权重和压缩包有MD5，但本轮没有下载/加载；权利字段显示Copyright。

### 方法、公式与可审计路径

先保存论文DOI、作者、正式/预印本版本、代码commit、文件blob SHA、数据记录版本和授权条款。复现应锁定Python/torch/scikit-learn/Pandas版本、原始图像SHA、split坐标列表、随机种子和训练命令；尤其不能仅凭“代码MIT”推断“数据也MIT”。只有可合法使用且安全解析的数据才可进入离线训练；不可信pickle、dill或未知torch.load权重不运行。

**本章公式F09**：

$$
\mathrm{Reproducible}=\mathrm{codeSHA}\land\mathrm{dataSHA}\land\mathrm{splitID}\land\mathrm{license}\land\mathrm{units}
$$

### 本章证据表T09

|对象|实审证据/数值|结论或限制|
|---|---|---|
|代码|MIT、固定commit已核|可读不运行|
|数据|Zenodo元数据/版权|未获许可结论|
|环境|setup.py依赖下界|需固定锁文件|


### 同数据Baseline、独立真值与反例

与“pip install后直接跑作者脚本”相比，固定源文件只读审查已经发现标签错配路径，且不触发3D打印机/机器人。此审计可作为VPEC、FEATS、ViPER未来主baseline候选的统一最低要求。

**失败/不可外推边界**：未知数据许可或训练/测试split不明时不能宣布学术主baseline可复现；公开仓库可见不等于允许修改用户仓库。

### 决策与来源

决策ID：`R59-D07`。来源：`S06、S07、S10`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：为什么代码MIT不能自动覆盖Zenodo数据？

**答案**：软件代码和数据是不同版权对象，需分别查看数据授权；网页显示Copyright不足以推出开放重用许可。


## 10. FR5 SDK力控与真实反馈有效性边界

### 本章决策问题

为什么FT_GetConfig成功不等于KWR75D实际力数据可信？

### 实际核验的新旧证据

FAIRINO 4.0.0 Python文档FT_SetConfig列Kunwei company17、device0=KWR75B，未列KWR75D；提供FT_GetForceTorqueOrigin/RCS接口。GitHub官方仓库Issue#20有第三方用户报告配置成功但UI与SDK F/T持续零；未经核实，不代表本项目实机状态。

### 方法、公式与可审计路径

将“型号/固件兼容”“传感器激活/零偏”“真实六维wrench”“有效采样时间”“载荷/重力补偿”“闭环FT_Control权限”拆成不同证据层。厂家示例包含Robot.RPC、FT_Activate和FT_SetZero，不在本研究环境运行。未来只能由获授权的非人体设备负责人先获取厂家书面KWR75D/[1,17,3,0]兼容矩阵和只读回传，再用已知静态/动态加载核对力、力矩、单位、时间和零漂；无此证据保持HOLD。

**本章公式F10**：

$$
\mathrm{ValidFT}=\mathrm{Compatible}\land\mathrm{Calibrated}\land\mathrm{ObservedChange}\land\mathrm{ClockVerified}
$$

### 本章证据表T10

|对象|实审证据/数值|结论或限制|
|---|---|---|
|官方4.0.0|17/0=KWR75B|KWR75D未知|
|Issue#20|第三方报告F/T为零|非官方实测|
|本项目|未连接/未取反馈|全HOLD|


### 同数据Baseline、独立真值与反例

弱baseline把配置回读当成有效F/T；强baseline要求零载、已知载荷和动态变化均可辨、校准可追溯；厂家FT_Control虽是未来工程强baseline，但当前无执行资格。

**失败/不可外推边界**：不能将配置号[1,17,3,0]未经确认映射成已支持KWR75D；更不能发送FT_Control或任何运动指令验证。

### 决策与来源

决策ID：`R59-D09`。来源：`S08、S09`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：为什么一直读到0N不能证明“接触力确实为0”？

**答案**：可能是传感器未激活、数据字段未更新、型号不兼容、配置错误或真零载；必须用独立已知载荷和时间变化辨别。


## 11. 本轮源码/消融/异常拒绝与DEV接口

### 本章决策问题

哪些可执行证据真正能交给DEV，而不是数学层数？

### 实际核验的新旧证据

R59新增author_split_audit.py、acquisition_evidence_gate.py、independent_sampling_oracle.py和论文表格审计。128组坐标分组人工案例发现固定代码语义错配；128组采集元数据案例弱baseline误收64/64缺陷，强baseline误收48/64，完整证据Gate误收0/64。所有完整人工案例仍仅METADATA_PRECHECK_ONLY，不授予物理或运动资格。

### 方法、公式与可审计路径

对同一人工输入运行三个判断：弱法只认型号/采集器；强法加实际帧率及最短事件必要条件；完整Gate再核int64 ns单调、帧间隙、原生counts SHA、校准、同步、独立三区参考、会话、许可和外部物理审计。独立真值是预先注入的故障清单和解析相位/原始行ID，不使用Gate自身输出。DEV-R31离线动作回放已实际读源码，下一步只建议在M1/M2/M3的JSON/HTML显示HOLD理由，不修改其轨迹或安全适配器。

**本章公式F11**：

$$
\Delta t_i=(t_i-t_{i-1})\cdot10^{-9}\ \mathrm{s},\quad\Delta t_i>0
$$

### 本章证据表T11

|对象|实审证据/数值|结论或限制|
|---|---|---|
|分组索引反例|128/128发现错配|作者代码待修|
|采集证据消融|64正常/64故障；全Gate 0误收|人工元数据|
|DEV接口|read-only evidence + HOLD|未写DEV|


### 同数据Baseline、独立真值与反例

R58侧重三区台架容量与证据合同；R59解决“数据标签是否对得上”“真实帧率是否知道”“接触区误差是否被全图掩盖”。这比增加一层控制优化更直接改变学术复现和整机可验证接口。

**失败/不可外推边界**：即使元数据全部完整也不等于实体压力真值；不能因166项定向测试通过就声称历史全部测试或机器人安全通过。

### 决策与来源

决策ID：`R59-D12`。来源：`T01、T02、L02`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：为什么full Gate对64个完整案例也不能输出“机器人可动”？

**答案**：因为案例只是人工元数据，真实压力原始字节、计量和硬件审批没有获得；Gate只证明软件拒绝逻辑。


## 12. 科学决策、退出Gate与里程碑边界

### 本章决策问题

何时应暂停首选创新，何时能开始备选控制研究？

### 实际核验的新旧证据

五篇主候选VPEC→FEATS→ViPER→ImplicitRDP→RDP的复现优先排序不变；R59新3DCal是标定方法学邻近研究，不抢占主论文。独立三区非人体原始真值0条，KWR75D真实反馈/时序未证，故I-A继续PAUSE_FOR_EVIDENCE、I-B继续HOLD_FOR_FR5_TIMING。

### 方法、公式与可审计路径

I-A退出暂停至少需要1条合法非人体完整会话：三路独立参考、原生counts/Pa/N、曲率、有限宽工具姿态、手眼/原点/重力偏置、int64 ns时间与许可、跨会话盲测。I-B需要FR5只读真实反馈与延迟、厂家兼容书面证明、独立非人体故障参考，且先与简单WORK-UNLOAD-OFFSURFACE_RETURN强baseline比较。二者都不允许越过DisabledRobotAdapter/ROS2只读边界；无伦理批准不做真人。

**本章公式F12**：

$$
\mathrm{PhysicalGate}=G_{truth}\land G_{cal}\land G_{clock}\land G_{license}\land G_{geometry}\land G_{safety}
$$

### 本章证据表T12

|对象|实审证据/数值|结论或限制|
|---|---|---|
|I-A|PAUSE_FOR_EVIDENCE|0条独立真值|
|I-B|HOLD_FOR_FR5_TIMING|实际反馈未证|
|R60里程碑|独立六文档审查|不重复计数|


### 同数据Baseline、独立真值与反例

继续堆叠数学模块既不能修复作者数据标签错配，也不能创造厂家实际采样率或独立真值。下轮若没有新反证、修复、数据或选型变化，应标记未通过内容Gate，不增AUTO号。R60等第10轮节点由独立里程碑任务审查六份开题文档，不能在普通研究轮重复计数。

**失败/不可外推边界**：没有任何研究成果可以绕过用户批准、非人体验证、FR5真实接口审查或伦理要求；物理仿真不得冒充独立物理真值。

### 决策与来源

决策ID：`R59-D10、R59-D11、R59-D12`。来源：`L01、L02、S08`。完整链接、级别与访问限制见本报告附录来源表。

### 研究自检题与答案

**题目**：如果下轮只新增100个合成测试但没有新反证或接口进展，能增AUTO号吗？

**答案**：不能。内容Gate要求决策性新证据、解决问题或可复现反例，合成数量本身不等于实质推进。



## 附录A：本轮证据、来源与实测限制

外部10组去重资源的完整URL、出版日期、访问层级与固定代码SHA列于随包`SOURCE_REGISTER.md`；重要来源：Tekscan厂家Electronics https://www.tekscan.com/electronics 、厂家FAQ https://www.tekscan.com/support/faqs/what-are-sensors-sampling-rates 、2026-10-15新平台公告 https://www.tekscan.com/news/tekscan-launches-i-scan-10-new-tekdaq-220-electronics-platform 、Kota等2026 RA-L DOI `10.1109/LRA.2026.3673994`及8页全文 https://arxiv.org/pdf/2511.03078 、固定MIT作者代码 https://github.com/rohankotanu/py3DCal/commit/fd04a700eca70f3618a8b25ce8458c12276253c3 、Zenodo数据 DOI `10.5281/zenodo.18462608`、FAIRINO官方4.0.0 https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html 。

本轮本地实验分类：`AUTHOR_TABLE_ARITHMETIC`=论文作者表格重新算术；`SYNTHETIC_CODE_SEMANTICS`=128组合成索引错配；`SYNTHETIC_CONTRACT`=128组人工元数据拒绝；`IDEAL_POINT_SAMPLING`=解析相位采样；`PHYSICAL_NONHUMAN_ZONE_TRUTH`=**0条**；`ROBOT_HARDWARE`=未连接；`PHYSICAL_SIMULATION`=未运行；`HUMAN`=禁止。没有将合成数值称为物理真值。

## 附录B：可复现命令与文件清单

在研究ZIP解压目录中运行：

```bash
python -m pytest -q test_r59.py
python author_split_audit.py > R59_SPLIT_ABLATION_REBUILT.json
python run_r59_evidence_ablation.py > R59_EVIDENCE_ABLATION_REBUILT.json
python independent_sampling_oracle.py > R59_SAMPLING_ABLATION_REBUILT.json
python paper_contact_metric_audit.py > R59_PAPER_TABLE_AUDIT_REBUILT.json
python verify_r59_documents.py
```

上述命令只使用本地Python/pytest/pandas/scikit-learn，不访问设备或网络，不执行作者源码、`.pth`、pickle/dill，不写GitHub。R57/R58定向回归见独立子目录和JUnit；R19历史LP/DEV全量未执行。

## 附录C：实验优先级与长期停止规则

第一优先：获取Tekscan 5051×TekDAQ220实际组合的书面采样/时钟/原生导出与许可；第二优先：合法非人体独立三区力台原始数据及曲率/标定；第三优先：固定学术baseline的原始数据、索引修复和会话分组；第四优先：向DEV移交只读HOLD原因可视化。若下一次没有新反证、实际数据、问题解决或决定性选型变化，内容Gate应判不通过、不提升轮号。R60节点由独立里程碑审查六份开题文档，不在本研究轮重复计算。


## 附录D：10组外部来源的独立阅读导航与证据级别

|ID|实际阅读资源|级别|使用限制|
|---|---|---|---|
|S01|Tekscan Electronics：https://www.tekscan.com/electronics|L2厂家|5051“all electronics 100Hz”，不能推定新TekDAQ220|
|S02|Tekscan Sampling FAQ：https://www.tekscan.com/support/faqs/what-are-sensors-sampling-rates|L2厂家|USB100/VersaTek250/high-speed20k，需具体组合|
|S03|Tekscan 2026-10-15公告：https://www.tekscan.com/news/tekscan-launches-i-scan-10-new-tekdaq-220-electronics-platform|L2厂家|未给5051新组合帧率|
|S04|Tekscan 5051：https://www.tekscan.com/products-solutions/pressure-mapping-sensors/5051|L2厂家|44×44，具体量程待定|
|S05|Kota等全文：https://arxiv.org/pdf/2511.03078|L1全文8页|几何深度，非独立局部压力|
|S06|作者代码固定commit：https://github.com/rohankotanu/py3DCal/commit/fd04a700eca70f3618a8b25ce8458c12276253c3|L1源码|MIT；未执行作者代码，分组后索引缺陷待作者确认|
|S07|作者Zenodo数据：https://zenodo.org/records/18462608|L2元数据|Rights仅Copyright；未下载原始字节|
|S08|FAIRINO Python 4.0.0：https://manual.fairino.support/latest/SDKManual/PythonRobotForceControl.html|L1官方API|Kunwei17/0=KWR75B；KWR75D未知|
|S09|SDK Issue#20：https://github.com/FAIR-INNOVATION/fairino-python-sdk/issues/20|L3用户报告|F/T全零未独立核验|
|S10|3D Cal项目页：https://rohankotanu.github.io/3DCal/|L2作者入口|代码/数据不等于本项目物理真值|

## 附录E：五篇主学术Baseline的元数据、复现与公平真值门槛

|排名|作者与论文、年份、DOI|原版代码/数据/许可/公式风险|FR5及2025–26竞争、最小补证|
|---|---|---|---|
|1 VPEC|Grady、Collins、Brahmbhatt、Twigg、Tang、Hays、Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*；2022；10.1109/IROS47612.2022.9982073|继承MIT代码commit前缀371b9ec2，170GB级数据/episode split、counts→Pa校准待核；视觉压力图|D435外部视角与夹爪不同；需真实独立三区压力和合法固定版本|
|2 FEATS|Helmut、Dziarski、Funk、Belousov、Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*；2025；10.1109/IROS60139.2025.11246486|继承MIT commit前缀7d1ae43；FEA空间力标签不是独立物理真值；训练测试会话分组待核|触觉相机不是FR5 D435；需非人体盲测和固定权重SHA|
|3 ViPER|Collins、Houff、Grady、Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*；2023；10.1109/IROS55552.2023.10342124|继承代码前缀08c7d2fc；许可、原生单位、图像/力时钟映射待核|需独立三路参考、传感器会话隔离及合法原始数据|
|4 ImplicitRDP|Chen、Xue、Wang、Zhou、Lv、Jin、Tang、Wen、Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*；2025/2026；10.1109/LRA.2026.3710031|原版commit/许可/训练测试/动作率未锁|FR5 SDK真实力矩/闭环接口未核；禁止运动，暂不选为主baseline|
|5 RDP|Xue、Ren、Chen、Zhang、Fang、Gu、Xu、Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*；RSS2025；10.15607/RSS.2025.XXI.052（继承未本轮重核）|原版commit/许可/权重/数据划分未锁|GPU推理率不等于FR5执行率；需要合法固定数据和非人体实测|

上述五篇的原始作者/DOI/代码信息来自**实际读取的R58跨轮登记表**，不是本轮逐篇重新联网全文核验；当前排序仅为复现审计优先级，不强制选赢家。厂家FT_Control+保守几何与简单总力均分均保留为未来公平对照，但目前硬件Gate未解除。

## 附录F：实际执行范围与不计数项

R59新增测试55/55，R58继承定向36/36，R57继承定向75/75，合计**166/166项**；分三个独立pytest进程，非全部历史研究/DEV测试。R57首次两次收集因复制依赖遗漏失败，补齐后75/75成功；不把失败尝试计入通过。作者3DCal代码未执行、原始数据未下载、FR5与ROS2未连接。具体JUnit、人工消融JSON、失败清单与校验脚本均随独立ZIP提供。对人工结果不宣称物理压力准确率。