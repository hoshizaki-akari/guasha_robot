# AUTO-R53｜博士级完整研究报告：真实公开元数据资格、FEA标签反证与整机证据收敛

**内容验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY。** 机器人运动/物理仿真/人体/FR5安全：HOLD。

本轮不是继续叠加LP/EKF/ESO/MPC，而是实际审计一个非人体机器人滑动接触数据集的公开元数据，核读FEATS完整方法/实验，复核EgoPressure原生单位与许可，并开发声明感知只读Gate。原始传感器payload与独立局部压力真值仍未取得。

**证据分级**：本轮真正读FEATS全文1篇；12个去重来源对象（论文、仓库、数据卡、手册等，非12篇论文）；完整公开CSV元数据读取312条源段+24会话；新增110项测试、重跑R52/R51/R50/R49 875项。没有作者模型复现。

## 01. 上一轮可访问归档与本轮差分

本轮以实际文件为起点，不把上一轮回答当成源码。读取`AUTO_R52_RESEARCH_REPORT.md`、`AUTO_R52_TEACHING_GUIDE.md`、`BASELINE_AND_INNOVATION_REGISTER.md`、`DECISION_RECORD.md`和`episode_evidence_gate.py`；将外部ZIP SHA256与校验清单比对，逐项检查ZIP CRC与24个内部文件SHA256，结果一致。随后解压并实际执行R52新增97项、嵌套R51新增442项、R50/R49研究336项。之前R39–R48的736项不在本轮重跑统计中。

R52核心新增是同episode、同钟有界时间区间的三流配对Gate、材料/episode分组和作者源码原生单位审计；其200组人工episode不能回答真实数据是否具备独立空间压力真值。R53**没有改写R52源码**，而是转向公开非人体接触数据集的可获取性、真实元数据切分语义、FEA标签是否属于独立真值。对工程而言，这比继续加一层数学优化更直接：先决定哪些公开数据有资格支撑“比厂家/简单Baseline更好”的论断。

**公式 F53-01：$N_{\rm rerun}=97+442+336=875$（上一轮及其可访问历史的本轮实际复跑；R39–R48未复跑）。**

**表 T53-01｜证据层级｜R52 ZIP 25成员/24内部SHA/CRC｜本轮实际复跑875｜FR5实体0。**

**来源/决策**：S53-01；D53-01。

## 02. 唯一科学问题、可证伪假设和退出门槛

唯一条件性首选I-A仍是：在刮板真实有限宽、工具yaw、接触线原点、纯接触力偶、跨设备时间不确定时，能否给出可验证的局部载荷部分可辨识区间和拒绝证书。它不要求从单个腕部wrench“神奇地恢复”逐格压力。单个合力和力矩对未知多点载荷、未知纯力偶与摩擦一般不唯一；没有独立空间压力参考，就不能声称比简单几何或厂家恒力更准确。

可证伪最小假设是：在同一批合法、计量可追溯的非人体柔性曲面上，使用同一相机/F/T/工具标定、相同接触序列及材料留出，I-A的独立三区误差或可信拒绝指标显著优于同信息强Baseline；不能只比较人工区间宽度。若拿不到数据或盲检无收益，暂停I-A，而不是叠加更多LP维度。唯一备选I-B是WORK—UNLOAD—OFFSURFACE_RETURN换向时的延迟鲁棒柔顺，但同样须先核实厂家控制接口与时序。

**公式 F53-02：$H_A:\;\Delta_{\rm blind}=L_{\rm strong}-L_{I\!A}>0$，前提是同一合法非人体盲检、独立局部真值及相同观测。**

**表 T53-02｜首选I-A｜有限宽偏载部分可辨识/可信拒绝｜备选I-B｜换向延迟鲁棒柔顺｜两者物理验证均HOLD。**

**来源/决策**：S53-02；D53-02。

## 03. 本轮检索范围、论文与源码证据级别

