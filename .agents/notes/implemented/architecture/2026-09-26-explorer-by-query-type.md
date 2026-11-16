# Agent Note: story-explorer 按查询类型分时刻加载；agent 模板去掉持久记忆

Status: implemented
Date: 2026-09-26
Related: [35K 加载上限](2026-09-26-per-call-load-ceiling-35k.md)、[拆文按阶段时刻加载](2026-09-26-analyze-stages-by-moment.md)

## Problem

- **story-explorer 每次调用都整读 10.7K**：模板里约一半是 `benchmark_style_load`（写前召回对标文风）的 13 步流程与输出示例，只有写章前召回对标时才用；作者平时问「江晨现在什么状态」「F003 回收了没」、日更查旧信息、审稿查设定，都要连带读这一半。
- **`context_load` 没有调用方**：`rg -n context_load skills scripts` 除 explorer 模板与它生成的 OpenCode / Codex 适配层外无命中。日更早已改为主会话读续写状态卡、组装脚本组写作包，文档里只剩流程、表格行和输出示例，描述里还写着「日更 Step 1 上下文加载」。
- **四个 agent 带 `memory: project`**：character-designer、narrative-writer、story-researcher、story-architect 的 frontmatter 开着 Claude Code 的持久 agent 记忆（落在项目 `.claude/agent-memory/`）。一个工作区常有多本书，上一本书、上一次任务的记忆会进这次调用，违背「时刻之间只靠落盘文件交接」；memory 还会隐性开启 Write/Edit，story-researcher 明确 `disallowedTools: [Edit]` 却因此拿到 Edit。加它的 #7、#27、#35 没有记录理由；#52 已因同一矛盾从两个只读 agent 去掉，其余四个没跟进。

## Decision

- **对标召回流程迁到按需参考**：原流程（步骤 1–13）、缺失即停规则与 `benchmark_style_load` 输出结构原文迁入 `skills/story-setup/references/agent-references/benchmark-style-load.md`（story-setup 自有文件，不属共享副本，无需登记 `shared-references.json`）。模板新增与其他 agent 一致的「参考文件路径规则」段，`benchmark_style_load` 小节只留一行「先 Read `story-setup/references/agent-references/benchmark-style-load.md`」。步骤 5 里重复的冲突说明两句合一。
- **删除 `context_load`**：表格行、流程、固定读取量说明、输出示例一并删除；description 改为实际调用点（写前对标召回、日更查旧信息、审稿查设定、路由提问）。
- **预算按查询类型分支**：`doc-budget.json` 的 story-explorer 路径改名「查资料」，分「普通查询」5,400（实测 5,349）与「写前对标召回」10,400（实测 10,326）两条分支锁住。
- **去掉四个模板的 `memory: project`**：七个 agent 都不再带持久记忆。
- **守卫跟着内容走**：`check-current-skill-contracts.py` 的主产物缺失即停、自对标忽略、书目录探针、profile_missing 区分四项断言改查新参考文件，「文风.md 不作书目录探针」的禁止规则把新文件加入扫描范围，`test-current-skill-contracts.py` 的变异用例改用新路径；`check-story-setup-deployment.sh` 的四条对标断言改查新文件，另加三条：模板必须指向新文件、模板不得再出现 `missing_primary_contract` 或 `context_load`、任何 agent 模板不得有 `memory:` 行。
- **适配层**：OpenCode 与 Codex 的 story-explorer 重新生成；Antigravity 在部署时由 `generate-antigravity-agents.mjs` 生成并改写参考路径，仓库内无产物。`memory` 本来就不进 OpenCode / Codex 产物，这四个 agent 的适配层不变。

## Alternatives considered

- **保留单文件、只删字压缩**：最强理由是 explorer 本来是便宜模型（haiku），少一次 Read 更稳，且不引入「忘读参考」的风险。不采用：对标召回的步骤是一串 fail-closed 契约（缺主产物即停、书目录与文风分开判、自对标排除），删字就是删契约；按查询类型分开后普通查询直接少一半，召回时多一次 Read，与写手、架构师读参考的方式一致。
- **把对标召回整个交还主会话、explorer 不再做**：最强理由是 benchmark-recall.md 已有主会话手工路径（a）–（g），两处描述同一件事。不采用：快捷路径让召回的大量中间读取（章节摘要、深度拆解、文风全文）留在子代理上下文，主会话只收结构化 JSON，这正是 explorer 存在的价值；本次只改加载时机，不改分工。
- **保留 `context_load` 以备将来**：最强理由是删掉后若又需要一次性写作包，要重写流程。不采用：写作包现由组装脚本确定性产出，文档里的旧流程与现行口径（续写状态卡、事务 draft）已开始漂移，留着只会让模型在没人调用的分支上花 1K 字。
- **四个 agent 的 memory 改为 `local` 或按书分目录**：最强理由是跨会话积累的写作偏好可能有用。不采用：记忆目录按项目根而不是按书分，`local` 只是换了位置；作者偏好已有按书落盘的作者记忆与 `设定/文风.md`，经提交校验，比 agent 自己写的记忆可审查。

## Consequences

- **收益**：explorer 普通查询 10,694 → 5,349 字（约 −50%），对标召回 10,694 → 10,326 字；删掉一个没人走的分支。四个创作 agent 每次调用只看落盘文件，多书工作区不再串书；researcher 的权限回到声明的 `[Read, Glob, Grep, Bash, Write]`。
- **代价**：对标召回多一次 Read；旧部署的项目要重新运行 `/story-setup` 才会拿到新模板与新参考文件（旧模板自带完整流程，未重跑也能照常工作）。已部署项目里既有的 `.claude/agent-memory/` 目录不会被删，重跑后不再被读写，作者可自行清理。未跑真实模型实验：这次只搬运文本与删除无调用方分支，召回契约字句未改。
