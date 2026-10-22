# AUTO-R16｜FAIRINO FR5刮痧机器人博士级完整研究报告：多点载荷交互导致单点满秩校准失效
> 日期：2026-10-20（Asia/Shanghai）。**离线数学/代码与文献内容验收通过；FR5实体Gate E0–E5未过。** 没有连接机器人、没有发运动指令、没有真人试验。
> 主目标：动态选择可复现主学术论文 + 厂家工程强Baseline + 唯一有证据的创新；所有选型保持待定。


## 01. 上一轮真实文件、测试与差分

实际读取R15研究报告、逐章教学讲义、BASELINE_AND_INNOVATION_REGISTER、DECISION_RECORD和非人体台架协议；读取R15包内QA、来源登记、算法及测试。文件哈希写入`prior/FILES_CHECKED.json`，不是仅引用上一轮口头总结。以正确PYTHONPATH重跑R15全部45项单元测试，45/45通过；第一次在错误模块路径下运行失败，已将原始错误及修正后的结果如实留档，未把失败掩盖为成功。R15的数学反例针对几何条件传力矩阵；本轮新问题是：**同一几何、满秩单点标定是否足以覆盖两个边缘同时受载？** R16给出反例、交互项秩判据及留出测试。没有连接机器人。

**E16-01（核心公式/判据）**：

$$G_{16}=G_{prior}\land G_{source}\land G_{derivation}\land G_{code}\land G_{falsification}$$

**T16-01（证据与决策对照表）**

|验收要素|R15已知|R16实际新动作|证据边界|
|---|---|---|---|
|真实文件|R15五个主要文件+ZIP|SHA256与文件大小核对|未重新验证R15所有历史引文|
|回归|R15 45项|本轮重跑45/45|仅软件|
|算法|几何条件LP|满秩单点不足反例+交互设计|合成无量纲|


## 02. 唯一决策问题、可证伪假设与备选

唯一首选Q16-A：在同几何下，有限宽刮板三分区的**同时多点载荷相互作用**能否被独立参考实验限定，使边缘风险上界可信且匹配RGB-D真正带来增益？H16-A：单点标定残差为零不保证多点残差有界；H16-B：经独立多压头留出后，匹配视觉必须优于wrench-only、线性分区及错配视觉。备选Q16-B：厂家恒力+固定限速+平滑卸载+离面返程；只有非人体独立力台证明简单方案不足时才允许提出延迟鲁棒创新。任何假设失败就修订/退出，不为复杂度加EKF、ESO或MPC。

**E16-02（核心公式/判据）**：

$$H_{16A}:\;\exists f_Lf_R>0:\;\|y-\widehat y_{single}\|_\infty>\epsilon_{ind}$$

**T16-02（证据与决策对照表）**

|假设|入门Gate|独立真值|退出Gate|
|---|---|---|---|
|I-A唯一首选|多压头独立载荷/几何/时序校准|各压头力+独立总力台+位置参考|跨条件留出失败或匹配视觉无增益|
|I-B唯一备选|本机厂家FT_Control与失联策略|独立力台与离面轨迹|简单状态机已达预注册要求|


## 03. 14项去重来源与访问层级

本轮实际检索并核对14项高相关论文或官方/作者来源，**2篇开放HTML全文论文与1篇检索索引正文方法/实验片段**核读方法与实验：Chen2026 v3、TensorTouch2025、Jiang2024；另外官方SDK页读函数签名/示例，作者仓库只读README。其余论文仅摘要或历史核读，见逐条访问层级。论文与其作者仓库不是两篇论文；来源数不是全文数。2026 Chen论文早期标题曾为Large-area Tomographic Tactile Skin with Air Pressure Sensing for Improved Force Estimation，R16以2026-08-27 v3实际标题为准。每一条原始网址和层级列在下表，不把二手综述算原文。

**E16-03（核心公式/判据）**：

$$L(s)\in\{\mathrm{fullHTML},\mathrm{abstract},\mathrm{officialDocs},\mathrm{repoREADME},\mathrm{historical}\}$$

**T16-03（证据与决策对照表）**