本轮筛查并登记12个去重的**来源对象**，不是12篇全文论文。唯一完整核读的方法/实验全文是Helmut等FEATS的arXiv HTML，覆盖数据采集、CalculiX、投影离散、U-net训练、材料验证、未见压头与跨传感器测试。另深度审查作者FEATS代码与HF数据卡、EgoPressure公开数据单位与许可证、VisTouch公开真实元数据及FR5厂家力控API。EgoPressure CVF全文入口返回403，只使用摘要/官方项目和公开数据契约，绝不冒充论文全文核读。

比较研究方法时把五个维度分开：合法可获取、原生单位、独立真值、分组泛化、FR5传感器/控制接口。一个公开仓库有MIT源码，不意味着数据文件、模型权重或额外MANO资产也都是MIT；一个真实机器人采集的图像，不意味着它对应的局部压力标签就是独立测量。完整来源及固定版本/读取边界见`SOURCE_REGISTER.md`。

**公式 F53-03：$E_{\rm valid}=E_{\rm access}\land E_{\rm license}\land E_{\rm units}\land E_{\rm split}\land E_{\rm truth}$。**

**表 T53-03｜A全文｜FEATS 1篇｜B官方源码/元数据｜FEATS/VisTouch/EgoPressure/FR5等｜C摘要｜EgoPressure论文｜H继承｜VPEC/ViPER。**

**来源/决策**：S53-03；D53-03。

## 04. FEATS全文：采集、FEA标签与数学契约

FEATS论文§III描述CNC压入实验：24个3D打印压头，19个训练、5个未见测试；共14279个真实压入样本，每个有GelSight Mini RGB、CNC位移与RESENSE-HEX-21六维F/T读数。关键是**标签生成机制**：作者把压入配置重建为CalculiX有限元模型，使用十节点四面体网格、Neo-Hookean近似，计算各接触单元三轴反力，再通过几何投影与按面积分配将其离散为24×32×3的局部力图。神经网络拟合的是这些**模拟计算的标签**。

论文材料模型以$C_{10}=0.0725$为主要设定，并使用四个不同深度的球形压入与F/T总力比较；优化得到0.0792、整体MAE 0.5166N（作者报告）。这能验证部分整体受力趋势，但不能唯一证实每个网格的真实局部力。FEATS的标签是“物理模型推断的伪真值”，不是造假；只是它与本项目要求的**独立空间压力参考**不同。论文在静态压入、避免滑动条件下处理接触，FR5刮痧是沿曲面滑动，摩擦/形变迁移不可直接成立。

**公式 F53-04：$\hat{\boldsymbol f}_{\rm grid}=\mathcal U_{\theta}(I_{\rm GelSight})\approx\Pi_{24\times32}(\boldsymbol f_{\rm FEA})$。**

**表 T53-04｜输入｜GelSight Mini 240×320 RGB｜标签｜CalculiX FEA三分量24×32网格｜独立测量｜F/T总力。**

**来源/决策**：S53-02；D53-04。

## 05. FEATS全文：公平实验、域迁移与实际可复现性

FEATS使用9084训练、538验证、1068已见压头测试以及2789未见压头测试，报告对未见压头和另一台同型号GelSight Mini的泛化。论文§IV的跨传感器测试总力平均误差为x=0.24N、y=0.27N、z=2.99N；同型传感器的相机/凝胶差异仍明显。作者在Quadro RTX5000上报告U-net单次推理4.1746±1.339ms、GelSight 25Hz；**这些不是FR5/双RTX3090实测，也不能推出外环控制带宽**。

作者仓库固定提交`7d1ae43...`声明MIT、Python3.11.10、FEA和模型脚本，HF FEATS卡标MIT、数据为`.npy`图像+FEA标签。相比ViPER的未知数据许可与配对证据，FEATS更适合做合法离线方法学复现，故R53将其从第3移到第2。但GelSight内部触觉图像不是FR5 D435的Eye-in-Hand背部外观图像；即使把作者模型运行成功，也不能把其输出当作刮板边缘实测压力。

**公式 F53-05：$\mathrm{MAE}_{\rm total}=\frac1N\sum_k\left|\sum_i\hat f_{ik}-\sum_i f^{\rm FEA}_{ik}\right|$；总力MAE≠逐格实测MAE。**

