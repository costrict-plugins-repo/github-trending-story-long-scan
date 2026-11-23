# Agent Note: 长短篇写作 references 按作者时刻和读者重组

Status: implemented
Date: 2026-10-17
Related: [开书按作者确认点分段加载](2026-09-26-opening-by-author-moments.md)、[短篇按作者时刻加载](2026-09-26-short-write-by-moment.md)、[长篇写作文档精修](../bug-fix/2026-10-14-long-write-doc-contract-gaps.md)、[短篇写作资料精修](../simplification/2026-10-14-polish-story-short-write.md)、[排纲方法层](2026-10-17-planning-method-layer.md)、[写手热路径技法与去味方法对齐](../simplification/2026-10-17-writer-hot-path-align-deslop.md)、[拆文资料按时刻重组](2026-10-17-analyze-docs-by-moment.md)、[扫榜资料按时刻重组](2026-10-17-scan-docs-by-moment.md)、[其余 skill 按时刻重组](2026-10-17-other-skills-by-moment.md)、[交接契约测试](../testing/2026-10-17-handoff-contract-fixtures.md)

## Problem

10 月 14 日两个写作 skill 的全部 references 已逐份做过句子层面的精修，维护者仍觉得乱。10 月 17 日的多代理审计（5 组分簇通读、守卫约束盘点、网文工作流视角、综合与复核）找到的原因在结构层：

- 工作流文件按作者时刻组织，方法文件按写作课来源堆放，参考索引又用 Phase 1–5。主流程读不到方法：plot-frameworks、commercial-core-methods、style-craft 全仓只有 reference-index 引用，而入口规定写正文不按索引加读。
- 写作端的老规则与去味三件套方向相反。writing-craft 要求「第一个写足，第二个减半」「最重的一句要最短」，narrative-focus 写「不规定递减次数」，deslop-gates 写「不用固定句长、详写比例或最短重拍验收」。写手按老规则写，去味再改回来。
- 模板与检查器、任务包与流程对不上：artifact-protocols 的卷纲模板原样落盘后，`outline_view.py --check --strict` 报 14 个缺作用域行的错误；workflow-volume 无对标时让读 outline-methods 的八节点，volume 任务包不带这份文件；workflow-outline 指向 workflow-daily 里不存在的补建流程；短篇 agent-calls 让写手按 short-craft 执行，写手的 agent-references 里没有这份文件。
- 长短篇共享副本搬运时没有裁剪：短篇钩子文件留着系统激活、副本收获等长篇男频示例；长篇情绪弧线按短篇字数分配；长篇开篇范例取自短篇追妻语料。
- 同一事项多处口径冲突，审计列出 20 多组，例如剧情单元体量有三种口径，感情线阶段有五张表、四套名字，短篇导语有慢放重写和顺接不重演两种做法，短篇平台集合有四套。

## Decision

内容按「作者时刻 × 读者」两轴放：每个时刻一份流程文件写给主会话，至多一份方法文件，同时是任务包与写手 prompt 的来源。被守卫钉住的文件名保留，内容迁进去。分批落地，每批跑完守卫再进下一批，agents_version 在这一轮只递增一次（35 → 36）。

