# AUTO-R27｜FAIRINO FR5刮痧机器人博士级完整研究报告

> 日期：2026-10-20。**离线内容验收：通过；FR5实体E0–E5：HOLD。** 这是一份可审计研究报告，不是实机实验报告。

> 唯一条件性首选I-A：有限宽刮板边缘载荷部分可辨识、校准可信包络与失败拒绝；备选I-B：工作—卸载—离面返程延迟鲁棒控制。主学术Baseline仍待定。

## 目录\n- 01 R26实物文件、代码与R27差分核查
- 02 本轮两个决策问题与证据层级
- 03 FEATS全文：局部压力标签究竟来自哪里
- 04 FeelAnyForce全文：独立总力测量的价值与边界
- 05 Peng全文：逆弹性力场也需要材料假设
- 06 Zhong全文：曲率导致校准失效的物理反证
- 07 十二项去重来源与开源资源审计
- 08 五节点有限宽线接触的坐标与可观测性
- 09 新增光学代理的数学含义及不可直接迁移性
- 10 校准增益、偏置与噪声的可行投影
- 11 带误差的LP与参考原点、纯偶矩Gate
- 12 解析独立判据与危险误放行反例
- 13 程序接口、状态机语义与测试证据
- 14 600组人工样本、对照与消融结果
- 15 主论文Baseline竞争矩阵与R26→R27排序
- 16 唯一首选I-A与备选I-B的收敛判断
- 17 FR5厂家强工程Baseline与E0–E5资格
- 18 M1/M2/M3三入口与统一轨迹合同
- 19 非人体独立真值、预注册与公平对照
- 20 资源阻塞、QA、下轮最小补证与研究结论


## 01. R26实物文件、代码与R27差分核查

本轮实际打开R26报告、讲义、决策日志、候选登记表、QA JSON和ZIP，抽取`src/partial_contact_bounds.py`及其测试，执行R26原32项测试；另外实际执行R25的43项、R24的66项回归。首次从错误工作目录运行R25测试导致`ModuleNotFoundError`，改到`r25_extracted/`后43项通过；失败日志与修正命令都记录，不将环境路径错误包装为算法失败。R26 QA JSON字段`chapters=18`，但其`chapter_pairs`含19项，实际报告和讲义均有19个编号二级标题，属于历史QA计数字段不一致，本轮按实际解析结果更正而不修改原件。R26的独立结论是同一wrench可以对应不同边缘载荷；R27要回答**什么新增视觉量才有信息，以及该量的标定误差会否造成危险的虚假放行**。这是R26的必要下一步，不重复声称发现wrench秩亏。

**公式 F27-01：**

$$
N_{\mathrm{prior}}=32+43+66=141,\quad N_{\mathrm{new}}=28,\quad N_{\mathrm{physical}}=0
$$

**表 T27-01｜R26实物文件、代码与R27差分核查**

|证据|R26实际复核|R27新增|可追溯差分|
|---|---|---|---|
|文件|报告/讲义/日志/矩阵/QA/ZIP|原文件只读核查|19章而非QA字段18章|
|代码|R26 LP 32项|新校准包络LP 28项|不修改R26实现|
|设备|无实体数据|仍无实体数据|E0–E5继续HOLD|


## 02. 本轮两个决策问题与证据层级

问题Q27-A：在五节点线载荷模型中，新增独立的形变特征能否把边缘载荷上界从10N收紧？问题Q27-B：当视觉-载荷增益随曲率、材料、传感器个体漂移时，如何阻止错误放行？先区分四种证据：公开论文全文的方法/实验、作者代码与数据卡、离线数学/合成测试、FR5物理真值。本轮前三类有部分证据，第四类没有。任何“视觉特征”都只是**待真实RGB-D标定的假设性代理量**，绝不是已经测得的FR5视觉特征。此处还将FEATS、FeelAnyForce、Peng与Zhong作为直接方法/反证来源，检索并去重12项论文及官方来源。

**公式 F27-02：**

$$
\mathcal E=\{\text{论文全文},\text{官方代码/数据},\text{离线证明},\text{FR5实测}\}
$$

**表 T27-02｜本轮两个决策问题与证据层级**

