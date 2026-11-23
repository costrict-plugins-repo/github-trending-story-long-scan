# Agent Note: 短篇通用技法与悬念资料并入现有专题

Status: implemented
Related: [短篇按作者时刻加载](../architecture/2026-09-26-short-write-by-moment.md)

## Problem

`story-short-write` 的 `genre-writing-techniques.md` 与 `short-suspense.md` 混有长篇续线、升级循环和拉长关系阶段的写法，也把条件技法、理论分类和相互冲突的规则合并为通用验收要求。继续在两份大全上补例外会增加加载与维护成本；用户要求只迁出独有有效内容，再删除两份文件。

## Decision

- `hooks-chapter.md` 承接全文核心问题、局部兑现与终局收束；同处的公众期/VIP 期循环、章数和钩子字数配额删除。质量速查不再重复震惊层级、情绪分值或“两长一短”验收。
- `short-reversal.md` 的报应型写法用现有伏笔回查清单记录施害场景、台词或物件，兑现时可攻守对调；不要求逐条返还。`scene-craft.md` 补对决的得失、优势、可用条件与以小搏大过程。
- 两份原文件和部署端共享悬念副本删除。索引、跨书召回、题材包、代理 profile 与模板改指现有专题；共享清单删除对应同步组和失去写作侧成员的派生组，短篇拆文的独立观察资料保留。
- OpenCode、Codex 代理由真源重新生成，Antigravity 生成到临时目录验证。迁移通过替换重复说明容纳，文件与调用预算不提高；ClawHub 按受影响包同步版本、精确依赖与指纹。

## Alternatives considered

- 保留两份文件并逐句改写，能沿用现有入口；但这会继续维护跨专题的第二套规则，也保留用户已明确要删的两份文件，因此不采用。
- 直接删除全部内容，改动最少；但会遗漏仍有价值的短篇决策方法，因此先对照已有资料迁移独有部分。

## Consequences

收益是短篇写作与代理只按专题读取，移除长篇续线和重复理论的干扰。代价是失去原来的技巧大全浏览入口，旧资料路径失效，当前调用与部署副本需要同步迁移。

## Verification

仓库静态检查、工作流编号、文档预算、共享资料与消费者可达性、共享资料回归、当前契约、参考加载门禁、决策笔记、部署检查及 OpenCode/Codex/Antigravity 适配检查通过。ClawHub 使用工作流固定的中央脚本 `c584f3c` 与 CLI `0.23.3`，通过基线版本和 12 个发布包的 SDK 清单比对。

skill-creator 的通用 `quick_validate.py` 不接受仓库既有的 frontmatter `version` 键；本次未修改两个 Skill 的 frontmatter，仓库自己的结构化静态与适配检查通过。
