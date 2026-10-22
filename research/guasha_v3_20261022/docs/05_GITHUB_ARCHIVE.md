# GitHub研究归档计划与安全上传前检查

## 1. 用户提出的目标

用户希望完整研究可放GitHub供未来Vibecoding Agent按需查阅，避免一百多轮研究只剩摘要而无法追溯原始报告。正确做法是建立**独立私有研究仓库（建议）**或用户明确指定的独立现存仓库，并保留原始报告/讲义/复现包与不可伪造的清单；同时建立能跨多个Agent上下文稳定工作的`AGENTS.md`和需求索引。

## 2. 当前可归档的资料真实范围

已找到并打包：**旧R01–R23全部23轮研究报告和教学讲义**，其中R02–R23大部分有原复现ZIP；**AUTO-R01研究报告/讲义/原包**；阶段审计、方案演进总览、过往V2.0手册、上一版任务书。以上被放在`research_archive/`目录。**没有收到完整AUTO-R02–R51和DEV-R01–R24原始各轮成果包**。可能历史上存在，但当前源文件不可访问，所以没有伪造和声称已全部上传。`manifests/coverage.csv`会逐号注明这98个研究/工程编号的文件状态。

不归档往届同学开题报告PDF/DOC（第三方作品版权/身份资料），不归档学生个人身份信息和学校填表材料，不归档GitHub token/SDK密码/机器人网络地址，也不把大型敏感真实人体数据放公开仓库。

## 3. 推荐目录

```
AGENTS.md
00_START_HERE.md
docs/
  01_MASTER_SPEC.md
  02_ALGORITHMS_MATH.md
  03_RESEARCH_TO_ENGINEERING.md
  04_TEST_AND_RELEASE.md
  05_GITHUB_ARCHIVE.md
  06_AGENT_HANDOFF_PROMPT.md
specs/
  requirements.json
  expected_coverage_report.json
  traceability.csv
  schemas/*.schema.json
research_archive/
  legacy/R01...R23/{RESEARCH_REPORT,TEACHING_GUIDE,REPRODUCIBLE_PACKAGE.zip}
  auto/AUTO_R01_*
  audits/*
  context/*
manifests/
  coverage.csv
  SHA256SUMS.txt
```

## 4. 允许写入前的必要确认

GitHub账户有可访问的仓库，但当前未指明应写入哪个仓库。过往已有明确要求：**不能修改旧 `hoshizaki-akari/robot` 仓库，也不能擅自创建或写入不相关仓库**。因此本版先准备好可直接提交的资料目录及ZIP，上传等待用户指定 `owner/repo` 和该仓库的私有/公开状态。优先指定一个新建的私有研究仓库，例如`hoshizaki-akari/guasha-research-archive`（**仅是建议仓库名，尚未创建**）。

GitHub公开仓库意味着文档、代码及第三方引用信息可被所有人查看；请用户决定可见范围。仓库接通后：先检查现有README和分支，不覆盖；创建`research/import-20261022`分支、批量上传本目录（排除自动生成cache和大数据），读取提交SHA及GitHub端文件清单与本地SHA对比，必要时PR而不直接合并。GitHub API单文件大小和二进制内容限制视API而定；大型复现ZIP可借助Git LFS或分批加入release，不使用文本内容API错误写入二进制。

## 5. 每次新研究产生时的归档协议

以`research_archive/auto/AUTO_RNN/`或`research_archive/dev/DEV_RNN/`保存**实际取得**的报告、代码、校验和、方法与边界；每轮更新`coverage.csv`以及`docs/03_RESEARCH_TO_ENGINEERING.md`的决定项；若原始文件不可得则留`MISSING_SOURCE`状态，不把任务prompt或对话摘要当原始报告。所有可供Agent检索的每轮摘要须引用对应实际文件路径。

## 6. Agent如何真正“按需阅读而不遗漏”

在根目录提供可发现的`AGENTS.md`，每次开发先按`requirements.json`中的ID检查全量覆盖。发生具体技术障碍时，使用`docs/03_RESEARCH_TO_ENGINEERING.md`索引定位相关旧轮次，才按需打开`research_archive/legacy/Rxx/...`原报告的核心章节。研究报告里旧的“以后每小时继续研究”或“M2实体画线”指令**不是新的开发指令**，不得覆盖本次V3.0的明确需求。每轮交付必须更新需求覆盖矩阵，历史资料和开发需求分层独立保存。