|决策问题|必须获得的证据|当前状态|
|---|---|---|
|Q27-A|独立标定的局部形变→载荷约束|只有假设模型；HOLD|
|Q27-B|跨曲率/材料的校准包络|只有离线错误注入；HOLD|
|FR5接触|厂家时序/独立力台/安全批准|均未通过|


## 03. FEATS全文：局部压力标签究竟来自哪里

实际核读Helmut、Dziarski、Funk、Belousov、Peters《Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis》arXiv:2411.03315v3（2026-01-13版本）III方法与IV实验；会议为IROS2025，DOI **10.1109/IROS60139.2025.11246486**（会议元数据核查）。FEATS用GelSight Mini内部视觉和CNC压入数据，非线性CalculiX有限元生成每网格3D力标签，U-Net学习图像到标签；14,279个样本，24个压头中19个训练、5个测试，主输出24×32×3。关键反证：**密集局部力标签是FEA推断，不是独立逐格实测**；腕部F/T用于校验总力和材料参数，不能把总力一致误说成局部压力逐格真实。作者使用Neo-Hookean模型、准静态硬接触和无滑移等前提；R27不得把这些前提套到动态柔性刮板。文中已包含形变→分布估计，因此普通“视觉重建局部力”不是空白。S27-01、S27-02、S27-03、S27-04。

**公式 F27-03：**

$$
U=C_{10}(\bar I_1-3)+D_1^{-1}(J-1)^2
$$

**表 T27-03｜FEATS全文：局部压力标签究竟来自哪里**

|FEATS审计维度|论文/官方资源证据|迁移到FR5的限制|
|---|---|---|
|标签|CalculiX FEA逐格力|不是独立分段载荷真值|
|校准|外部六轴F/T检验总力|不能唯一证局部峰值|
|运行|作者GelSight Mini 25Hz/RTX5000|FR5 Eye-in-Hand非内置触觉镜头|


## 04. FeelAnyForce全文：独立总力测量的价值与边界

实际核读Shahidzadeh等《FeelAnyForce》arXiv:2410.02048的III数据采集/模型和IV实验。作者使用GelSight Mini、不同压头和独立力传感器收集大量压入数据，以RGB及深度相关特征回归3D**总接触力向量**，并在推物体估重、杯子变形等任务检验。作者在未见物体上报告归一化力误差，在跨传感器校准与漂移部分也披露限制。它证明独立力参考与跨传感器校准重要，却**没有**证明可恢复柔性刮板的边缘分布。其力值、误差和控制频率都属于作者装置，不是FR5。S27-05、S27-06。

**公式 F27-04：**

$$
\hat{\mathbf f}=f_\theta(I_{\rm tactile},D),\qquad \mathbf f_{\rm GT}=\mathbf f_{\rm external\ F/T}
$$

**表 T27-04｜FeelAnyForce全文：独立总力测量的价值与边界**

|比较维度|FeelAnyForce|FR5所需额外条件|
|---|---|---|
|输出|3D合力|局部边缘载荷上界|
|真值|外部F/T总力|机械分段载荷/独立力台|
|迁移|GelSight Mini/DIGIT校准|EIH RGB-D看不到硅胶内部形变|


## 05. Peng全文：逆弹性力场也需要材料假设

实际核读Peng、Yao、Hauser《3D Force and Contact Estimation for a Soft-Bubble Visuotactile Sensor Using FEM》ICRA2024，DOI **10.1109/ICRA57147.2024.10610233**，arXiv:2310.11372的III–V方法、优化、实验。作者将气压变化、RGB-D膜形变、线弹性面内应力与L1正则组合，计算接触力场。论文在UR5+Punyo气泡触觉传感器上比较外部F/T总力与接触区域；报告其正则项容易漏检约1N轻触，并指出大曲率变化下线性平面应力模型表现差。这一直接反证表明“使用FEM/稀疏优化”既非新颖性本身，也不能消除弱接触漏检。作者约1–2Hz模型求解速率与FR5无关。S27-07、S27-08。

**公式 F27-05：**

$$
\mathbf F_{\rm ext}=-\big(K\mathbf U+\delta p\,\mathbf a\odot\mathbf n\big)
$$

**表 T27-05｜Peng全文：逆弹性力场也需要材料假设**

