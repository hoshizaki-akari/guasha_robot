# AUTO-R20｜FAIRINO FR5刮痧机器人博士级完整研究报告

**2026-10-20｜主题：厂家力控接口文档—源码参数契约审计、2026曲面擦拭竞争与十轮选型证据快照。**

> 证据声明：公开源码和手册的静态检查≠真实FR5运动或控制器响应；本轮未连接机器人、未运行原论文模型、未取得真实压力/力台数据。




## 01｜上一轮原件复核与真实差分

本轮以AUTO-R19的真实文件而不是聊天摘要为上一轮基准：逐一读取报告、讲义、候选登记表、决策日志、QA及ZIP；核对SHA256，抽取R19源代码和测试，并在本次环境重新执行`test_ft_integrity_guard.py`，103/103通过。注意这不是“全部历史轮次”的重新运行，R19报告里的更早轮数也不能累计到本轮。R19的新发现是`FT_Control`打包`B[0],B[0]`、F/T返回0不等于新鲜数据；本轮发现更广泛的**厂家手册示例—SDK签名参数顺序冲突**。对固定提交的公开源码逐段核读，和厂家3.9.5手册进行双向比较；4个例子中3个位置参数绑定不符，另一个例子与SDK一致但手册原型顺序不符。所有发现只在Python参数绑定层成立，不声称真实机器人已发生错误动作。R19登记的源码行号与本轮raw展示的行号不一致，因此本轮以commit、方法名和具体表达式定位，不将行号单独作为证据身份。

**F20-01｜本章数学/判据**

$$
G_{20}=G_{R19\,read}\land G_{103\,regression}\land G_{source}\land G_{49\,new}\land G_{decision};\quad G_{FR5}=0
$$

**T20-01｜本章证据/决策表**

| 复核对象 | R19可访问内容 | R20差分 |

|---|---|---|

| 报告/讲义/候选/决策/QA | 六份实体文件哈希 | 不是仅复述摘要 |

| R19测试 | 103项实际回归通过 | 新增四方法参数契约 |

| 实体设备 | 上一轮无 | 本轮仍无 |

**本章可审计索引**：F20-01、T20-01；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 02｜决策问题、可证伪假设与退出条件

本轮聚焦三个可决定研究路线的问题：Q20-A，能否从厂家示例复制调用而不改变真实的探索距离、力阈值和速度？Q20-B，在2026年已有真实曲面擦拭和多频率力控论文后，“视觉+六维力+柔顺控制”是否还具备独立新颖性？Q20-C，R10–R19积累的理论与代码是否足以让唯一主学术Baseline、边缘风险创新、厂家强Baseline进入实体资格？假设H-A：固定SDK签名与公开示例在至少两种力控相关运动方法中存在参数错位；可用Python静态绑定反证。H-B：最新论文覆盖普通擦拭/分层力控，必须把创新限制为有限宽刮板独立可证伪的边缘风险收益。H-C：没有本机固件、独立力台与多压头时，所有在线接触纠偏仍应拒绝。三个假设都有明确反证条件，且不会以触发次数替代验收。

**F20-02｜本章数学/判据**

$$
H_A:\ \exists f\in\mathcal F,\ \operatorname{Bind}_{SDK}(\operatorname{Example}(f))\ne\operatorname{Intent}(f)
$$

**T20-02｜本章证据/决策表**

| 问题 | 证据 | 失败/退出 |

|---|---|---|

| Q-A签名 | 固定源码+手册+AST | 版本不同需重核 |

| Q-B新颖性 | 三篇2026全文 | 普通融合不作创新 |

| Q-C实体资格 | 本机设备缺 | 保持HOLD |

**本章可审计索引**：F20-02、T20-02；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 03｜来源级别与15项去重证据

本轮实际核查15项去重来源：厂家固定源码1、厂家文档2、三篇2026年公开全文方法与实验3、作者项目/代码/数据及其他学术资料9。FP2、UMI-FT、UniForce均核读方法、实验设置、对照和限制；TacEva的出版社正文入口出现访问问题，虽然搜索索引含方法/实验片段，**仅标为公开正文片段，不计全文核读**。VPEC、ViPER、Jiang、Zuo的历史证据与本轮重核范围分开。来源层级记作C=官方源码、D=厂家文档、F=本轮可读公开全文、P=正文片段、R=作者README、H=历史核读、A=摘要/元数据。来源条目列于下表，URL、年份、证据限制写入JSON，避免“论文名存在”冒充已复现。

