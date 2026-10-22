# AUTO-R37｜FAIRINO FR5刮痧机器人博士级完整研究报告

**日期：2026-10-21；状态：PASS_OFFLINE_RESEARCH_CONTENT_ONLY；实体E0–E5 HOLD；主学术Baseline待定。**

**唯一聚焦：单向接收时间是否能识别真实采样时刻？** 本轮以R36实际归档为起点，新增不可辨识性证明、二维LP、共享时钟配对、错误证书反例；全部测试为离线数学数据，零FR5、D435、KWR75D、分段力台实测。

## 01｜R36归档、DEV-R10只读接口与真实差分

本轮实际打开R36研究ZIP（38成员，37项内部SHA记录）、独立研究报告、逐章讲义、候选矩阵、决策日志、R36测试源码和结果，并对照外部ZIP哈希；同时只读打开DEV-R10完整工程ZIP（834成员），核查clock_sync_gate.py的仿射FIT/BLIND契约。R36把时间差当已有外部上界传播到载荷矩阵误差；DEV-R10用参考脉冲误差盒拟合仿射时钟。二者均未明确证明“host receive timestamp = independent capture event time”。R37只补这一证据缺口，不重命名既有算法，也不重建DEV工程。R36报告的277项历史执行记录不直接算R37执行；本轮重新解压并实际运行。

**公式 F37-01**

$$
\Delta_{37}=\{\text{one-way latency alias},\ \text{shared-clock projection},\ \text{certificate counterexample}\}
$$

**表 T37-01**

| 证据 | 上一轮状态 | 本轮新证据 |
| --- | --- | --- |
| R36时间Gate | 给定δ与速率的读数误差界 | 一方向到达延迟不等于采样时刻 |
| DEV-R10 clock LP | 脉冲FIT/BLIND、HOLD | 显式未知传输延迟与共享映射关联 |

**本章来源**：S37-01,S37-02；**决策**：D37-01,D37-02。

## 02｜三项决策问题、预注册假设与Gate

Q1：仅有设备事件时间与主机接收时间，能否唯一分离时钟偏移和单向传输延迟？Q2：若传输延迟确有独立可验证上下界，能否用共享仿射时钟模型取得既覆盖真值又不过分宽的事件时间区间？Q3：同一个时钟域两帧的相对采样时间，能否比逐帧独立区间更准确地判断同步资格？H1为存在不可辨识的偏移—延迟变换；H2为真实延迟在证书盒内时LP覆盖；H3为共享偏移抵消改善相对时间判定。证伪材料必须包括外部双端时间证据、已知时钟真值、跨设备漂移和未知排队延迟故障。

**公式 F37-02**

$$
G_{37}=G_{clock\_id}\land G_{latency\_certificate}\land G_{FIT/BLIND}\land G_{capture\_semantics}
$$

**表 T37-02**

| 问题 | 独立判据 | 失败动作 |
| --- | --- | --- |
| Q1 | 两个物理世界同到达序列 | 禁止点时差结论 |
| Q2 | 盒内真值覆盖/区间宽度/拒绝率 | 扩大区间或HOLD |
| Q3 | 相同数据下共享与独立盒对照 | 不满足容差则HOLD |

**本章来源**：S37-01,S37-03,S37-11；**决策**：D37-02,D37-04,D37-11。

## 03｜RFC5905四时间戳与非对称假设

实际核读RFC5905官方完整标准的§8、§10、§11和四时间戳公式。其T1客户端发送、T2服务器接收、T3服务器发送、T4客户端接收定义偏移θ和往返δ。若两个方向传播时间不同，经典对称近似将半个方向差吸收到偏移。标准的dispersion、root distance是网络时间可信度统计，不能自动成为D435曝光时刻的物理证书。本轮没有配置NTP，也没有复现RFC的协议栈。

**公式 F37-03**

$$
\theta=\tfrac12[(T_2-T_1)+(T_3-T_4)],\quad \delta=(T_4-T_1)-(T_3-T_2)
$$

**表 T37-03**

| 来源 | 数学信息 | FR5迁移限制 |
| --- | --- | --- |
| RFC5905 | 四时间戳、偏移、往返时延 | 网络主机同步≠曝光中点同步 |
| R37单向证据 | 只有设备事件与主机到达 | 必须另给单向延迟上下界 |

