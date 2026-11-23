# Agent Note: ZCode 通过插件注册专业子代理，不再按宿主固定降级

Status: implemented
Date: 2026-10-26
Related: [Discussion #497](https://github.com/zenstory-ai/oh-story-claudecode/discussions/497)、[按宿主部署](../architecture/2026-09-26-setup-by-host.md)

## Problem

ZCode 用户每次更新都得提醒 Agent 已支持子代理。仓库把旧版“不执行项目/plugin custom agents”写成宿主固定判断；插件也没有发布专业角色，单改一句文案不能让审查和写作真正派发。

## Decision

- 依据 [ZCode 插件文档](https://zcode.z.ai/en/docs/plugin) 与 [子代理文档](https://zcode.z.ai/en/docs/subagents)，从 Claude 角色真源确定性生成七份插件角色，由 ZCode manifest 声明目录。保留工具白名单、禁用工具与轮数，模型继承当前会话，不照搬 Claude 模型别名或 inline hooks。
- 七个会派角色的 skill（含 story 路由的查资料助手） 改为检查当前 ZCode 的注册角色与 Agent 工具；角色缺失、调用失败或递归限制才降级。setup 只维护项目 skills/commands/hooks/AGENTS，不创建未获官方文档支持的 `.zcode/agents`，不写用户 home。插件角色读取 workspace 的 `.zcode/skills` 资料，缺资料提示 setup。
- 部署/使用/贡献文档与守卫同步当前能力；导入的 deep-analysis 与审查可选检索员也按注册状态分流，所有调用侧冷路径只保留目标角色与 prompt，工具参数统一按顶部宿主映射；不再给未限定的 Claude 调用语法。版本不匹配时 ZCode 提示先更新插件，再刷新 workspace。
- 合入前架构复核发现短篇构思/精修、导入验证/反推细纲仍有旧调用语法或仅查目录条件；调用侧守卫递归扫描七个入口及全部 Markdown references，回归先定位出九处旧调用/目录条件，再修正并通过正反例，避免只检查入口和生成物。
- v0.9.1 发布复核确认旧 workspace Skills 优先于 plugin，会让未限定来源的 setup 继续部署旧版本。部署规范与升级指南要求从更新后的插件副本执行；若旧副本仍遮蔽，受管 story-setup 目录先移出 skill root 留备份，由新版部署重建，不删除其他用户 Skills。适配守卫同时锁住两处来源提示。
- current-contract 的共享 spawn 预检也按“当前宿主角色来源”验证，覆盖插件注册而非只认项目文件；对应 fixture 先红后绿。复审还移除 story、长篇写作与审稿版本提示里残留的“只检查 canonical 目录”，并补负例锁住该分支；版本提示不能把插件注册判断改回文件检查。
- 生成器复用已有 Codex 的 frontmatter/工具解析与带回滚发布，发布 helper 只新增文件匹配参数（Codex 默认仍是 `*.toml`）。工具白名单、deny 优先、模型继承、无效输入零写入、退役文件清理与漂移均有回归。部署清单合并重复 Hook 校验，保持现有预算，不上调。

## Alternatives considered

- **只删“不支持”文案**：改动最小，也立即消除误导；但插件未包含角色，用户仍不能按同名专业角色派发，因此补齐发布面。
- **把角色复制到项目 `.zcode/agents` 或用户 home**：前者能沿用其他宿主部署方式，后者是官方支持的用户级位置；但项目目录发现尚无官方依据，自动写 home 又扩大 setup 的项目作用域，因此走插件注册。

## Consequences

- 收益：支持插件角色的 ZCode 可以使用同一套专业分工，不再因宿主名或多端项目里的 `.zcode` 目录被强制串行。
- 代价：新增确定性生成与检查入口；只装独立 skills 而没启用插件的项目仍会降级。ZCode 的 per-agent inline hooks 无已证实接口，拆文助手的一份批次输入写入范围由角色正文约束，不能声称有 Claude/OpenCode 同等的单角色路径隔离。

## Verification

针对性生成、ZCode/Codex/OpenCode 适配与 Hook、正文守卫跨宿主 parity 通过；静态、编号、文档预算与共享检查通过。独立文档推演覆盖“七角色已注册、没有项目 agent 目录、混合宿主标记”和“独立 Skills 没有插件角色”两条路径；推演找出的旧串行分支已修复。ClawHub 按 origin/main 基线给七个修改包及 story 的精确 companion 合计八个条目升补丁并重锁摘要，固定中央控制器的离线校验通过，未发布。

未在 ZCode GUI 实测角色发现、并行派发与读取 workspace reference；插件版本在正式发布时统一提升，本补丁不切产品版本。
