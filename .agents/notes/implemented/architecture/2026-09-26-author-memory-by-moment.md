# Agent Note: 作者记忆协议按作者时刻拆分，记一条偏好只读常用部分

Status: implemented
Date: 2026-09-26
Related: [35K 加载上限](2026-09-26-per-call-load-ceiling-35k.md)、[作者记忆两级 store](2026-09-18-author-memory-two-level-store.md)

后续归属调整见[作者记忆集中到长短篇写作 skill](../simplification/2026-10-15-author-memory-write-only.md)，本篇的时刻拆分与数据协议保持。

## Problem

`skills/story/references/author-memory.md` 是共享 reference（同步到长篇、短篇、去 AI 味、审稿四份副本），作者只要说出一条长期偏好（「以后对话都短一点」），agent 就要整读这份 8954 字（去空白）的协议。其中大半只在少见时刻才用得上：单书布局与 `state.book` 报错、存量迁移 `migrate`、预算提醒的估算细节与查询装填顺序、`作者画像.md` 的 12288 字节硬上限、「整理作者记忆」、多事件原子 `commit`、`check`、冲突候选怎么落定、升级前的旧条目。各 skill 入口也已写好本任务的 `query` 命令，协议里的四行 kind 映射表对它们是重复。

## Decision

- `author-memory.md` 只管最常见的时刻：作者说出一条偏好，或要确认、替换、忘掉某条。保留边界与优先级、存放与路由（定位规则、ID 前缀路由、跨 store 拆事件）、查询时的解释规则（低优先级倾向、`omitted_ids` 非空即超编、待确认不进 prompt）、记不记与记成什么、确认／替换／忘掉与冲突标记、回执话术、`record` 与 `query` 两条命令、单事件格式。开头一段列出少见时刻并指向维护文件。
- 新增 `skills/story/references/author-memory-maintenance.md`，在 `scripts/shared-references.json` 登记为 `author-memory-maintenance-reference`，同步到与 `author-memory.md` 相同的四个目录。内容全部原样搬自旧协议：文件树、任务映射表、注入预算与容量（估算口径、装填顺序、12288 上限）、整理作者记忆、冲突候选落定（`replace` 同时列旧条目与候选，或 `decide=reject`）、单书布局、存量迁移、旧条目、`init` / `commit` / `migrate` / `check`。
- 查询的映射表移入维护文件：短篇、去 AI 味、审稿的入口已有自己的 `query` 命令，长篇正文由组装脚本代查；主文件只留一句「长篇设定、大纲等没写命令的任务查 `story_design` + `workflow` + `interaction`，只给主会话」。`author_memory_commit.py` 里缺 `--kind` 的报错与预算注释改指向维护文件（五份运行时副本经 `sync-shared-assets.py` 同步）。
- 指路：story 入口的「作者记忆」一节在整理、超编、单书布局报错、项目级还有本书条目时加载维护文件；story-deslop 资料表与 story-review 参考表各加一行。
- 守卫：`test-author-memory-commit.py` 的「不引导推断写入」断言扩到 `author-memory*.md`；新增时刻锚点——主文件必须指向维护文件并含回执、捕获小节与 `record` / `query` 命令，维护文件必须含整理、映射表、单书布局、迁移、冲突候选、12288 与四条冷命令，且这些冷规则不得回流主文件。`scripts/doc-budget.json` 给 `author-memory.md` 登记 5450 预算锁住拆分收益；维护文件是冷路径，不登记。

字数（去空白）：作者说一条偏好时 8954 → 5353；维护文件 3964。

## Alternatives considered

- **只删重复、不拆文件**：把映射表和几处重复说法删掉，最省事，也不多一份共享副本。但迁移、单书布局、预算估算、整理这些都是真规则，删不掉，主文件仍在 7K 以上，每次记一条偏好照样全付，所以不采用。
- **拆成三份（记一条／查询／维护）**：查询时刻的解释规则单独成文，记一条偏好的时刻还能再少约 700 字。但查询规则只有几句，而且各写作时刻解释偏好时同样要读边界与优先级；再拆一份就要在长篇、审稿等处多一个指路，还要多一组共享副本，收益抵不过维护成本。
- **把映射表留在主文件**：报错信息和预算估算注释原本都指向它，不改运行时脚本。但表中四行各 skill 入口都已写成具体命令，留在主文件是重复；改两处字符串换来主文件更贴近「记一条偏好」时刻，值得。

## Consequences

- 最常见的「作者说一条偏好」时刻少读约 3.6K 字；少见时刻多读一份文件（主文件 5.4K + 维护文件 4.0K，比原来多约 0.4K 的开头指路与小节标题）。
- 新增一组共享 reference，四个 skill 各多一个副本；改维护规则时要在 `skills/story/references/` 改源文件再跑 `shared-references.py sync`。
- 主文件 5.4K 仍高于最初设想的 4.5K：剩下的是事件 JSON 格式、记不记的判定表与查询解释规则，都是这个时刻真正要用的，没有为凑数删字。
- 冲突候选的落定办法此前只写「不能直接 activate」，本次照工具实际行为（与回归测试一致）写明用 `replace` 同时列旧条目与候选，或 `decide=reject`。