1. **契约断链**：卷纲模板补作用域行，原样落盘能过 `outline_view.py --check --strict` 并有回归；缺细纲、追踪说明读取时机、作者记忆查询类别各统一为一种说法；volume 任务包带八节点，outline 任务包带章首章尾钩子类型（只带名称与公式两列）；短篇派写手时由主会话把题材包要点写进 prompt。缺细纲时写指定章先补建点名章细纲再写并在汇报里说明，日更批内停在该章前问作者，旧细纲只缺字段时照旧字段写、不回写。组装脚本的题材卡匹配改为最长匹配优先。
2. **冷库方法文件清理与规划时刻分层**：style-craft、plot-frameworks、commercial-core-methods、outline-structure-theory、plot-emotion-system、plot-special-topics、outline-conflict、outline-rhythm、character-design-methods 的独有内容迁入权威文件后删除；新建 revision-diagnosis；参考索引按时刻和症状查。workflow-volume、workflow-outline 文件内分「主会话」「排纲方法」两节，任务包只截方法节；排纲刻度只在一处定义；outline-methods 管卷结构与节奏，plot-core-methods 管细纲与章级方法，long-genre-mechanics 管核心梗与金手指，character-relations 管感情线，genre-readers 管读者与平台。细纲保留五段。细节见[排纲方法层](2026-10-17-planning-method-layer.md)。
3. **写正文热路径与去味三件套对齐**：writing-craft、style-combat-face、long-suspense、emotion-on-page、dialogue-mastery 里与去味相反的配额改成判据，逐份 A/B 测不到退步才上线；钩子与情绪方法的示例换成长篇语料原句。细节见[写手热路径技法与去味方法对齐](../simplification/2026-10-17-writer-hot-path-align-deslop.md)。
4. **短篇重组**：writing-workflow 并入 workflow-design，hooks-paragraph 并入 hooks-chapter，short-prose-quality 并入 workflow-revision，写作侧 output-contract 删除；submission-craft 是平台、导语、字数、付费点的唯一出处，钩子与反转文件去掉长篇内容。导语规则按七猫热门短篇语料定（导语即正文开头，约 150-300 字；导语停在正在发生的一刻时第 1 节顺接，写到结局或后果时第 1 节可以倒回去讲经过，不按题材分），[2026-09-26-short-write-by-moment](2026-09-26-short-write-by-moment.md) 的 150-220 字口径由此改定；题材包只保留各自导语剧透什么。平台表六个平台的字数写成阿拉伯数字（番茄、红果没有语料，用 8000-12000），作者没定平台时「目标平台」写 `未定`，构思检查接受 `未定`、旧名「盐选」「番茄短篇」，并把「约 1-1.3 万」这类写法换算成整数。写正文时小节标记和引号只看 `设定.md`「平台基调」，没记就沿用 `正文.md` 已有写法，再没有用 `###1.` 与 `""`；`check-reference-gates.js` 守住格式协议和题材包不再指回 submission-craft。hooks-chapter 是构思时付费点断点拿不准时查的「2. 节尾」一节，写正文不读。
5. **拆文、扫榜与其余 skill 按同一方法重组**：拆文、扫榜见各自笔记；导入、审稿、去味、部署、路由、封面、browser-cdp 见[其余 skill 按时刻重组](2026-10-17-other-skills-by-moment.md)。跨 skill 交接由 `scripts/test-handoff-contracts.py` 的夹具守卫，见[交接契约测试](../testing/2026-10-17-handoff-contract-fixtures.md)。

以下没有在本轮做：

- short-craft 没有拆进各题材包。它承担写正文时刻题材包没写到的补位，拆开会改动 10 个题材包的正文写法，要先有按题材的配对盲评；本轮只删长篇内容，short-craft 仍是默认写法，题材包的腔调、开篇与情绪烈度覆盖它。
- 写手模板里「同类递减」的写法没有改，要做正文 A/B。
- 书名、简介、平台第一问、数据驱动回炉等新增的作者交互没有做。
- 去味、审稿、部署里会改写手或审稿输出的项（写手改读以 short-format 为源的短篇格式、solo 审稿加读体裁质量清单、Claude 规则进一步收缩等）留待 A/B，清单在[其余 skill 按时刻重组](2026-10-17-other-skills-by-moment.md)的 Consequences。

## Alternatives considered

- **再做一轮逐句精修**：改动面可控，不碰守卫。不采用：10 月 14 日已做过，审计查出的问题集中在文件归属、加载时刻和规则冲突上，句子改得再顺，主流程也读不到方法，冲突也还在。
- **主会话层与方法层拆成新文件**：读者分得最干净。不采用：workflow-volume、workflow-outline 的文件名被作者汇报守卫、契约清单、参考门禁和 story-import 副本钉住；任务包生成器已能按小节截取，文件内分节就能达到同样效果。
- **冷库文件移进归档目录、不再路由**：保留原文备查。不采用：skill 只描述当前状态，不新增顶层目录；留着的文件仍会被检索到，其中的冲突规则还会被模型读到。
- **细纲五段式改成承接、推进、落点三项**：更贴近低压章的实际写法。不采用：消融实验显示决定正文质量的是目标情绪和主角选择两个字段，改五段的收益接近零，代价是检查器、story-import 与作者已有细纲的兼容。

## Consequences

- 收益：每个作者时刻读到的方法与它要填的模板字段一一对应；写作端与去味端不再拉锯；模板能过自己的检查器；长短篇各自只看本侧内容；跨 skill 的交接点改名时 CI 当场报错。
- 代价：删除和合并文件同步了 doc-budget、共享资料清单、agent-references、代理模板与适配层；用户要重跑 story-setup 部署助手版本 36 才生效，只装 skills 的宿主在升级前仍运行旧副本。热路径以外的改动只过了守卫和夹具，主会话走法的变化（导入、去味、审稿、路由的新出口）还要在真实会话里跑一遍。
- 10 月 14 日两篇精修笔记与 9 月 26 日两篇按时刻加载笔记中，任务包整份带流程、全书体量写在卷纲时刻等事实，已在排纲方法层笔记里另写，不在原笔记上改成相反的决定。