|ID|作者/来源|标题与年份|DOI/arXiv|本轮获取层级|原始入口|
|---|---|---|---|---|---|
|S16-01|Haofeng Chen; Jiri Kubik; Bedrich Himmel; Matej Hoffmann; Hyosang Lee|Pneumatic-Tomographic Tactile Skin for Multicontact Localization and Force Estimation (2026)|arXiv:2503.13036v3; DOI 10.48550/arXiv.2503.13036|full HTML, sections II-V, methods, calibration, single/multi-contact tests|https://arxiv.org/html/2503.13036|
|S16-02|Won Kyung Do; Matthew Strong; Aiden Swann; Boshu Lei; Monroe Kennedy III|TensorTouch: Calibration of Tactile Sensors for High Resolution Stress Tensor and Deformation for Dexterous Manipulation (2025)|arXiv:2506.08291; DOI 10.48550/arXiv.2506.08291|full HTML, methods III, evaluation V, appendix measurement details|https://arxiv.org/html/2506.08291|
|S16-03|Jinlei Jiang; Jingjing Luo; Hongbo Wang; Xiuhong Tang; Fan Nian; Lizhe Qi|Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment (2024)|DOI 10.3390/act13020062|search-indexed full-body excerpts of sections 3.2/3.3, 4, 5, 6; direct page returned HTTP 429; NOT independently accessed complete HTML|https://www.mdpi.com/2076-0825/13/2/62|
|S16-04|FAIRINO|Python Robot Force Control SDK manual, 3.9.5 website (2026)|documentation site, no DOI|official page, prototype and examples inspected|https://documentation.fairino.co.in/SDKManual/PythonRobotForceControl.html|
|S16-05|FAIR-INNOVATION|fairino-python-sdk (2026)|GitHub repo, no DOI|README and license metadata only|https://github.com/FAIR-INNOVATION/fairino-python-sdk|
|S16-06|Stanford armlab|tensor-touch (2026)|GitHub repo, no DOI|README and root listing only|https://github.com/armlabstanford/tensor-touch|
|S16-07|Healthcare-Robotics|VPEC (2022)|GitHub repo, no DOI|README and root license metadata|https://github.com/Healthcare-Robotics/VPEC|
|S16-08|Healthcare-Robotics|ViPER (2023)|GitHub repo, no DOI|README, installation and data split only|https://github.com/Healthcare-Robotics/ViPER|
|S16-09|Luoyan Zhong; Heather Jin Hee Kim; Dylan P. Losey; Cara M. Nunez|Curvature-Aware Calibration of Tactile Sensors for Accurate Force Estimation on Non-Planar Surfaces (2025)|arXiv:2510.25965; 2026 IEEE Haptics DOI 10.1109/HAPTICS66823.2026.11495507|abstract this run; full read historical R15 only|https://arxiv.org/abs/2510.25965|
|S16-10|Zhuo Chen et al.|UniForce: A Unified Latent Force Model for Robot Manipulation with Diverse Tactile Sensors (2026)|arXiv:2602.01153|abstract only|https://arxiv.org/abs/2602.01153|
|S16-11|Carmelo Sferrazza; Adam Wahlsten; Camill Trueeb; Raffaello D'Andrea|Ground Truth Force Distribution for Learning-Based Tactile Sensing: A Finite Element Approach (2019)|DOI 10.1109/ACCESS.2019.2956882|abstract only this run|https://arxiv.org/abs/1909.04000|
|S16-12|Jin-Hwan Borm; Chia-Hsiang Meng|Determination of Optimal Measurement Configurations for Robot Calibration Based on Observability Measure (1991)|DOI 10.1177/027836499101000106|abstract only, publisher restricted|https://journals.sagepub.com/doi/abs/10.1177/027836499101000106|
|S16-13|Morris R. Driels; Uday S. Pathre|Significance of Observation Strategy on the Design of Robot Calibration Experiments (1990)|DOI 10.1002/rob.4620070206|abstract only, publisher restricted|https://onlinelibrary.wiley.com/doi/abs/10.1002/rob.4620070206|
|S16-14|Mingxuan Li; Lunwei Zhang; Yen Hang Zhou; Tiemin Li; Yao Jiang|EasyCalib: Simple and Low-Cost In-Situ Calibration for Force Reconstruction with Vision-Based Tactile Sensors (2024)|arXiv:2403.10256; version/year metadata inconsistent in aggregators|abstract and bibliographic snippet only|https://arxiv.org/abs/2403.10256|


## 04. 全文深读A：Chen2026空间+总力融合的直接反证

S16-01（arXiv:2503.13036v3，2026-08-27）在II节采用EIT导电变化重建接触ROI，利用气压层给出总量；把总压力按ROI导电变化总和比例分配，并在位置$(x,y)$上拟合二次增益与偏置场。III节使用15mm压头、约1–10N、25位置、1350个校准样本，FH100称重传感器作单点总力参考。IV节跨10/20/25mm压头，位置校正气压法RMSE 0.59N，未校正气压0.91N，EIT-only 1.45–1.48N；多点8种砝码组合，每接触RMSE由0.91降到0.55N，部分接近接触难分割。**EIT采集仅约1.3帧/s，气压20Hz，算法处理约61ms**，不能据此宣称刮板快速换向控制可用。S16-01已覆盖“空间线索+独立标量总力+位置校正”，普通融合新颖性进一步受压。