**本章来源**：S37-03,S37-05；**决策**：D37-03,D37-08。

## 04｜Zhang等2024非对称PTP论文的方法、实验与限制

本轮取得官方检索索引的§4方法与§6实验正文片段，核读窗口最小延迟选择、漂移补偿、模糊PI以及网络负载/交换机跳数对比。作者在100Mbps台架与背景流量≤70Mbps时报告max|TE|通常≤0.35μs；当80Mbps、1518B时报告37.8μs，说明最小包选择会在排队普遍存在时失效。其比较还涉及LF-PI、KF-PI、Fuzzy-PI与QIDACM；未重跑作者平台。尝试直接取得原站全文/PDF遭429，故本轮严格记为“正文方法实验片段”，不是已下载全文。该论文提供反证：单向延迟/排队补偿已是成熟研究，不能成为FR5论文的主创新。

**公式 F37-04**

$$
\widehat{\theta}_{window}=\tfrac12\bigl(\min t_{21}^{\prime}-\min t_{43}^{\prime}\bigr)+\widehat{y}N
$$

**表 T37-04**

| 方法/条件 | 作者报告 | 不可外推 |
| --- | --- | --- |
| 窗口+漂移+模糊PI | 背景≤70Mbps时max|TE|≤0.35μs | 仅作者PTP台架 |
| 高负载80Mbps | 1518B时max|TE|=37.8μs | 非D435或KWR75D |

**本章来源**：S37-04,S37-05；**决策**：D37-08,D37-12。

## 05｜2023与2026邻近工作：延迟非对称不能宣称首创

IEEE/ACM ToN 2023论文DOI 10.1109/TNET.2022.3229407的官方摘要描述以非对称变化的导数和状态空间模型估计时钟偏移，作者报告在其非对称链路真实数据上有明显改善；本轮仅摘要可读，未核全文推导。2026 PubMed索引的多时域延迟非对称预测研究用铯钟参考和硬件时间戳进行铜/光纤链路验证，仍只读摘要。与Zhang2024窗口最小值+模糊PI相比，前者偏向模型驱动状态估计，后者强调拥塞窗口鲁棒性，2026研究强调外部高精度真值。三者共同否定“首次处理时钟非对称”的新颖性，且均不提供本FR5的分段接触真值。

**公式 F37-05**

$$
\widehat{\theta}=\theta+\tfrac12(d_{forward}-d_{reverse})\quad(\text{对称估计偏差})
$$

**表 T37-05**

| 文献 | 数学/实验来源层级 | 迁移风险 |
| --- | --- | --- |
| ToN2023 | 摘要：状态空间/非对称导数、真实链路 | 全文和参数未核 |
| Zhang2024 | 正文片段：窗口+模糊PI、台架 | 高负载失效 |
| 2026多时域预测 | 摘要：铯钟硬件参考 | 设备不同，非本项目 |

**本章来源**：S37-04,S37-05,S37-06；**决策**：D37-08,D37-12。

## 06｜ReDAG-RT 2026全文核读：执行调度不是采样时间

实际打开arXiv:2603.18238v1完整HTML，核读§III全局Rate-Priority DAG队列、§IV Humble合成工作负载实验及§V结论。作者比较SingleThreadedExecutor、MultiThreadedExecutor和ReDAG-RT，报告P99回调响应49ms→28ms；“29.7%下降”是其4线程→8线程配置内对比，不可误写为与默认ROS2同条件。最佳配置合并deadline miss仍为0.476，说明作者自己实验也未达到零丢期。作者仓库README声明Apache-2.0与Humble容器；未运行作者代码/固定commit。调度回调的开始/结束时间与D435曝光时刻、KWR75D采样时刻不是同一个随机变量，调度优化不能替代独立采样时间证据。

**公式 F37-06**

$$
R_i=f_i-r_i,\quad L_i=f_i-d_i,\quad \mathrm{MR}=N_{miss}/N_{jobs}
$$

**表 T37-06**