**F20-03｜本章数学/判据**

$$
\mathrm{Evidence}\in\{C,D,F,P,R,H,A\};\qquad F\not\Rightarrow\mathrm{FR5\;reproduction}
$$

**T20-03｜本章证据/决策表**

| 证据级别 | 本轮代表 | 不能外推 |

|---|---|---|

| C/D | 厂家源码/手册 | 本机固件 |

| F | FP2、UMI-FT、UniForce | FR5实测 |

| P | TacEva正文片段 | 全文核读 |

| R/H/A | 作者README/历史/摘要 | 模型复现 |

**本章可审计索引**：F20-03、T20-03；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 04｜FT_FindSurface参数顺序的关键反证

关键工程反证来自厂家3.9.5手册与固定提交`9541beaab0662dc8c267c0b9ba925aaf14858a77`的`Robot.py`。`FT_FindSurface`的真实Python签名依次是`rcs,dir,axis,disMax,ft,lin_v,lin_a,strategy`，但手册例子传`rcs,dir,axis,lin_v,lin_a,maxdis,ft_goal`。用手册变量`lin_v=3,lin_a=0,maxdis=50,ft_goal=2`做**静态**绑定，得到`disMax=3 mm,ft=0 N,lin_v=50 mm/s,lin_a=2 mm/s²`，与文字意图的50mm、2N、3mm/s、0mm/s²不同。这里“50mm/s”仅是错误参数绑定的数值，**不是机器人真实速度**。官方美国站和印度站手册均出现同样示例形式，不能把它们当成两套独立软件证据。`FT_FindSurface`属于主动表面探索，鉴于潜在力阈值错位，绝不可直接复制到人体或非人体接触程序。

**F20-04｜本章数学/判据**

$$
\operatorname{Bind}_{FindSurface}(3,0,50,2)=(d_{max}=3,\ f_{stop}=0,\ v=50,\ a=2)
$$

**T20-04｜本章证据/决策表**

| 字段 | 手册意图 | 静态绑定 |

|---|---|---|

| disMax mm | 50 | 3 |

| ft N | 2 | 0 |

| lin_v mm/s | 3 | 50 |

| lin_a mm/s² | 0 | 2 |

**本章可审计索引**：F20-04、T20-04；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 05｜FT_LinInsertion/SpiralSearch/RotInsertion逐项审计

第二个例子`FT_LinInsertion(rcs,force_goal,lin_v,lin_a,disMax,linorn)`与实际签名`(rcs,ft,disMax,linorn,lin_v,lin_a,strategy)`不一致。以手册定义`force_goal=20,lin_v=0,lin_a=0,disMax=100,linorn=1`代入，静态绑定得到`ft=20 N,disMax=0 mm,linorn=0,lin_v=100 mm/s,lin_a=1 mm/s²`；用户以为的100mm最大插入距离却成为速度参数。`FT_SpiralSearch(rcs,dr,fFinish,t,vmax)`对照签名`(rcs,ft,dr,max_t_ms,max_vel,strategy)`，会使力阈值由意图的1变为0.7，螺旋步进由0.7变为1。反之，`FT_RotInsertion`示例调用的参数顺序与实际SDK一致，但手册所印原型把`ft,orn`放在`angVelRot`之前，因此属于“手册原型错误”，而不是“示例错误”。R20没有推断控制器会如何处理异常参数；它证明的是公开Python入口的参数绑定不等价。

**F20-05｜本章数学/判据**

$$
\operatorname{Bind}_{Lin}(20,0,0,100,1)=(f=20,d_{max}=0,s=0,v=100,a=1)
$$

**T20-05｜本章证据/决策表**

| 方法 | 手册示例 | R20静态判断 |

|---|---|---|

| FT_LinInsertion | ft,lin_v,lin_a,disMax,linorn | 参数错位 |

| FT_SpiralSearch | dr,fFinish,t,vmax | ft与dr错位 |

| FT_RotInsertion | angVelRot,forceInsertion,angleMax,orn | 示例匹配SDK，原型错 |

**本章可审计索引**：F20-05、T20-05；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 06｜软件契约与R19阻尼问题的关系