|方法组成|有效前提|失败边界|
|---|---|---|
|RGB-D形变|膜网格与光流可信|遮挡/大变形失真|
|FEM弹性|材料参数、边界已知|大曲率下近似差|
|L1稀疏|较少接触区域|弱接触可能漏检|


## 06. Zhong全文：曲率导致校准失效的物理反证

实际核读Zhong、Kim、Losey、Nunez《Curvature-Aware Calibration of Tactile Sensors for Accurate Force Estimation on Non-Planar Surfaces》arXiv:2510.25965v2（2025-10-31）的IV–VII校准、实物实验与VIII限制。其柔性电阻阵列在不同曲率下，平面标定可能低估受力；作者通过无载基线推断曲率并将曲率纳入力回归，在五个日用品曲面做独立ATI参考对照。文中明确只处理一维曲率与静态校准，尚非任意双曲率软体表面动态刮擦。这为本轮“曲率分箱有效性”和“标定包络外拒绝”提供了实物反证。S27-09。

**公式 F27-06：**

$$
F=0.009625S-0.000014S^2-0.000372SC+0.000005SC^2
$$

**表 T27-06｜Zhong全文：曲率导致校准失效的物理反证**

|变量|作者物理含义|FR5迁移限制|
|---|---|---|
|S|作者电阻阵列读数|不是FR5 RGB-D像素|
|C|一维曲率指标|背部双主曲率/软变形未覆盖|
|F|作者独立力传感器对照|不能推断刮板局部边缘峰值|


## 07. 十二项去重来源与开源资源审计

本轮核查12个去重学术/官方来源：FEATS论文全文、作者GitHub、HF数据卡、IROS DOI元数据；FeelAnyForce全文与作者项目页；Peng全文与机构DOI页；Zhong全文；VPEC与ViPER作者仓库；R26中Caron论文作为理论对照（本轮复查其arXiv摘要/历史记录，未重复宣称全文新读）。FEATS GitHub README明确MIT代码，HF数据卡显示MIT数据、`.npy`、约3.98GB，作者README指向预训练`.pt`模型；但本机`raw.githubusercontent.com`和`huggingface.co`的下载请求DNS解析失败，未下载任何权重/数据，未取得本轮不可变commit SHA。不能把“网页列出模型”写成“已加载模型”。VPEC/ViPER本轮只核作者README与R26历史记录，不重算旧测试为新实验。S27-01至S27-12见文末来源索引。

**公式 F27-07：**

$$
N_{\rm source}=12,\qquad N_{\rm paper\ full\ method/results}=4
$$

**表 T27-07｜十二项去重来源与开源资源审计**

|资源|已核级别|尚缺|
|---|---|---|
|FEATS论文|公开全文III/IV|FR5同构物理真值|
|FEATS代码/数据|官方README/卡，MIT标识|不可变commit、下载SHA、实际运行|
|VPEC/ViPER|官方README+历史R26矩阵|新数据与权重未取得|


## 08. 五节点有限宽线接触的坐标与可观测性

继承R26模型，刮板横向归一化节点u=(-2,-1,0,1,2)，物理位置y=(-20,-10,0,10,20)mm，节点法向载荷p_i≥0（单位N）。腕部在明确参考原点、坐标、重力/偏置补偿和纯偶矩界下，仅给出N=Σp_i与M=Σy_i p_i两个独立线性约束。边缘总载荷E=p_0+p_4，不等于边缘压力Pa；压力还需独立面积标定。令A_w=[1;y]，rank(A_w)=2，nullity=3。R27新增假设形变特征q=Σu_i²p_i，系数行与wrench两行线性独立，可缩小E的可行区间，但不会恢复全部5个p_i。

**公式 F27-08：**

$$
\mathbf A_w=\begin{bmatrix}\mathbf1^T\\\mathbf y^T\end{bmatrix},\quad \operatorname{rank}(\mathbf A_w)=2,\quad E=p_0+p_4
$$

**表 T27-08｜五节点有限宽线接触的坐标与可观测性**

|量|单位|是否真实FR5测量|
|---|---|---|
|N|N|本轮人工设定10N|
|M|N·mm|本轮人工设定0|
|E|N|模型目标，不是压力|
|q|N×无量纲|纯假设光学代理，不是RGB-D实测|