| 指标 | 作者实际比较 | 对本项目的解释 |
| --- | --- | --- |
| P99响应 | 默认49ms、ReDAG28ms | 回调延迟，非传感器时钟精度 |
| 29.7% | 作者4→8线程内部比较 | 不可与默认执行器混作公平比较 |
| 最佳miss | 0.476 | 仍非硬实时保证 |

**本章来源**：S37-07,S37-08,S37-09；**决策**：D37-07,D37-08。

## 07｜D435/ROS2 Humble时间戳语义与来源证据

官方RealSense rs_frame.h列出HARDWARE_CLOCK、SYSTEM_TIME、GLOBAL_TIME三种时间域；SENSOR_TIMESTAMP定义为曝光中点，FRAME_TIMESTAMP为帧读出/传输开始，TIME_OF_ARRIVAL为主机收到，BACKEND_TIMESTAMP为驱动时间。官方metadata说明还提示部分属性依赖固件、驱动和每帧可用性。ROS2 Humble message_filters官方教程用header.stamp近似配对，并提示QoS匹配；它不会核验header的物理采集语义。R37仅接受显式EXTERNAL_PULSE_EDGE作时钟拟合样本，不把帧读出、消息接收伪装成曝光事件；未来需要曝光偏移和实际KWR75D采样语义独立校准。

**公式 F37-07**

$$
t_{arrival}=t_{exposure}+d_{readout}+d_{transport}+d_{scheduler}
$$

**表 T37-07**

| 时间字段 | 语义 | 直接当接触时刻？ |
| --- | --- | --- |
| SENSOR_TIMESTAMP | 曝光中点（需设备支持） | 需时钟域映射 |
| FRAME_TIMESTAMP | 读出/传输开始 | 不能 |
| TIME_OF_ARRIVAL | 用户态到达 | 不能 |
| ROS header.stamp | 消息声明时间 | 需核发布者语义 |

**本章来源**：S37-09,S37-10,S37-11；**决策**：D37-07,D37-11。

## 08｜核心不可辨识性证明：一方向偏移与延迟等价

设设备事件时间d_i，主机到达r_i，主机时钟相对设备的仿射偏移a与漂移q，未知单向延迟ℓ_i。观察方程r_i=h_0+x_i+a+q x_i/10^6+ℓ_i。对任意常数κ，只要ℓ_i−κ仍满足物理范围，令a′=a+κ、ℓ_i′=ℓ_i−κ，则全部观测r_i完全相同。即使FIT样本无限多，恒定未知传输延迟也无法从时钟偏移中唯一分离。构造40组两世界观测完全一致的固定种子反例，算法正确地保持区间而不输出唯一时差。若延迟证书完全未知，任何窄点时钟估计都没有物理依据。

**公式 F37-08**

$$
(a,\ell_i)\mapsto(a+\kappa,\ell_i-\kappa),\quad r_i\ \text{不变}
$$

**表 T37-08**

| 真实世界A | 真实世界B | 观测 |
| --- | --- | --- |
| 偏移100μs，延迟500μs | 偏移300μs，延迟300μs | 到达均为600μs附加量 |
| 40组随机等价对 | 偏移/延迟各平移100μs | 40/40到达序列完全一致 |

**本章来源**：S37-03,S37-04,S37-05；**决策**：D37-03,D37-08。

## 09｜有界单向延迟LP与可复现的最小算法

若有独立证据给出每次传输延迟ℓ_i∈[L_i,U_i]，将大纪元整数时间戳先相减，再以offset_us和ppm为优化变量。每个脉冲提供两条线性不等式：r_i−h0−x_i−U_i ≤ 1000a_us+q x_i/10^6 ≤ r_i−h0−x_i−L_i。冻结FIT所有约束形成二维多面体；对目标事件时间分别最小化/最大化同一线性函数。BLIND仅追加约束做共同可行性检查，不把BLIND重新用于缩窄FIT投影。算法还检查证书SHA、时钟ID、样本digest、FIT/BLIND episode隔离、因果可用性、漂移先验与时间外推范围；所有输出HOLD。

**公式 F37-09**

$$
r_i-h_0-x_i-U_i\le1000a_{us}+q x_i/10^6\le r_i-h_0-x_i-L_i
$$

**表 T37-09**