R19发现的`FT_Control`阻尼打包缺口与R20发现的“示例错位”属于不同失效机制：前者是包装层从输入到远程载荷的映射`[M0,M1,B0,B0,...]`，即使调用者正确传`B1`也不能影响该载荷；后者是调用者依据错误示例，把本来正确的Python签名喂入错位位置参数。前者修复需要核验控制器协议及厂家版本，后者可先通过严格的静态关键字绑定和版本指纹阻断。两者都不能靠“RPC返回0”验收。独立测试不调用厂家SDK：仅解析手工摘录的固定公开签名与示例，通过`inspect.Signature.bind`及受限AST解析符号值，拒绝任意表达式和不明参数；所有测试均不发送运动指令。

**F20-06｜本章数学/判据**

$$
P_{FT\_Control}(B_0,B_1)=[M_0,M_1,B_0,B_0,\theta_0,\theta_1,a_0,a_1]
$$

**T20-06｜本章证据/决策表**

| 风险 | 发生层 | 必须补证 |

|---|---|---|

| B1丢失 | SDK远程参数打包 | 厂家协议/版本 |

| 位置参数错位 | 手册示例→SDK签名 | 本机签名/关键字审计 |

| API返回0 | 反馈层 | 独立参考/时间 |

**本章可审计索引**：F20-06、T20-06；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 07｜FP2 2026全文：曲面擦拭与分层力控

Fang等2026年9月29日公开FP2（arXiv:2609.37433）已经将上层动作生成与下层接触调节拆开。其RFM约2Hz生成动作，插值为50Hz参考，50Hz策略预测交互坐标系、力/位选择掩码与目标wrench，底层混合控制内部1kHz；真实平台是Flexiv Rizon4、FT-03S和两台D415 RGB-D，非FR5。论文对四项任务（包括曲面擦拭）做真实对照，并对同骨干模型加入FP2进行配对比较；其NFE只在成功执行样本上定义，**不能等同失败样本或刮板局部边缘风险**。项目网页在本轮显示代码Coming soon，故论文数学和实验可核读，权重/训练脚本的原样复现资格仍不成立。对本项目的直接反证是：上层路线规划+下层50Hz力觉调节及普通曲面擦拭，已不是独立创新。迁移还受FR5外环可用频率、真实延迟和底层1kHz权限限制，不能引用论文频率当FR5能力。

**F20-07｜本章数学/判据**

$$
f_{FP2}^{RFM}=2\,\mathrm{Hz},\ f_{FP2}^{policy}=50\,\mathrm{Hz},\ f_{FP2}^{inner}=1000\,\mathrm{Hz}\quad(\mathrm{not\ FR5})
$$

**T20-07｜本章证据/决策表**

| FP2维度 | 论文事实 | FR5迁移限制 |

|---|---|---|

| 硬件 | Flexiv Rizon4+FT-03S | 非FR5 |

| 控制频率 | 2/50/1000Hz | 不可直接移植 |

| 任务 | Wipe Curve等四项 | 非刮板边缘真值 |

| 代码 | 项目页Coming soon | 尚不能原样复现 |

**本章可审计索引**：F20-07、T20-07；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 08｜UMI-FT 2026全文：双指力觉顺应擦拭

Choi等2026 UMI-FT（arXiv:2601.09988）把双指六维F/T与RGB-D/位姿数据结合，使用顺应策略预测目标位姿、抓取力和刚度，在擦白板、刺穿蔬菜和灯泡插入上验证。作者报告机器人顺应环500Hz、夹爪30Hz；其公开仓库声明MIT并提供采集和训练入口，代码可获得性比FP2更好，但本轮没有下载运行。关键限制：双指夹爪内力可独立测量并不等于腕部一个六维F/T可反演刮板有限宽边缘分布；白板擦拭任务的成功率、力阈值及硬件带宽不可移植为FR5真人刮痧指标。该文强化了“先做厂家/简单顺应强Baseline”的必要性，也压缩了把‘顺应擦拭’作为博士核心创新的空间。

**F20-08｜本章数学/判据**

$$
v_G=K_p(x_G^*-x_G)+K_f(f_G^*-f_G)\quad(\mathrm{UMI\!\!\!\!\!-FT\ author\ control})
$$

**T20-08｜本章证据/决策表**

| UMI-FT维度 | 作者研究 | 本项目限制 |

|---|---|---|

| 传感器 | 双指CoinFT+RGB-D | 腕F/T缺分区 |

| 频率 | 机器人500Hz/夹爪30Hz | 不能搬到FR5 |

