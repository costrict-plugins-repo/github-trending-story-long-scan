# Agent Note: 按五步精修短篇写作资料并保留专业术语

Status: implemented

## Problem

短篇写作入口与全部 36 份参考资料存在碎句、重复元说明、旧路径与分支范围冲突。用户追加要求保留网文专业术语，逐份完成 prose-gap-polish，并按 Skill 提交 PR。

## Decision

全部 37 份文档逐文件依次完成语言、去冗余、消歧义、事实回查、逻辑修复五步；保留爽点、钩子、卡点、收口、场景引擎、节拍等专业表达。专业术语需要说明时补具体语境，不把术语改成笼统替代词。修正短篇对标路径、无反转与无反派分支、Stage 6 完成与续跑说明；共享资料从真源同步。保持引文、代码围栏、机械字段、编号及必要门禁，不增加预算。

## Alternatives considered

- 用更普通的词替换所有专业表达，可以减少入门门槛；但会损失术语精度并违背作者要求，因此只清理自造词和晦涩表达。
- 将三个短篇 Skill 合在一个 PR，验证集中且容易同步；但用户要求按 Skill 评审，因此拆成依次衔接的独立 PR，共享副本仍随真源一起提交。

## Consequences

收益是写作资料更清楚，专业语义与执行契约保持一致。共享真源的修改会影响其他消费者，必须验证同步、预算与适配层；拆文、扫榜的独有整理留在后续各自的 PR。

## Coverage

下表记录逐文件通读的五步审查；结果由实际编辑与差异复核确认，机器守卫只验证契约。

| 文件 | 五步审查 | 结果 |
|---|---|---|
| `SKILL.md` | 1→5 完成 | 已修改 |
| `references/agent-calls.md` | 1→5 完成 | 已修改 |
| `references/author-memory-maintenance.md` | 1→5 完成 | 已修改 |
| `references/author-memory.md` | 1→5 完成 | 已修改 |
| `references/banned-words.md` | 1→5 完成 | 已修改 |
| `references/benchmark-recall.md` | 1→5 完成 | 已修改 |
| `references/cross-book-recall.md` | 1→5 完成 | 已修改 |
| `references/dialogue-mastery.md` | 1→5 完成 | 已修改 |
| `references/emotional-methods.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/世情打脸.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/双男主.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/复仇打脸.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/宅斗宫斗.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/总裁豪门.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/悬疑.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/民俗怪谈.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/沙雕脑洞.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/甜宠.md` | 1→5 完成 | 已修改 |
| `references/genre-styles/追妻火葬场.md` | 1→5 完成 | 已修改 |
| `references/genre-writing-formulas.md` | 1→5 完成 | 已修改 |
| `references/hooks-chapter.md` | 1→5 完成 | 已修改 |
| `references/hooks-paragraph.md` | 1→5 完成 | 已修改 |
| `references/output-contract.md` | 1→5 完成 | 已修改 |
| `references/reference-index.md` | 1→5 完成 | 已修改 |
| `references/scene-craft.md` | 1→5 完成 | 已修改 |
| `references/short-craft.md` | 1→5 完成 | 已修改 |
| `references/short-deslop.md` | 1→5 完成 | 已修改 |
| `references/short-format.md` | 1→5 完成 | 已修改 |
| `references/short-prose-quality.md` | 1→5 完成 | 已修改 |
| `references/short-reversal.md` | 1→5 完成 | 已修改 |
| `references/style-resolution.md` | 1→5 完成 | 已修改 |
| `references/submission-craft.md` | 1→5 完成 | 已修改 |
| `references/villain-and-reveal.md` | 1→5 完成 | 已修改 |
| `references/workflow-design.md` | 1→5 完成 | 已修改 |
| `references/workflow-draft.md` | 1→5 完成 | 已修改 |
| `references/workflow-revision.md` | 1→5 完成 | 已修改 |
| `references/writing-workflow.md` | 1→5 完成 | 已修改 |