| 算法 | 输入 | 输出/安全 |
| --- | --- | --- |
| FIT二维LP | 设备事件、主机到达、延迟证书盒 | 冻结(a,q)可行域 |
| BLIND一致性 | 不同episode脉冲 | 仅冲突/HOLD，不重新拟合 |
| 事件投影 | 待核时刻、冻结可行域 | 整数ns保守区间/HOLD |

**本章来源**：S37-03,S37-15；**决策**：D37-04,D37-06。

## 10｜共享时钟相关性与有限宽工具时序配对

两个事件来自同一设备时钟，映射分别为h(x_a)=h0+x_a+a+q x_a/10^6、h(x_b)=h0+x_b+a+q x_b/10^6。相减后公共offset a完全抵消。若先分别求每帧独立区间再相减，相当于允许两帧取不同a，会过度保守。R37新增pair_same_clock，在同一冻结FIT多面体上直接优化差值；与pair_independent_boxes在同一120组合成事件中公平比较。容差判据用所有可能时差的最大绝对值≤阈值才标记时间资格通过（仍HOLD）；若最小绝对值>阈值则拒绝，否则不确定。该资格仅表示时间关系，不表示有限宽工具接触安全。

**公式 F37-10**

$$
\Delta h=(x_a-x_b)\bigl(1+q/10^6\bigr),\quad a\ \text{抵消}
$$

**表 T37-10**

| 配对方法 | 变量相关性 | 本轮120组合成判据 |
| --- | --- | --- |
| 独立盒相减 | 丢失共享offset | 0/120可保证≤1ms |
| 共享LP差值 | 保留同一a、q | 120/120可保证≤1ms |
| 真值生成器 | 已知真实时钟 | 120/120实际≤1ms |

**本章来源**：S37-03,S37-09；**决策**：D37-05,D37-07。

## 11｜独立真值、BLIND与错误证书的决定性反例

正常盒内FIT/BLIND通过不等于独立物理真值通过。R37故意构造60组系统性错误证书：真实单向延迟恒为1.2ms，元数据却声称[0.1,0.9]ms；FIT与BLIND都来自同一错误链路。结果60/60 BLIND仍共同可行，但真实时钟偏移区间覆盖0/60。另60组BLIND单独发生3ms偏移跳变，冻结FIT联合约束全部拒绝。两种故障必须同时报告：算法能发现与FIT不一致的变化，却无法仅靠自身发现始终一致的错误计量证书。SHA256只能发现字节变化，不能证明证书测量正确。

**公式 F37-11**

$$
G_{truth}\neq G_{FIT/BLIND};\quad \mathrm{consistent}\not\Rightarrow\mathrm{correct}
$$

**表 T37-11**

| 故障 | BLIND结果 | 独立真值 |
| --- | --- | --- |
| 盒内正常 | 120/120一致 | 120/120事件真值覆盖 |
| 3ms跳变 | 60/60拒绝 | 拒绝而非覆盖 |
| 错误延迟证书 | 60/60仍通过 | 0/60真实偏移覆盖 |

**本章来源**：S37-03,S37-06,S37-11；**决策**：D37-06,D37-12。

## 12｜严格单位、坐标、数值稳定与代码边界

本轮输入全部为非负int64 ns的设备事件/主机接收/可用时间，漂移ppm，offset prior ns，优化变量offset_us。先做Python整数减法再转浮点，避免约1.8e18ns的Unix纪元超过2^53导致纳秒丢失。限定FIT时间跨度至少1s、计算域≤1000s，投影两端加20ns数值防护；这是离线数值约束，不是经过形式化验证的所有SciPy平台误差上界。力学坐标继续使用TOOL_CONTACT_LOCAL与ENV_ON_TOOL，法兰、TCP、柔性真实接触线不混淆。R37没有从时钟区间推断压力分布或摩擦系数，所有SDK/ROS2运动适配器不存在。

**公式 F37-12**

$$
t_{host}=h_0+x+1000\,a_{us}+q\,x/10^6\quad (x\in\mathbb{Z}_{ns})
$$

**表 T37-12**

