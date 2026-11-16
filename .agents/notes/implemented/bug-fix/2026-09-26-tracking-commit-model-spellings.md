# Agent Note: 追踪提交接受常见同义写法与条目形状，带状态含义的动作词要和状态一致

Status: implemented
Date: 2026-09-26
Related: [35K 加载上限](../architecture/2026-09-26-per-call-load-ceiling-35k.md)

## Problem

v0.8.1 solo A/B 的 10 章，首次 `chapter commit` 全部被退回，原因都是模型猜不到条目形状：章号写成字符串 `"22"`、自造 `planned_chapter`、`action` 写 `add`、新书快照的 `identity` 写成列表。每次退回都要多一轮修改重提，而且退回信息只说「不支持」，不说该怎么写。

放宽同义写法之后又留下一个口子：`resolve`／`回收` 与 `advance`／`推进` 都被映射成 `upsert`，但动作词本身说了伏笔走到哪一步。`action=回收` 配 `status=已埋` 会被照单全收，伏笔在状态里仍是已埋，作者以为收了的线其实没收；另外章号用 `isdigit` 判断，遇到上标数字「²」会通过判断、在 `int()` 处抛出 traceback。

## Decision

- `tracking_commit.py draft` 输出 `shapes`：每类条目的字段、整数与枚举，直接从校验常量生成，不另写一份会漂移的说明。
- 含义明确的写法照收并规范成正式形状：纯数字字符串章号（用 `isdecimal`，全角数字可收，上标数字按普通错误退回）；`planned_chapter`、`payoff_chapter` 等同义字段名；`add`／`新增`／`resolve`／`回收` 等动作同义词；单句与列表字段互换。
- 退回信息给出修法：未知字段附允许字段清单，`action` 列出可选值，缺快照指向 `current_snapshots`。
- 伏笔条目的 `resolve`／`回收` 意味着 `已回收`，`advance`／`推进` 意味着 `已埋`：`status` 缺省时按动作词补上；`status` 与动作词矛盾时退回，报错写明「回收了就写 status=已回收」或「只是推进、还没回收就写 status=已埋」，不是这个意思就把 `action` 改成 `upsert` 并写实际状态。
- 源在 `skills/story-long-write/scripts/tracking_commit.py`，经 `shared-assets.json` 同步到 story-import 与 story-review 副本；`scripts/test-tracking-commit.py` 覆盖首次提交常见写法、缺省补状态、四种矛盾组合与上标章号。

## Alternatives considered

- **只在报错里写清正式形状，不放宽**：最强理由是状态文件只有一种写法、校验最简单。不用：实测每章都要多一轮往返，模型每次都会换个说法猜；含义明确的同义写法照收不损失任何信息。
- **动作词与状态矛盾时以动作词为准自动改状态**：最强理由是提交一次过。不用：矛盾时无法判断是动作词用错还是状态写错，静默改哪一边都可能把没回收的伏笔记成已回收（或反过来），而追踪状态是后续每章「不知道就会写错」的来源；只在 `status` 缺省、意图只有动作词一个来源时才自动补。
- **把 `resolve`／`advance` 从同义词表里删掉**：最强理由是杜绝歧义。不用：这两个词是模型最常用的写法，删掉等于退回首次提交必失败的状态。

## Consequences

- 收益：首次提交不再因措辞与形状被退回；伏笔「收没收」不会因动作词与状态打架而记错；退回信息直接给出修法。
- 代价：同义词表与校验常量要一起维护，新增同义写法需同步三份副本（由 `check-shared-files.sh` 守卫）；动作词与状态矛盾时仍要多一轮修改。
