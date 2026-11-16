# Agent Note: 精修审稿资料并对齐模式、事实修订与平台提示

Status: implemented

## Problem

story-review 的 10 份自有文档存在重复模式说明和碎片式判断；局部指令与既有契约不一致：版本过旧仅提示，综合裁决却把 stale 当成禁止额外核查的降级原因；文字 reviewer 把 AI 味级别写进 category，与统一枚举冲突；追踪检查失败要求重跑旧事务，未区分提交后派生视图被改动的修复方式。平台 rubric 还把正文线索直接换算为完读、追读、跳失百分比，未提供样本或计算来源；盐言字数、第一人称与都市/升级类默认写法被扩大成通用判错条件。

## Decision

集成时，审稿入口的版本提示明确交回现有 Phase 0 处理缺失、异常、工具不可用、递归限制与调用失败。开发守卫原先逐字要求“只有文件缺失才降级”，会强迫文本保留与 Phase 0 的矛盾；本次仅调整 story-review 的对应锚点并补正反例，其余 spawn skill 的校验不变。

完整审阅全部 20 份文档，逐份依序完成 prose-gap-polish 五步；10 份自有文档直接整理，10 份共享副本只读并记录 canonical 同步事项。自有文档去重复元说明、写清判断主体与范围；按现有 Phase 0、Findings Schema、文风裁决和 tracking_commit.py 修复上述冲突；solo 逐词核查也沿用本书文风裁决，不把默认词表变成作者不能覆盖的绝对命中。平台参数保留有依据的经验提示，无法由正文计算的留存数字改为待实际数据验证的风险，不声称平台官方标准。保留专业术语、真实引文、命令、编号、模板字段与 schema，不增加确认门禁，不改变 full/lean/solo 权限或共享真源。

沿用 [作者报告约定](../../implemented/feature/2026-09-24-author-facing-reports.md) 和 [审稿按时刻读取](../../implemented/architecture/2026-09-26-per-call-load-ceiling-35k.md)，不翻转原有决定。

## Alternatives considered

- **直接整理共享副本**：每份文件都能当场呈现改善，但会制造同一规则的多个版本；共享真源已由根侧与长篇批次负责，本轮只记录审阅与同步依据。
- **保留留存预测数字作为统一阈值**：看起来便于快速判定，但没有数据来源或计算方法，不能从钩子形态推出精确平台指标；改为对应的阅读风险与实测验证。

## Consequences

收益是审稿模式、问题分类、追踪修复与平台判断前后一致，作者仍能获得具体故事问题。代价是平台 rubric 不再提供无依据的精确效果预测；共享文档的最终改善依赖根侧从 canonical 同步。本轮不改运行脚本、manifest、预算或其他 skill，不提交与同步。

## Verification

- 20/20 份全文审阅，按五步顺序完成；10 份自有文档实际修改，10 份共享副本只读。逐份语言、冗余、歧义、事实与逻辑证据见 `/tmp/prose-story-review-coverage.json`。
- 共享副本已对照根侧提交 `84612e5`：10/10 与各自 canonical 字节一致，复用长篇及作者记忆真源已完成的五步证据；本隔离工作树中的旧副本未修改，也不交回覆盖新版。
- 回查 `tracking_commit.py` 的 `check_project`、`draft_transaction`、事务修订校验及 `commit --help`；区分失败旧事务重试和提交后以新 revision 重建派生视图。`check-ai-patterns.js` 的 blocking/advisory 类型与 Findings Schema 保持。
- 平台表无留存率计算来源，移除从钩子形态直接推出的百分比；保留开篇、爽点、升级、日更等题材参考及 4000＝2000×2 的原示例。盐言字数改随本次锁定范围和已核实投稿要求，不新增默认范围。
- 10 份自有文档的 frontmatter、标题锚点、作者报告模板、统一 YAML schema、真实引文与专业术语保持。prompt 中只修正 AI 味级别写入 issue/category 用 prose，以及开放钩子的事务处理说明，不改真实字段或占位符。
- static、编号、现行契约、作者报告、文档预算、notes、diff-check 通过。三个现有追踪回归覆盖写入失败重试、派生视图比较、修订预填，均通过。
- 本工作树的共享检查仍只报上一批长篇题材资料的 34 份部署副本漂移；这些已在根侧提交同步，本轮 story-review 没有新增共享漂移。


## Coverage

20 份文档依次完成语言、赘述、歧义、事实回查、逻辑五步。共享副本按已精修源及审查消费场景复核。

| 文件 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| `skills/story-review/SKILL.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/agent-prompts.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/anti-ai-writing.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/author-memory-maintenance.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/author-memory.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/banned-words.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/batch-review.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/character-relations.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/dialogue-mastery.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/plot-core-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/quality-rubric.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/review-quality.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/review-tracking.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/rubrics/fanqie.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/rubrics/qidian.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/rubrics/zhihu.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/solo.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/style-resolution.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/tracking-initialization.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-review/references/tracking-transaction.md` | ✓ | ✓ | ✓ | ✓ | ✓ |

集成后现行契约、版本预检正反例与守卫回归通过；守卫只为审稿既有 Phase 0 处理方式调整文案锚点，其他 spawn skill 校验不变。
