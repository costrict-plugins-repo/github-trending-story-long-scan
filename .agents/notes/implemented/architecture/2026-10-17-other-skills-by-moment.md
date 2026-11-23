# Agent Note: 导入、审稿、去味、部署与路由按作者时刻重组，以脚本为准

Status: implemented
Date: 2026-10-17
Related: [写作 references 按时刻重组](2026-10-17-write-references-by-moment.md)、[导入按时刻分文件](2026-09-26-import-by-moment.md)、[去味三层诊断](2026-10-15-deslop-three-layer.md)、[导入文档契约断点](../bug-fix/2026-10-14-import-doc-contract-gaps.md)、[审稿文档精修](../bug-fix/2026-10-14-polish-story-review-docs.md)、[去味文档契约断点](../bug-fix/2026-10-14-deslop-doc-contract-gaps.md)、[部署文案与部署上下文](../simplification/2026-10-14-setup-prose-and-deployment-context.md)、[路由精修](../simplification/2026-10-14-polish-story-router.md)、[封面文档精修](../bug-fix/2026-10-14-polish-story-cover-docs.md)、[browser-cdp 文档精修](../bug-fix/2026-10-14-polish-browser-cdp-docs.md)、[交接契约测试](../testing/2026-10-17-handoff-contract-fixtures.md)

## Problem

写作、拆文、扫榜按时刻重组后，对其余 skill 的五份审计查出同一类问题，核心是跨 skill 交接没有守卫，生产方模板落后于消费方的门：

- 导入的卷纲剧情单元是一张表，写作端 `outline_view.py --unit` 取不出单元，写正文的降档召回永远不成立；导入的短篇 `设定.md` 按旧模板写，构思检查必然拦下 5 项；导入后让作者「说日更」，而导入不产出第 N+1 章细纲，日更第一章就停；书自己的拆文目录被 `build_writer_prompt.py` 当成对标；超过 200 章的「简化摘要」没有任何步骤生成。
- 审稿 prompt 让代理读 story-review 的副本，代理模板却禁止跨 skill 读取；consistency-checker 收不到修订号；写作端「目标平台」的取值（盐言、番茄短篇、小程序、晋江）在审稿端落不到平台表；改过正文后修追踪会被 `tracking_commit` 拒绝，审稿没有出口。
- 去味：检测器 22 类里有 7 类 blocking 没写进方法索引；SKILL 与 expression 对 blocking 的说法不一致，也没提 `去味:跳过` 标记；Gate A–G 字母在写手、长短篇调用方和组装脚本里各写一套；派写手的任务标签写手不认得；没有多章去味和撤回的走法。
- 部署：「管理块」合并分支走不到，宿主专属节不在受管名单里；占位符替换会误改路径通配；版本门写了三遍；UPGRADING 堆着 v23–v32 历史契约，对 hook 拦截的描述与实际不符；Claude 规则抄了长篇模板并已漂移。
- 路由把原始 `Notice:` 念给作者，派 explorer、researcher 时缺模板要的字段；「什么算一本书」有四种口径；封面不读已落盘的书名与题材；browser-cdp 直接给出带 `--yes` 的结束 Chrome 命令。
- 没人读的副本：审稿的 plot-core-methods、dialogue-mastery、character-relations、review-quality，导入的 state-tracking、tracking-transaction，长篇写作下的 4 份去味方法副本。

## Decision

**导入（story-import）**

- 长篇卷纲改成单元卡写法：作用域行、`### 剧情单元 L1-01`、`- 单元ID：`、章节范围、单元节拍、单元情绪引擎；写定卷纲后跑 `outline_view.py --check --strict`。导入带一份与长篇写作逐字节相同的 `scripts/outline_view.py`，登记在 shared-assets 的 `outline-volume-view` 组。`build_outline_brief.py` 按单元卡取卡，删掉简化摘要回退，本批第一章另带上一章的章尾钩子。
- 超过 200 章的分档删除，整本书都走拆文管道（chapter-extractor 分批处理体量）；细纲仍按需全量反推。
- 长短篇书目录统一为 `{导入书名}/`，与拆文目录同名。长篇下一步是「写第N章」（N 是导入后的下一章，会先补这一章细纲），短篇下一步是「补完构思」。短篇 `设定.md` 按写作端构思模板写，题材参考和核心招式两项留给续写构思；正文只整理外壳，分段和引号保留原样。
- 角色卡对齐主角卡和配角卡；关系按拆文的 10 列有向表转换，新增「表面与真实」小节；经作者同意把本书文风存成 `设定/文风.md`。对标登记、资料同步表和冲突规则拆到 `benchmark-sync.md`，只在导入记录绑定了外部对标时读。
- 删除 state-tracking、tracking-transaction 副本；format-and-structure 的源改由 story-setup 的 `agent-references/format-and-structure.md` 持有，内容不变，共享组撤销。
- `build_writer_prompt.py` 判断有无对标时，跳过拆文库里与书目录同名的目录。`check-outline-contract.js` 回报的模板出处只列本 skill 带着的文件（长篇写作是 workflow-outline.md，导入是 outline-reverse-rules.md）。长篇写作里「再 `/story-import` 同步」的说法改成首次引用时同步，「题材正文提示卡导入项目可能已有」删掉。README、docs/hosts、docs/hosts_EN、docs/ai-long-novel-character-consistency 、README_EN 和导入自己的入口与特殊情况的导入后下一步统一为 `写第N章`。