**表 T53-05｜作者实验｜已见压头1068测试、未见压头2789测试、跨同型传感器｜限制｜标签FEA且跨设备法向误差增大。**

**来源/决策**：S53-03；D53-05。

## 06. EgoPressure：原生单位、许可和人体数据边界

EgoPressure是Zhao等CVPR2025的21名人体参与者手部接触数据；论文摘要与官方项目可读，CVF全文本轮403，**不能宣称全文核读**。实际读取官方数据契约`docs/DATA.md`固定blob与LICENSE：源Sensel力网格105×185是原生counts，文档明确`counts/1736→N`，每格面积按1.25mm×1.25mm换算kPa；压力UV贴图是同一传感器测量的几何重映射，不是第二个独立传感器。原始同步时间以µs epoch/PTP记在sidecar，**不在发布的shards中**；深度图仍在深度传感器坐标，不能按同像素直接当RGB深度。

这给R52 VPEC“1736 counts旁注gram、函数标N”带来**新反证**：独立ETH工具包也采用1736→N，说明不能仅凭旁注断言VPEC必然错101.97倍；但两个代码一致仍不能替代Sensel厂家计量证书，所以绝对Pa依旧HOLD。许可同样重要：EgoPressure仓库源码MIT，但数据是CC BY-NC-SA4.0、MANO资产另有许可；本项目坚持非人体研究验证，不能偷偷下载人手数据冒充非人体刮板接触。

**公式 F53-06：$F_{\rm documented}=\mathrm{counts}/1736\;\mathrm N,\quad p_{\rm documented}=F/(0.00125\,\mathrm m)^2$。**

**表 T53-06｜EgoPressure代码｜MIT｜数据｜CC BY-NC-SA 4.0｜MANO｜单独许可｜人体数据｜排除非人体真值。**

**来源/决策**：S53-06；D53-06。

## 07. VisTouch公开仓库：数据来源、许可与下载边界

VisTouch 2026公开仓库描述机器人灵巧手在黄铜、亚麻、纸、涤纶、丝绸、氨纶、石材、木材上的按压/滑动，固定相机30fps、音频16kHz、手部触觉力通道，数据CC BY4.0、脚本MIT。我们通过官方GitHub只读接口核实固定commit`a569c7e...`与LICENSE/DATA_LICENSE，并读取完整`metadata/samples.csv`和`sessions.csv`；仓库实际原始视频/触觉/音频数据位于外部百度网盘，未下载。

作者README称公开10498个对齐微片段，但`clips_index.csv`和`frames_index.csv`太大，本轮接口未返回内容；不能以312条源段等同10498片段，也不能宣称已复核其微片段划分。VisTouch是当前更贴近“非人体滑动接触”的公开**采集流程参考**，但灵巧手触觉通道不等于独立三分区压力阵列；论文引用2022通信文章也不意味着当前2026数据已被原论文逐条验证。

**公式 F53-07：$N_{\rm source\ segments}=312\ne N_{\rm claimed\ microclips}=10498$（不同层级，后者未审计）。**

**表 T53-07｜VisTouch README｜10498微片段、8材料｜本轮实际读｜312源段+24会话元数据｜原始payload｜未取得。**

**来源/决策**：S53-07；D53-07。

## 08. VisTouch真实公开元数据：312源段、24会话的独立审计

这不是合成数据统计。本轮实际读取VisTouch官方固定commit的`samples.csv` **312条源段记录**和`sessions.csv` **24条会话记录**，按`session_id`、`material_english`、`force_n`、`split`重建分组，独立检查样本时段是否重叠、`end-start`与duration一致性。结果：每个session恰有13条源段；8种材料×3种标称法向力设定点(3/6/9N)=24会话。训练208条/16会话来自3和6N，测试104条/8会话来自9N；训练测试会话不交叉，**8种材料却全部重叠**。312条中未见同session相邻源段重叠，duration误差超过0.005s的记录为0。