| 字段 | 类型/单位 | 拒绝条件 |
| --- | --- | --- |
| device/receive/available | int64 ns、明确域 | 布尔/浮点/负数/可用性倒置 |
| offset/ppm | ns与ppm | 先验反向/非有限/超域 |
| contact frames | 法兰/TCP/真实线独立 | 用净wrench冒充压力分布 |

**本章来源**：S37-10,S37-11,S37-15；**决策**：D37-04,D37-07。

## 13｜代码实现、真实测试回归与归档证据

新增原创src/one_way_clock_gate.py、tests/test_one_way_clock_gate.py、run_ablation.py；没有复制NTP、PTP、ReDAG作者代码。实际执行R30–R36七套继承pytest共277项（45+44+20+55+40+43+30），R37新增78项，总计355/355。新增覆盖一方向偏移—延迟等价、隐藏BLIND跳变、证书系统错误、共享/独立配对、哈希篡改、坐标时间域/证书版本、FIT/BLIND隔离、int64大纪元、30组随机真值覆盖。历史测试均从实际嵌套ZIP解压重跑，不使用R36旧日志代替执行。所有结果为离线数学验证。

**公式 F37-13**

$$
N_{pytest}=277_{history}+78_{new}=355
$$

**表 T37-13**

| 测试层 | 本轮实际执行 | 性质 |
| --- | --- | --- |
| R30–R36 | 277项全部通过 | 历史独立解压 |
| R37 | 78项全部通过 | 新增数值/拒绝/反证 |
| 物理实验 | 0次 | FR5/D435/KWR75D均未接入 |

**本章来源**：S37-01,S37-02,S37-15；**决策**：D37-01,D37-11。

## 14｜公平消融：正常、隐藏漂移、证书失真与别名

固定种子370037、120组正常合成episode、同一设备事件和已知时钟生成器，比较到达即采样点估计、漂移先验强区间基线、带独立延迟盒的LP以及独立盒/共享LP配对。正常120组中，到达点估计在±1μs标准下0/120覆盖，平均绝对误差约505μs；先验盒120/120覆盖但平均宽10ms；LP120/120覆盖、平均宽0.487494ms。共享配对120/120保证≤1ms，独立盒0/120；隐藏BLIND跳变60/60拒绝；错误延迟证书60/60 BLIND仍过而0/60覆盖；40组别名完全一致。报告全体样本分母，不筛掉拒绝者；这些数值不是任何FR5实测性能。

**公式 F37-14**

$$
\mathrm{coverage}=N_{truth\in interval}/N_{all},\quad W=U-L
$$

**表 T37-14**

| 同条件方法 | 真值覆盖/配对保证 | 平均宽或失败 |
| --- | --- | --- |
| 到达即采样点估计 | 0/120（±1μs） | MAE≈505μs |
| 漂移先验盒 | 120/120 | 10.000ms |
| 有界延迟LP | 120/120 | 0.487494ms |
| 独立盒配对→共享LP | 0/120→120/120 | 同一冻结FIT |

**本章来源**：S37-01,S37-03,S37-15；**决策**：D37-05,D37-06。

## 15｜五篇主学术Baseline竞争矩阵与工程强基线

当前投入排序维持VPEC、ViPER、FEATS、ImplicitRDP、RDP，但不是性能排名。VPEC作者仓库本轮重新确认MIT代码与约170GB数据，数据许可、commit和权重SHA仍缺；ViPER作者仓库本轮重新查看根目录无显式LICENSE，Sensel API、OneDrive权重/数据授权仍未核；FEATS历史登记GelSight Mini/FEA与D435不同；ImplicitRDP历史登记Flexiv高频力控接口未迁移；RDP本轮RSS官方页确认DOI和慢快策略，但没有FR5实时可执行证据。厂家FT_Control恒力+平滑UNLOAD+OFFSURFACE_RETURN仍为规划工程强baseline，SDK v2.3.0/robot v4.0.0未接入。时钟校准论文和ReDAG不进入主学术baseline竞争，因为它们不是刮板接触载荷/控制算法。

**公式 F37-15**

$$
B^{*}\in\{\mathrm{VPEC,ViPER,FEATS,ImplicitRDP,RDP}\},\quad B^{*}=\text{待定}
$$

**表 T37-15**