**E16-04（核心公式/判据）**：

$$\widehat{\Delta P_i}=\frac{C_i}{\sum_j C_j}\Delta P,\quad \widehat F_i=g(x_i,y_i)f(\widehat{\Delta P_i})+b(x_i,y_i)$$

**T16-04（证据与决策对照表）**

|Chen2026核读项|原论文具体条件|FR5迁移限制|
|---|---|---|
|独立标签|单点FH100；多点标定砝码|不是连续刮板压力线真值|
|双模态|EIT空间+气压总量|FR5只有外部RGB-D和腕部F/T|
|频率|EIT约1.3帧/s；气压20Hz|动态峰值不能由此认证|
|结果|单点0.59N；多点0.55N RMSE|只属于原论文装置|


## 05. 全文深读B：TensorTouch的FEM标签与复现资格

S16-02（arXiv:2506.08291，作者仓库标T-RO2026）III节描述独立光学运动捕捉120Hz、ATI Mini45 200Hz、可更换压头、手工双压头与ROS1记录；FEA使用超弹性Yeoh模型，网格输出应力/形变映射为15通道图像，真实ATI总力用于校正仿真。V节主要以ATI合力与mocap接触位置检验，像素级应力来自仿真而非逐像素物理压力阵列。作者仓库S16-06可见TorchHub推理、训练代码说明，但数据仍标待发布，根目录未见LICENSE；本轮没有下载模型或运行推理。相对于VPEC/ViPER，TensorTouch多接触模型更丰富，但硬件依赖内部光学触觉、专门硅胶及FEA，不能原样移植Eye-in-Hand刮板。

**E16-05（核心公式/判据）**：

$$\mathcal D_{train}=\{I_{t},\sigma^{FEM}_{t}\},\qquad F^{ATI}_{t}\approx\int_{A}\sigma^{FEM}_{n,t}\,dA$$

**T16-05（证据与决策对照表）**

|TensorTouch证据|论文/仓库事实|不允许的推断|
|---|---|---|
|外部真值|120Hz mocap与200Hz ATI|不等于每像素压力实测|
|训练标签|FEA应力/形变图|仿真标签非独立压力阵列|
|复现资源|TorchHub推理示例与训练说明|未下载权重、数据未核、许可未见|
|FR5适配|内部光学触觉和ROS1|不等于外部RGB-D+ROS2|


## 06. 正文片段核读C：Jiang2024与FR5外环的冲突

S16-03（DOI 10.3390/act13020062，**本轮仅核读搜索索引正文片段；直达网页HTTP 429，不能称全文已读**）在3.2–3.3节给出积分自适应导纳：环境刚度/位置在线估计、积分补偿力误差，再更新笛卡尔参考轨迹；仿真与实验的外环周期明确写2ms。第5节的RGB-D、六维F/T、机器人控制器通过网络集成，在平面皮肤、肾和心脏仿体上报告特定6N参考力控制结果；它们是**该论文实验**，不是FR5。没有核实可用源码和FR5固件的2ms笛卡尔伺服权限，因此R16仍有条件保留而非默认复现。比较时不能只对“传统AC”，还必须有厂家FT_Control与简单固定限速/卸载状态机。

**E16-06（核心公式/判据）**：

$$f_e+k_i\int_0^t f_e(\tau)d\tau=m(\ddot x_d-\ddot{\hat x}_r)+b(\dot x_d-\dot{\hat x}_r)+k(x_d-\hat x_r)$$

**T16-06（证据与决策对照表）**

|Jiang2024要素|原论文|FR5未满足的前提|
|---|---|---|
|算法|积分自适应导纳+在线环境估计|外环反馈与控制权限未知|
|时序|2ms闭环|ROS2消息时间戳不等于采集时间|
|实验|特定超声仿体6N任务|不能移植力阈值和带宽|
|公平对照|传统导纳AC|必须补厂家FT_Control和简单限速|


## 07. 其他来源与厂家官方接口审计

