# AUTO-R55｜博士级完整研究报告：R27路径点间覆盖反例与I-A证据暂停

**内容验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY。** 只读研究，不连接FR5，不输出机器人运动；无人体、非人体实体实验或物理仿真。

本轮唯一新增工程问题：DEV-R27局部7×7拟合与段长限制之间存在未检查的中间网格区域；唯一科学路线I-A因连续两轮无独立三区真值转为PAUSE_FOR_EVIDENCE。

## 01. 归档真实性与相对R54的新旧差分

R54完整研究ZIP实际存在，外部SHA256与清单一致，35个成员CRC通过、34个内部SHA通过。已读取上一轮研究报告、讲义、候选登记、决策日志、源码、失败清单和测试；并重新运行其新增26项测试（含9个subtests）。另核验DEV-R27完整ZIP SHA及实际抽取`curved_m2.py`，定向运行34项测试。R54研究方向曲率本身；R55新增的是实际源码级的“相邻WORK点之间未被检查的窄条”，没有重写DEV工程，也没有伪称取得实机。

**公式 F55-01：** `E55=verified(R54)+audit(DEV-R27)+new_counterexample`

**表 T55-01：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | R54 ZIP SHA/CRC/34内部SHA通过；R27源码实读；历史1022项未全量重跑 |

**来源：** S55-10,S55-11。**决策：** D55-01。

## 02. 唯一科学问题与证据暂停条件

长期唯一首选I-A仍为有限宽柔性刮板局部载荷部分可辨识及可信拒绝，备选I-B为卸载—换向—离面返程延迟鲁棒柔顺。R54明确“再两轮证据审计仍无独立三区原始真值/设备计划则暂停”。本轮新审ToucHD、商业压力阵列等仍未得到一条合法、可计量、可独立三区盲检的非人体原始episode，故把I-A从有条件活跃降为`PAUSE_FOR_EVIDENCE`；这是暂停新增模型复杂度，不是宣布该科学问题被证伪。I-B亦因FR5反馈时序缺失HOLD，不强行选赢家。

**公式 F55-02：** `I_A_active = independent_tri_zone_truth AND clock_calibration AND legal_use`

**表 T55-02：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | I-A：PAUSE_FOR_EVIDENCE；I-B：HOLD_FOR_FR5_TIMING |

**来源：** S55-04,S55-06,S55-07。**决策：** D55-02。

## 03. 十项去重来源、许可与访问层级

本轮筛查10个新外部来源：RSS2013 TrajOpt全文、FCL官方README/LICENSE、MoveIt官方概念、ToucHD-Force数据卡、VisTouch公开元数据、Tekscan官方压力系统、XSENSOR官方传感器、FAIRINO力控手册、RealSense D435规格、AnyTouch2 arXiv摘要。深入比较RSS2013、FCL、ToucHD，另审FR5手册与压力仪器。RSS2013全文方法V节、基准VI节、实体VIII节及局限X节实际核读；AnyTouch2仅摘要/数据卡，未声称读其全文。未运行或复制外部作者代码，FCL LICENSE确认为BSD样式但未锁定可执行版本，故只作为候选。

**公式 F55-03：** `usable_source = accessible AND versioned AND licensed AND fit_for_claim`

**表 T55-03：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | RSS2013全文A；FCL官方B；ToucHD卡B但数据门控；AnyTouch2摘要C |

**来源：** S55-01,S55-02,S55-04,S55-12。**决策：** D55-03。

## 04. 论文全文核读：TrajOpt连续扫掠为何重要

Schulman等RSS2013 DOI 10.15607/RSS.2013.IX.031的第V节指出离散路径点无碰撞不保证插值段无碰撞；纯平移刚体的连续扫掠体可用端点形状的凸包表达，第V节式(17)–(18)。含旋转时凸包会低估扫掠，论文讨论角度修正并说明其具体实验忽略了小于1cm的修正；这个工程近似不能搬到FR5人体接触。第VI节报告198个7DOF臂与96个18DOF全身规划问题，对比OMPL/CHOMP；第VIII节PR2实物任务，但不是刮痧或接触压力。第X节作者公开BSD TrajOpt及OpenRAVE/Bullet依赖；本轮没有运行原论文代码。

**公式 F55-04：** `Sweep(A)=union_{t in [0,1]} A(t); for pure translation: Sweep(A)=conv(A0,A1)`

**表 T55-04：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | RSS2013 198臂规划+96全身规划；PR2实物任务；非FR5 |

**来源：** S55-01。**决策：** D55-04。

