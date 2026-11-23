# Agent Note: 长短篇拆文按阶段时刻加载，每个时刻只读此刻的规则

Status: implemented
Date: 2026-09-26
Related: [35K 加载上限](2026-09-26-per-call-load-ceiling-35k.md)、[开书按作者确认点分时刻](2026-09-26-opening-by-author-moments.md)、[主会话不中转内容](../simplification/2026-09-24-long-analyze-no-relay-and-digest.md)

## Problem

拆文两个 skill 的入口没有按阶段指明该读什么，模型只能把方法论和模板整份读进来：

- **长篇**：SKILL.md 8.4K，末尾一句把 pipeline-ops（7.6K）、output-templates（15.1K）、material-decomposition（13.0K）一起点名；Stage 3–5 的时刻连同 author-facing（4.3K）约 48K，还没算 digest 取回的语料。Stage 1 没有自己的小节，模板藏在 output-templates 中段；Stage 3 的小节没有指向聚合方法。入口常驻着罕见分支（章号对不上的三选一，约 700 字）、与 pipeline-ops 重复的计划/提交/拆分说明，以及只有维护者用得上的回归与语义验收样例。Stage 1 停下来问时，还要作者在串行、有限并行、不限顺序三种派发方式里选，这是工程选择。
- **短篇**：Stage 2–6 一口气做完，「必须加载」的五份（output-contract、output-templates、material-decomposition、source-story-quality、analysis-report-style）连同 SKILL 约 42.5K，再加全文约 60K。output-contract 里写作侧怎么读产出、维护者烟雾测试也被当成全程加载；SKILL.md 的验收四步与 output-contract「验收接入点」重复。

拆文各阶段之间真正依赖的只有落盘产物和进度（长篇 `_progress.md` 的阶段状态与 `mark-stage`，短篇 `_meta.json` 的 `stages_completed`），不依赖上一阶段的操作说明。

## Decision

- **长篇按 Stage 分时刻**：SKILL.md 新增「按时刻读」表，一行一个时刻：开头三章（Phase 1–2、Stage 0–1）读 `stage1-golden-chapters.md`；Stage 2 读 `pipeline-ops.md`，子代理不可用时加 `stage2-extraction.md`；Stage 3/4/5 各读 `synthesis-inputs.md`（digest 取料、阶段标记、合成阶段事实保真）加本阶段的 `stage3-plot-rhythm.md` / `stage4-characters-settings.md` / `stage5-report.md`；Stage 6 读 `style-profile-generator.md`（Stage 6 字段速查并入）；全部拆完读 `final-checks.md`。output-templates.md 与 material-decomposition.md 按阶段拆进上述文件后删除。Stage 1 在入口有了自己的小节。
- **长篇入口瘦身**：章号对不上三选一与原文变化的细则移到 `index-rebuild.md`，脚本停下时才读；入口的 Stage 2 只留一段指向 pipeline-ops；维护者回归与语义验收样例移到 `final-checks.md`「维护者回归」。
- **派发方式替作者定好**：Stage 2 默认有限并行（每轮 3 批），停下来问的模板不再列三种拆法；作者问起更快或更稳时，按 author-facing 新增的「作者问起怎么拆」解释并切换。三档本身与 story-import 自动续跑的有限并行不变。
- **短篇分两个时刻**：Stage 2–3 读 output-contract、`analysis-method.md`（拆解思路，两个时刻共用）、`stage2-3-structure-emotion.md`、`quality-checklist.md`（逐阶段清单与质量标准，BLOCK 项扫描的依据）、analysis-report-style；Stage 4–6 把阶段文件换成 `stage4-6-reversal-summary.md`，Stage 6 评源文好坏时加 source-story-quality。同类对比、平台适配、详细节奏移到 `optional-modules.md`。
- **短篇阈值统一**：反转铺垫线索按 output-contract（单一权威）写 ≥3 条、无反转/报应型不查，入口管道表、Stage→文件映射、stage4-6 方法与质量清单同口径（没有检查脚本，靠文字对齐）。数「有反差人物」（计入 `character_archetypes`）的最小判定写进 stage4-6：三层标签至少一层与另两层相反且原文有行为对照；Stage 5 只在细拆反差手法时才按标题查 analysis-character-design 一节。
- **短篇去重**：SKILL.md 的验收只保留本 skill 的具体做法（表达自检跳过源文引用、无反转合法、BLOCK 扫 quality-checklist、补不出时用故事话告诉作者）和完成汇报，步骤本身以 output-contract「验收接入点」为准；输出目录树、Stage→文件映射、写盘协议改为指向 output-contract。
- **output-contract 瘦身**：共享源（story-short-write）删去「下游消费规范」「写作流程建议」「维护者本地烟雾测试」与只对维护者有用的 sync-policy、版本约定细则；构思时用得上的读法（`_meta.json` 可选、按反转类型选骨架、情节节点排节奏、手法与原文只学写法）并成 benchmark-recall 的一段，不再另设文件；两份副本经 shared-references 同步。验收接入点三步与入口一致：表达自检命中时分析者自己修订报告本身，BLOCK 项扫本地质量清单，通过后按 SKILL 的完成汇报告诉作者。
- **守卫**：check-current-skill-contracts 新增 `analyze-moment-routing`（两个入口的按时刻读表必须链接每份时刻文件）与 `analyze-dispatch-default`（停下来问的模板不得让作者选派发方式），test-current-skill-contracts 有正反例回归。原有锚点（pipeline-ops 派发清单与 schema_version、SKILL 的章号连续校验、选题决策 Phase、author-facing 模板工程词、短篇观察标尺路由）都留在原文件或随内容保留。

