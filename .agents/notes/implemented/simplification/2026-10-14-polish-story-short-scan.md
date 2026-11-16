# Agent Note: 精修短篇扫榜全部资料

Status: implemented

## Problem

短篇扫榜把榜单互动推为故事质量、把原始简介当正文开头，也在无榜单模式要求读取不存在的聚合文件。历史平台表含缺乏采样依据的精确统计。

## Decision

入口与五份reference均已按prose-gap-polish五步精修，保留扫榜、情绪热度、钩子、反转等专业表达。按实际采集与聚合字段说明可得信息，区分简介与正文、单期与趋势、历史候选与当前事实；无数据模式跳过聚合并消费候选结论。报告缺数据项省略，原有模板与命令不改。市场共享真源随前序拆文PR修改，本次复核。

## Alternatives considered

- 让内置历史资料继续生成当前排行，报告更完整；但无法由真实样本复核，因此只给待验证候选。
- 为不同指标设计统一热度分数，方便跨平台排序；但没有已验证换算口径，因此保留各指标原始含义与同口径比较。

## Consequences

报告结论与采集能力一致，减少无数据时的伪精确判断。可得信息较少的平台会输出较少结论，需要补充样本才能分析趋势；不新增采集能力，不提高预算。

## Coverage

| 文件 | 五步审查 | 结果 |
|---|---|---|
| `SKILL.md` | 1→5 完成 | 已修改 |
| `references/manual-rank-input.md` | 1→5 完成 | 已修改 |
| `references/platform-dianzhong.md` | 1→5 完成 | 已修改 |
| `references/platform-heiyan.md` | 1→5 完成 | 已修改 |
| `references/real-market-data.md` | 1→5 完成 | 前序共享真源已修改并复核 |
| `references/topic-match.md` | 1→5 完成 | 已修改 |