## 05. R27实际源码接口审计与边界

从DEV-R27 ZIP实际读取`src/fr5guasha/curved_m2.py` SHA256=6ba838574e67cbfb796e36f750f3175a30431f5e9c59c67a89e102572d720cb1。`audit_curved_m2`仅对每个`ordered_pixels`的7×7邻域拟合局部二次曲面、计算横向sag；相邻WORK点间只检查三维距离≤0.015m，没有枚举整段有限宽走廊中的所有深度格点。其状态明示`PASS_SYNTHETIC_CURVED_GEOMETRY_ONLY`、`HOLD_NO_MOTION`，所以这是未来连续扫掠资格的覆盖缺口，不是“R27已宣称物理安全却错误”。R22仿射凸ROI模块另有连续胶囊条件，但其证明不适用于未知曲面。

**公式 F55-05：** `checked_region_R27=union_i Patch7x7(p_i) != swept_corridor(path,width)`

**表 T55-05：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | R27 7×7点局部拟合；段长≤15mm；无全走廊覆盖 |

**来源：** S55-11。**决策：** D55-05。

## 06. 独立反例：两端通过但中间存在窄障碍

构造81×81人工平面XYZ网格，沿路径x像素步长1mm、y步长5mm，六个WORK路标分别在u=15,25,35,45,55,65且v=40，宽度20mm；在u=20/30/40/50/60中点放置2mm凸起或NaN洞，横向可位于中心或边缘。所有路标局部7×7不包含这些中点，路标间距10mm<15mm；实测调用原R27函数，对干净、中心凸起、边缘凸起、中心洞、边缘洞都返回`PASS_SYNTHETIC_CURVED_GEOMETRY_ONLY`。独立人工故障清单就是Oracle，不来自新Gate输出。

**公式 F55-06：** `gap_pixels=10-2*3=4; injected_height=0.002m > demo_limit=0.0005m`

**表 T55-06：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | 原R27：干净与四类中段故障均局部PASS；全部仍HOLD_NO_MOTION |

**来源：** S55-11。**决策：** D55-06。

## 07. 最小算法：整条有限宽网格走廊审计

新`audit_grid_corridor`只针对合成规则平面：把相邻WORK路标连成XY线段，对网格顶点计算点到线段的最小距离，落在宽度w/2内即为走廊证据格点；任何NaN或偏离已知平面高度超过人工阈值都拒绝。它检查全部有限宽格点而非仅路标或中心线；仍不能证明格点之间的真实连续表面，更不能证明真实工具边缘、柔性变形、动态碰撞或力安全。输入含SHA、SI米、非负int64 ns、坐标/标定ID，永远HOLD_NO_MOTION。

**公式 F55-07：** `d(p,segment)=min_{t in [0,1]} ||p-(a+t(b-a))||; scan if d<=w/2`

**表 T55-07：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | 新Gate：有限宽网格顶点覆盖；不是连续实体安全证书 |

**来源：** S55-01,S55-11。**决策：** D55-07。

## 08. 公平消融、独立Oracle与失败拒绝

固定256组相同合成网格：128干净、128注入故障（中心凸起、边缘凸起、中心洞、边缘洞各32）。原R27路标局部Gate、同网格密集中心线强Baseline、R55整宽走廊Gate使用完全相同的输入、阈值与20mm宽度。独立Oracle由事先注入的故障manifest定义，不读取任何预测结果。结果：三法干净正确接收均128/128；原R27错误接收128/128故障，密集中心线错误接收64/128，R55整宽错误接收0/128；这些是**特制合成反例的检测率**，绝非真实D435安全性能。新算法没有提高对未知子像素凸起的保证。

**公式 F55-08：** `false_accept = count(corrupted AND method_accepts)`

**表 T55-08：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | 128坏例：R27误接128；密集中心线误接64；整宽误接0 |

**来源：** S55-11。**决策：** D55-08。

## 09. 为什么全格点扫描仍不等于连续资格

对任意等间隔x_i，可构造h(x)=A sin(pi x/dx)，在所有格点处为0、格点中间可达任意幅值A，故无独立连续变化上界时不存在硬认证。若独立验证残差函数的二维Lipschitz常数L，网格最大步长dx,dy，则每个网格单元内任意点到某顶点距离≤0.5 sqrt(dx²+dy²)，充分条件为max_grid_abs_residual + L sqrt(dx²+dy²)/2 ≤ limit。新代码默认L未验证就HOLD；即使传入验证标志也只输出`CONDITIONAL_ANALYTIC_BOUND_ONLY`，不会授予动作。