S16-04官网3.9.5页面列出`FT_Control(flag,sensor_id,select,ft,ft_pid,adj_sign,ILC_sign,max_dis,max_ang,...)`，示例同时含力控和MoveL；但同站Python SDK引言声明适用于SDK-v2.0.0，网页版本不等于安装版本，示例的力和速度**绝不可直接执行**。S16-05官方Python仓库README可见Apache-2.0许可，未读取本机固件。S16-09 Zhong2025摘要/历史全文证明曲率校准非新颖；S16-10 UniForce2026摘要证明跨触觉表征与力平衡先例；S16-11 Sferrazza2019摘要提醒FEM标签不是独立压力真值；S16-12 Borm1991与S16-13 Driels1990仅摘要已讨论观测配置设计；S16-14 EasyCalib仅摘要。以上没有任何作者模型运行或真实FR5连接。

**E16-07（核心公式/判据）**：

$$\mathrm{SDK\_doc\_version}\not\Rightarrow\mathrm{installed\_firmware\_version}$$

**T16-07（证据与决策对照表）**

|审计点|实际来源|风险与决策|
|---|---|---|
|FT_Control|S16-04官方文档函数原型|需本机签名/固件/反馈，只读先行|
|Python SDK|S16-05官方README Apache-2.0|仓库许可不等于模型数据许可|
|曲率/跨模态|S16-09/10摘要与R15历史|普通融合/校准不算新颖|
|可观测性设计|S16-12/13摘要|rank/D-optimal仅工具不是论文主创新|


## 08. 核心新反例：满秩单点标定仍会误判双边载荷

定义真实三区载荷$f=(f_L,f_C,f_R)^T\ge0$，令传感读数$y=f+\gamma f_Lf_R(-1,2,-1)^T$。这是明确标为**人工合成**的非线性交互项，代表“两个边缘同时受载时，部分读数向中间迁移”的可能性，绝不是已测物理定律。只加载一个区时$f_Lf_R=0$，任意单点校准都完美；而$f=(0.4,0.2,0.4),\gamma=1$得到$y=(0.24,0.52,0.24)$。另一种无交互接触$f'=y,\gamma'=0$产生完全相同的三区读数、总力1、对称一阶矩0，但真实边缘份额分别0.80与0.48。**即使R15几何已知且单点校准矩阵满秩，仍不保证多点分布可辨识。**

**E16-08（核心公式/判据）**：

$$y=f+\gamma f_Lf_R(-1,2,-1)^T,\quad e(f)=\frac{f_L+f_R}{\mathbf1^Tf}$$

**T16-08（证据与决策对照表）**

|合成状态|真实三区载荷|读数|总力/一阶矩|真实边缘份额|
|---|---|---|---|---|
|A：双边交互γ=1|0.40/0.20/0.40|0.24/0.52/0.24|1/0|0.80|
|B：线性γ=0|0.24/0.52/0.24|0.24/0.52/0.24|1/0|0.48|


## 09. 单点标定的秩、条件数和实验设计

若单点传力近似线性，$Y=XB$，$X=[f_L,f_C,f_R,1]$，$B\in\mathbb R^{4\times3}$，必须至少4个线性独立加载条件才能估计截距与三区系数。R16代码检查矩阵秩和条件数，拒绝只加载中央或仅左右对称组合。即使rank=4、条件数有限，也仅说明**线性参数在给定模型内可识别**，不是模型在双接触时正确。离线D-optimal风格贪心子集只帮助安排加载顺序；已有1990/1991观测配置文献，不能作为原创。真实试验需可溯源各压头力，不可用三区待校准传感器自身作为标签。

**E16-09（核心公式/判据）**：

$$X=\begin{bmatrix}f_{L,1}&f_{C,1}&f_{R,1}&1\\\vdots&\vdots&\vdots&\vdots\end{bmatrix},\quad \mathrm{rank}(X)=4$$

**T16-09（证据与决策对照表）**

|加载设计|X的可辨识性|能否证明多点正确|
|---|---|---|
|只压中间|rank<4|不能|
|只做左右对称|rank<4|不能|
|零+左+中+右|rank=4（非退化）|仍不能|
|再做独立双/三点留出|可检验模型失配|仅限被覆盖的条件域|


## 10. 双接触交互项的可辨识性与必要加载

为诊断最小非线性，可扩展特征$X_5=[f_L,f_C,f_R,1,f_Lf_R]$。只有单点时第五列恒为0，故$\mathrm{rank}(X_5)\le4$；加入至少一组独立左+右同时加载并保持其余特征非退化，才可能rank=5。R16新代码对单点交互拟合返回HOLD；加双边样本后，在指定合成模型中可恢复交互系数$(-1,2,-1)$，并对一组独立三点样本作解析对照。但该简单双线性项可能不覆盖摩擦、滞回、材料变化或接触线连续分布，不能据此宣称物理模型成立。