因此，基于这份**源段级**公开元数据，可以把测试称为“未见力设定点9N的跨条件评估”，但不能称为“未见材料泛化”。设定点字段`force_n`不等于独立力台实际法向力真值。数据的实际波形、微片段和时钟硬界未审计。我们把24组摘要保存为`VIS_TOUCH_AUDITED_SESSION_SNAPSHOT.json`，包含源CSV的Git blob SHA，明确不是原始CSV字节；本地当前不能通过常规网络下载原CSV，故未声称本地完成原CSV字节重验。

**公式 F53-08：$S_{\rm train}\cap S_{\rm test}=\varnothing,\quad M_{\rm train}\cap M_{\rm test}=8\text{种材料},\quad F_{\rm train}=\{3,6\},F_{\rm test}=\{9\}\,\mathrm N$。**

**表 T53-08｜312源段｜train208/test104｜24会话｜train16/test8、无会话重叠｜材料｜8/8跨split｜设定点｜3/6→9N。**

**来源/决策**：S53-08；D53-08。

## 09. 真实元数据的时间边界：不能把采样率当硬同步

对24条`sessions.csv`真实元数据计算，video统一30fps、audio 16kHz，而`tactile_est_fs_hz`在30.72到128.87之间，原始触觉行数2156到10694；其首尾epoch与音频首尾差值范围分别为-2至+2秒、-7至+13秒。**这些是源流覆盖区间与估计采样率差异，不是证明同步失败**：作者可能在后处理时裁剪共同区间并插值。只是不能从该目录推导“RGB、触觉、音频每个物理样本的同刻误差小于某硬界”。

R50/R51/R52要求原始捕获时刻、时钟域、跨钟校准及不确定性证书，依然适用。VisTouch README称严格事件级对齐，应在合法取得原始payload和`clips_index`/`frames_index`后，用独立触发事件或硬同步证据复核。D435本身也有HARDWARE_CLOCK、SYSTEM_TIME、GLOBAL_TIME时间域，ROS2 ApproximateTime只是消息头近似配对。未经同钟验证的浮点epoch秒不应直接填入DEV统一`int64 ns`的物理时间证据。

**公式 F53-09：$\Delta t_{\rm physical}\not\le 1/f_{\rm nominal}$；帧率/同一epoch标签不构成跨模态误差硬界。**

**表 T53-09｜会话目录｜video30fps、audio16000Hz、tactile估计30.72–128.87Hz｜结论｜同步硬界未知。**

**来源/决策**：S53-08；D53-09。

## 10. 新增最小算法：声明感知的数据准入Gate

新写`metadata_claim_gate.py`，只用Python标准库、Decimal、CSV与SHA，严格拒绝错误类型、未知声明、非有限数、重复session、缺train/test、符号链接、原始CSV SHA不匹配、重复sample ID、跨行会话材料/力设定点冲突、时间重叠与duration异常。支持`new_session`、`new_material`、`new_force_setpoint`和`new_session_and_material`四种**数据划分声明**。同一份24组源数据对新会话与新设定点返回`PASS_METADATA_SPLIT_ONLY`，对新材料及联合新材料声明返回`REJECT_UNSUPPORTED_GENERALIZATION_CLAIM`。

所有返回值都含`HOLD_NO_MOTION`、`hardware_permission=False`、`independent_spatial_pressure_truth=False`。即使调用者声称七类物理证据全部为真，`physical_data_gate`也只返回`REVIEW_REQUIRED_NO_CERTIFICATION`，绝不自动认证实物。可选`--source-samples`接受用户自己取得的固定上游CSV，先按Git blob SHA验证原始字节，再重建312行分组与随包摘要逐项对照；不会跟随CSV中外部路径，也不会执行pickle/torch权重。

**公式 F53-10：$G(c)=\bigwedge_{g\in R(c)}[G_{\rm train}(g)\cap G_{\rm test}(g)=\varnothing]$，其中$R(c)$由具体泛化声明决定。**

**表 T53-10｜new_session｜查session｜new_material｜查session+material｜new_force_setpoint｜查session+force标签。**

**来源/决策**：S53-08；D53-10。

