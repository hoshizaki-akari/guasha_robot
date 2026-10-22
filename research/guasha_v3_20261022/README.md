# Guasha Offline Research & Agent Implementation Package V3.0

**用途：**研究资料保全 + Vibecoding Agent完整离线算法开发规格。**当前交付的是开发规格和研究归档，不是Agent已完成的软件，也不是实机验证结果。**

先看 [00_START_HERE.md](00_START_HERE.md) → [AGENTS.md](AGENTS.md) → [完整主规格](docs/01_MASTER_SPEC.md) → [算法数学](docs/02_ALGORITHMS_MATH.md) → [机器验收](specs/requirements.json)。

**本次用户纠正：M2主线为电脑屏幕画线，而非实体墨迹颜色识别。** 主规格优先，旧研究保留原样。

**完整实现目标：**M1/M2/M3三入口 → 统一RGB-D几何及3D路径 → 有限宽刮板扫掠 → 离线数值接触与六轴力觉 → C0/C1/C2/C3控制 → 卸载离面返程状态机 → 统计/图和拒绝结果。每个模块同等重要，版本冻结后全量验收。

**研究范围：**旧R01–R23完整本地报告与讲义、AUTO-R01原始包、阶段审计及关联总结。AUTO-R02–R51和DEV-R01–R24原始包未取得，已在`manifests/coverage.csv`逐轮标为`MISSING_SOURCE`；不可夸称全部上传。

**GitHub：**本地材料已整理为可推送的独立研究库目录；本项目归档位置为 GitHub `hoshizaki-akari/guasha_robot/research/guasha_v3_20261022/`，具体已归档条目以 GitHub 实际目录为准。

**防止Agent遗漏：**68项机器需求、18个端到端场景、专门的数据Schema、每轮研究映射以及“需求→测试→证据”清单，帮助大模型跨上下文逐项核对。