**E16-10（核心公式/判据）**：

$$X_5=[f_L,f_C,f_R,1,f_Lf_R],\quad f_Lf_R=0\;\forall\;single\Rightarrow\mathrm{rank}(X_5)\le4$$

**T16-10（证据与决策对照表）**

|数据条件|交互特征|可估参数|解释边界|
|---|---|---|---|
|全部单点|恒0|γ不可估|线性拟合可能完美|
|至少独立双边|非0|模型内γ可估|需要真值与留出|
|不同材料/动态|未知变化|γ不一定常数|必须分组重新检验|


## 11. 离线源码、测试、独立反例与负结果

`src/multicontact_calibration_audit.py`实现输入验证、rank/条件数、线性标定、独立多点留出残差、两边交互反例、交互扩展模型与最小选点。新测试**47/47**通过，R15回归**45/45**通过。严格反例的单点拟合最大误差约$5.55\times10^{-16}$，双边留出最大通道残差0.32（人工无量纲），独立预设合成残差界0.01时返回HOLD。800组固定种子Dirichlet合成中，错误直接读数比例在合成阈值0.6下有297组错误分析放行；利用**离线已知真实载荷**计算的oracle筛选后为0，但此oracle无法在线使用！真正可部署的结论只是：**如果离线多点验证失败，整类模型不能进入在线放行Gate。**

**E16-11（核心公式/判据）**：

$$r_{heldout}=\|Y_{heldout}-X_{heldout}\widehat B\|_\infty,\quad r_{heldout}>\epsilon_{ind}\Rightarrow\mathrm{HOLD}$$

**T16-11（证据与决策对照表）**

|离线验收|本轮实得|证据性质|
|---|---|---|
|新增测试|47/47|代码逻辑|
|R15回归|45/45|历史算法兼容|
|单点拟合最大误差|约5.55e-16|解析合成|
|双边最大残差|0.32|解析合成|
|800组合成直接比例误判|297|指定随机模型|
|实体FR5/压力阵列|0次|E4/E5未过|


## 12. 公平对照、消融与统计判据

同一非人体仿体、同一压头、同一材料、同一轨迹、同一力台和同一时间参考，比较B0腕部wrench-only、B1直接三区比例、B2跨几何线性标定、B3独立多点标定模型、B4 B3+匹配RGB-D、B5 B3+错配RGB-D。主要指标为独立真值边缘风险上界覆盖、区间宽度、拒绝率和实际错误放行；辅助指标为接触线位置、法向与卸载峰值。训练/测试必须按完整加载组和材料分组，不能把同一次加载的相邻视频帧随机分配。实验判据与样本量必须在真实噪声预实验后预注册；不把合成0.6阈值变成人体安全阈值。

**E16-12（核心公式/判据）**：

$$\Delta_{vision}=\mathrm{Metric}(B3)-\mathrm{Metric}(B4),\quad \Delta_{shuffle}=\mathrm{Metric}(B3)-\mathrm{Metric}(B5)$$

**T16-12（证据与决策对照表）**

|公平对照|唯一改变|必须观察|
|---|---|---|
|B0→B1|新增三区读数|是否有独立真值增益|
|B1→B2|几何标定|跨几何泛化|
|B2→B3|多点交互校准|多接触留出残差|
|B3→B4|匹配RGB-D|可证实的额外信息|
|B3→B5|错配RGB-D|排除数据泄漏/伪相关|


## 13. 非人体独立真值台架与多压头协议

R15提出三区机械隔离、独立总力与跨几何标定；R16新增**每个压头独立施加载荷参考**和**同时多点加载矩阵**。零载、左/中/右单点用于线性参数识别；左+右、左+中、中+右及三点作为锁定留出；再跨曲率、倾角、材料、温度、加载/卸载次序和日期验证。单点参考可用经校验的静态砝码，但须验证传力摩擦与重力方向；双压头需要独立载荷路径或独立力传感器，不能仅靠三区读数反推标签。独立总力台用于闭合校验而不是代替逐区真值。数据模板与详细协议随包提供；**本轮一条真实记录也没有**。

**E16-13（核心公式/判据）**：

$$|F_{total}^{ref}-(F_L^{ref}+F_C^{ref}+F_R^{ref})|\le\epsilon_{closure}$$

**T16-13（证据与决策对照表）**

|台架阶段|真实独立参考|必要结果|
|---|---|---|
|单点|每压头力+位置|rank、偏置、重复性|
|双/三点|每压头力+独立总力|交互与力闭合|
|跨条件|曲率/材料/倾角/温度|留出泛化/误差包络|
|动态|同步高速参考|峰值带宽与时延|


