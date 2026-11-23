# Agent Note: 逐份精修短篇拆文资料

Status: implemented

## Problem

短篇拆文资料存在写作命令与观察标尺混用、无来源精确统计、主观评价与评分冲突、节点顺序和完成数组歧义。上一轮尚有六份短标尺只审阅而未修改，用户要求全部文件走五步精修并保留专业术语。

## Decision

入口和22份reference已逐文件完成五步精修，保留钩子、核心梗、金手指、结构模式、主角代理权等术语。已有改动已回查专业语义；补清六份标尺中的指代、钩子判断范围、悬念对象及旁观型主角的观察方式。保持引文、围栏模板、编号、机械字段与门禁；市场共享真源同步扫榜副本。

## Alternatives considered

- 六份较短标尺只列已读，可以减少改动；但其中仍存在重复元说明和判断范围缺口，因此继续按五步精修落地。
- 让所有主角拥有决定终局的行动权，可方便比较主动性；但灵魂旁观等短篇的体验正来自无力干预，因此记录实际作用与读者承诺，不将单一模式当成合格条件。

## Consequences

拆文更接近可复核的文本观察，减少把题材模型强加给源文的风险。简化部分绝对判断后，分析者需要结合文本解释效果；机械验收不变，字数预算不提高。

下表中的人物、读者、技法、段落钩子、拆文实例、知乎风格与市场数据文件已合并或删除，阻断门槛也改为只查是否写齐，见 [拆文资料按时刻重组](../architecture/2026-10-17-analyze-docs-by-moment.md)。

## Coverage

逐文件完成五步通读；output-contract.md 已随前序写作真源同步，本 PR 再核对完成与续跑约定。

| 文件 | 五步审查 | 结果 |
|---|---|---|
| `SKILL.md` | 1→5 完成 | 已修改 |
| `references/analysis-character-basics.md` | 1→5 完成 | 已修改 |
| `references/analysis-character-design.md` | 1→5 完成 | 已修改 |
| `references/analysis-character-relations.md` | 1→5 完成 | 已修改 |
| `references/analysis-method.md` | 1→5 完成 | 已修改 |
| `references/analysis-paragraph-hooks.md` | 1→5 完成 | 已修改 |
| `references/analysis-reader-profile.md` | 1→5 完成 | 已修改 |
| `references/analysis-report-style.md` | 1→5 完成 | 已修改 |
| `references/analysis-short-genres.md` | 1→5 完成 | 已修改 |
| `references/analysis-short-hooks.md` | 1→5 完成 | 已修改 |
| `references/analysis-short-mechanics.md` | 1→5 完成 | 已修改 |
| `references/analysis-short-patterns.md` | 1→5 完成 | 已修改 |
| `references/analysis-short-suspense.md` | 1→5 完成 | 已修改 |
| `references/analysis-writing-techniques.md` | 1→5 完成 | 已修改 |
| `references/deconstruction-examples.md` | 1→5 完成 | 已修改 |
| `references/optional-modules.md` | 1→5 完成 | 已修改 |
| `references/output-contract.md` | 1→5 完成 | 前序共享真源已修改并复核 |
| `references/quality-checklist.md` | 1→5 完成 | 已修改 |
| `references/real-market-data.md` | 1→5 完成 | 已修改 |
| `references/source-story-quality.md` | 1→5 完成 | 已修改 |
| `references/stage2-3-structure-emotion.md` | 1→5 完成 | 已修改 |
| `references/stage4-6-reversal-summary.md` | 1→5 完成 | 已修改 |
| `references/zhihu-style.md` | 1→5 完成 | 已修改 |
