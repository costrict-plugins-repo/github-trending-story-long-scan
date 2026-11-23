# Agent Note: story-setup 逐文精修与部署语境核对

Status: implemented
Date: 2026-10-14
Related: [按宿主拆分部署](../../implemented/architecture/2026-09-26-setup-by-host.md)、[对标按查询类型加载](../../implemented/architecture/2026-09-26-explorer-by-query-type.md)、[部署复制身份检查](../../implemented/bug-fix/2026-08-22-deployment-manifest-self-copy-guard.md)

## Problem

story-setup 同时维护部署流程、Claude agent 真源、宿主适配产物与共享写作资料副本。文字中的重复和指代会掩盖这些归属：OpenCode 部署仍称有三个只读 agent，但 chapter-extractor 已有受限写入能力；Antigravity 产物统一名为 agent.md，验证却要求 name 与文件名一致。引用资料的复制说明也未处处写明包含子目录。

## Decision

对全部 132 份 Markdown/template 与 7 份 TOML 逐文执行五步精修。只在 story-setup 真源直接改文；共享副本复用 canonical 的五步证据，并核对部署路径、profile 和调用语境，按唯一源更新；宿主生成件由现有生成器重建后核对。按实际模板、生成器和 helper 补齐事实，保留原有专业术语、引文、命令、字段、编号、schema、门禁与作者报告协议，不增预算、框架或确认步骤。

## Alternatives considered

- 逐份手改部署副本：最强理由是能在当前文件内立即修正措辞；不采用，因为副本会偏离 canonical，生成器和共享检查也会覆盖这些手改。
- 只精修部署入口：最强理由是范围小、验证快；不采用，因为 agent 模板与参考资料同样直接影响执行，入口清楚不能消除它们内部的歧义。

## Consequences

部署说明与实际产物更容易对应，源、共享和生成关系保持可追溯。代价是需要逐文件记录证据，并在改模板后重跑生成器和相关适配检查；现有宿主能力及其限制不变。


## Integrated corrections

- 受管文件按清单更新，用户配置按管理块合并；部署标记只记录成功目标，全部目标停止时保留旧标记。
- 修正 OpenCode 只读代理描述、Antigravity 父目录名称校验、ZCode 受管 hooks 迁移、Reasonix 安装源路径与占位符默认值。
- Profile 清单允许读取表内索引点名的题材卡；情绪方法中的短篇小节示例保留，并在 long-write 真源和 setup 派生件中明确适用范围。
- 一致性检查区分作者真相与读者当前认知；extractor 返修和自检按输入模式限制取料，旧成果补摘要使用本批 source_files，逐点按文本披露顺序。
- 写手使用调用方逐字标题；架构师的作用域明确二选一；查询员核实命中段与当前位置，JSON 转义保持原文；研究员不把8000字符预览冒充全文，按实际工具权限查询。
- 对标召回区分任一主产物缺失、文风样本可用性和章节长度比较；升级指南区分历史版本事实与当前操作。
- 四份规则修正对话功能判断方向和示例范围；引文、命令、机器字段、工具权限与阶段编号保留。

## Verification

139/139 份文档逐份依序完成语言、去赘述、消歧义、真值回查、逻辑五步：62 source、62 shared、15 generated。共享资料复用真源证据并核对部署语境；26份简短命令包装的协议已清楚，保留原样。

OpenCode、Codex 生成结果已重建并提交；Antigravity 生成到临时项目验证。静态检查、编号、现行契约、文档预算、共享文件、作者报告、笔记、Python调用与reference-gates均通过；Claude/Codex/OpenCode/Antigravity/ZCode/OpenClaw/Reasonix七宿主适配、完整部署回归和固定版本 ClawHub 离线验证通过。未运行新的真实模型创作或原生 GUI 写作测试。

## File coverage