## 09. 新增光学代理的数学含义及不可直接迁移性

构造用于可证伪研究设计的假设代理q=4(p_0+p_4)+(p_1+p_3)，相当于以横向位置平方加权的载荷二阶矩。该特征不是现成RGB-D算法输出；要从图像得到它，必须建立刮板几何、材料形变、相机视角、遮挡和力之间的**独立物理标定映射**。若新观测只是总力q'=Σp_i，与wrench行重复，LP区间不会变窄。即使矩阵增加了独立行，也不代表噪声下能可靠判别：灵敏度、条件数、漂移和标定有效域决定实际价值。FEATS与Peng已提供视觉形变/FE模型先例，故本研究的创新候选不能是普通“视觉估计力”，而必须是**有校准失效证据的边缘风险区间与拒绝**。

**公式 F27-09：**

$$
q=\sum_{i=0}^{4}u_i^2p_i=4E+(p_1+p_3)
$$

**表 T27-09｜新增光学代理的数学含义及不可直接迁移性**

|新增观测|是否独立于合力/力矩|理论结果|
|---|---|---|
|q=Σu²p|是（在五节点模型）|可缩小E上界|
|q′=Σp|否|不缩小可行集|
|未标定图像轮廓|未知|不得收紧E|


## 10. 校准增益、偏置与噪声的可行投影

将假设光学读数写成z=gq+b+e，q≥0，g∈[g_min,g_max]且g_min>0，b∈[b_min,b_max]，|e|≤δ。对所有可能增益/偏置存在性投影，可得q_min=max(0,(z−δ−b_max)/g_max)，q_max=(z+δ−b_min)/g_min。这是**在区间已独立保证有效、各参数可同时取值、读数模型正确**条件下的集合投影，不是无需验证的统计置信区间。增益不确定越大，q区间越宽。若真实增益低于g_min，理论包含性立即可能失败。Zhong论文提供曲率改变增益的物理反证；必须按曲率/材料/传感器身份分层验证。

**公式 F27-10：**

$$
q_{\min}=\max\!\left(0,\frac{z-\delta-b_{\max}}{g_{\max}}\right),\quad q_{\max}=\frac{z+\delta-b_{\min}}{g_{\min}}
$$

**表 T27-10｜校准增益、偏置与噪声的可行投影**

|假设读数z|假设增益范围|可行q区间|
|---|---|---|
|2|[1,1]|[2,2]|
|2|[0.5,1]|[2,4]|
|1.4|[0.5,1]|[1.4,2.8]|


## 11. 带误差的LP与参考原点、纯偶矩Gate

使用SciPy HiGHS求解非负p的线性规划：在|Σp−N_obs|≤ε_N、|Σy p−M_obs|≤ε_M+ε_couple、q_min≤Σu²p≤q_max约束下分别最大/最小化E。未知纯接触偶矩没有可信界时HOLD；F/T坐标原点、法兰与标定TCP、柔性真实接触中心必须分别记录。视觉代理不能替代完整六维wrench坐标变换和时间锚；误差范围必须覆盖同步、几何和补偿不确定性。LP的数值残差检查只是求解器质量，不是物理安全认证。

**公式 F27-11：**

$$
E_{\max}=\max_{\mathbf p\ge0}\{\mathbf c_E^T\mathbf p:\ |\mathbf A_w\mathbf p-\mathbf w|\le\boldsymbol\epsilon,\ q_{\min}\le\mathbf c_q^T\mathbf p\le q_{\max}\}
$$

**表 T27-11｜带误差的LP与参考原点、纯偶矩Gate**

|约束|独立证据要求|不满足时|
|---|---|---|
|F/T力与力矩|坐标/采集时刻/补偿有效|HOLD|
|纯偶矩误差|可验证上界|HOLD|
|光学q包络|曲率/材料/身份覆盖|HOLD|
|数值可行性|LP成功及残差检查|HOLD|


## 12. 解析独立判据与危险误放行反例