| 实验 | 白板擦拭/蔬菜/灯泡 | 非刮痧仿体 |

| 代码 | MIT作者仓库 | 本轮未运行 |

**本章可审计索引**：F20-08、T20-08；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 09｜UniForce 2026全文：准静态平衡假设

Chen等2026 UniForce（arXiv:2602.01153）在Franka FR3的双指相对夹持中，以准静态力平衡构造跨GelSight、TacTip、uSkin的配对训练，利用逆向力潜变量、前向重建、KL及两侧潜变量一致性损失；在跨传感器零样本力估计与擦拭任务上做对照。该方法的重要假设是两侧对置接触在准静态下满足相应力平衡，且几何与外部扰动受控。对单块柔性刮板，多点压力、纯偶矩、未知切向摩擦和非对称材料可破坏把“相同合力”当作“相同压力分布”的推论。作者HTML称代码与数据将公开，本轮没有取得可运行版本。它不是本FR5配置的主Baseline，但对‘只靠合力弱监督恢复边缘真值’构成明确反证。

**F20-09｜本章数学/判据**

$$
\mathcal L_{UniForce}=\mathcal L_{recon}+\lambda_{kl}\mathcal L_{KL}+\lambda_{eq}\mathcal L_{eq}
$$

**T20-09｜本章证据/决策表**

| UniForce维度 | 作者证据 | 失败迁移条件 |

|---|---|---|

| 训练 | 双指准静态力平衡 | 刮板多点+偶矩 |

| 数学 | 重建+KL+平衡损失 | 非独立压力真值 |

| 设备 | Franka FR3+三种触觉 | FR5无这些触觉 |

| 代码 | 论文称将公开 | 未取得可运行版本 |

**本章可审计索引**：F20-09、T20-09；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 10｜TacEva 2026：独立真值与测量评价

Cong等2026 TacEva（DOI 10.1002/aisy.202501179）提出视觉触觉传感器的本征、标准性能与鲁棒性三维评价，搜索索引公开的正文片段描述了外部六维F/T、已知接触位置、重复性和照明鲁棒性等测试。它提供一个实验设计参照：必须把真实传感器带宽、几何标定、接触力精度、空间鲁棒性、跨条件重复性分开测量。限制是其外部F/T可以提供合力真值，但对有限宽刮板的**局部压力分布**仍不足以单独给出独立真值；且本轮全文阅读链接失败，不将片段算作完整核读。结合FP2、UMI-FT、UniForce，当前2026竞争已覆盖普通力觉融合、分层控制、擦拭和力潜变量迁移，唯一可保留的科学问题必须更窄且能被真实边缘载荷证伪。

**F20-10｜本章数学/判据**

$$
\mathrm{TacEva}=(\mathrm{intrinsic},\mathrm{standard\ performance},\mathrm{robustness})
$$

**T20-10｜本章证据/决策表**

| TacEva维度 | 能提供 | 不能提供 |

|---|---|---|

| 外部六维F/T | 合力真值 | 逐元压力分布 |

| 接触位置 | 标定点位置 | 柔性线完整形状 |

| 鲁棒性 | 光照/空间/重复 | FR5安全资格 |

| 阅读级别 | 正文片段 | 未计全文 |

**本章可审计索引**：F20-10、T20-10；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 11｜主学术Baseline竞争矩阵与排序

主论文选型继续实行“同一证据门槛、不同任务适配”的竞争，而不是自动锁定Jiang。VPEC IROS2022有MIT源码、明确测试入口，但约170GB数据、外部RGB视角、默认验证配置隔离风险；ViPER IROS2023更接近EIH，公开训练/测试脚本和外链数据，但许可、权重及测试独立性仍需补证；Jiang 2024积分自适应导纳属于力控路线，缺本机实际外环/固件证据；UMI-FT 2026有MIT源码和擦拭顺应，但双指硬件不同；FP2 2026曲面擦拭实验强但作者代码尚未公开且需Flexiv内层控制；UniForce 2026的双指平衡假设与单刮板接触不等价；Zuo 2026适合遮挡定位方法学反证但工具几何与独立接触线真值缺。EgoPressure是可用数据预研资源，不是刮板主论文。所有排序和最小补证列于登记表。

**F20-11｜本章数学/判据**

$$
Q_{baseline}=Q_{license}\land Q_{data}\land Q_{model}\land Q_{interface}\land Q_{independent\ truth}
$$