| 文件 | 来源类型 | 五步 | 结果 |
|---|---|---|---|
| `skills/story-setup/SKILL.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/UPGRADING.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/agent-quality.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/agent-reference-profiles.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/anti-ai-writing.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/banned-words.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/benchmark-style-load.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/character-basics.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/character-design-methods.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/character-relations.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/deslop-gates.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/dialogue-mastery.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/emotion-on-page.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/emotional-arc-design.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/format-and-structure.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/东方仙侠.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/传统玄幻.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/历史古代.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/历史脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/双男主.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/古言脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/古风世情.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/女频悬疑.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/女频种田.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/宫斗宅斗.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/年代.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/快穿.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/悬疑灵异.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/悬疑脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/战神赘婿.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/抗战谍战.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/星光璀璨.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/民国言情.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/游戏体育.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/玄幻脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/玄幻言情.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/现言脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/科幻末世.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/职场婚恋.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/西方奇幻.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/豪门总裁.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/都市修真.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/都市日常.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/都市种田.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/都市脑洞.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/都市高武.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards/青春甜宠.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-prose-cards.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/genre-readers.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-chapter-hooks.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-emotional-methods.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-genre-catalog.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-genre-mechanics.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-quality.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-reversal.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/long-suspense.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/opening-design.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/outline-conflict.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/outline-methods.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/outline-rhythm.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/plot-core-methods.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-chapter-hooks.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-emotional-methods.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-genre-formulas.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-paragraph-hooks.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-quality.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/short-reversal.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/style-combat-face.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/style-genre-modules.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/style-resolution.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/agent-references/writing-craft.md` | shared | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/antigravity/rules/oh-story.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/AGENTS.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/chapter-extractor.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/character-designer.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/consistency-checker.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/narrative-writer.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/story-architect.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/story-explorer.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/codex/agents/story-researcher.toml` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-antigravity.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-claude-code.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-codex.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-generic.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-openclaw.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-opencode.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-reasonix.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/deploy-zcode.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/diagnostics.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/generic/AGENTS.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/openclaw/AGENTS.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/AGENTS.md.tmpl` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/chapter-extractor.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/character-designer.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/consistency-checker.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/narrative-writer.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/story-architect.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/story-explorer.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/agents/story-researcher.md` | generated | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/opencode/commands/browser-cdp.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-cover.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-deslop.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-import.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-long-analyze.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-long-scan.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-long-write.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-review.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-setup.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-short-analyze.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-short-scan.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story-short-write.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/opencode/commands/story.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/reasonix/AGENTS.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/CLAUDE.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/chapter-extractor.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/character-designer.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/consistency-checker.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/narrative-writer.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/story-architect.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/story-explorer.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/agents/story-researcher.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/rules/story-consistency.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/rules/story-format.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/rules/story-narrative.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/templates/rules/story-outline.md` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/zcode/AGENTS.md.tmpl` | source | 1→5 完成 | 精修稿 |
| `skills/story-setup/references/zcode/commands/browser-cdp.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-cover.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-deslop.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-import.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-long-analyze.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-long-scan.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-long-write.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-review.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-setup.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-short-analyze.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-short-scan.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story-short-write.md` | source | 1→5 完成 | 协议清楚，保留 |
| `skills/story-setup/references/zcode/commands/story.md` | source | 1→5 完成 | 协议清楚，保留 |

## Repository coverage

本轮13个skill合计367份文档：353 Markdown、7模板、7 TOML。341份有实际改动，26份命令包装完成五步后保留；没有未覆盖路径。各skill单独提交堆叠PR，按表中顺序合并。共享源只维护一处，副本与生成件均已核对。各PR笔记保留逐文件覆盖表。

| Skill | 文档数 | PR |
|---|---:|---|
| `story-short-write` | 37 | [#472](https://github.com/zenstory-ai/oh-story-claudecode/pull/472) |
| `story-short-analyze` | 23 | [#473](https://github.com/zenstory-ai/oh-story-claudecode/pull/473) |
| `story-short-scan` | 6 | [#474](https://github.com/zenstory-ai/oh-story-claudecode/pull/474) |
| `story` | 3 | [#475](https://github.com/zenstory-ai/oh-story-claudecode/pull/475) |
| `story-cover` | 2 | [#476](https://github.com/zenstory-ai/oh-story-claudecode/pull/476) |
| `browser-cdp` | 1 | [#477](https://github.com/zenstory-ai/oh-story-claudecode/pull/477) |
| `story-import` | 15 | [#478](https://github.com/zenstory-ai/oh-story-claudecode/pull/478) |
| `story-long-analyze` | 16 | [#479](https://github.com/zenstory-ai/oh-story-claudecode/pull/479) |
| `story-long-scan` | 10 | [#480](https://github.com/zenstory-ai/oh-story-claudecode/pull/480) |
| `story-long-write` | 87 | [#481](https://github.com/zenstory-ai/oh-story-claudecode/pull/481) |
| `story-deslop` | 8 | [#482](https://github.com/zenstory-ai/oh-story-claudecode/pull/482) |
| `story-review` | 20 | [#483](https://github.com/zenstory-ai/oh-story-claudecode/pull/483) |
| `story-setup` | 139 | 本PR |