在N=10N、M=0、q确定时，写I=p_1+p_3、C=p_2，得到q=4E+I、N=E+I+C。由I,C≥0推出E≤q/4、E≥(q−N)/3；再与0≤E≤N合并：E∈[max(0,(q−N)/3),min(N,q/4)]，该对称模型的界可达到，故是独立于SciPy的解析oracle。人工真值p=(0.5,0,9,0,0.5)N，N=10，M=0，q=4，实际E=1N。假设真实g=0.5则z=2：错误固定g=1时E_max=0.5N，在人工演示阈值0.75N下错误“模型低于阈值”；包络g∈[0.5,1]时E_max=1N，返回HOLD。该阈值只用于数学反例，不是FR5限力。

**公式 F27-12：**

$$
\max\!\left(0,\frac{q-N}{3}\right)\le E\le\min\!\left(N,\frac q4\right)
$$

**表 T27-12｜解析独立判据与危险误放行反例**

|人工方案|E上界(N)|对0.75N人工阈值|
|---|---|---|
|只用wrench|10.0|HOLD|
|错误固定增益g=1|0.5|错误模型低风险|
|正确包络g∈[0.5,1]|1.0|HOLD|


## 13. 程序接口、状态机语义与测试证据

新增`optical_envelope_bounds.py`（独立Python模块）及`tests/test_optical_envelope_bounds.py`。入口`bounds`要求frame/time/compensation/normal_model四类布尔证据；若使用光学代理还要求独立校准、曲率分箱、传感器身份三项有效。`projected_q_interval`先投影校准区间，再求解上下界LP；`offline_classification`只输出`MODEL_BELOW_LIMIT`、`MODEL_ABOVE_LIMIT`或`HOLD`，**绝不输出机器人动作命令**。新测试28项全部通过；R26 32项、R25 43项、R24 66项均实际回归通过。R25首次错误工作目录造成一次收集失败，已记录修复。运行环境为本机Python/SciPy/NumPy，无GPU模型与实体传感器。

**公式 F27-13：**

$$
N_{\rm passed}=28+32+43+66=169
$$

**表 T27-13｜程序接口、状态机语义与测试证据**

|代码文件|实际运行|边界|
|---|---|---|
|optical_envelope_bounds.py|SciPy LP+输入Gate|仅离线模型|
|test_optical_envelope_bounds.py|28/28通过|含250组人工随机可行性|
|R24/R25/R26回归|66+43+32通过|不是新增科学实验|


## 14. 600组人工样本、对照与消融结果

运行`run_r27_comparison.py`，固定随机种子270027，在600个**人工生成**的五节点非负载荷episode上随机生成真实g∈[0.5,1]、偏置∈[-0.05,0.05]、噪声∈[-0.02,0.02]。每组分别算wrench-only和正确校准包络的E区间，真实E均被包含（600/600）；平均区间宽度从9.8190N缩至5.2927N。该数值只是模型假设成立时的信息收益，不是RGB-D或FR5实测。冗余“视觉量=总力”时上界仍10N；曲率分箱无证据时HOLD；故意令真实g=0.35低于假设g_min=0.5时，z=1.4会导致E_max=0.7N，在人工阈值0.75N下错误低风险。**离开标定域，包络就不是保证。**

**公式 F27-14：**

$$
\overline{\Delta E}_{\rm wrench}=9.8190\,\mathrm N,\quad \overline{\Delta E}_{\rm proxy}=5.2927\,\mathrm N\quad(\text{人工合成})
$$

**表 T27-14｜600组人工样本、对照与消融结果**

|对照/消融|人工模型结果|科学解释|
|---|---|---|
|wrench-only|均宽9.8190N|观测欠定|
|已校准代理|均宽5.2927N|前提成立可收紧|
|冗余代理|示例上界仍10N|无新增信息|
|超域g=0.35|示例错误模型低风险|必须跨域验证/拒绝|


## 15. 主论文Baseline竞争矩阵与R26→R27排序

跨轮登记维持VPEC、ViPER、FEATS、UMI-FT、Jiang IAAC、FP2、Cheng BITC、VisualForce、Wrench-ACT九项。**R27将FEATS新列为#3离线方法学复现候选**（R26无此候选），原因是IROS2025全文、MIT代码与MIT数据卡、模型路径和数学标签链条清楚；但其标签来自FEA、输入是GelSight Mini内置触觉图像，与FR5 Eye-in-Hand RGB-D不同，因此不升为主论文第一，也不声称模型已运行。VPEC#1与ViPER#2维持条件性复现优先；UMI-FT降为#4纯属投入排序顺延，并非性能变差。Jiang/FP2等受低层力矩、反馈频率、权重和真实真值阻塞。详见独立`BASELINE_AND_INNOVATION_REGISTER.md`，作者/DOI/许可未核项目明确待补证。