| 候选 | 复现资格 | 本轮判断 |
| --- | --- | --- |
| VPEC #1 | MIT代码，数据/版本未冻结 | 有条件保留 |
| ViPER #2 | 源码许可证不明，Sensel/权重授权未核 | 条件保留但不得直接复现 |
| FEATS #3 | GelSight/FEA与外部D435不同 | 方法对照 |
| ImplicitRDP #4 | Flexiv/力接口未核 | 暂缓实体 |
| RDP #5 | RSS DOI确认，快策略FR5未核 | 暂缓实体 |

**本章来源**：S37-12,S37-13,S37-14,S37-17,S37-18；**决策**：D37-09,D37-12。

## 16｜唯一创新与备选：收敛而非堆模型

唯一条件性首选I-A仍为有限宽刮板边缘载荷的部分可辨识区间、经过独立分段载荷真值与RGB-D证据校准后的可信收紧及失败拒绝。R32净wrench非唯一、R33跨组覆盖漂移、R34串扰、R35激励/迟滞、R36时差伪迟滞、R37延迟证书失真构成逐层必要Gate。备选I-B为WORK→UNLOAD→OFFSURFACE_RETURN延迟鲁棒控制。新颖性反证包括VPEC/ViPER视觉压力、FEATS/UniForce光学触觉、2023–26同步/ROS调度；本轮不把时钟LP当独立博士创新。入门条件：独立标准器证书、足秩加载、跨材料/曲率/速度/时间盲检、真实D435非冗余增益；退出条件：同覆盖与拒绝率下没有净收益或参考证书失败。

**公式 F37-16**

$$
G_{I-A}=G_{ref}\land G_{clock}\land G_{latency\_truth}\land G_{vision}\land G_{fair}
$$

**表 T37-16**

| 科学问题 | 状态 | 退出判据 |
| --- | --- | --- |
| I-A有限宽部分可辨识 | 唯一条件首选 | 无独立真值/无同条件净收益 |
| I-B换向延迟鲁棒 | 唯一备选 | FR5厂家接口不可执行 |
| 泛化首次同步/视觉压力 | 新颖性主张淘汰 | 先行论文已覆盖 |

**本章来源**：S37-12,S37-13,S37-16,S37-17；**决策**：D37-09,D37-10。

## 17｜M1/M2/M3统一路径与硬件安全边界

M1保留人工有序3D选点与指定路线；M2保留背部实体画线识别、起终方向确认及有序3D曲线；M3保留解剖标志候选、专家审核可编辑3D关键点图、两点路线或经审核刮痧模板。三入口统一输出位置p、切向t、法向n、置信/协方差、工具姿态、连续有限宽扫掠资格及WORK/UNLOAD/OFFSURFACE_RETURN模式，接触前必须用户审核。工具真实接触中心/线和边缘偏载不等于法兰或标定TCP；腕部合力/力矩不能在未知接触偶矩和柔性多点情况下唯一反演压力。R37时钟Gate只读离线，没有ROS2实体节点、运动接口或任何接触许可。

**公式 F37-17**

$$
\gamma(s)=\{p(s),t(s),n(s),\Sigma(s),R_{tool}(s),q_{sweep}(s),mode(s)\}
$$

**表 T37-17**

| 入口 | 人工审核 | 统一模式 |
| --- | --- | --- |
| M1 | 人工路线和点序 | WORK/UNLOAD/OFFSURFACE_RETURN |
| M2 | 画线起终与方向确认 | 同一曲线契约 |
| M3 | 解剖/穴位候选与模板专家审定 | 同一曲线契约 |

**本章来源**：S37-01,S37-02,S37-11；**决策**：D37-10,D37-11。

## 18｜局限、证据阻塞、下轮自适应与退出路径

最大阻塞是完全没有真实独立单向延迟证书、D435曝光元数据、KWR75D硬件采样时间、参考标准器同步脉冲或FR5反馈日志。R37算法的LP证据在合成盒内有效，但错误证书反例明确否定“BLIND通过即可物理认证”。本轮仅访问ReDAG全文HTML，Zhang2024只读到方法/实验索引片段，ToN2023与2026 PTP论文只读摘要；未运行作者代码或FR5模型。下轮优先冻结采集链路版本与只读字段清单，设计可外部验证的双端脉冲/光电触发/示波器同步，分离曝光中点、读出、USB传输、ROS排队；若仍无设备，开展预注册证据可得性与强baseline合法复现审计，不虚增物理轮数。所有GitHub旧仓库不读不写、不新建仓库。