**T20-11｜本章证据/决策表**

| 候选 | 当前定位 | 下一步 |

|---|---|---|

| VPEC | 条件优先离线 | 合法权重/划分 |

| ViPER | EIH直接竞争 | 许可/评估协议 |

| Jiang IAAC | 暂缓 | 本机外环 |

| UMI-FT | 有条件新对照 | 代码/硬件迁移 |

| FP2 | 方法强反证 | 等待代码 |

**本章可审计索引**：F20-11、T20-11；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 12｜唯一创新、备选、可观测性与退出

唯一条件性首选I-A仍为**有限宽刮板接触线/中心/偏载风险的部分可辨识估计、时间/几何可信上界与拒绝纠偏**；备选I-B为**工作—平滑卸载—离面返程的延迟鲁棒柔顺控制**。I-A要比wrench-only B1、匹配RGB-D+F/T B3、错配视觉B4在同放行覆盖率下比较边缘载荷上界违例率和定位误差；无独立三区/多压头真值或观测缺失必须HOLD。普通RGB-D+F/T姿态纠偏、摩擦ESO/EKF和复杂MPC不是独立创新；FP2与UMI-FT新增的曲面擦拭和分层力控反证使I-B的创新性门槛进一步提高。若I-A在预注册的真实留出条件下对B1/B4无显著独立收益，或视觉遮挡使其覆盖率不足，则退出而不包装为博士成果。

**F20-12｜本章数学/判据**

$$
\Delta_{I-A}=\operatorname{Risk}(B1\mid c)-\operatorname{Risk}(B3\mid c),\quad c=\mathrm{same\ coverage}
$$

**T20-12｜本章证据/决策表**

| 方向 | 新颖性 | 退出 |

|---|---|---|

| I-A唯一首选 | 有限宽风险可信拒绝 | 真实独立收益为零 |

| I-B唯一备选 | 卸载/离面返程 | 不胜简单基线 |

| 普通融合 | 已有竞争论文 | 不单列创新 |

**本章可审计索引**：F20-12、T20-12；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 13｜坐标链、工具真实接触线与误差

必须区分世界系W、机器人基座B、法兰F、标定工具TCP T、相机C和实际柔性接触线L。刚体链为`T_WT=T_WB*T_BF*T_FT`，但`T_WL`不是固定`T_WT`：柔性工具弯曲、滑移、刮板宽度和边缘受载会改变L。腕部wrench从F转到工具参考点需要旋转与力矩平移：`F_T=R_TF F_F`，`tau_T=R_TF(tau_F-r_FT×F_F)`（r方向定义须与所用坐标一致）。在未知接触偶矩和多点分布下，6D合力/合矩不唯一决定压力场。R20还新增了**软件参数语义误差**：即使空间标定、传感器都准确，调用接口的速度/阈值/距离也可能错位；故软件契约必须作为独立Gate，不能由几何校准替代。

**F20-13｜本章数学/判据**

$$
T_{WT}=T_{WB}T_{BF}T_{FT},\qquad T_{WL}\ne T_{WT}\ \mathrm{in\ general}
$$

**T20-13｜本章证据/决策表**

| 参考系 | 意义 | 主要误差 |

|---|---|---|

| F法兰 | 刚性接口 | 机械标定 |

| T TCP | 标定工具点 | 工具几何 |

| L真实接触线 | 受压形变 | 偏载/遮挡 |

| C相机 | RGB-D深度 | 时钟/手眼 |

**本章可审计索引**：F20-13、T20-13；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 14｜非人体独立真值、公平对照与消融

可证伪最小非人体实验按阶段执行：E0本机只读SDK/固件与安全链路版本指纹；E1离面轨迹及工具TCP/边缘、RGB-D/手眼、F/T偏置与重力补偿；E2机械隔离三区、双压头/三压头加载与独立总力台、时钟锚点；E3低能量单向恒力和独立离面返程；E4动态换向/有限宽风险对照；E5仅在独立伦理和风险审查后考虑真人。每组比较B0厂家简单力控、B1腕wrench-only、B2简单几何/恒力、B3可信融合、B4错配视觉、B5去掉拒绝/去掉同步/去掉几何条件的消融。相同工具、仿体、速度、轨迹、外部参考、随机区组与盲测；压力膜不得同时作为训练标签和唯一独立真值。退出Gate：真实多压头留出误差超预注册上界、延迟/缺帧不可界定、厂家参数映射未确认、急停与外部保护未验证，即停止接触研究。

