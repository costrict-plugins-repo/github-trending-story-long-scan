# Agent Note: 长短篇拆文资料按作者时刻与读者重组，以脚本为准

Status: implemented
Date: 2026-10-17
Related: [拆文按阶段时刻加载](2026-09-26-analyze-stages-by-moment.md)、[写作 references 按时刻重组](2026-10-17-write-references-by-moment.md)、[长篇拆文契约精修](../bug-fix/2026-10-15-polish-long-analyze-contracts.md)、[短篇拆文逐份精修](../simplification/2026-10-14-polish-story-short-analyze.md)、[交接契约测试](../testing/2026-10-17-handoff-contract-fixtures.md)

## Problem

写作端按时刻重组之后，拆文两个 skill 的审计查出同一类结构问题：

- 文档写的流程与脚本行为不一致。`manage_analysis_run.py` 在 enhance 下照样为缺章算出 `RAW` 批次，pipeline-ops 却写「原文读取数必须为 0」；`chunk_range` 只认 3 章与 2.5 万字两个上限，stage2-extraction 与 chapter-extractor 模板却让模型按长章、卷界自选分块；文风协议要求填句长百分比并标 `confidence: high`，仓库里没有任何统计脚本；`--compact` 会删掉文档让人去看的逐章 `missing_*` 键。
- 时刻表漏了作者真实会走到的入口。写作端把作者送去「补拆 Stage 3+」「缺文风」，长篇拆文的时刻表没有补缺 / 增强这一行；灵感库时刻没有登记加载路径。
- 孤儿与迁移残留。deconstruction-notes 只剩桥段词表有用；style-profile-protocol 与 generator 各写半套，其中「下游怎么用」指向全仓已不存在的 Gate D；semantic-acceptance-fixtures 只被 final-checks 引用，没有任何测试使用。短篇拆文从长篇写作搬来的人物、读者、技法五份文件约 33K 字，真正被短篇各阶段用到的只有关系四类、动机链、反派四要素、三层反差和感情线阶段；zhihu-style、deconstruction-examples 不挂在任何阶段上。
- 交接契约静默断开：import 读拆文报告里不存在的三节；写前召回按旧名读情绪模块卡（「读者需求、触发器、可替换要素、反抄袭提醒」），而 `inspiration_index.py` 的门禁字段是「读者想看什么、情绪链、戏剧单元、可替换项、不可照搬」；节奏.md 没有「扩写技法」一列；短篇情节节点没有位置字段；短篇题材标签有两套；扫榜推荐的平台名写作端不认。
- 短篇阻断门槛（钩子数等计数下限）没有消费者，作者补不出来时没有出口，导入要求阶段含 6 时可能反复重跑；Stage 4–6 多份人物标尺叠加超过 35K 且未登记叠加。

## Decision

**长篇拆文（story-long-analyze）**

- SKILL.md 新增「运行约定」（`{PYTHON}` 按 python3、python、py -3 探测；原文转 UTF-8 存 `原文/原文.txt`；全文不足 2 万字建议改用短篇拆文，导入调用时不提示）。Stage 0–6 各节合并为「各阶段要点」。「Phase 2」改成两张表：按 `inspect_existing_assets.py` 的 `recommended_path` 五个值写做法（`continue_partial` 先补齐缺的 Stage 1 产物并标 `stage1`），按 `stage_repairs` 代码写补哪一步：`stage3_emotion`、`stage3_rhythm`、`stage6_style` 是文件缺了要重写，不带后缀的 `stageN` 只表示阶段没标完成，产物在且合格就只 `mark-stage`。情绪模块卡字段不全时检查器不报缺，修法（备份旧文件、只改出错的卡、标 `stage3`）写在 Phase 2 末段，灵感库与收尾检查指向它。时刻表新增「补缺 / 增强」「灵感库」两行。
- pipeline-ops 的 enhance 说法与 `manage_analysis_run.py` 一致：逐章成果覆盖全书时不读原文，仍有缺章时计划照样列出 `RAW` 批次；`--compact` 下缺章看 `semantic_coverage.missing_ranges`。
- Stage 2 分块范围以计划为准，stage2-extraction 与 chapter-extractor 模板删掉自选分块写法，两边同序同条；证据等级 A/B/C 的定义写在 synthesis-inputs（给 Stage 3–5）、chapter-extractor 模板（给子代理）、stage2-extraction（主会话自己写批次时）三处，各对应一种读者。
- style-profile-protocol 并入 style-profile-generator 后删除。文风不再自称「机械统计」：每项结论写一行「依据」（已有资料 / 原文锚点 / 样本不足），句长、标点、段落节奏最多写「原文锚点」，不再输出英文置信度代码；「不可模仿」一节改名「不建议模仿」，与拆文报告一致。下游 benchmark-style-load、style-resolution 只读「文风可用」、原文锚点与可借鉴技巧，不按 `confidence: high` 优先采纳。
- deconstruction-notes 的桥段词表并入 stage3-plot-rhythm 后删除。stage3 的剧情字段、类型、自检各留一份，覆盖率只按情节点算；stage4 两张分级表合成一张，archetype、金手指类型改中文，去掉伪数值置信度。semantic-acceptance-fixtures 删除，final-checks 的维护者一节一并删除。
- author-facing 改正深拆文件名，报告「基本信息」改为逐行「字段：值」（检查器正则能匹配「总章数：1200」），并列出有下游在读、保留原名的小节清单。stage5 写清 `选题决策.md` 的查找位置与回填格式。

