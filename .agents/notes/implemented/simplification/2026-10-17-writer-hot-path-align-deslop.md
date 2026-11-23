# Agent Note: 写手热路径技法与去味方法对齐

Status: implemented
Date: 2026-10-17
Related: [写作 references 按时刻重组](../architecture/2026-10-17-write-references-by-moment.md)、[排纲方法层](../architecture/2026-10-17-planning-method-layer.md)

## Problem

写手每章都读的 writing-craft、emotion-on-page、本章技法和 dialogue-mastery，有一批规则与去味三件套（expression、narrative-focus、story-logic、deslop-gates）方向相反：writing-craft 要求「第一个写足，第二个减半」，narrative-focus 写「不规定递减次数」；dialogue-mastery 要求「每个主要角色一个口癖」，expression 反对按配额加口头禅；long-suspense 写「周围人都有反应」「没有角色反应＝悬念落空」，narrative-focus 要求压缩群体反应；style-combat-face 一处写「别让主角委屈」，另一处把「狠压式」称作核心。写手按这些规则写，去味阶段再改回来。long-chapter-hooks 的示例取自短篇或带「哐啷！！！」这类违反标点规则的写法；长篇 emotional-methods 的案例是短篇追妻语料。

## Decision

- 冲突规则改成判据：写清看什么、什么时候适用，不写数量。打脸的压法统一为「可以压，但主角不自贬、不认输，读者看得到反击的筹码」；悬念与打脸场面的反应改为有利害的一两个人反应；口癖与语气词改为有来由才给。
- writing-craft 结构单位统一为场景和章；第 8 节拆成平级小节，每节规则在前。保留两条短句压力：「最重的一句通常短，常单句成段」和「过场 1-2 句带过」。
- long-chapter-hooks、emotional-methods 的示例换成长篇语料原句（七猫、黑岩，均为十万字以上的书，来源记录在实验目录）。
- long-chapter-quality 只做单章检查，严重度用「必须修 / 建议看 / 仅提示」；banned-words、dialogue-mastery 去掉历史说明和内部代号。
- 每份文件不长于改前。

## Alternatives considered

- **按候选一版整体上线**（只把冲突规则改成判据，两条短句压力一并删除）：评委一致胜负 6:4，看起来更好。不采用：落逗句比的在带率从 14/15 降到 10/15，三章都往长句偏，隔离实验确认是 writing-craft 拿掉短句压力所致。
- **只改 writing-craft 之外的文件**：改动面最小。不采用：在带率同样降到 10/15，评委 4:7。
- **不改，维持现状**：没有风险。不采用：写作端与去味端拉锯会一直在，blocking 检测命中是现状的两倍。

## Consequences

- 实测（2026-10-17，demo《让你管账号》第 8、10、21 章，写手 opus，评委 codex gpt-5.6-sol，双顺序一致才计胜负）：每臂 30 篇。落逗句比在书自身 p10-p90 带内 27/30 对 26/30（均值差 +0.015，置换检验 p≈0.60）；blocking 检测命中 13 次降到 6 次，advisory 16 次升到 19 次；退化检测两臂都是 0。评委一致胜负基线 10、本版 8，符号检验 p≈0.82；同臂互评的一致判决率 75%，与本次的 60% 相当，说明评委分辨的是样本好坏而不是版本。结论只能写「测不到退步，blocking 命中减半」，不能写「质量提升」。
- 代价：判断写法比口号略难执行，写手偶尔会把可压缩的反应写全；narrative-writer 模板「分辨率」字段里仍有「同类递减」一词，本次未改，留待模板下次修改时与 writing-craft 对齐。
- 回退方式：本次改动集中在一个提交里，单独回退即可恢复旧规则，不影响排纲与短篇的改动。