**F20-14｜本章数学/判据**

$$
G_{contact}=G_{version}\land G_{FT}\land G_{sync}\land G_{geometry}\land G_{truth}\land G_{protection}
$$

**T20-14｜本章证据/决策表**

| Baseline | 输入 | 独立比较 |

|---|---|---|

| B0厂家简单 | 厂家F/T+状态机 | 独立力台 |

| B1仅wrench | 腕六维 | 边缘真值 |

| B3可信融合 | 匹配RGB-D+F/T | 相同覆盖率 |

| B4错配视觉 | 错配RGB-D+F/T | 排除伪融合 |

| B5消融 | 去同步/拒绝 | 证明必要性 |

**本章可审计索引**：F20-14、T20-14；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 15｜FR5厂家工程强Baseline与安全Gate

厂家`FT_Control`保留工程强Baseline候选，但不能因其是官方接口而默认可用。R19的B1阻尼打包缺口加上R20三处官方示例参数错位，意味着**必须从真实本机版本读取函数签名、建立关键字调用与参数回显/厂商确认**；不能把手册示例复制进运动程序。只读审计需记录Python SDK commit、固件、控制器型号、FT设备型号、原始/补偿六维力、时间戳来源、丢帧、急停/保护输入和外环实测周期。任何示例中出现的速度/力值都不作为本机允许阈值。当前运行环境无法访问本机FR5；本轮只运行纯Python离线签名绑定程序，真实状态、运动命令和力台数据均为0。厂商源码和手册之间冲突是否影响特定控制器仍待厂家确认。

**F20-15｜本章数学/判据**

$$
\mathrm{SDK\_return}=0\ \not\Rightarrow\ \mathrm{FT\_fresh}\land\mathrm{force\_safe}
$$

**T20-15｜本章证据/决策表**

| Gate | 实际状态 | 最小补证 |

|---|---|---|

| SDK/固件 | 未核本机 | 只读版本指纹 |

| F/T | 未校准本机 | 已知外载 |

| 安全链路 | 未验 | 独立保护 |

| FT_Control | 未调用 | 厂家参数语义确认 |

**本章可审计索引**：F20-15、T20-15；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 16｜M1/M2/M3三入口统一3D曲线契约

系统保留三入口：M1人工有序点与路线；M2人体背部实体画线识别、起终方向确认、曲面回投影与有序三维曲线；M3解剖标志自动候选、专家审核、可信可编辑关键点图及曲面图搜索或审核模板。统一输出`{p_i,t_i,n_i,Σ_i,q_tool,i,width_sweep_ok_i,mode_i,provenance_i}`，其中`mode∈{WORK,UNLOAD,OFFSURFACE_RETURN}`，每段须有接触前人工审核。M2不能从2D涂线直接假定深度和法向，M3穴位不是未经审核的自动临床结论。工具扫掠资格要求宽度内所有点有有效几何/遮挡置信度；不合格则暂停并离面。上层使用成熟检测、标注、图搜索基线，绝不把M1/M2/M3功能堆砌冒充博士唯一算法创新。

**F20-16｜本章数学/判据**

$$
\Gamma=\{(p_i,t_i,n_i,\Sigma_i,q_i,w_i,m_i)\}_{i=1}^{N}
$$

**T20-16｜本章证据/决策表**

| 入口 | 产生方式 | 必须审核 |

|---|---|---|

| M1 | 人工有序点 | 路径方向 |

| M2 | 实体画线RGB-D回投 | 起终/遮挡 |

| M3 | 解剖候选+图搜索 | 专家穴位/模板 |

| 统一曲线 | 位置切法线+模式 | 有限宽扫掠 |

**本章可审计索引**：F20-16、T20-16；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 17｜AUTO-R20十轮证据快照与独立开题交接

AUTO-R20是编号上的十轮里程碑，应向**独立开题与选型审查任务**提交证据快照，而不代替该任务或擅自声称六份开题文档已完成V1/V2/V3。R10–R19逐步建立空间不可辨识、时间证书、三区/多压头校准、源码公平复现及FR5参数疑点；R20新增厂家多个方法的调用契约反证。十轮累计不等于十次FR5实验：迄今可核验的本轮FR5物理实验仍是0，主论文仍未锁定，创新仍有条件，工程强Baseline没有接触执行资格。开题评审应围绕证据等级、独立真值是否可采购、实际FR5反馈率、主论文数据/权重许可证与可执行性，形成保留/淘汰结论。六份文档仅在真实实证支持后分版修订，不能把离线合成结果写成实体性能。