**公式 F37-18**

$$
G_{next}=G_{independent\_pulse}\land G_{exposure\_metadata}\land G_{load\_reference}\land G_{version\_freeze}
$$

**表 T37-18**

| 缺失证据 | 最小补证 | 失败处理 |
| --- | --- | --- |
| 真实单向延迟 | 双端硬件脉冲/示波器/证书 | 无证书拒绝窄时差 |
| D435真实曝光 | 固件+SDK+SENSOR_TIMESTAMP可用性 | 禁止用arrival代替 |
| FR5厂家FT_Control | SDK/固件/错误码/外环日志 | 不下发运动 |
| 独立分段力台 | 多单元LOAD/UNLOAD盲检 | I-A保持条件保留 |

**本章来源**：S37-06,S37-09,S37-10,S37-11；**决策**：D37-11,D37-12。

## 附录A｜完整去重来源审计与获取层级

**A**=本轮实际打开完整可访问HTML/标准并核读方法实验；**B**=本轮官方摘要、源码、README或检索正文片段；**H**=上一轮登记但本轮未重读原论文；**LOCAL-READ**=实际读取本地归档。访问失败必须如实标明。

|ID|来源/年份|本轮访问层级|实际阅读内容/限制|URL或本地位置|
|---|---|---|---|---|
|S37-01|AUTO-R36本地完整ZIP (2026)|LOCAL-READ|逐成员SHA/CRC、报告/讲义/决策/代码/测试实读|prior/AUTO_R36_REPRODUCIBLE_PACKAGE.zip|
|S37-02|DEV-R10本地工程ZIP (2026)|LOCAL-READ|clock_sync_gate.py与时钟契约实读，未运行DEV工程|/mnt/data/fr5_dev_release_DEV_R10/FR5_GUASHA_DEV_R10_COMPLETE.zip|
|S37-03|Mills et al., RFC5905 NTPv4 (2010)|A-完整官方标准|§8–11及四时间戳/误差距离；不是本项目时钟精度|https://www.rfc-editor.org/rfc/rfc5905.html|
|S37-04|Zhang, Li, Wang & Chen, Sensors 24(7):2369, DOI 10.3390/s24072369 (2024)|B-正文方法与实验索引片段|§4窗口/漂移/PI、§6实验与失效；PDF/原站全文429，未称全篇完整核读|https://www.mdpi.com/1424-8220/24/7/2369|
|S37-05|Clock Offset Estimation for Systems With Asymmetric Packet Delays, DOI 10.1109/TNET.2022.3229407 (2023)|B-IEEE摘要|状态空间抑制链路非对称；方法细节与复现未核|https://ieeexplore.ieee.org/document/10005157/|
|S37-06|Multi-Horizon Delay Asymmetry Prediction and Compensation for IEEE 1588 (2026)|B-PubMed摘要|铯钟参考PTP链路实测；非本项目同步数据|https://pubmed.ncbi.nlm.nih.gov/42515219/|
|S37-07|Hasan et al., ReDAG-RT, arXiv:2603.18238v1 (2026)|A-全文HTML|§III调度模型、§IV实验及结果、§V限制；未运行作者代码|https://arxiv.org/html/2603.18238|
|S37-08|ReDAG-RT作者公开仓库 (2026)|B-官方README与许可|Apache-2.0声明、Humble容器、合成工作负载；未固定commit/运行|https://github.com/Mehedi16009/ReDAG-RT|
|S37-09|ROS2 Humble message_filters 4.12.0官方教程 (2026访问)|B-官方文档|按header时间戳近似匹配；不是物理同时采样认证|https://docs.ros.org/en/humble/p/message_filters/doc/Tutorials/Approximate-Synchronizer-Python.html|
|S37-10|RealSense rs_frame.h timestamp domains/metadata (2026访问)|B-官方源码master|HARDWARE/SYSTEM/GLOBAL、曝光中点与读出起点区别；目标固件未核|https://github.com/realsenseai/librealsense/blob/master/include/librealsense2/h/rs_frame.h|
|S37-11|RealSense frame metadata官方说明 (2026访问)|B-官方文档|FRAME/SENSOR/ARRIVAL/BACKEND metadata与设备/内核可用性|https://dev.realsenseai.com/docs/frame-metadata/|
|S37-12|Grady et al. VPEC IROS2022 DOI 10.1109/IROS47612.2022.9982073 (2022)|B-作者仓库本轮打开|MIT代码、约170GB数据、Python3.10/PyTorch1.12.1；数据许可仍未证|https://github.com/Healthcare-Robotics/VPEC|
|S37-13|Collins et al. ViPER IROS2023 DOI 10.1109/IROS55552.2023.10342124 (2023)|B-作者仓库本轮打开|README有train/test/weak splits，仓库根未显示LICENSE；授权未证|https://github.com/Healthcare-Robotics/ViPER|
|S37-14|Xue et al. RDP RSS2025 DOI 10.15607/RSS.2025.XXI.052 (2025)|B-正式会议页|作者与DOI、慢快策略；动作/接口/权重未复现|https://www.roboticsproceedings.org/rss21/p052.html|
|S37-15|SciPy BSD-3-Clause license / HiGHS MIT (2026访问)|B-官方源码许可|本轮仅调用已安装SciPy linprog，未复制论文代码|https://github.com/scipy/scipy|
|S37-16|Bae et al. vision/force delayed estimator DOI 10.1186/s40648-016-0054-2 (2016)|H-R36历史全文|本轮未重新核原文；视觉延迟补偿非新颖主张|https://link.springer.com/article/10.1186/s40648-016-0054-2|
|S37-17|Helmut et al. FEATS IROS2025 DOI 10.1109/IROS60139.2025.11246486 (2025)|H-R36历史登记|MIT历史登记；GelSight+FEA≠D435/物理分段真值|https://github.com/feats-ai/feats|
|S37-18|Chen et al. ImplicitRDP arXiv:2512.10946 (2025)|H-R36历史登记|Flexiv慢快策略；FR5接口、权重许可与正式DOI未核|https://arxiv.org/abs/2512.10946|