**公式 F27-15：**

$$
\operatorname{priority}_{R27}:\quad VPEC\succ ViPER\succ FEATS\succ UMI\text{-}FT\quad(\text{条件性复现投入})
$$

**表 T27-15｜主论文Baseline竞争矩阵与R26→R27排序**

|候选|R26→R27|本轮证据性决定|
|---|---|---|
|VPEC|1→1|保留，EIH视角迁移未证|
|ViPER|2→2|保留，弱标签与许可缺口|
|FEATS|新入→3|MIT代码/数据+FEA标签，非独立局部真值|
|UMI-FT|3→4|顺延，双指硬件不匹配|
|Jiang/FP2/其余|顺延/保留|低层控制与复现缺口|


## 16. 唯一首选I-A与备选I-B的收敛判断

唯一条件性首选I-A继续是“有限宽刮板边缘载荷风险的部分可辨识区间、可信校准与失败拒绝”，R27新增**校准包络必须覆盖曲率/材料/传感器身份**的进入Gate。其核心可证伪假设H27：在独立分段载荷真值和相同放行覆盖率下，经外部校准的RGB-D特征可相对于wrench-only稳定缩小边缘载荷上界，且超域拒绝不会漏报。FEATS/Peng/Zhong直接削弱“普通视觉分布估计/FEM/曲率补偿”本身的新颖性。备选I-B是工作—卸载—离面返程的延迟鲁棒控制；由于FP2/BITC等先例，继续保留低优先级。若真实标定无法得到可靠独立视觉特征，I-A降级为只做可信拒绝的工程安全模块，重新评估I-B或其他科学问题。

**公式 F27-16：**

$$
H_{27}:\quad \mathbb E[\Delta E_{\rm fusion}\mid\mathrm{valid}]<\mathbb E[\Delta E_{\rm wrench}]\ \land\ \mathrm{OOD}\Rightarrow\mathrm{HOLD}
$$

**表 T27-16｜唯一首选I-A与备选I-B的收敛判断**

|创新候选|支持/反证|进入/退出|
|---|---|---|
|I-A唯一首选|R26可辨识反例+R27包络反例|独立分段真值改善才进入；失败降级|
|I-B唯一备选|厂家状态机可做工程强基线|外环不可用或无收益则退出|
|普通视觉压力图|FEATS/VPEC/ViPER已有|不独立立创新|


## 17. FR5厂家强工程Baseline与E0–E5资格

工程强Baseline仍是厂家`FT_Control`/ROS2 C++服务+独立限速/平滑卸载/离面返程，**当前只作为待验收候选**。R19发现Python包装B[1]疑点、R20示例参数错位、R21时间戳与wrench原点问题、R23 ROS2驱动错误传播/第六轴NaN遗漏均未取得本机修复与实测。先核本机SDK/固件/接口契约、只读F/T原始采集时间、偏置/重力/动态补偿和独立已知载荷，再做非人体离面运动、低能量单向接触、卸载和离面返程。无有效接口资格时禁止将本轮离线分类接入机器人控制。安全限力不能抄作者其他机器人的数值。

**公式 F27-17：**

$$
E_0\rightarrow E_1\rightarrow E_2\rightarrow E_3\rightarrow E_4\rightarrow E_5\quad(\text{当前均HOLD})
$$

**表 T27-17｜FR5厂家强工程Baseline与E0–E5资格**

|Gate|最低证据|本轮状态|
|---|---|---|
|E0/E1|本机版本/坐标/传感器/安全|HOLD|
|E2/E3|独立几何与非人体载荷|HOLD|
|E4/E5|离面轨迹、低能量接触与卸载|HOLD|


## 18. M1/M2/M3三入口与统一轨迹合同