**F20-17｜本章数学/判据**

$$
\mathrm{AUTO\!\!\!-R20}\not\Rightarrow\mathrm{E4/E5\ physical\ evidence}
$$

**T20-17｜本章证据/决策表**

| 审查维度 | R20证据 | 未达标 |

|---|---|---|

| 算法/数学 | 有不可辨识反证 | 真实边缘真值 |

| 厂家接口 | 四方法静态审计 | 本机/固件 |

| 主论文 | 候选竞争矩阵 | 模型/数据复现 |

| 开题文件 | 仅交接证据 | 独立六文档V1–V3 |

**本章可审计索引**：F20-17、T20-17；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 18｜本轮真实工作量、验收和下一轮适应

本轮实际完成：读取并SHA256核查R19六项文件；从ZIP抽取源代码与测试；本机重新执行R19的103项测试全部通过；新增49项静态签名/AST/错误示例/安全拒绝测试全部通过；对四个官方力控相关方法做参数绑定审计，确认三处示例错位与一处文档原型错位；核查15项去重来源，其中FP2、UMI-FT、UniForce三篇公开全文方法/实验核读，TacEva仅正文片段；产生可审计JSON结果。没有下载原论文模型/原始EgoPressure数据，没有运行机器人，没有力台或人体数据。下轮第一优先不是增加合成模型，而是取得本机只读SDK签名与版本、厂商关于四方法示例/`B[1]`的书面确认、独立已知力加载日志；若无法获取，继续记录具体阻塞，不虚增实体Gate。文档覆盖、章节均衡、ZIP及哈希另由自动检查写入QA。

**F20-18｜本章数学/判据**

$$
N_{new}=49,\quad N_{rerun}=103,\quad N_{FR5}=0,\quad N_{fulltext}=3
$$

**T20-18｜本章证据/决策表**

| 本轮计数 | 实际结果 | 边界 |

|---|---|---|

| 新增测试 | 49/49 | 离线 |

| R19回归 | 103/103 | 本轮重跑 |

| 全文核读 | 3篇 | 非FR5 |

| 来源审查 | 15项 | 去重 |

| 实体实验 | 0 | E0–E5未过 |

**本章可审计索引**：F20-18、T20-18；对应的来源与决策请核对第03章来源总表及`DECISION_RECORD.md`，不得以本章推导代替实体证据。


## 附录A｜全部15项来源逐条对照（来源级别不等于实测）

|ID|作者/组织、题名、年份|获取层级|真实入口/限制|

|---|---|---|---|

|S20-01|FAIRINO，Python SDK Robot.py 固定提交（2026）|official code|https://raw.githubusercontent.com/FAIR-INNOVATION/fairino-python-sdk/9541beaab0662dc8c267c0b9ba925aaf14858a77/linux/fairino/Robot.py；已核读FT_Control/FindSurface/LinInsertion/SpiralSearch/RotInsertion五段源码；未取得全文件本地副本；commit 9541bea...；无机器人连接|

|S20-02|FAIRINO，Python Force Control manual 3.9.5（version 3.9.5）|official docs|https://documentation.fairino.co.in/SDKManual/PythonRobotForceControl.html；已核对12.21–12.31四个示例与函数原型；不能代表本机固件|

|S20-03|FAIRINO USA，Python manual 12.27–12.30（undated）|manufacturer docs|https://www.fairino.us/cobots-manual/python-12.27.-examples-of-instruction-code-for-spiral-exploration%2C-straight-line-insertion%2C-etc；交叉核对FT_FindSurface相同示例顺序；不是独立SDK实现|

|S20-04|Hongjie Fang et al.，FP2: Equipping Robotic Foundation Models with Force Control（2026-09-29）|full HTML methods/experiments|https://arxiv.org/html/2609.37433；核读III–IV，Flexiv Rizon4, 2/50/1000Hz层级；论文未证FR5适配|

|S20-05|FP2 authors，FP2 project site（2026）|author project|https://force-policy.github.io/fp2/；作者页面Code Coming soon；没有下载或运行权重|

