# GitHub研究归档状态与缺失边界

记录日期：2026-10-22；目标GitHub：`hoshizaki-akari/guasha_robot`，分支`main`，目录`research/guasha_v3_20261022/`。

## 已实际上传和检查

- 顶层导航`00_START_HERE.md`、`AGENTS.md`、`README.md`。
- `docs/01_MASTER_SPEC.md`：统一离线17模块开发方案。
- `docs/02_ALGORITHMS_MATH.md`：算法数学推导。
- `docs/03_RESEARCH_TO_ENGINEERING.md`：研究到工程转译。
- `docs/05_GITHUB_ARCHIVE.md`与`docs/06_AGENT_HANDOFF_PROMPT.md`：背景与指令；注意05原稿中“尚未指定仓库”的历史句子已过时，以本状态文件为准。
- `specs/requirements.json`：68条MUST要求。
- `research_archive/legacy/R01–R23/`：23份原始报告 + 23份原始讲义，全部是可读Markdown。
- `research_archive/auto/`：AUTO-R01报告及讲义。
- `research_archive/audits/`：两份阶段审计。
- `research_archive/context/`：两份早期架构文档。
- [逐轮可点击索引](research_archive/INDEX.md)。
- `research/README.md`和`research/AGENTS.md`作为仓库入口，原有`research/0`完全未修改。

## 未上传和原因

**不能宣称已完整上传107文件或全部98轮原始资料。** V3.0本地包107个文件，当前GitHub已上传其61份原始子文件（上述索引文件属于新增导航不计入原包）。还有46份来自该本地包未入库，包括：

- 23个历史研究复现ZIP（二进制；尚未逐包完成第三方数据/源码再分发许可审查）。
- 23个其他辅助规格/样例、schema、traceability及manifest文件。开发Agent若需要这些可从本地完整ZIP获取；GitHub现有`specs/requirements.json`只能作为任务要求，而不是完整JSON Schema全集。
- 后期AUTO-R02–R51与DEV-R01–R24的**完整原始报告/代码**原先就未取得，无法上传；它们不是上面46份本地包遗漏文件。

## 原始完整资料包

本地存档名称：`FR5刮痧机器人_完整离线算法V3.0_Agent开发与研究资料库.zip`，SHA256:
`4b399da2425bf3c7b9c284278fa5687be5aa3af6560e52515d12f1982a31e5d7`。

该本地ZIP可由此会话的附件下载，并不意味着GitHub上已有该ZIP。若使用它作为Agent附加参考，应先核验SHA256。

## 禁止误解

- “已上传”=远端存在且文件可读；不等于原始科研命题已通过实际硬件验证。
- 当前实现目标**完全离线**；不接真机/相机，不执行真实FR5指令。
- M2默认**屏幕画线**；早期实体画线算法只能作扩展。
- 缺失的AUTO/DEV源码、实验、DOI、测量值一律`UNKNOWN`，不得伪造。