## 11. 同条件强Baseline、公平消融和独立Oracle

**真实元数据四声明**：同一24组数据下，简单“train/test存在”基线与“session互斥”强基线均接受四种声明；独立集合真值表明新会话和新设定点为真、新材料与联合新材料为假。R53仅接受两个真实成立的声明。为避免只挑一个案例，又使用固定种子530052对同一24组公开元数据做256种**人工重分割**（按材料留出、按力设定点留出、随机会话留出），每种检查4个声明，共1024次。所有方法读取完全相同的session/material/force/split字段；独立Oracle在`run_ablation_r53.py`单独用集合运算计算逻辑真值，不调用候选Gate。

人工重分割中，真声明512、假声明512。弱基线和会话强基线均接受1024，故假接受各512；R53接受512个真实声明、拒绝512个不成立声明，假接受0、假拒绝0。**这不是压力预测准确率或统计显著性实验**，而是可复现的元数据语义正确性验证。没有使用模型推理、真实局部力、FR5或人体数据；同一输入强Baseline之所以不够，是因为它只验证session、不验证声明本身。

**公式 F53-11：$\mathrm{false\ acceptance}=\sum_{j=1}^{1024}\mathbf1[\hat G_j=1\land G_j^{\rm oracle}=0]$。**

**表 T53-11｜256组合成重分割×4声明｜弱/会话强baseline各512次假接受｜R53零假接受、512真接受｜仅元数据逻辑。**

**来源/决策**：S53-08；D53-11。

## 12. 主论文Baseline竞争：保留、修订与淘汰

主候选五篇不变：VPEC（IROS2022，Grady等，DOI`10.1109/IROS47612.2022.9982073`）仍第1，等待合法Sensel真实episode、计量和安全权重；FEATS（Helmut等，IROS2025，出版DOI沿R52、arXiv2411.03315）升第2，理由是可读全文、固定MIT作者代码、HF数据卡和明确FEA标签，适合优先做**离线方法学**审计；ViPER（Collins等，IROS2023，DOI`10.1109/IROS55552.2023.10342124`）降第3，仍缺数据许可、原始配对和安全权重；ImplicitRDP与RDP因高频接口与FR5不匹配证据缺口维持4、5。

排名变化不等于FEATS比ViPER真实压力估计更好，也不是对作者论文的否定。相邻EgoPressure2025提醒独立Sensel几何/单位与真人数据许可，RCT2026提醒按完整接触与材料留出，VisTouch2026提供非人体滑动流程但没有局部压力真值。主论文最终胜者仍待定。每篇的作者、题名、年份、DOI核验层级、源码/数据/许可/版本、控制接口、2025–2026邻近反证及最小补证见完整`BASELINE_AND_INNOVATION_REGISTER.md`。

**公式 F53-12：$\mathrm{rank}\ne\arg\min \mathrm{MAE}_{\rm crosspaper}$；当前排序基于可复现许可/数据/硬件/真值证据。**

**表 T53-12｜R52→R53｜VPEC 1→1｜FEATS 3→2｜ViPER 2→3｜ImplicitRDP 4→4｜RDP 5→5｜赢家待定。**

**来源/决策**：S53-10；D53-12。

## 13. FR5厂家FT_Control强Baseline与接口HOLD

FAIRINO官方4.0.0 Python力控手册明确`FT_SetConfig(company,device,softversion=0,bus=0)`中坤维厂商17、设备0写的是**KWR75B**；不能据此推出用户规划的KWR75D、控制器通常`[1,17,3,0]`与SDK v2.3.0/robot v4.0.0完全兼容。`FT_Control(flag,sensor_id,select,ft,ft_pid,...)`以N/Nm描述力/力矩，`FT_GetForceTorqueOrigin()`和`FT_GetForceTorqueRCS()`区分原始传感器与参考坐标数据，负载质心API以mm计；这和DEV统一SI米单位必须有明确转换。官方示例中的力值、速度、位移上限均**不得**视为本FR5刮痧安全阈值。

