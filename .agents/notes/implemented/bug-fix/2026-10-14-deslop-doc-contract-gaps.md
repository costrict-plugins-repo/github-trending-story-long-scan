# Agent Note: 去味 Gate 与检测脚本说明对齐

Status: implemented
Date: 2026-10-14
Related: [去味单一定档](../../implemented/bug-fix/2026-09-27-deslop-one-grading.md)、[概念链路闭环](../../implemented/architecture/2026-09-25-concept-chain-closure.md)、[长篇核心文档精修](../../implemented/bug-fix/2026-10-14-long-write-doc-contract-gaps.md)、[其余 skill 按时刻重组](../architecture/2026-10-17-other-skills-by-moment.md)（其后去掉 Gate A–G 字母，改按检测类别限定范围，取代本篇「Gate 范围保持」）

## Problem

Gate A 默认把禁用词改成动作，与 Gate C 允许准确直写、不强配微动作的要求冲突。入口称扫描器只看引号外，但破折号和部分工整并列规则实际检查台词；退化脚本默认有任意 finding 就退出 1，不能把退出 1 一概当作 blocking。入口还把定档六指标和脚本 findings 的关系留空，且未说明相对脚本命令的运行目录。

## Decision

本 skill 三份本地或 canonical 文档完成精修：入口、Gate 细则和 agent prompt；五份共享 target 完整审阅后等待真源同步。按当前脚本说明扫描范围、退出码与只读/写入分工；按现有保护协议澄清 Gate A/E/G 的改写条件，删除重复例外说明。六项定档阈值、Gate 范围、比例上限、轮次、命令与机器字段保持。

## Alternatives considered

- **改扫描脚本去适应旧说明**：可让“只查引号外”成立，但会丢失台词标点与工整并列的既有检查，故只修文档。
- **重新设计定档标准**：可处理更多边界，但会翻转已有单一定档决定，超出本轮精修范围。

## Consequences

收益是执行者按真实检测结果分流，去套话时不会自动新增微动作；代价是 Gate 真源改动后需同步 long-write 与 setup 副本。共享 target 与脚本留原样。


## Coverage

8 份文档逐份依次完成语言、赘述、歧义、真值回查、逻辑五步；共享副本对照长篇真源与本 skill 消费语境核对。

| 文件 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| `skills/story-deslop/SKILL.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/agent-calls.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/anti-ai-writing.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/author-memory-maintenance.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/author-memory.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/banned-words.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/deslop-gates.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-deslop/references/style-resolution.md` | ✓ | ✓ | ✓ | ✓ | ✓ |

集成后同步 long-write 与 setup 两份去味 Gate 副本，并消除全部待同步状态。
