# AUTO-R17｜FAIRINO FR5刮痧机器人博士级完整研究报告：主论文评估协议的公开源码反证与复现资格收敛
> 2026-10-20 Asia/Shanghai。**离线源码审计与程序研究内容验收通过；实体E0–E5未通过。** 没有连接FR5、没有运行原论文模型、没有人体实验。主论文与唯一创新点仍待定。

**研究范围声明**：本轮优先研究论文评估可信性而不是再构造同类合成边缘载荷故事。源码证据为公开GitHub只读获取并记录blob SHA；R17程序只实现评估契约，**没有在原作者仓库运行模型**。文献数不等于全文数。

## 01. 上一轮真实文件、代码与差分

已实际读取AUTO-R16研究报告、教学讲义、候选登记表、决策日志、非人体协议，并从R16 ZIP解压QA、源码、47项测试及来源审计；对主要文件计算SHA256。**真实重新运行R16 47项测试，47/47通过**。R16的新增贡献是多点加载交互使满秩单点标定失效；R17不继续重复同类合成载荷，而转向主论文复现资格与评估真值的真实源码审计。R17新发现：VPEC和ViPER公开训练入口均可能在训练期读取test命名的数据；ViPER还使用有放回评估。风险仅针对所审计的默认代码路径，不指控论文作者实际训练过程。R17没有访问FR5、没有发送运动指令。历史R16材料是可读本地文件，不等于其历史论文已在本轮重新全文核读。

**E17-01（核心公式）**

$$G_{17}=G_{prior\_read}\land G_{source\_audit}\land G_{independent\_criterion}\land G_{executable\_test}\land G_{falsifiable\_design}$$

**T17-01（本章证据/对照表）**

|核查项|R16|R17实际动作|状态|
|---|---|---|---|
|报告/讲义/日志/矩阵|R16有文件|读取并哈希；记录差分|已核|
|历史代码|47项|本轮独立重跑47/47|通过|
|新代码|多点交互反例|评估契约、抽样覆盖率与数据真值门控|48/48通过|
|实体数据|无|仍无；不虚构|E0–E5未过|

## 02. 本轮三项决策问题、唯一首选与可证伪假设

Q17-A：VPEC和ViPER谁具有**真正可审计的最小离线复现资格**？Q17-B：公开评估脚本是否保证独立测试集与公平指标？Q17-C：2026最新论文是否已覆盖普通视觉→力觉预测的原创空间？可证伪假设H17-1：在本轮审计的默认配置下，ViPER训练期可能读到final_test目录；用不同验证目录可消除此风险。H17-2：有放回抽样在有限集上的独立覆盖少于抽样次数；固定全量索引可避免抽样方差。H17-3：有真实独立分区载荷、时间同步、可见性后，matched RGB-D+F/T在固定放行覆盖率下优于wrench-only与错配视觉，否则I-A退出。唯一首选I-A仍条件保留，备选I-B仍为厂家力控+卸载返程状态机的增量问题。

**E17-02（核心公式）**

$$H_{17}:\ \Pr(\mathrm{false\_release}\mid B3,\mathrm{coverage}=c)<\Pr(\mathrm{false\_release}\mid B1,\mathrm{coverage}=c)$$

**T17-02（本章证据/对照表）**

|问题|独立判据|入门Gate|退出Gate|
|---|---|---|---|
|Q-A主论文|许可+权重+测试集+运行脚本|可合法获取最小资源|原论文不能复现且替代方案更强|
|Q-B公平评估|不混训练/测试+全量/固定抽样+独立真值|独立会话标签|无法建立独立评价|
|Q-C原创性|matched比wrench/错配视觉强|RGB-D可见/时间/分区真值|无可重复独立收益|

## 03. 14项去重论文/官方来源与获取层级

本轮逐条核对14项去重学术/官方来源，其中**4篇公开HTML全文的相关方法、实验和限制确实阅读**：VPEC2022、ViPER2023、ZeroTouch2026、EgoTac2026。另对VPEC/ViPER公开仓库进行14个源码/配置文件只读检查（7+7，逐文件SHA见审计JSON）；FAIRINO官方力控手册核接口。Chen、TensorTouch、TacGraph、Cho本轮只读摘要/项目/元数据；Jiang和Sferrazza仅使用历史记录，不计本轮全文。文献与同论文仓库是不同类型来源，**不是14篇论文全文**。EgoTac核对2026-10-16 v2，ZeroTouch核对2026-09-18 v1。作者代码仅只读，未下载权重、未执行原论文网络。

