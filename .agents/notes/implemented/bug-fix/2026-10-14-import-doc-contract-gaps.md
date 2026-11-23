# Agent Note: 导入文档对齐现有脚本与拆文产物

Status: implemented
Related: [导入按时刻加载](../../implemented/architecture/2026-09-26-import-by-moment.md)

## Problem

导入流程的分时刻设计已落地，但部分说明仍与当前实现冲突：收尾自检要求尚未执行的激活项已完成；角色快照误写列表最多 8 条，并要求扩大范围后重新初始化已有状态；短篇结构分段、铺垫与情绪强度的来源写错；长篇迁移后半段又无条件阻断前文允许复用的旧成果。

## Decision

依据 `tracking_commit.py`、`build_outline_brief.py` 与长短篇拆文的现有输出契约，文档明确收尾自检只查此前进度，快照列表按字节限制，分批分析完成后只初始化一次。短篇的结构划分取自拆文报告，铺垫取自反转分析，情绪强度区分两套量表；长篇迁移统一旧成果兼容范围。篇幅分流以作者声明直接锁定；未声明时的信号冲突在既有 Phase 1 确认中处理，并补全少于 5 章时的字数兜底。命令、字段、编号与机械门禁保留，脚本不变。

短篇迁移保存已有内容与续写基线，不再承诺直接通过写作侧构思检查；未来设计仍由写作 skill 的现有流程补齐。格式参考允许有依据的情绪起伏，标点默认服从已有的书级文风覆盖规则。两份共享追踪参考只记录建议，留待其真源批次处理。

## Alternatives considered

- 只润色语句，改动最小；但会保留会让执行者自锁或取不到数据的指令，因此同步修复可核实的说明错误。
- 改脚本迁就旧说明，可以保留旧文档；但现有脚本和上下游契约已经一致，无需扩大实现范围。

## Consequences

收益是导入说明能够按现有实现执行，续写所需的资料来源更明确。代价是维护时必须同时核对上游产物与本 skill 的迁移说明；两份共享追踪参考已随 long-write 真源批次整理并同步，维护时仍须遵循唯一源。

## Coverage

15份文档均已完整通读并顺序完成五步；两份共享追踪资料随long-write真源批次修改并同步，当前记录已复核并完成同步的结果。

| 文件 | 五步审查 | 结果 |
|---|---|---|
| `SKILL.md` | 1→5 完成 | 已修改 |
| `references/character-state-reverse.md` | 1→5 完成 | 已修改 |
| `references/deep-analysis.md` | 1→5 完成 | 已修改 |
| `references/format-and-structure.md` | 1→5 完成 | 已修改 |
| `references/import-report.md` | 1→5 完成 | 已修改 |
| `references/import-special-cases.md` | 1→5 完成 | 已修改 |
| `references/import-tracking.md` | 1→5 完成 | 已修改 |
| `references/length-routing.md` | 1→5 完成 | 已修改 |
| `references/outline-reverse-rules.md` | 1→5 完成 | 已修改 |
| `references/outline-reverse.md` | 1→5 完成 | 已修改 |
| `references/state-tracking.md` | 1→5 完成 | 已修改 |
| `references/structure-mapping-long.md` | 1→5 完成 | 已修改 |
| `references/structure-mapping-short.md` | 1→5 完成 | 已修改 |
| `references/tracking-initialization.md` | 1→5 完成 | 共享真源批次处理 |
| `references/tracking-transaction.md` | 1→5 完成 | 共享真源批次处理 |