保留完整功能：M1人工有序选点和指定路线；M2人体背部实体画线检测、起终点和方向人工确认、投影为有序三维曲线；M3机器人观察背部，识别解剖标志并产生需专家审核的穴位候选，维护可编辑可信3D关键点图，用户选择两点线路或经审核刮痧模板。三入口在接触前由用户确认，统一输出有序`position/tangent/surface_normal/confidence/uncertainty/tool_pose/continuous_finite_width_sweep_eligibility/WORK|UNLOAD|OFFSURFACE_RETURN`。曲面图搜索与成熟视觉网络作为上层工程基线；不得把普通检测网络当主博士创新。法兰、标定TCP、柔性真实接触线与局部偏载必须在数据结构中分离。

**公式 F27-18：**

$$
\Gamma(s)=\big(\mathbf r,\mathbf t,\mathbf n,\Sigma,T_{\rm tool},\chi_{\rm sweep},m\big)(s)
$$

**表 T27-18｜M1/M2/M3三入口与统一轨迹合同**

|入口|输入来源|共同安全输出|
|---|---|---|
|M1|人工点与路线|审核后3D曲线|
|M2|实体画线+方向确认|审核后3D曲线|
|M3|解剖标志/穴位候选+专家审核|审核后3D曲线|


## 19. 非人体独立真值、预注册与公平对照

设计机械隔离3–5段刮板线载荷台架，独立校准每段载荷和交叉耦合，另设独立总力台与外部几何跟踪；先不接机器人，手动/低能量台架按不同曲率、材料、刮板倾角、速度和可见性采集同步RGB-D/六维F/T/分段真值。按物理episode划分训练/校准/测试，防止同一刮动多帧泄漏。预注册W0=wrench-only、W1=未校准视觉（反证）、W2=校准包络融合、W3=视觉乱序、W4=仅几何、W5=厂家简单工程强基线（需实体Gate通过）；比较同覆盖率下的E上界覆盖、宽度、危险放行条件风险上界、HOLD率、超域拒绝率。阈值必须由独立风险评估给出，不用本轮0.75N。

**公式 F27-19：**

$$
\mathrm{coverage}=\Pr(\mathrm{accept}),\qquad R_{\rm cond}=\Pr(E_{\rm true}>E_{\rm limit}\mid\mathrm{accept})
$$

**表 T27-19｜非人体独立真值、预注册与公平对照**

|对照|独立真值|失败退出|
|---|---|---|
|W0 vs W2|分段载荷+总力台|无同覆盖收益→降级I-A|
|W2 vs W3|相同episode打乱视觉|乱序仍有效→警惕泄漏|
|曲率/材料留出|未见曲率材料的分段载荷|超域错误放行→退出|


## 20. 资源阻塞、QA、下轮最小补证与研究结论

本轮实际完成：核查12项去重学术/官方来源、深入核读4篇开放全文的方法和实验、实现1个可运行LP+校准包络模块、28项新测试、R24–R26共141项回归、600组固定种子人工合成对照与独立解析oracle（另有100组解析随机检查、250组人工包含性检查，均属程序内部测试）。未连接FR5、未取得真实RGB-D或分段载荷、未运行VPEC/ViPER/FEATS原模型。容器对raw GitHub与HF下载均DNS失败；网页可读不等于数据已下载。R27通过**离线内容验收**但E0–E5均HOLD。下一轮第一优先是本机只读F/T日志/SDK版本和独立分段载荷最小切片；其次取得合法FEATS或VPEC模型权重+SHA进行最小推理；如继续缺实物，优先审查可复现资源阻塞，不用新合成数量凑成果。绝不写旧GitHub仓库，也不擅自建新仓库。

**公式 F27-20：**

$$
\mathrm{accept}_{\rm research}=1,\quad \mathrm{accept}_{\rm FR5\ physical}=0
$$

**表 T27-20｜资源阻塞、QA、下轮最小补证与研究结论**

|验收项|实证状态|限制|
|---|---|---|
|研究与讲义|20章逐章匹配；公式/表/来源/决策审计|仅离线|
|新算法|28项通过；600人工样本|不证明真实风险|
|实体证据|0次FR5/0个真实分段样本|E0–E5 HOLD|


---

## 来源索引｜12项去重来源（本轮新核查，不等于12篇全文）

|ID|来源/元数据|实际获取层级|官方或全文URL|

|---|---|---|---|