**E17-03（核心公式）**

$$\mathrm{evidence\_level}(s)\in\{\mathrm{full\_methods\_experiments},\mathrm{abstract},\mathrm{official\_docs},\mathrm{source\_audit},\mathrm{historical}\}$$

**T17-03（本章证据/对照表）**

|ID|作者/来源|年份与论文/官方资料|DOI/编号|本轮真实访问层级|入口|
|---|---|---|---|---|---|
|S17-01|Patrick Grady et al.|2022｜Visual Pressure Estimation and Control for Soft Robotic Grippers|10.1109/IROS47612.2022.9982073|open full HTML method and experiments|https://arxiv.org/html/2204.07268|
|S17-02|Jeremy A. Collins, Cody Houff, Patrick Grady, Charles C. Kemp|2023｜Visual Contact Pressure Estimation for Grippers in the Wild|10.1109/IROS55552.2023.10342124|open full HTML method and experiments|https://arxiv.org/html/2303.07344|
|S17-03|Dmitriy Kosenkov et al.|2026｜ZeroTouch: Tactile-Supervised Visual Contact Estimation for Contact-Rich Manipulation|10.48550/arXiv.2609.21726|open full HTML sections III-VI|https://arxiv.org/html/2609.21726|
|S17-04|Wenkang Zhang et al.|2026｜EgoTac: In-the-wild Tactile Prediction from Egocentric Vision|10.48550/arXiv.2608.15060|open full HTML version 2 sections 4-5 and appendix contents|https://arxiv.org/html/2608.15060|
|S17-05|Haofeng Chen et al.|2026｜Pneumatic-Tomographic Tactile Skin for Multicontact Localization and Force Estimation|10.1109/JSEN.2026.3704916; arXiv:2503.13036|abstract/metadata this round; R16 full historical|https://arxiv.org/abs/2503.13036|
|S17-06|Won Kyung Do et al.|2025｜TensorTouch: Calibration of Tactile Sensors for High Resolution Stress Tensor and Deformation for Dexterous Manipulation|10.48550/arXiv.2506.08291|project summary this round; R16 full historical|https://tensor-touch.github.io/|
|S17-07|Mark Van der Merwe et al.|2026｜Simultaneous Extrinsic Contact and In-Hand Pose Estimation via Distributed Tactile Sensing|10.1109/LRA.2026.3653324|publisher/research-lab metadata and abstract|https://www.merl.com/publications/TR2026-002|
|S17-08|Jinlei Jiang et al.|2024｜Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment|10.3390/act13020062|R16 indexed fragments historical; not full text this round|https://doi.org/10.3390/act13020062|
|S17-09|Junhwi Cho et al.|2026｜EIT-Pneumatic Hybrid Robotic Skin for Practical and Accurate Force Map Reconstruction|10.48550/arXiv.2605.28468|abstract only|https://arxiv.org/abs/2605.28468|
|S17-10|Healthcare-Robotics|2022｜VPEC public repository and evaluator|none|read-only 6 source files; SHA in provenance|https://github.com/Healthcare-Robotics/VPEC|
|S17-11|Healthcare-Robotics|2023｜ViPER public repository and evaluator|none|read-only 7 source files; SHA in provenance|https://github.com/Healthcare-Robotics/ViPER|
|S17-12|FAIRINO|2026｜Python Robot Force Control Manual 3.9.5|none|official documentation sections 12.11-12.22|https://documentation.fairino.co.in/SDKManual/PythonRobotForceControl.html|
|S17-13|FAIR-INNOVATION|2026｜fairino-python-sdk public repository|none|repository listing/Apache-2.0 metadata; not local FR5|https://github.com/FAIR-INNOVATION/fairino-python-sdk|
|S17-14|Carmelo Sferrazza et al.|2019｜Ground Truth Force Distribution for Learning-Based Tactile Sensing: A Finite Element Approach|10.1109/ACCESS.2019.2956882|historical abstract; not reread this round|https://arxiv.org/abs/1909.04000|

## 04. ViPER全文方法：弱标签、压力图与真实适配限制

S17-02全文第III节：ViPER使用Eye-in-Hand RGB观测软夹爪可见形变；在可控场景用压力阵列+六维F/T收集全标注数据，在复杂场景仅用F/T收集弱标注。模型包含图像编码、压力估计、力矩辅助分支及域判别；源码trainer_weak.py明确将压力损失、域损失、source/target的wrench弱监督损失相加。**弱标注只能约束压力图的一些合力统计，不能提供独立逐元压力真值**。第IV/V节对多环境、不同光照/纹理及抓取任务比较；结果仅属于原作者软夹爪和Sensel场景。FR5工具为柔性刮板，接触线可能被遮挡，深度相机视线、镜面反射、动态频率和工具形变均未证实适配。原论文推理仅RGB，不能把其训练用F/T误写成在线F/T融合。

