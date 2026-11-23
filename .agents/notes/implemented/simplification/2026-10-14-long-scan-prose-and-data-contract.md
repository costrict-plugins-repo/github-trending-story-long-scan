# Agent Note: 长篇扫榜文档精修与采集口径对齐

Status: implemented
Date: 2026-10-14
Related: [扫榜聚合与交接](../feature/2026-09-26-scan-aggregate-and-handoff.md)、[书库新书](../feature/2026-10-10-scan-library-new-books.md)

## Problem

长篇扫榜文档有碎散指令、未解释的缩写和无样本来源的数量结论。与当前脚本对照后，还发现番茄单份条目量、起点移动端替代榜与详情参数、晋江详情覆盖范围、聚合新书列出现条件等说明不准确。聚合的题材本数已去重，但抽样命中条数未去重；刺猬猫跨榜指标及作者缺失会影响混合聚合的解读。“可行性上限为中”与“一律给中”也使低可行性条件产生歧义。

## Decision

10 份文档逐文件完成五步精修，保留网文专业术语、采集命令、机械字段、工作流编号及真实引文。文档按当前采集与聚合代码解释来源、口径与覆盖范围，脚本和输出格式保持不变。样本不足和内置知识均以“中”为上限，素材或平台条件不足仍可判“低”；样本数量门槛不变。平台机制与读者偏好采用有适用语境的观察方向，移除无法核实的固定流量、时间、人口和配比结论。

## Alternatives considered

- 只改句子，能使变更最小；但会继续让错误口径影响选题结论，因此同时修正文档与代码可核实的不一致。
- 同时修改聚合器，可直接解决刺猬猫跨榜混算；但本轮按 Skill 整理文档，不扩大到运行时行为，先准确说明现有结果的使用边界。

## Consequences

收益是作者与 Agent 能从采集产物推到有依据的结论，减少把样本假设当成平台规则。代价是历史门槛和人口标签不再提供现成答案，部分选题需依据本次样本判断；刺猬猫聚合限制仍在代码中存在。

## Verification

静态检查、工作流编号、现有文档预算、共享文件、作者报告模板与扫榜聚合回归通过。另按采集器实际 Markdown 格式构造输入，经公开聚合 CLI 复核：新书列只在新旧榜并存时显示；同书两榜聚合计一本，抽样仍命中两条；刺猬猫作者缺失导致跨榜同书未合并，同字段的不同榜单值取最大。这些是代码与夹具验证，不是实时市场样本。

组织文档复核补充：平台与方向已有答案时直接沿用；晋江详情覆盖定位到采集文件头。移除长篇扫榜里的短篇标题公式和盐言专节，既有书名、卖点样本保持原文，只作命名与情绪入口观察。简介方法兼容无金手指题材，不把特殊能力设为所有长篇的前提。采集脚本、数量门槛与交付格式不变。


## Coverage

10 份文档均按顺序完成语言、赘述、歧义、事实回查、逻辑五步。

| 文件 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| `skills/story-long-scan/SKILL.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/topic-decision.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/publishing-guide.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/reader-profiling.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/genre-trends.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/platform-qidian.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/platform-fanqie.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/platform-qimao.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/platform-jjwxc.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-scan/references/platform-ciweimao.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