## 附录B｜完整决策日志镜像

|ID|类型|本轮判断|
|---|---|---|
|D37-01|归档|R36 ZIP与内部SHA/CRC核验、实读报告讲义日志源码测试；DEV-R10源码只读核查，不把它算本轮复现。|
|D37-02|科学问题|一方向到达时间的未知传输延迟与真实时钟偏移混淆，优先检验此不可辨识边界。|
|D37-03|证明|offset+κ与latency−κ给出完全相同的到达时间；未知latency不能靠增加同类样本消除。|
|D37-04|算法|引入有独立上下界的单向延迟约束，冻结FIT二维LP可行域、BLIND仅检一致性。|
|D37-05|关联|同一时钟两帧的相对时间必须共享漂移变量，offset应代数抵消；独立盒可过度保守。|
|D37-06|关键失败|若独立延迟证书本身系统性错误，FIT/BLIND全部通过仍可能100%漏真值，拒绝把内部盲检当外部计量认证。|
|D37-07|时间语义|D435 SENSOR_TIMESTAMP曝光中点与FRAME_TIMESTAMP读出起点不同，ROS header近似匹配不等于真实同时接触。|
|D37-08|新颖性反证|2023/2024/2026已有PTP非对称补偿，2026已有ROS调度优化；不把时钟LP/调度当博士主创新。|
|D37-09|Baseline|VPEC>ViPER>FEATS>ImplicitRDP>RDP仅投入顺序不变，主论文仍待定；ViPER许可证证据仍不足。|
|D37-10|创新|I-A条件保留并增加G_latency_truth；I-B换向延迟鲁棒控制为唯一备选。|
|D37-11|安全|所有算法返回HOLD，零运动输出，E0–E5、ROS2实体运行、真人测试均未开始。|
|D37-12|下轮|先做真实只读独立延迟/曝光/硬件同步证据清单与时间域漂移盲测，不能从合成覆盖推物理安全。|