| 时刻（主会话规则部分，去空白字数） | 改前 | 改后 |
|---|---|---|
| 长篇 开头三章（含 Stage 0） | 27.8K | 14.4K（章号对不上 15.8K） |
| 长篇 Stage 2 | 20.3K（自己写批次 48.4K） | 17.0K（自己写批次 22.6K） |
| 长篇 Stage 3 | 48.4K | 25.4K（查桥段词表 29.5K） |
| 长篇 Stage 4 / 5 | 48.4K | 18.3K / 15.0K |
| 长篇 Stage 6 | 32.2K | 16.6K |
| 短篇 Stage 2–3 | 42.5K | 20.1K |
| 短篇 Stage 4–6 | 42.5K | 26.9K（评源文 31.1K；人物标尺各一份 32.7–33.2K；最重的写作技法表分支 34.9K） |

改前按入口实际点名的文件计，Stage 3–5 与短篇为同一套全量加载。

## Alternatives considered

- **不拆文件，在入口精确点名小节**：最强理由是文件数不变、改动面小、git 历史连续。不采用：模型读参考文件是整份 Read，点名小节约束不了实际加载；output-templates 与 material-decomposition 各自横跨全部阶段，只靠点名，每个时刻仍要付 15K+13K。
- **短篇也按 Stage 一阶段一时刻**：最强理由是每次加载最小。不采用：短篇全文只有 5000–15000 字，Stage 2–3、Stage 4–6 各自前后依赖紧（情感曲线要用节点，人物与综合评估要用反转与手法），分成五个时刻交接成本高于省下的规则字数；两个时刻已在 35K 内。
- **派发方式仍请作者选，只把话术改得更白**：最强理由是作者对速度与稳定的偏好可能不同。不采用：三种方式的差别是并发与接续的工程权衡，作者没有依据判断；默认有限并行本来就是作者不选时的行为，作者问起时仍可切换。
- **保留 output-contract 的下游用法，只在拆文入口说「跳过该节」**：最强理由是共享契约一字不动。不采用：跳过的约定靠模型自觉，文件仍整份进上下文；下游用法本来就是写作侧的内容，放在写作侧更符合归属。

## Consequences

- 收益：长篇每个时刻 ≤25.4K（含条件分支 ≤29.5K），短篇两个时刻 ≤27K（分支 ≤35K）；Stage 1 有了入口小节，罕见分支与维护者内容不再常驻；作者不再面对工程选择。时刻之间只靠落盘交接，续跑与换新对话走同一条路。
- 代价：长篇参考文件从 9 份变 14 份，短篇核心文件从 5 份变 6 份，维护时要按阶段找文件；「按时刻读」表成为新的路由契约，加阶段文件必须同步入口（由新守卫拦）。短篇 Stage 4–6 读 analysis-writing-techniques 整份时正好 35.0K，入口要求只按标题查其中一张表；再往 Stage 4–6 文件加内容需先删等量文字。各时刻与人物标尺分支已登记 doc-budget 路径，Stage 5 的人物标尺一次只查一份。
- 行为：阶段编号、脚本命令、验收与交接不变；唯一的可观察变化是 Stage 1 停下来问时不再列出三种拆法、直接按每次三段继续。本次没有做真实模型对照评测。