工程强Baseline仍是厂家法向恒力+保守连续有限宽扫掠+平滑UNLOAD+OFFSURFACE_RETURN，但目前只能写为**待验证接口规划**。FR5法兰、F/T测量原点、标定TCP、柔性刮板真实接触中心/接触线必须分开；腕部wrench不能唯一反演多点压力或摩擦。D435时间域、KWR75D偏置/重力/动态补偿、厂家控制环周期、ROS2 Humble只读边界、官方模型授权都没有实体证据；本轮零硬件命令、零物理仿真、零人体实验。

**公式 F53-13：$\boldsymbol\tau_C=\boldsymbol\tau_S-\boldsymbol r_{SC}\times\boldsymbol F$，前提是坐标轴、单位、方向和SE(3)标定一致。**

**表 T53-13｜厂家文档｜FT_Control N/Nm、FT_GetForceTorqueOrigin/RCS、负载质心mm｜HOLD｜KWR75D/SDK2.3.0/真实反馈。**

**来源/决策**：S53-09；D53-13。

## 14. 向DEV移交、非人体最小实验和停止条件

本轮向DEV移交`metadata_claim_gate.py`、24组真实公开源段的**派生**快照、实际split审计JSON与同信息Baseline消融；DEV可在M1人工有序3D点、M2合成画线/方向确认、M3专家审核可编辑点图和模板的离线HTML中显示`source_kind`、session/material/force-setpoint、时间域、单位、raw SHA、是否仅元数据通过及拒绝原因。统一3D位置、切向、法向、曲率/不确定性、真实有限宽工具姿态/连续扫掠资格、WORK—UNLOAD—OFFSURFACE_RETURN不变，接触前必须人工批准；本轮没有修改DEV工程或GitHub仓库。

下一轮最小可证伪实证不是再加模型，而是**合法取得一条非人体完整episode**：相机曝光与D435时间域、F/T原点和校准、独立至少左中右三分区力台原生counts/Pa及计量、柔性工具边缘曲率与SE(3)、各流时间硬界、原始字节SHA、许可、未见接触/材料盲检。先离线样例、后授权官方FR5模型的无接触物理仿真资格，最后在独立风险审查下做低能量非人体实验；人体另需伦理与风险审批。若合法独立局部真值持续不可得，正式暂停I-A并将资源转向DEV整机集成与I-B前置厂家时序证据。

**公式 F53-14：$G_{\rm next}=L\land U\land T\land X\land P\land B$（合法、单位、时间、空间注册、独立局部压力、盲检）。**

**表 T53-14｜DEV可用｜只读metadata_claim_gate与24会话快照｜仍HOLD｜实机/物理仿真/独立局部力台/真人。**

**来源/决策**：S53-08；D53-14。

## 附录：审计边界与配套文件

全部12项来源索引：S53-01 R52可访问归档；S53-02 FEATS全文；S53-03 FEATS代码；S53-04 FEATS数据卡；S53-05 EgoPressure摘要；S53-06 EgoPressure数据契约；S53-07 VisTouch仓库；S53-08 VisTouch真实CSV元数据；S53-09 FAIRINO手册；S53-10 VPEC/ViPER继承审计；S53-11 RealSense时间域；S53-12 ROS2消息时间配对。

完整候选论文元数据、DOI、许可证、版本、数据率/接口匹配、排名变化与退出Gate见`BASELINE_AND_INNOVATION_REGISTER.md`；来源实际读取层级见`SOURCE_REGISTER.md`；独立测试与公平消融见`R53_TEST_REPORT.md`、`R53_ABLATION.json`、`R53_ACTUAL_METADATA_AUDIT.json`；失败清单与DEV只读接口分别见`R53_FAILURES_AND_BLOCKERS.md`、`DEV_INTEGRATION_HANDOFF.md`。

本轮从未取得或执行VisTouch/FEATS/EgoPressure/VPEC/ViPER原始传感器payload/作者模型权重。所有“真实数据”仅指**真实公开源段CSV元数据**，不能替代非人体压力真值；未连接机器人、未发送动作、未进行物理仿真或人体实验。