**公式 F55-09：** `max_cell_residual <= max_vertex_residual + L*sqrt(dx^2+dy^2)/2`

**表 T55-09：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | 未独立核验L：HOLD；核验L：仅条件数学包络 |

**来源：** S55-01,S55-09。**决策：** D55-09。

## 10. 2026真实数据与压力仪器：物理真值仍未获得

本轮新增核查ToucHD-Force官方数据卡：2026公开说明722436对触觉—腕部3D总力样本、5种触觉传感器、71种压头及四方向滑动；数据卡标MIT但要求登录并同意共享账户信息，366GB文件未下载。其标签为腕部总力序列，不是三区独立压力。VisTouch有非人体滑动多模态元数据但无独立三区压力。Tekscan I-Scan和XSENSOR官方说明存在空间压力映射商业设备，但本轮未采购、未取得设备型号量程/单格面积/标定/时钟和授权数据。不能把可购买仪器或作者整体F/T冒充已经有真值。

**公式 F55-10：** `F_total = F_L+F_M+F_R, but F_total alone does not determine each F_i`

**表 T55-10：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | ToucHD腕部3D总力≠三区真值；商业压力阵列=潜在设备非现有数据 |

**来源：** S55-04,S55-05,S55-06,S55-07。**决策：** D55-10。

## 11. FR5、ROS2和三入口对DEV的具体移交

FAIRINO官方Python力控文档仍列`FT_SetConfig(company,device,softversion,bus)`的坤维17、device0为KWR75B；不证明KWR75D与规划[1,17,3,0]兼容。D435官方RGB滚动快门、深度立体视觉及相机范围仅作接口规划，不能用官方标称深度精度替代真实曲面误差界。建议DEV-R28在M1/M2/M3统一3D路径的每个WORK段输出`coverage_kind={WAYPOINT_PATCH,GRID_CORRIDOR,CONTINUOUS_CERTIFIED}`、`missing_grid_cells`、`source_sha256`、`frame_id`、`stamp_ns`、`tool_width_m`及`reason`，UI把“网格已扫描”和“连续真实安全”分开。M2方向必须人工确认、M3必须专家审核；工作、卸载、离面返程只读。

**公式 F55-11：** `trajectory_evidence = (route, swept_grid_coverage, frame, stamp_ns, uncertainty, HOLD)`

**表 T55-11：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | DEV建议新增网格覆盖字段；M1/M2/M3及状态机保留；ROS2只读 |

**来源：** S55-03,S55-08,S55-09。**决策：** D55-11。

## 12. 验收、可证伪下轮实验与博士路线收敛

本轮执行R55新增30项、R54新增26项（另9个subtests）、DEV-R27曲面模块定向34项，共90项pytest主测试通过；未重跑历史1022项或DEV完整1798项。256组公平消融可重建，R54/R27原始ZIP按SHA保留在研究包，提供独立解压复测、JUnit、SHA256。下一轮DEV应在人工曲面网格显示整条带状区域与拒绝原因，先做M2并保持三入口数据模型；研究侧须优先明确三区力台/压力阵列型号、校准协议、数据授权与同步方案，或得到合法原始非人体三区数据。I-A暂停扩模，若没有物理真值则主论文仍待定，I-B不因暂停自动胜出。

**公式 F55-12：** `N_retested = 30(R55)+26(R54)+34(DEV_R27)=90`

**表 T55-12：** | 本章证据 | 本章结论 |
|---|---|
| 实际审查或测试 | 90项实际执行；256组合成；无实机、无物理仿真、无人体 |

**来源：** S55-10,S55-11。**决策：** D55-12。

## 附录｜实际工作量、失败边界与复现命令

实际核读RSS2013全文方法/实验，核查10项新外部来源；深入对照RSS2013、FCL和ToucHD；只读审计R27原版源码与R54归档；256组合成同数据公平对照；R55新增30、R54新增26、R27定向34项测试。所有原始作者数据和商业传感器均未被下载、采购或使用。

```bash
python -m pytest -q test_r55.py
python run_r55_ablation.py --dev-zip previous/FR5_GUASHA_DEV_R27_COMPLETE.zip --out R55_ABLATION.json
```

详细来源见`SOURCE_REGISTER.md`，完整候选论文和许可缺口见`BASELINE_AND_INNOVATION_REGISTER.md`，决策见`DECISION_RECORD.md`，失败/阻塞见`R55_FAILURES_AND_BLOCKERS.md`。