|S27-01|Helmut et al., FEATS, IROS 2025, DOI 10.1109/IROS60139.2025.11246486; arXiv 2411.03315v3|全文III–IV核读|https://arxiv.org/html/2411.03315|

|S27-02|FEATS作者仓库 feats-ai/feats, MIT|作者README/许可页面核查；未取得commit SHA|https://github.com/feats-ai/feats|

|S27-03|FEATS官方HF数据卡, MIT, npy, 3.98GB|数据卡核查；未下载数据|https://huggingface.co/datasets/erikhelmut/FEATS|

|S27-04|FEATS IROS2025 DOI会议元数据|出版元数据核查|https://www.nature.com/nature-index/article/10.1109/iros60139.2025.11246486|

|S27-05|Shahidzadeh et al., FeelAnyForce, ICRA 2025, arXiv 2410.02048|全文III–IV核读|https://arxiv.org/html/2410.02048|

|S27-06|FeelAnyForce作者项目页|作者项目/代码与数据入口核查|https://www.prg.cs.umd.edu/FeelAnyForce|

|S27-07|Peng, Yao, Hauser, Soft-Bubble FEM, ICRA 2024, DOI 10.1109/ICRA57147.2024.10610233|全文III–V核读|https://arxiv.org/html/2310.11372|

|S27-08|Peng ICRA2024机构出版元数据|DOI与会议核查|https://experts.illinois.edu/en/publications/3d-force-and-contact-estimation-for-a-soft-bubble-visuotactile-se/|

|S27-09|Zhong et al., Curvature-Aware Calibration, arXiv 2510.25965v2|全文IV–VIII核读|https://arxiv.org/html/2510.25965|

|S27-10|VPEC作者代码仓库|README核查；全文为历史R24层级|https://github.com/Healthcare-Robotics/VPEC|

|S27-11|ViPER作者代码仓库|README核查；全文为历史R24层级|https://github.com/Healthcare-Robotics/ViPER|

|S27-12|Caron, Pham, Nakamura 2015 contact wrench cone, arXiv 1501.04719|R26历史全文，本轮仅复核来源，不计新全文|https://arxiv.org/abs/1501.04719|


---

## 决策日志索引｜11项

|ID|决定|

|---|---|

|D27-01|纠正R26 QA章节计数字段：实际19/19，不修改原始QA。|

|D27-02|FEATS新颖性反证：局部力图FEA标签不是独立逐格真值；不把普通视觉力场估计当新创新。|

|D27-03|新增FEATS为条件性离线方法学候选#3；VPEC#1/ViPER#2不变，UMI-FT顺延。|

|D27-04|仅独立标定的视觉/形变代理可缩小局部边缘载荷可行集；冗余观测无收益。|

|D27-05|增益、偏置、噪声必须投影为可行区间；未知曲率/材料/传感器身份时HOLD。|

|D27-06|固定增益错误放行与超域漂移错误放行的人工反例均被复现。|

|D27-07|I-A仍唯一条件性首选，增加校准有效域与同覆盖率物理真值Gate；I-B唯一备选。|

|D27-08|厂家FT_Control/ROS2 C++服务继续HOLD；本轮算法绝不连接机器人。|

|D27-09|非人体分段载荷与独立总力台是必需的物理真值，FEA/腕部wrench不能充当分段真值。|

|D27-10|下一轮优先真实只读日志/分段台架或合法最小真实数据，停止同类合成堆量。|

|D27-11|本轮只认可离线内容验收；0次FR5实体、0次作者模型推理、0次GitHub写入。|


## 研究真实性与复现说明

代码运行：`python -m pytest -q tests/test_optical_envelope_bounds.py`；`python run_r27_comparison.py`。历史回归参见`results/`。本轮代码依赖NumPy/SciPy/pytest；输入数据完全人工合成。文献全文只通过公开arXiv HTML核读；FEATS原始权重、HF真实数据、FR5只读日志均未下载。论文中作者设备的力、帧率、误差、校准参数不得当成本项目实测。

本轮新增可证伪最小解法：独立分段载荷台架→多曲率/材料独立标定视觉特征→冻结校准区间→同episode同覆盖率wrench-only/融合/视觉乱序对照→失效退出。真人接触需独立伦理与风险批准。