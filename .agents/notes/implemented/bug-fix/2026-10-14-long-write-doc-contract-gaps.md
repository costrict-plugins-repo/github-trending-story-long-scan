# Agent Note: 长篇写作文档精修与现有契约对齐

Status: implemented
Date: 2026-10-14
Related: [概念链路闭环](../../implemented/architecture/2026-09-25-concept-chain-closure.md)、[开书分段加载](../../implemented/architecture/2026-09-26-opening-by-author-moments.md)、[导入文档补齐](../../implemented/bug-fix/2026-10-14-import-doc-contract-gaps.md)

## Problem

长篇核心工作流与技法资料中仍有信息断点：初始化把六项输入与七栏派生视图混称；工具不可用一概归因为缺 Node；格式说明漏掉作者明确字数范围的优先级；部分技法把章纲比例、示例篇幅和结构模型写成覆盖当前细纲契约的通用要求。少数表格计数、指针和因果描述前后不一。

## Decision

逐份依 prose-gap-polish 五步通读 52 份文档，其中 49 份可编辑文档完成精修，按 `storyctl.py`、`tracking_commit.py`、细纲检查器及当前工作流补齐解释。保留专业术语、具体反推方法、机械字段、命令、编号和既有门禁。作者记忆与去味 Gate 三份共享 target 只审阅，建议交真源维护者。其余共享真源只修改 source，由集成者统一同步。日更批末检查覆盖尚未单独核查的章节；不明经验比例删除，保留具体因果检查；示例计数、细纲指针与初始化字段数按当前内容纠正。

## Alternatives considered

- **只改句子、不核对契约**：改动小，但会留下照文档执行时遇到的歧义，无法完成信息与逻辑检查。
- **把技法统一改成泛化建议**：能消除表面冲突，但会丢失具体写法；本次只注明模型适用尺度、修正明确错误。

## Consequences

收益是文档与当前实现一致，读者能区分模板、技法模型和工程门禁；代价是共享真源修改后须在集成时同步副本。脚本与预算不变。


## Coverage

本 skill 87 份文档全部完成五步：下表52份核心资料，另35份题材资料见同日 polish-long-genre-docs 笔记。两份作者记忆已由 story 真源精修并同步；deslop-gates 已由 story-deslop 真源精修并同步。

| 文件 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| `skills/story-long-write/SKILL.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/agent-calls.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/anti-ai-writing.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/artifact-protocols.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/author-memory-maintenance.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/author-memory.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/banned-words.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/benchmark-recall.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/character-basics.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/character-design-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/character-relations.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/commercial-core-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/cross-book-recall.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/deslop-gates.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/dialogue-mastery.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/emotion-on-page.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/emotional-arc-design.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/emotional-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/female-audience-writing.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-chapter-hooks.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-chapter-quality.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-format.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-genre-mechanics.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-reversal.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/long-suspense.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/opening-design.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/outline-conflict.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/outline-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/outline-rhythm.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/outline-structure-theory.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/plot-core-methods.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/plot-emotion-system.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/plot-frameworks.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/plot-special-topics.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/project-files.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/reader-contract-and-progression.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/reference-index.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/stop-and-fix.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/style-combat-face.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/style-craft.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/style-genre-modules.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/style-resolution.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/tracking-initialization.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/tracking-transaction.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-chapter.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-daily.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-outline.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-revision.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-setup.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/workflow-volume.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/writer-prompt-fallback.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-write/references/writing-craft.md` | ✓ | ✓ | ✓ | ✓ | ✓ |

集成时修正题材索引对单章工作流的称呼，使共享部署副本不指向未部署文件；同步71份共享副本，不修改预算。

组织复核补充：入口将场景目标写成主导情绪，保留其他情绪同时成立的空间；日更区分改变既定安排的确认与已授权的常规追踪提交；修订外部事实待核时先查证再回改写与检测。卷纲自查的低压及过场约15%与下方题材分级统一为均衡参考，不再在一处写上限、一处允许按女频关系容量增加。步骤、脚本、文案示例与预算不变。

组织复核继续对齐该事实：卷纲模板及结构自查采用相同的题材分档，分档指针归到 workflow-volume「章节定位与张弛」。模板围栏只修这一句规则，不改正文样例或正反例。结构理论按所选模型应用，不以五幕模型覆盖其他结构；人物弧和潜台词按题材安排，支线事件仍与主线关联。写前召回的缺失处理限定在全量路径，成熟项目短路径仍按 build_writer_prompt.py 的现有判定执行，不增加绕过条件。