**审稿（story-review）**

- 删除 plot-core-methods、dialogue-mastery、character-relations、review-quality 四份文件及 `character-relations-consumers` 派生组；代理的补充资料按各自读取表读。
- 通用 rubric 每次都加载，平台表叠加在上面。SKILL.md 有一张按篇幅的平台映射表：长篇起点叠 qidian、番茄叠 fanqie，晋江、其他、未定不叠加；短篇所有平台共用 zhihu.md（标题改为「短篇 Quality Rubric」）。平台来源只认 `设定/题材定位.md` 或 `设定.md`，拆文报告的「目标平台」是对标书的平台，不用。
- consistency-checker 的 prompt 带 `last_committed_chapter`、`state_revision`，派它之前先跑 `tracking_commit.py check`。tracking-transaction 只在提交报错时读；遇到「正文已改」拒绝时不重试，请作者转 `/story-long-write`。solo 不改追踪，遇到追踪事实错漏时给作者白话出口；hook 文案不动。
- 去掉「≤5 字」；句长让位于文风；发布门槛表加「报告总体判断」一列；严重度统一成白话；rubric 合并重复行、删「日更支持」、数字阈值改为参考值。

**去味（story-deslop）**

- expression 末节是检测器 22 类提示的唯一索引，加粗的是 blocking，只标每类对应本文件哪一节，判断规则不变；长篇 banned-words 的重复索引删掉，该节改名「使用边界」。
- deslop-gates 去掉 A–G 字母，按检测类别或问题名限定范围（小节「按检测类别或问题限定范围」）。长短篇 agent-calls、长篇 workflow-chapter、style-resolution、`build_writer_prompt.py` 与写手模板统一改称「检测类别」「调用方限定的检查范围」；短篇派写手的任务标签改为写手认得的「审查+去AI味」，格式检查写进检查分工；写手的覆盖自检只在长篇写作时做。
- SKILL.md：作者只说「去AI味」按改稿处理；长篇剧情级改动转 `/story-long-write`「修改第X章」；`/story-review` 的结论可作为去味范围；多章逐章完成再进下一章；作者要求时处理 `去味:跳过` 的章，清零后删掉标记；改文件前备份到 `.story/work/去味原稿/`，汇报里说明怎么撤回；扫描与标点写入分两步，先按语义改破折号，复扫没有 blocking 后再跑标点脚本，整份文件的完整去味默认整理标点。
- 长篇写作下 expression、narrative-focus、story-logic、deslop-gates 四份副本删除：主会话自己写时按检测器提示改，不读去味细则，完整去味走 `/story-deslop`。

**部署（story-setup）**

- 项目指令每个宿主一个管理块 `<!-- oh-story:{宿主} 开始 … -->` 到 `<!-- oh-story:{宿主} 结束 -->`，由新增的 `scripts/merge-project-instructions.py` 合并：已有块整块替换；旧版无标记的部署内容迁移一次，「Skill 路由表」「文件结构」「写作约束」只在内容带 oh-story 痕迹时才算旧部署；重复部署逐字节相同。占位符只替换 `{项目名}`；版本门只在 Phase 1 留一处。
- UPGRADING 只留作者向内容，改正 hook 拦截的说法，新增升级步骤、助手版本 36 必须重跑和从 v0.8.5 升级三节，并写明旧导入卷纲的表格单元在写正文时取不到、续写新章不受影响。
- 写手、架构师、角色设计、一致性检查、查资料、资料研究模板：「升级路径」改为交付里写「待主会话裁决」；角色设计内联的方法摘要换成「任务 → 读哪份哪节」表；一致性检查与查资料加短篇分支；资料研究用 node 探测浏览器端口，端口没开交回主会话。Claude 规则只留不会过时的条目。OpenCode、Codex 产物重新生成，Antigravity 只改生成器。

**路由、封面、browser-cdp**

- 路由：版本与降级提示给作者一句白话，原文只进技术备注行，路由与导入都列进 `AUTHOR_NOTE_PREFLIGHT_SKILLS`；写明查询意图到 `query_type` 的对照表和 explorer、researcher 的 prompt 字段。书目录判定以 hook 为准，另认 `设定/题材定位.md`，以及同时有 `设定.md` 和 `小节大纲.md` 的目录，排除 `拆文库/`、`对标/`。更新办法按安装渠道分开写；部署和更新完都提示新开会话。
- 封面先读 `.active-book` 与项目设定，读到的不再问，再按词推断题材；先用一句话描述 2–3 个方案让作者挑，只生成选中的一张；有书目录时默认存到 `{书目录}/封面/`；API 回退移到按时刻读的 `references/api-fallback.md`。
- browser-cdp 结束 Chrome 统一为「先 `--dry-run`，作者同意后再加 `--yes`」，Windows 清理以 CIM 为主。