## 14. 主学术论文Baseline竞争与排序

本轮保留至少五个可比较的主论文候选：VPEC、ViPER、Jiang2024、Chen2026、TensorTouch以及TacGraph。竞争表逐项标注作者、题名、年份、DOI、访问层级、源码/数据/许可、数学/控制接口、FR5硬件与时序、独立真值和最小补证。**主论文不锁定**：VPEC源码MIT与评估入口优势仍在，但模型权重/170GB数据未取得；ViPER与Eye-in-Hand形式更接近，但软夹爪可见性与许可未解决；Jiang适合法向力控但2ms外环无法直接移植；Chen直接反证普通空间总力融合的新颖性但1.3帧/s和额外EIT硬件不适合当前FR5；TensorTouch推理入口公开但FEM像素标签及许可/数据仍受限。厂家FT_Control单独作为工程强基线。

**E16-14（核心公式/判据）**：

$$\mathrm{eligible}=\mathrm{source}\land\mathrm{license}\land\mathrm{interface}\land\mathrm{data}\land\mathrm{truth}$$

**T16-14（证据与决策对照表）**

|候选主论文|年份/DOI与获取层级|源码/数据/许可/版本|数学、重要参数及FR5兼容性|真值与2025–26竞争|相对R15及最小补证|
|---|---|---|---|---|---|
|VPEC（Grady等，Visual Pressure Estimation and Control for Soft Robotic Grippers）|2022；10.1109/IROS47612.2022.9982073；R07历史全文，R16作者README|MIT，Python3.10/PyTorch1.12.1，170GB数据及权重未下载，evaluator可见|RGB可见软夹爪→压力图，非FR5；GPU可能但Ubuntu22.04兼容未核，非低层力矩依赖|压力标签与刮板边缘遮挡未知；ViPER2023已有后续|离线复现资格领先**不变**；先核权重、合法许可和最小评估集|
|ViPER（Collins/Houff/Grady/Kemp，Visual Contact Pressure Estimation for Grippers in the Wild）|2023；10.1109/IROS55552.2023.10342124；R15历史全文，R16 README|训练/采集代码和权重链接可见；Ubuntu20.04/Sensel；许可未核，未运行|Eye-in-Hand RGB+六维F/T弱监督，软夹爪变形可见；ROS2/FR5图像遮挡与频率未核|已有视觉压力估计强反证；独立压力阵列只在全标签域|同层竞争**不变**；需许可证、权重、刮板图像|
|Jiang等（Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment）|2024；10.3390/act13020062；R16索引正文方法/实验片段|源码/数据/许可不明，未复现|积分自适应导纳，论文2ms外环；FR5厂商FT_Control权限和ROS2真实时序未核|软仿体法向力跟踪；非刮板边缘独立真值；Foster2026固定限速竞争|条件力控**不变**；需本机SDK、反馈和外环证明|
|Chen等（Pneumatic-Tomographic Tactile Skin for Multicontact Localization and Force Estimation）|2026更新版；10.48550/arXiv.2503.13036；R16全文|代码/数据/许可未核，未运行|EIT空间+气压标量，空间增益偏置校正；EIT 1.3帧/s，气压20Hz，需额外传感硬件|多接触独立砝码真值，已覆盖位置校正+总力融合；不覆盖FR5动态刮板|**新增直接方法学强竞争**，但原样FR5不适配；补代码与遮挡/动态真值|
|TensorTouch（Do/Strong/Swann/Lei/Kennedy，TensorTouch）|2025预印本/仓库标T-RO2026；10.48550/arXiv.2506.08291；R16全文|作者README公开TorchHub推理与训练说明；数据仍待发，根目录LICENSE未见；未下载权重|内部光学触觉+FEM超弹性+15通道密集输出；120Hz mocap/200Hz ATI，ROS1，FR5外部RGB-D不匹配|ATI仅独立总力/压头位置，像素应力标签来自FEM；不等于真实逐元压力真值|**新增复现观察位**；核许可、模型下载和标注定义|
|TacGraph（Van der Merwe等，Simultaneous Extrinsic Contact and In-Hand Pose Estimation via Distributed Tactile Sensing）|2026；10.1109/LRA.2026.3653324（R15历史）；本轮未重核全文|源码/数据/许可/版本未核|几何+非穿透+力平衡因子图，GelSight/KUKA，非现有FR5配置|单汇总接触点，无完整刮板压力分布真值|方法学强竞争**不变**；补可用源码与传感器匹配|


## 15. 唯一创新点、四候选、入门退出Gate

