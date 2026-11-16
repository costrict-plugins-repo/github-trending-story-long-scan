# Agent Note: 长篇拆文文档五步精修

Status: implemented
Date: 2026-10-15

## Problem

16 份文档存在提取顺序混用、摘要缺口路由误写、合成阶段回读边界不一致，以及文风分项预算超过总预算的问题。部分方法说明把经验判断写成没有样本条件的结论。

## Decision

16 份文档逐文依次完成语言、赘述、歧义、信息断点、逻辑断点五步；以现有脚本和已落地管道为依据修正文档，保留阶段编号、字段、原文示例与网文专业术语。

## Alternatives considered

- 只改语句能缩小差异，但无法消除错误路由和前后矛盾，因此同时修复可核实的契约说明。

## Consequences

收益是章节提取、旧成果复用和文风生成的读取边界一致；代价是旧文档中的经验数值需要明确适用范围，不能继续充当普适质量结论。


## Verification

对照 manage_analysis_run.py 的 min_plot_points、stage_required_outputs、mark_stage_command，以及现有索引/旧成果路由核对。保留所有命令、枚举和阶段编号；经验比例仍保留作复核线索。修正摘要缺口路由、文本披露顺序、原文回读边界、散落情节归档和文风预算（1800+2100+100=4000）。原文案例和专业术语保留，去掉没有样本依据的通用倍速保证。

组织文档复核补充：作者提问规则区分推荐选项与实际授权，需确认的选择仍等待回答，已授权的常规工作继续；运行说明将 `input_file` 点明为子代理输出路径，并合并已在文件开头说明的 schema 复用规则。阶段、命令与运行行为不变。

同轮复核修正 Stage 2 两处残留概述：`existing-results` 有摘要缺口时也输出紧凑章块，提交器据此补投影，不能概括成只返回观察或仅原文模式能补摘要。人物合并沿用现有 nickname 类型容纳有明确同指证据的带姓名职务称呼，泛称不触发合并；关系数量只作漏提与误合并复核，不要求改变原书关系。拆书参考里的写作口令改成观察问题，单书分析不再要求先凑五本，作品名、公式与原文案例保留。

聚合归属按目标与因果判断，角色或主题重合只用于定位候选；散落归档明确为 `剧情/散落情节.md`，与检查器从剧情单元中排除该文件的路径一致。

## Coverage

每份文档均按顺序完成：1 语言；2 赘述；3 歧义；4 代码/资料回查；5 逻辑核对。

| 文件 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| `skills/story-long-analyze/SKILL.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/author-facing.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/deconstruction-notes.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/final-checks.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/index-rebuild.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/inspiration-library.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/pipeline-ops.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/semantic-acceptance-fixtures.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/stage1-golden-chapters.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/stage2-extraction.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/stage3-plot-rhythm.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/stage4-characters-settings.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/stage5-report.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/style-profile-generator.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/style-profile-protocol.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `skills/story-long-analyze/references/synthesis-inputs.md` | ✓ | ✓ | ✓ | ✓ | ✓ |
