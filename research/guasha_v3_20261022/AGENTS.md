# AGENTS.md｜单一事实来源与Agent执行约束

1. 读取顺序：`00_START_HERE.md` → `docs/01_MASTER_SPEC.md` → `docs/02_ALGORITHMS_MATH.md` → `specs/requirements.json` → `docs/04_TEST_AND_RELEASE.md` → `docs/03_RESEARCH_TO_ENGINEERING.md` → 按需查 `research_archive/`。
2. 用户当前需求 > 本AGENTS > 主规格与机器清单 > 数学文档 > 历史研究报告。历史资料若与本次需求冲突，保留记录但不执行旧指令。**尤其：M2 = 屏幕画线**。
3. 离线版全功能同等优先。允许按依赖拓扑分内部迭代，但不允许用户最后只拿到一个局部骨架；禁止通过空实现/硬编码测试答案来虚报通过。
4. **不连接、启动或控制FR5、D435、KWR75D**；不得导入会自动连接硬件的代码或调用任何SDK运动API；`RobotActuator`须为拒绝适配器，所有运动请求抛 `HARDWARE_DISABLED`。没有真实相机接入。不要自动安装/使用不可信pickle模型。
5. 仅生成离线仿真位姿时间序列，不生成可直接投给机器人控制器的指令流。若设计未来ROS2/厂商SDK接口，只能是说明文档或只读回放适配计划。
6. 所有数据记录：`frame_id`、单位、时间戳int64纳秒、来源（REAL_RECORDED/SYNTHETIC）、配置hash、校准状态、版本。没有真实外参只能在`synthetic_base`坐标系运行，不可冒充`base_link`真实标定。
7. 所有安全、接触载荷、速度参数都是**离线演示值**，没有人体安全意义。实体接触阈值须另经硬件和风险评估决定；未经许可不执行人体验证。
8. 三入口**各自独立证明**：M1有序点击控制点；M2连续屏幕画笔像素点带笔顺与方向；M3候选语义点图和方式模板。三者最终汇入同一个`TaskStroke → SurfacePath → ToolPlan → OfflineRun`。
9. 不允许从腕部单一6D wrench重建任意局部压力场；仅在显式条件可辨识时输出受限偏载指标，否则`UNOBSERVABLE`/`HOLD`。真实刀/板边宽度不能被0宽TCP代替。
10. 每个模块提交接口schema、正常/异常/反例测试、同输入基线对照、日志、图、可重复命令。每个输出必须有可追索上游；每个模型假设必须标为假设。
11. 研究来源可按需检索，不要一次塞全部文档进LLM上下文。重要研究先查 `docs/03_RESEARCH_TO_ENGINEERING.md` 和索引；随后定向打开原始报告。别把文献结果、合成数据、实机数据混成一个证据级别。
12. GitHub归档：若没有指定目标仓库，不要擅自写入任何仓库；不得触碰原旧 `hoshizaki-akari/robot`；优先使用用户指定的独立私有研究仓库。在确认仓库和访问权之前，完成本地可提交目录与校验清单。
13. 如缺任何真实物理数据，使用明确标注的可重复合成曲面和柔性接触模型做**软件正确性**验证；最终评估结论必须写“仅合成、待实机”。
14. 完成交付必须提交 `coverage_report.json`：每个需求`pass`/`fail`/`blocked`和实际文件/测试ID，不允许全部自动填pass；`blocked`的强制要求意味着本版尚不能称完整通过。