**短篇拆文（story-short-analyze）**

- 题材标尺按 SKILL 的 13 个标签重排，`genre_detected` 只取这 13 个值或「通用」。情节节点模板加 `位置{第N节，约X字}`，节点的「类型」改叫「功能」，与写作端召回一致。
- 阻断门槛改为只看是否写齐：字段齐全、`beats` 含开端/发展/高潮/结局、取值合法；计数照实填，可以为 0，源文没有的写明「源文没有」也算写了。
- 人物五份合并为 analysis-characters；书名、简介、导语、付费断点、平台写法新建 analysis-submission（平台集合对齐写作端 submission-craft）。删除 analysis-character-{basics,design,relations}、analysis-reader-profile、analysis-writing-techniques、analysis-paragraph-hooks、deconstruction-examples、zhihu-style；对白五级并入 stage4-6，不公平伤害并入 analysis-characters，围观者并入钩子标尺。real-market-data 只留在 short-scan，原共享组取消。

**跨 skill 与守卫**

- import 长篇改读拆文报告真实存在的小节；写前召回与 benchmark-style-load 改用情绪模块卡的五个门禁字段；节奏参考改用「扩写方式」等真实列名；短篇写作端读节点的「位置、功能和事件」；短篇扫榜 topic-match 加「扫榜平台 → 写作目标平台」对照表（点众写成「小程序」）；import 短篇的可复用结构有几条写几条，源文没有时写「源文没有」；import 读短篇报告的小节一律加「」引用，交接测试才能覆盖。
- doc-budget 登记「长篇拆文 补缺 / 增强」「长篇拆文 灵感库」「短篇拆文 拆完后同类对比或逐节节奏」三条新路径；短篇 Stage 2–3、Stage 4–6 的条件读取各登记 stacking（最坏 24.5K、29.4K）；删除指向已删文件的分支，其余拆文路径按实测收紧到约 100 字余量。`check-short-analysis-scope.py` 把两份新文件纳入观察标尺，并把已删的旧人物、读者、技法文件列为不许回流。
- 新增 `scripts/test-handoff-contracts.py` 接入 static-check job。

## Alternatives considered

- **给文风加一个确定性句长统计脚本，保住 `high`**：数据真有来源，置信度说法也就成立。不采用：下游没有任何读取方按 `confidence: high` 采纳文风，加脚本要多维护一套跨平台计数与夹具，换来的只是一个没人用的标签；删掉虚假说法、按样本量定 med/low 就消除了误导。
- **semantic-acceptance-fixtures 移进 scripts/bench 保留**：留一份人工验收样例备查。不采用：全仓没有脚本读它，移过去仍是没人跑的文本；内容留在 git 历史里。
- **短篇的人物、读者、技法三份文件按原名各留一份精简版**：改动小，doc-budget 与共享清单不用大动。不采用：三份里大部分是长篇或写作内容（大弧 Boss、男频恋爱攻略、填空模板），短篇各阶段只用到五个概念，分三份读会让同一时刻多读两份文件。
- **analysis-paragraph-hooks 三节全部并入钩子标尺**：一次删一份文件，去向简单。不采用：对白五级和不公平伤害不是钩子，放进钩子标尺会让同一概念出现在两个时刻；按真正用它的阶段放。
- **保留短篇计数下限、给补不出时加出口**：数值门槛能逼模型多找证据。不采用：写作端只用 `reversal_type`，其余计数只作对照；下限与标尺自己的「不设固定数量」相反，硬凑出来的钩子会污染下游的对照。

## Consequences

- 收益：拆文各时刻读到的文档与脚本行为一致，enhance、分块、缺章检查不再让模型照着错误说法做；写作端送作者来补拆时有对应时刻；短篇拆文资料从约 80.8K 降到约 42.5K，Stage 4–6 最坏叠加从超 35K 降到 29.4K；交接点有测试守着，改名会当场报错。
- 代价：文风可信度降档后，`文风.md` 不再出现 `high`，依赖它的旧项目说法会变（下游不读这个字段，行为不变）。短篇阻断门槛放宽和 stage4-6 报告结构变化还没跑过真实短篇的完整管道，需要用追妻、甜宠、悬疑各一篇确认报告各段齐全、写作端能映射到题材包、导入不会反复重跑。长篇 stage1 只加了女频关系路由与「言情/宅斗」爽点形态，没有语料验证。chapter-extractor 模板改动随 agents_version 36 发布，用户重新部署后生效。