唯一条件性首选I-A仍是“有限宽刮板边缘风险的部分可辨识估计+经物理校准的空间/时间可信上界+证据不足拒绝”，**不是**普通RGB-D+F/T融合、普通位置校准或完整压力图恢复。R16把“多点交互独立留出”列为I-A新的硬Gate。备选I-B为工作→平滑卸载→离面返程延迟鲁棒控制，只有厂家简单方案在独立力台上失败才升级。摩擦/法向耦合分离缺少独立摩擦真值，暂缓；动态TCP更新已被Arefin等覆盖，不单列创新。所有方向必须明示可观测性假设、误差参考和失败退出。

**E16-15（核心公式/判据）**：

$$I_A\;\mathrm{Gate}=E0\land E1_{single}\land E1_{multi}\land E2_{time}\land E3_{FR5}\land E4_{nonhuman}$$

**T16-15（证据与决策对照表）**

|方向|新颖性反证|可执行性/独立真值|状态|
|---|---|---|---|
|I-A有限宽风险界|Chen/ViPER/TensorTouch已覆盖普通融合/压力估计|离线可运行；实体多压头与RGB-D缺|唯一条件性首选|
|I-B卸载返程鲁棒控制|厂家FT_Control、Jiang、Foster|本机控制接口/独立力台缺|唯一备选|
|摩擦扰动分离|普通wrench摩擦分离已广泛研究|无独立摩擦真值|暂缓|
|动态接触位姿|Arefin/TacGraph已有方法|法兰/TCP/真实接触线需区分|I-A后续功能|


## 16. FR5坐标、时序、工具边缘与E0–E5安全

必须区分基座$B$、法兰$F$、标定工具TCP$T$、柔性刮板实际接触线$C$：$^BT_C=^BT_F\,^FT_T\,^TT_C$，最后一项随接触变形可能变化，不能把法兰姿态当真实接触姿态。腕部wrench在未知纯偶矩、多点分布和摩擦下不能唯一恢复压力或摩擦系数。R12/R13发现官方ROS2软件时间戳不等于精确采集时间，本轮仍无本机日志。E0几何与工具边缘、E1力偏置/重力/独立多点真值、E2曝光/力时钟、E3厂家SDK/固件/FT_Control/失联、E4离面及非人体低能量单向、E5卸载离面返程依次审核。缺证则拒绝纠偏并卸载；真人必须独立伦理与风险审批。

**E16-16（核心公式/判据）**：

$$\mathbf w_C=\begin{bmatrix}\mathbf F\\\mathbf M_F-(\mathbf p_C-\mathbf p_F)\times\mathbf F\end{bmatrix},\quad \mathbf M_C^{unknown}\ne0$$

**T16-16（证据与决策对照表）**

|对象/Gate|已知|尚缺|
|---|---|---|
|法兰/TCP/接触线|概念与变换|实物边缘、手眼与柔性偏载标定|
|F/T与时钟|官方消息与SDK文档|本机偏置、重力、动态补偿与采集时间|
|厂家力控|FT_Control官方原型|本机固件、SDK、外环反馈与失联策略|
|实体/人体|仅非人体预注册方案|E0–E5均未过；真人伦理未批|


## 17. M1/M2/M3三入口、统一3D曲线与状态机

保留M1人工有序选点和指定路线；M2背部实体画线识别、起终点/方向人工确认及有序3D曲线；M3相机看背部、解剖标志与专家审核的穴位候选，构建可信可编辑3D关键点图，用户选两点路线或审核模板自动路线。三入口均统一输出有序3D点、切向、法向、置信度/不确定度、工具姿态、连续有限宽扫掠资格与WORK/UNLOAD/OFFSURFACE_RETURN模式；接触前用户审核。成熟网络/曲面图搜索作上层Baseline，不占主创新。实际接触纠偏只有E0–E4过且可信时才允许，未知几何/力/时序即HOLD并卸载。`schemas/TRAJECTORY_PREVIEW_R16.schema.json`仅静态预览合同，不发运动命令。

**E16-17（核心公式/判据）**：

$$\Gamma=\{(p_i,t_i,n_i,\Sigma_i,R_i,sweep_i,mode_i)\}_{i=1}^N$$

**T16-17（证据与决策对照表）**

|入口|输入与人工确认|统一输出|
|---|---|---|
|M1|人工有序点与路线|审核后的3D曲线|
|M2|实体画线、起终方向确认|审核后的3D曲线|
|M3|解剖标志、专家穴位与模板审核|审核后的3D曲线|
|全部|接触前人工审核|工作/卸载/离面返程及扫掠资格|