**守卫与预算**

- `test-handoff-contracts.py` 新增导入长篇卷纲过写作端取段器、细纲检查回报出处存在、写作端平台到审稿平台表、审稿到 consistency-checker 字段、去味索引与写手任务标签、导入短篇过构思检查、路由派助手字段几组断言，见[交接契约测试](../testing/2026-10-17-handoff-contract-fixtures.md)。`test-writer-pipeline.py` 加导入卷纲模板与本书拆文目录不算对标的回归。
- doc-budget：导入结构迁移长篇 14950、短篇 10200，新增「长篇·绑了外部对标」16700；追踪初始化改登记 tracking-initialization（12050）；确认来源与范围收紧到 7550 / 8700；去味 expression 3800；审稿加「提交报错时读修订协议」分支 28800。部署写作环境登记 8 个宿主的叠加，最坏 44406，列进 `path_ceiling.exempt`。

## Alternatives considered

- **导入超过 200 章时保留简化摘要档，补一个生成它的时刻**：长书导入更快。不采用：拆文管道的 chapter-extractor 已按批处理体量，再多一档就多一套摘要格式要下游兼容，而这一档至今没有步骤生成。
- **导入长篇卷纲保留表格，在取段器里加表格解析**：老项目不用迁移。不采用：取段器是写作端唯一的卷纲解析器，加第二种写法会让长篇写作的严格检查也要接受表格，两边的单元字段从此分叉。
- **审稿的三份方法副本保留、改成让代理读**：代理拿到的资料与写作端一致。不采用：代理模板禁止跨 skill 读取，它们各自的读取表已经带着同名文件；副本只会和源头漂移。
- **Gate A–G 字母保留，补一张字母到检测类别的换算表**：老用户说「查 G」仍可用。不采用：字母本身不带意思，调用方、写手和组装脚本各写一套已经对不上；直接用检测类别和问题名，换算表就不需要了。
- **多宿主部署限制为最多 4 个宿主一次**：能守住 35K 上限。不采用：已部署项目的 `target_cli` 由 sentinel 决定，限制会让一次升级拆成几次会话；部署时刻不写正文，部署文件都是机械清单。

## Consequences

- 收益：导入产出能直接过写作端的取段器和构思检查，导入后第一次写章不再停；审稿按项目自己的平台选表；去味的范围在调用方、写手和组装脚本里是同一套说法；部署合并不再覆盖作者自己写的段落；路由、导入的技术提示不会念给作者。各交接点改名时 CI 当场报错。
- 代价：已经导入、卷纲还是表格形的老项目，旧单元用 `outline_view --unit` 取不到，要重写旧章时先补单元卡；多宿主部署一次读满 8 份部署文件会超过 35K；导入的新提问（本书文风、表面与真实关系）、去味的备份撤回与多章流程、审稿的新出口、路由的书目录判定都只过了守卫和夹具，没有在真实会话里跑过。
- Deferred（要做正文或审稿 A/B，零退步才上）：
  - 写手改读以 short-format 为源的短篇格式（黑岩一句一段、节尾钩子与 format-and-structure 的冲突），以及删掉 format-and-structure 开头混进来的长篇说明；
  - 写手模板「同类递减」的写法；
  - solo 审稿按篇幅加读 long-quality / short-quality，并把这两份的源头迁到审稿。review-quality 已删（含长篇黄金三章、爽点间隔与连续无冲突的节奏检查、五维评分和短篇专项），这一项落地前审稿没有按篇幅的专项清单，只靠通用 rubric 和平台表；
  - expression 索引和检测类别改名虽未改判断规则，发版前跑一次写手去味 A/B；
  - 去味 SKILL.md 新增的行为规则：作者只说「去AI味」按改稿处理、长篇剧情改动转 `/story-long-write`、改前备份、多章逐章、`去味:跳过` 标记的处理、退化检查改为只拦 blocking、整份文件完整去味默认整理标点。这些只过了守卫，发版前随上一条跑一次去味真实会话或 A/B。
- Deferred（本轮不做或待定）：
  - 维护者已定不做：去味把作者偏好同步进文风或白名单；审稿报告落盘；
  - 升级时把必做项写进安装报告，要先拿旧版项目真实升级一次；
  - 审稿只同步 tracking-transaction 的「修订事务」一节并删 tracking-initialization，要先改长篇源头；
  - ZCode 部署命令强行指定宿主、Phase 1 检测了不用、7 份宿主模板里手抄的路由表没有一致性守卫；
  - 封面补年代种田、无限流、电竞的风格，以及起点、晋江、七猫等平台的上传尺寸，都要先核实；
  - 短篇逐节字数由脚本测；路由与 hook 书目录判定的一致性守卫；style-resolution 里 `_文风摘要.md` 与短篇「文风约定」两处指针。