**E17-04（核心公式）**

$$\mathcal L_{ViPER}=\lambda_p\mathcal L_{pressure}+\lambda_d\mathcal L_{domain}+\lambda_s\mathcal L_{wrench,source}+\lambda_t\mathcal L_{wrench,target}$$

**T17-04（本章证据/对照表）**

|要素|ViPER实际事实|FR5迁移约束|
|---|---|---|
|训练输入|全标注RGB+压力+wrench；弱标注RGB+wrench|FR5没有已标注刮板压力图|
|推理输入|单帧Eye-in-Hand RGB|刮板接触边缘可能不可见|
|训练目标|压力/域/6D wrench弱监督|弱监督不提供压力分布唯一性|
|实验真值|全标注域Sensel压力|需要独立非人体分区载荷参考|

## 05. ZeroTouch 2026全文：视觉预测六维wrench的新颖性反证

S17-03（arXiv:2609.21726v1，2026-09-18）全文III–VI：腕部RGB经冻结DINOv2提取视觉token，抓手位置与局部重力方向FiLM调制，18×24触觉query预测稠密变形、6D wrench及抓取期望压缩力。270个episode/15316同步样本，187训练、31验证、46测试、6诊断；验证Fz MAE从state-only 2.017N降至完整模型0.531N，**独立测试Fz MAE为0.828N，不得用验证值冒充测试值**。物理抓取3种偏移条件各20次，成功率19/20、16/20、18/20；不是FR5刮痧。论文VI明确承认内部DM-Tac W2作为训练与离线参考，**没有独立外部F/T校准**；某些不可见内部载荷无法仅靠RGB预测。此文直接压缩“RGB+状态预测wrench/接触变形”本身的新颖性。

**E17-05（核心公式）**

$$\big(I_t,q_t,g_t\big)\longmapsto\big(\widehat D_t,\widehat w_t,\widehat F_t^*\big),\qquad \widehat w_t\in\mathbb R^6$$

**T17-05（本章证据/对照表）**

|ZeroTouch项目|原论文实数/设计|本项目不能外推|
|---|---|---|
|数据|270 episodes，15316帧；187/31/46/6|非FR5、非刮板|
|Fz误差|验证0.531N；测试0.828N|不当作FR5定位/力控误差|
|物理成功|19/20、16/20、18/20|仅抓取终止任务|
|真值边界|同一触觉系统作为参考；无独立外部F/T校准|不能证明独立边缘压力分布|

## 06. EgoTac 2026全文：时序视觉触觉预测已被研究

S17-04的2026-10-16 v2公开全文第4–5节：5.7M多来源人手图像-触觉/接触配对样本，MANO统一手部表面，时序视觉编码并预测密集触觉和接触。训练报告4张A10 GPU、batch128；EgoTac-SC留出上的活动区域MAE 0.056N，时间相关系数0.506。跨传感器OpenTouch由于缺逐taxel面积和校准，作者不报告牛顿绝对MAE，而在归一化后评价相对动态趋势。**不能把归一化OOD曲线当真实绝对载荷验证**。EgoTac是人手视角，FR5非人体刮板不同；但“时序RGB→触觉图+接触分类”已是2026直接先例，简单增加时序Transformer不足以构成博士创新。本章E17-06是**教学性损失类别示意**，并非逐字引用论文损失原式。

**E17-06（核心公式）**

$$\mathcal L_{EgoTac}=\lambda_F\mathcal L_{force}+\lambda_C\mathcal L_{contact}+\lambda_T\mathcal L_{temporal}$$

**T17-06（本章证据/对照表）**

|EgoTac证据|论文说明|FR5适配限制|
|---|---|---|
|数据|5.7M配对，多源MANO手部|FR5工具不是MANO人手|
|训练|4×A10，batch128|GPU有但数据/预算未核|
|原域|活动区域MAE 0.056N，相关0.506|不是FR5精度|
|OOD|OpenTouch仅归一化相对动力学|无跨传感器绝对牛顿真值|

## 07. VPEC 2022全文与val命名的源码澄清