|S20-06|Hojung Choi et al.，In-the-Wild Compliant Manipulation with UMI-FT（2026）|full HTML methods/experiments|https://arxiv.org/html/2601.09988；核读IV–V擦拭/顺应、500Hz机器人/30Hz夹爪；阈值不迁移|

|S20-07|real-stanford，UMI-FT official code（2026）|author code README|https://github.com/real-stanford/UMI-FT；MIT许可证与安装说明可见；本轮未下载运行|

|S20-08|Zhuo Chen et al.，UniForce: A Unified Latent Force Model for Robot Manipulation with Diverse Tactile Sensors（2026）|full HTML methods/experiments|https://arxiv.org/html/2602.01153；核读III–V；静态双指力平衡不等于刮板分布真值|

|S20-09|Qingzheng Cong et al.，TacEva: A Performance Evaluation Framework for Vision-Based Tactile Sensors（2026-02-11）|full text excerpt/search-index sections|https://advanced.onlinelibrary.wiley.com/doi/10.1002/aisy.202501179；DOI 10.1002/aisy.202501179；公开页面正文片段与摘要；全文链接访问报错，不计完整全文核读|

|S20-10|Healthcare-Robotics，VPEC official repository / IROS2022（2022）|author code README|https://github.com/Healthcare-Robotics/VPEC；MIT、约170GB、Python3.10/PyTorch1.12；未运行模型|

|S20-11|Healthcare-Robotics，ViPER official repository / IROS2023（2023）|author code README|https://github.com/Healthcare-Robotics/ViPER；EIH RGB，依赖/数据外链；许可未确认；未运行模型|

|S20-12|ETH SIPLAB，EgoPressure dataset toolkit / CVPR2025（2025）|author code/docs|https://github.com/eth-siplab/EgoPressure；工具MIT、数据单独许可；原始Parquet仍未取得|

|S20-13|Kaiwen Zuo et al.，A Model-based Visual Contact Localization and Force Sensing System for Compliant Robotic Grippers（2026-05-01）|full HTML prior recheck|https://arxiv.org/html/2605.00307；本轮只重核摘要/章节结构；R19曾全文核读|

|S20-14|Jinlei Jiang et al.，Force Tracking Control Method for Robotic Ultrasound Scanning System under Soft Uncertain Environment（2024）|metadata/prior abstract|https://doi.org/10.3390/act13020062；DOI 10.3390/act13020062；本轮未取得全文|

|S20-15|FAIRINO，FR collaborative robot ROS2 repository（2026）|official code README|https://github.com/FAIR-INNOVATION/frcobot_ros2；可见仓库；本机ROS2版本、反馈率未核|


## 附录B｜10项决策编号逐条对照

|决策|当前动作|证据缺口/退出条件|

|---|---|---|

|D20-01|R19文件SHA与103项回归通过|没有重跑全部更早轮|

|D20-02|FT_FindSurface手册示例判为位置参数错位|本机SDK不同需重新审计|

|D20-03|FT_LinInsertion示例判为位置参数错位|不做真实插入|

|D20-04|FT_SpiralSearch示例ft/dr错位|不做真实搜索|

|D20-05|FT_RotInsertion示例与SDK一致，手册原型错位|禁止凭印刷原型改写调用|

|D20-06|FT_Control B1丢失保持R19强疑点|厂家协议/新版本未核|

|D20-07|FP2与UMI-FT已覆盖普通擦拭/分层顺应|I-B新颖性门槛提高|

|D20-08|UniForce静态双指平衡不可当边缘真值|I-A需独立多压头|

|D20-09|主学术Baseline待定，I-A唯一条件首选，I-B唯一备选|同覆盖率真实收益缺失|

|D20-10|AUTO-R20证据交接独立十轮开题任务，实体Gate不升级|六份独立开题文档尚未由本轮修订|


## 附录C｜可复现执行入口与实体证据声明

```bash

python src/run_r20_study.py

python -m pytest -q tests/test_sdk_contract_audit.py

cd prior_R19 && python -m pytest -q tests/test_ft_integrity_guard.py

```

`src/sdk_contract_audit.py`只做受限AST参数绑定，`results/R20_CONTRACT_AUDIT.json`是静态结果，不是机器人日志；`results/R19_REGRESSION.txt`是本轮实际回归输出。无本机版本/独立真值/安全验证时禁止任何接触执行。