## 18. 内容验收、物理缺口与下一轮自适应

本轮真实完成：读取5份R15主文件及包内QA/源码；重跑R15 45项通过；核查14项去重来源，其中2篇开放HTML全文+1篇正文片段方法/实验核读；新增47项测试通过；完成满秩单点不足的严格反例、交互特征rank判据、800组合成与独立留出消融；更新Baseline矩阵、非人体台架协议、决策日志与数据模板。两份Markdown由同一章节索引生成但教学文字独立编写，自动核对18章、18公式、18张表、14来源、9决策及前后章节篇幅比例。**E0–E5实体Gate均未过**：没有FR5连接、真实刮板、RGB-D样张、分区载荷或独立力台数据。下一轮若仍缺硬件，应优先做厂家只读SDK与模型许可/权重资格，而非继续堆合成风险模型。

**E16-18（核心公式/判据）**：

$$\mathrm{research\_accepted}=1,\qquad\mathrm{physical\_validated}=0,\qquad\mathrm{main\_paper}=\mathrm{TBD}$$

**T16-18（证据与决策对照表）**

|内容Gate|本轮结果|仍然缺失|
|---|---|---|
|文献与全文|14来源，2篇全文+1篇正文片段|部分论文仅摘要/许可未核|
|新算法/测试|47新增+45回归、800合成|实体独立真值0|
|文档|18章/18公式/17表/14来源/9决策|教学语义仍需人工审阅|
|路线收敛|淘汰单点满秩即真值捷径|主论文与创新点未锁定|


## 附录A：本轮决策ID索引

# AUTO-R16｜跨轮决策日志（基于可访问证据）

|决策|相对AUTO-R15的新旧差分|证据与访问层级|退出/补证|
|---|---|---|---|
|D16-01|**修订**：R15要求跨几何校准；R16进一步规定必须做**同几何同时双点/三点载荷**校准和独立留出，否则单点拟合不能证明边缘真值|E16-08严格反例、47项离线测试；Chen2026全文多接触对照|真实独立双加载台架若证实严格线性且跨域成立，可简化模型|
|D16-02|**实质性淘汰**“仅靠单点载荷满秩校准就让三区阵列作为边缘真值”的实验捷径|E16-09满秩但双接触失败；R16程序|独立物理证据证明所有预定双接触域相互作用上界足够小|
|D16-03|**新颖性收窄**：空间位置校正+独立总力融合及多接触比例分配已有直接2026论文|S16-01全文II–V|研究只剩可辨识的风险界与证据不足拒绝；若也有直接先例再淘汰|
|D16-04|**排序修订**：Chen2026纳入同层方法学强竞争；TensorTouch2025/2026作为代码/物理标签审计观察位，VPEC/ViPER/Jiang暂不换冠军|S16-01/02全文、S16-06作者仓库|代码/许可、真实可见性和独立压力真值可改变排序|
|D16-05|**工程审查修订**：官网“3.9.5”页与Python SDK-v2.0.0说明并存，FT_Control示例不代表本机固件或安全阈值|S16-04/05官方来源|读取本机版本、只读签名和反馈日志|
|D16-06|**验收限制**：单点与多点合成对照只是模型充分性单元测试，无法证明FR5性能或在线拒绝机制|47新增、45回归，800合成|非人体独立载荷/时钟/相机数据|
|D16-07|**唯一首选I-A条件保留**；备选I-B暂缓，摩擦/动态TCP不新开主线|S16-01/02/03及竞争矩阵|若真实匹配RGB-D不优于wrench-only和错配RGB-D，退出I-A|
|D16-08|**台架入门Gate**：单点零/左/中/右确保rank4；左+右、左+中、中+右和三点是必要独立留出，不能混到训练里|E16-10实验设计与代码检查|独立真实多压头加载装置及每压头参考力|
|D16-09|**保持主论文待定**：VPEC有MIT/评估入口但权重数据未复现；ViPER许可待核；Jiang外环2ms不匹配证据不足|S16-03、S16-07/08|真实FR5 SDK、源码许可、最小评估数据|

本轮不连接FR5、不发送指令、不执行人体试验；未访问或修改任何用户旧仓库，未创建仓库。


## 附录B：可复现运行方式与真实证据状态

`python -m unittest discover -s tests -v`；`python -m src.run_offline_study`。历史回归：`cd prior && PYTHONPATH=src python -m unittest discover -s tests -v`。所有源文件离线，无机器人SDK调用；随机种子20261020，numpy版本由本机环境确定。结果文件见`results/`。无物理数据；压头、RGB-D和FR5尚未接入。