S17-01全文III–V：软夹爪在平面压力阵列上采集RGB/压力标签，约650k帧，单帧图像回归压力图；Stretch RE1在12Hz视觉推理下用简单控制完成表面接触/压力轨迹及小物抓取。论文承认曲面/未知纹理未覆盖，接触边界1–2N存在低估。R17实际审计VPEC的prediction/evaluator.py SHA 0512dc...：调用build_model(...,['val'])；model_builder.py SHA 7ebb...中val_dataset使用VAL_FILTER；config/stretch_nopinch.yml SHA 9cc3...明确VAL_FILTER=**data/test/stretch/**。**新增核读trainer.py SHA 20feac...：训练脚本在每轮调用val_epoch，因而在该默认配置下会在训练期间访问data/test路径。** 这说明不能仅凭变量名定罪，但追完整调用链后存在条件性最终测试独立性风险；仍不能据此断言原论文发表实验确实发生数据泄漏。原代码中`test`阶段显式报错，未提供真正独立test API。

**E17-07（核心公式）**

$$\mathrm{VPEC\_eval\_samples}=\mathrm{ForceDataset}(\mathrm{VAL\_FILTER}),\quad \mathrm{VAL\_FILTER}\subset\mathrm{data/test/stretch}$$

**T17-07（本章证据/对照表）**

|审计证据|SHA前缀/位置|正确解释|
|---|---|---|
|evaluator.py|0512dcb3，116–155|评估执行val_dataset|
|model_builder.py|7ebb92d5，43–72|val_dataset读取VAL_FILTER|
|stretch_nopinch.yml|9cc3f878，1–8|VAL_FILTER指向data/test|
|trainer.py|20feacbf，90–118|训练期每epoch调用val_epoch，可能触碰data/test|
|LICENSE|aa994b82，1–8|MIT许可证存在|

## 08. ViPER真实源码风险：final_test与训练期val路径

R17只读审计ViPER公开默认分支：config/ViPER_final_model.yml SHA 52dc...指定`TEST_FOLDER: [data/final_test_DONT_TOUCH]`；prediction/model_builder.py SHA a9e...把`val_dataset`绑定到`config.TEST_FOLDER`；prediction/trainer_weak.py SHA 2928...在训练期间构造val/val_weak并调用val_epoch，通常每轮执行，skip_val为真也会在最后一轮执行。**若使用该默认配置及训练入口，final_test会被用作训练期验证，不能再把它称为完全未触碰的最终测试集。VPEC公开训练入口也有类似问题，但ViPER的目录名更明确。** 这是代码路径条件性风险，不证明论文作者实际使用该组合生成发表指标，更不能推断恶意泄漏。必须让train/validation/final_test三个目录彻底分离，并保存运行命令、配置哈希和会话ID。ViPER仓库根目录本轮未见LICENSE文件，须进一步取得授权说明。

**E17-08（核心公式）**

$$\mathrm{TRAIN}\cap\mathrm{VAL}=\varnothing,\quad \mathrm{TRAIN}\cap\mathrm{FINAL}=\varnothing,\quad \mathrm{VAL}\cap\mathrm{FINAL}=\varnothing$$

**T17-08（本章证据/对照表）**

|源码链|实际值/行为|判定|
|---|---|---|
|ViPER_final_model.yml|TEST_FOLDER=final_test_DONT_TOUCH|名为最终测试|
|model_builder.py|val_dataset←TEST_FOLDER|训练期验证会访问该路径|
|trainer_weak.py|每epoch或最后epoch调用val_epoch|条件性测试集污染风险|
|本轮事实|无作者训练命令/权重运行日志|不指控论文结果已泄漏|

## 09. 有放回1000样本评估：数学覆盖率与公平性

ViPER prediction/evaluator.py SHA 7927...在evaluate_labeled与evaluate_weak分别构造`RandomSampler(replacement=True,num_samples=1000)`。若测试集合有N个独立样本、每次抽m次，则样本i从未被抽中的概率为(1-1/N)^m，期望独立样本数为N[1-(1-1/N)^m]。**当N=m=1000时，期望只覆盖632.30个不同样本**。R17以固定种子20261020运行Python标准库抽样，得到629个不同索引、371次重复；仅演示抽样性质，不是ViPER真实数据统计。此采样不能与VPEC顺序遍历全量测试帧直接比较。需冻结完整测试索引、按episode等权聚合、报告置信区间，必要时保留有放回抽样作为明确标注的另一个协议。

**E17-09（核心公式）**

$$\mathbb E[U]=N\left[1-\left(1-\frac1N\right)^m\right]$$

**T17-09（本章证据/对照表）**

|评估方法|数据访问|结果解释|
|---|---|---|
|ViPER源码|1000次有放回随机抽样|不是固定全量测试集|
|R17纯数学N=m=1000|E[U]=632.30|理论期望，非原论文N|
|R17固定种子示例|629个唯一、371次重复|人工索引模拟，非ViPER数据|
|公平方案|全量固定索引+会话等权|可与同样协议的VPEC比较|

## 10. 独立真值数据契约：不让wrench替代压力分布

本轮实现`require_independent_truth`：要求sensor_id、标定文件哈希、有效几何、采集时间来源、时钟误差界、压力真值类型、接触边缘几何真值、加载工况和独立会话ID。若仅有腕部合力/力矩，返回`HOLD`；若三区传感器的几何标定无效，仍HOLD。这里HOLD是**离线研究证据拒绝**，不是已认证的机器人安全控制指令。上一轮证明多点载荷交互可能让三区传感器产生相同读数；因此独立参考必须有多压头/隔离传力验证，且记录传感器插入是否改变接触。报告只允许以真实参考检验上界违反率，绝不把优化器自身的上界当独立真值。

**E17-10（核心公式）**

$$\mathrm{PASS}_{truth}=\bigwedge_{k\in\mathcal K}\mathrm{observed}(k)\land \mathrm{valid\_calibration}\land\mathrm{independent\_reference}$$

**T17-10（本章证据/对照表）**

|必需字段|作用|缺失时|
|---|---|---|
|独立压力/三区载荷参考|边缘份额外部真值|HOLD|
|多点几何标定哈希|跨曲率/材料可迁移性|HOLD|
|采集时间来源+时钟误差界|防视觉力觉错配|HOLD|
|会话/材料/工具标识|防数据泄漏|HOLD|
|工具真实边缘姿态参考|法兰/TCP/接触线分离|HOLD|

## 11. 时间错配负对照与会话级公平比较

本轮代码`negative_control_shift`只允许在**同一episode内部、相隔至少指定时间**的位置构造错配图像索引；`per_episode_paired_mae`按episode等权计算B1与B3误差差。错配视觉应进一步控制材料、几何、姿态、载荷区间，不能通过跨域差异制造虚假优势；时间滞后必须大于相机/力觉的相关窗口，且负对照不替代独立力真值。源码48项测试覆盖无候选拒绝、会话隔离、固定随机种子、元数据缺失和episode等权。R17没有RGB-D图像、没有训练任何融合网络，故没有“视觉改善百分比”。该负对照属于实验设计工具而非论文创新本身。

**E17-11（核心公式）**

$$\Delta_{episode}=\frac1K\sum_{e=1}^{K}\left[\frac1{n_e}\sum_{t\in e}(|y_t-\widehat y_{B1,t}|-|y_t-\widehat y_{B3,t}|)\right]$$

**T17-11（本章证据/对照表）**

|对照|输入条件|必须共同固定|
|---|---|---|
|B1|wrench+几何|独立真值、会话、阈值、覆盖率|
|B2|RGB-D-only|同相机/工具/标定|
|B3|匹配RGB-D+wrench|同采样时间界|
|B4|错配RGB-D+wrench|同会话/材料/几何，远离时间相关窗|
|B5/B6|留出域/去拒绝|同独立参考、同风险阈值|

## 12. FR5接口、坐标系、工具有限宽与安全Gate

厂家官方3.9.5 Python手册S17-12可见`FT_Control(flag,sensor_id,select,ft,ft_pid,adj_sign,ILC_sign,max_dis,max_ang,...)`，并有`FT_GetForceTorqueOrigin`与`FT_GetForceTorqueRCS`；**这只是文档签名，不证明本机固件支持、周期、外环权限或实际安全性能**。六维wrench需明确法兰F、传感器S、标定TCP T、柔性真实接触中心C及接触线L。力矩变换必须考虑平移臂，未知纯接触偶矩/摩擦/多点接触使中心压力估计不唯一。上层三入口M1人工有序点、M2实体画线、M3解剖标志专家审核穴位图都必须输出带位置/切向/法向/不确定性/工具姿态/有限宽扫掠资格/WORK-UNLOAD-RETURN模式的有序三维曲线；接触前必须用户审核。仅保留离面返程，贴面返程后续单独审查。

**E17-12（核心公式）**

$$\tau_S=R_{SF}\tau_F+p_{SF}\times(R_{SF}F_F),\quad T_{WC}\neq T_{WT}\neq T_{WF}$$

**T17-12（本章证据/对照表）**

|Gate|独立证据|本轮状态|
|---|---|---|
|E0|相机/手眼/刮板边缘/TCP实物标定|未过|
|E1|F/T偏置重力补偿+独立分区真值|未过|
|E2|相机曝光/力觉采集时间同步误差|未过|
|E3|FR5本机SDK固件FT_Control反馈与失联|未过|
|E4|非人体离面/低能量单向力控|未过|
|E5|非人体平滑卸载离面返程|未过|

## 13. 主学术Baseline竞争矩阵与FR5工程强基线

本轮根据**论文全文+作者公开代码实际可审计性**重排：VPEC继续在许可和最小评估入口领先，但170GB数据与权重未取得，且训练期val默认读取data/test；ViPER更接近Eye-in-Hand且有弱监督机制，却因final_test验证路径、有放回评估和许可证未核而降低**立即复现资格**；ZeroTouch/EgoTac作为2026直接新颖性竞争，不因结果新就自动成为FR5主论文；Jiang需2ms外环证据；Chen需EIT+气压额外硬件；TacGraph需分布触觉。**主论文仍待定**。独立工程强Baseline为厂家FT_Control+保守固定限速+WORK→UNLOAD→OFFSURFACE_RETURN，具体参数由本机非人体标定决定，不抄论文阈值。

**E17-13（核心公式）**

$$B_{main}=\arg\max_{p\in\mathcal P}\mathrm{evidence\_qualified}(p),\quad B_{engineering}=\mathrm{FR5\_FT\_Control+simple\_state\_machine}$$

**T17-13（本章证据/对照表）**

|论文候选|作者/年/DOI|全文/源码/数据/许可/版本|方法、参数与FR5适配|独立真值/2025–26反证|R16→R17排序与最小补证|
|---|---|---|---|---|---|
|VPEC|Grady等2022；10.1109/IROS47612.2022.9982073|R17全文；GitHub MIT；PyTorch1.12.1/Python3.10；约170GB数据与权重未取|外部RGB→平面压力图；软夹爪可见形变，非刮板；GPU可能；不需底层关节力矩|原文平面压力阵列；2023 ViPER更贴近EIH；遮挡无证据|许可/入口领先但评估独立性也有条件性风险；需隔离真正final_test、核权重/遮挡|
|ViPER|Collins/Houff/Grady/Kemp 2023；10.1109/IROS55552.2023.10342124|R17全文；公开训练/评估代码；模型OneDrive未取；根目录未见LICENSE；Ubuntu20.04|EIH RGB+六维F/T弱标签训练；推理仅RGB；依赖可见软夹爪；ROS2未核|全标注Sensel压力，弱标注只有wrench；ZeroTouch2026已覆盖腕部RGB→wrench|**复现审计降级**：训练final_test配置及有放回评估；须拆分会话/固定索引/核许可|
|Jiang IAAC|Jiang等2024；10.3390/act13020062|R16仅索引片段，R17未重核全文；源码/许可/数据不明|积分自适应导纳，论文2ms外环；FR5是否支持未知|超声软仿体；厂家FT_Control及简单限速必比|有条件保留不变；本机只读接口、周期和固件|
|Chen多接触|Chen等2026；10.1109/JSEN.2026.3704916|R16全文、R17摘要；源码/许可/数据未核|EIT空间+气压总量，1.3帧/s EIT/20Hz气压；需额外皮肤|多接触砝码真值；已覆盖位置相关校准与总量融合|方法学强反证不变；原样不适配FR5|
|ZeroTouch|Kosenkov等2026；10.48550/arXiv.2609.21726|R17全文；代码/数据/许可未独立核验|腕部RGB+gripper状态+重力→密集变形/6D wrench/期望力；DINOv2|DM-Tac W2自身作参考，论文承认无独立外部FT校准|**新增新颖性强竞争**；不是可直接复现FR5刮板|
|EgoTac|Zhang等2026；10.48550/arXiv.2608.15060|R17 v2全文；项目资源可见，权重/许可未核|5.7M人手样本；4×A10训练；MANO手部，不是FR5|OOD力仅归一化相对动力学，无跨传感器绝对牛顿标定|**新增新颖性强竞争**；仅相关先例，暂缓主论文|
|TacGraph|Van der Merwe等2026；10.1109/LRA.2026.3653324|R17实验室摘要；源码/数据/许可未核|几何/非穿透/力平衡因子图；分布触觉+夹持|已覆盖普通几何接触融合；非刮板独立压力图|方法学竞争不变；本轮未升主Baseline|

## 14. 四个创新候选、唯一首选/备选与跨轮决策

I-A唯一条件性首选：**有限宽刮板边缘风险的部分可辨识区间与证据不足可信拒绝**。其潜在原创不在RGB/力觉融合本身，而在明确可辨识假设、独立分区真值、空间-时间误差证书与固定覆盖率对照；ZeroTouch/EgoTac/ViPER/Chen已压缩普通融合新颖性。I-B唯一备选：WORK-UNLOAD-OFFSURFACE_RETURN延迟鲁棒控制，但厂家FT_Control和固定限速若足够则不做主创新。摩擦-法向耦合分离、动态TCP只作功能或暂缓，未知摩擦/纯偶矩不可唯一分解。本轮D17-01..09全部记录于独立DECISION_RECORD.md，**本轮实质修订的是复现资格和评估可信性，不是宣称已经解决真实压力估计**。

**E17-14（核心公式）**

$$\mathrm{release}=1\ \Longrightarrow\ U_{edge}(t)<r_{max}\land G_{geometry}\land G_{time}\land G_{reference}\land G_{visibility}$$

**T17-14（本章证据/对照表）**

|方向|直接新颖性反证|可观测/执行/独立真值|R17判断与退出|
|---|---|---|---|
|I-A有限宽边缘风险区间+可信拒绝|ViPER/ZeroTouch/EgoTac/Chen已覆盖常规视觉-力估计|wrench不唯一；RGB-D可见性未知；必须独立多压头/力台/时钟|唯一条件性首选；同覆盖率B3不优于B1/B4即退出|
|I-B WORK→UNLOAD→OFFSURFACE_RETURN|Jiang IAAC/厂家FT_Control/简单限速已有|本机FT_Control、反馈、独立力台缺|唯一备选；简单状态机足够则退出主创新|
|摩擦-法向-变形扰动分离|已有摩擦/触觉观测；本轮无新直接全文|未知纯偶矩、多点压力、摩擦系数不可唯一反演|暂缓；不叠ESO|
|RGB-D+F/T接触TCP纠偏|TacGraph与动态接触估计先例|法兰/标定TCP/真实接触线不同；需独立几何真值|仅作为I-A必要功能，不另立博士创新|

|决策ID|实际变化|退出/补证|
|---|---|---|
|D17-01|VPEC复现资格领先但未复现；ViPER与FR5视角更接近|源码许可/权重/遮挡需核|
|D17-02|**新增完整调用链反证**：VPEC训练每轮val_epoch访问VAL_FILTER=data/test|仅公开默认路径风险，不证明原论文实验泄漏|
|D17-03|ViPER final_test_DONT_TOUCH可能进入训练期val|仅条件性源码风险，不能指控原论文|
|D17-04|ViPER 1000有放回评估不是全量确定性|固定索引与分会话重评|
|D17-05|wrench弱标签不能作为独立压力真值|多压头/力台/压力阵列独立校准|
|D17-06|ZeroTouch/EgoTac压缩普通RGB→触觉新颖性|保留边缘风险可信拒绝条件性主线|
|D17-07|I-A同覆盖率匹配/错配RGB-D对照强制|无真实可见性/同步则退出|
|D17-08|FT_Control工程基线继续，但本机未核|先只读SDK/固件/反馈|
|D17-09|本轮不再累加合成压力“性能”|转向实际源码复现资格和实体数据|

## 15. 非人体物理验证路线、三入口契约与消融

真实验证必须从**无机器人动作的标定**开始：E0手眼/TCP/边缘，E1腕部F/T零偏/重力/动态补偿及独立多压头/力台，E2独立同步事件，E3厂家本机SDK/固件/反馈时序；通过后才可E4离面轨迹、低能量单向恒力，E5平滑卸载与离面返程。贴面轻力返程后续审查，真人需独立风险与伦理批准。M1手工有序点、M2实体画线起终方向确认、M3解剖标志/专家审核穴位图均产生有序3D关键点曲线并由用户接触前确认。消融B0厂家简单力控、B1 wrench-only、B2 RGB-D-only、B3 matched融合、B4 mismatched、B5跨材料几何留出、B6去拒绝；所有方法同独立参考、同阈值和覆盖率。

**E17-15（核心公式）**

$$\mathrm{curve}_i=(p_i,\hat t_i,\hat n_i,\Sigma_i,R_{tool,i},q_{sweep,i},m_i),\quad m_i\in\{WORK,UNLOAD,RETURN\}$$

**T17-15（本章证据/对照表）**

|模块/对照|输出或实验|不满足时|
|---|---|---|
|M1|人工选点与指定路线→有序3D曲线|拒绝接触|
|M2|实体画线识别/方向确认→有序3D曲线|拒绝接触|
|M3|背部标志/专家审核穴位图→路线/模板|拒绝接触|
|B0–B6|同会话、同独立真值、同覆盖率|不宣称算法收益|
|真人|独立风险和伦理审批|禁止开展|

## 16. 风险覆盖率、零错误与统计证据的正确解释

研究者容易把“合成数据零错误”或“10次实验零失败”说成安全保证。R17明确预注册：先固定风险放行覆盖率c，再在独立非人体会话上计算上界违反率；即使观测0次错误，也必须给出统计上界。对于K个真正独立Bernoulli会话、观察k个错误，可用Wilson上置信界描述不确定性。**若K=10且k=0，95% Wilson上界仍约0.2775**，不是0；若连续帧高度相关，则K应按独立会话而非帧数计。本轮`wilson_upper`代码与5项新增测试实现该统计门控，仍无真实FR5会话。公平对照应报告覆盖率、上界违反率、拒绝率、各材料/几何留出和误差置信区间，且不得以全部HOLD获得零错误。

**E17-16（核心公式）**

$$U_{Wilson}=\frac{\widehat p+z^2/(2K)+z\sqrt{\widehat p(1-\widehat p)/K+z^2/(4K^2)}}{1+z^2/K}$$

**T17-16（本章证据/对照表）**

|统计对象|正确单位|错误解释|
|---|---|---|
|边缘风险上界违反|独立非人体会话/加载工况|把相邻帧当独立试验|
|风险放行覆盖率|同阈值同覆盖率比较|全部HOLD伪安全|
|0/10观察错误|Wilson95%上界约0.2775|声称真实失败率为零|
|R17实现|Wilson函数+5单测|不代表已有FR5物理统计|

## 17. 本轮可执行代码、真实结果、QA与下一轮自适应

R17新增`src/evaluation_contract.py`、48项`unittest`、只读GitHub blob SHA审计JSON、14项来源登记、空白非人体数据清单、预注册协议。**48/48新增测试通过，47/47 R16回归通过**。第一次新增测试因浮点`0.9999999999999998 != 1`失败，保留原始失败日志，修正为容差比较后重跑通过。真实计算：N=m=1000理论唯一632.304575，固定种子抽到629个唯一索引；这只是Python抽样例子，不是ViPER模型运行。**原论文权重下载0、原论文推理0、FR5实验0、人体实验0**。下一轮优先获得真实非人体RGB-D遮挡/独立多点载荷/本机只读反馈，若不可得则尝试合法最小VPEC权重/样本及公开ViPER许可确认；没有这些证据不得锁定主论文。

**E17-17（核心公式）**

$$\mathrm{QA}_{17}=\mathrm{tests}_{48/48}\land\mathrm{regression}_{47/47}\land\mathrm{chapter\_mirror}_{17/17}\land\mathrm{source\_decision\_coverage}$$

**T17-17（本章证据/对照表）**

|实际产出|数量/状态|不能据此宣称|
|---|---|---|
|新增测试|48/48；首次1失败已留档修复|非FR5力控性能|
|R16回归|47/47|非真实传感器精度|
|抽样精确分析|期望632.304575；固定样本629|非ViPER实际测试覆盖|
|论文全文|4篇方法/实验核读|非14篇全文|
|硬件与模型|0次机器人、0次权重运行|非论文复现|

## 附录A. 源码审计与独立真值清单

源码只读SHA/行号见`provenance/GITHUB_SOURCE_AUDIT.json`；来源访问层级见`provenance/SOURCE_REGISTER.json`；数据请求字段见`protocol/NONHUMAN_DATA_REQUEST_R17.csv`；完整公平对照预注册见`protocol/EVAL_PREREGISTRATION.md`。R16上一轮完整文件与源码位于`prior/`，但上一轮的源码回归和R17源码审计是两件不同的事。

**实验阻塞**：FR5本机SDK/固件、Eye-in-Hand相机曝光时刻、六维F/T采集时刻、真实刮板边缘/TCP、独立多点压头/力台、跨材料/几何/同时加载校准均缺失。先只读采集；任何接触运动需独立安全审查。

**仓库合规**：没有修改hoshizaki-akari/robot，没有创建任何仓库，没有向GitHub写入。