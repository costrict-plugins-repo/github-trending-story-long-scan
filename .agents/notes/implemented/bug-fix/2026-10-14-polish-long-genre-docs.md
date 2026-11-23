# Agent Note: 精修长篇题材资料并对齐题材卡加载与裁决

Status: implemented

## Problem

32 张长篇题材卡有“见结果一个”“小见结果”“钱、账资源”等病句，部分场面与阶段写法省略动作主体。三份题材索引与选题资料还存在可核实的冲突：索引要求正文先整读索引，实际组装脚本已按卡头直接匹配；索引把本书文风排在题材卡之后，与现有文风裁决相反；追妻框架把不回头写成唯一核心，结局却允许回头；西幻速查的换皮建议与西幻卡的制度写法相反。读者资料把男频都市暧昧支线扩大为全部都市文要求，并把简介成功承诺套到失败型和悬疑型。

## Decision

35 份文档逐份按 prose-gap-polish 五步整理。保留网文术语、题材招式、样本记录、可执行字段、锚点与节点占比，修复病句、碎片说明和不清楚的指代。正文加载以现有 build_writer_prompt.py 的卡头匹配结果为入口；人工索引用于选题或手动查阅。本书表达维度沿用 style-resolution.md 的优先级，事实与情节限制保持。追妻区分冷拒主线与复合变体，西幻保留骑士、巫师、领主等写法并对齐该题材卡。读者画像与平台差异作为样本校验的定位假设；都市暧昧要求限定在对应支线，简介承诺与已有文案类型一致。无来源的精确流量占比不作统计结论。“基于民族主义的圣主平等”可追到 0d555cf 父提交的 genre-frameworks-unified.md:2063–2069，属于未标引出处的资料标签；删去压缩后无法独立理解的标签，保留其仁义施惠换忠诚和立场一致的具体写法，限定为对应历史爽文路线。

与已有 [预算叠加与 solo 读取决策](../../implemented/architecture/2026-09-26-budget-stacking-and-solo-reads.md) 衔接，不改变脚本、省读索引或主辅两卡的设计。

## Alternatives considered

- **把题材卡改为一套通用场景判断**：可以消除大量重复措辞，但会抹掉不同题材的爽点、节拍与场景抓手；保留具体写法，仅整理说明。
- **恢复正文先读整份索引**：人工路由直观，但与现有脚本和加载预算设计冲突；说明现有匹配入口即可。

## Consequences

收益是题材卡能继续直接用于正文，资料间的加载、表达裁决与变体边界一致。代价是保留各卡必要的独立说明和原有参数；缺少原始样本名单的统计不重新估算、不扩成全题材结论。本次不改运行脚本、共享清单、预算和其他长篇资料，部署副本由根侧统一同步。

## Verification

- 35/35 份全文依序完成合句措辞、去冗余、消歧义、事实回查和逻辑检查，逐份证据输出到 `/tmp/prose-long-write-genres-coverage.json`。
- 对照 HEAD：35 份标题锚点与代码围栏、32 张卡的 frontmatter、样本说明与正文参考行不变；专业术语计数未减少。目录六套八节点占比逐套合计 100%。
- 回查 `build_writer_prompt.py` 的卡头匹配、主辅排序、无匹配分支，以及 `workflow-chapter.md`、`style-resolution.md` 和既有预算架构笔记。画像验证回查 `commercial-core-methods.md` 的评论情绪关键词法，简介类型与本文件五类表对齐。
- `static-check.sh`、`skill-numbering.py check`、`check-current-skill-contracts.py`、`check-doc-budget.sh`、题材卡匹配与无匹配回归、`git diff --check` 通过；预算上限未改。
- `check-shared-files.sh` 仅报告本轮 34 份真源对应部署副本的预期漂移；未在本工作树同步。`genre-readers.md` 所属 `analysis-reader-profile` 为 intentional divergence 组，由根侧按部署用途核对副本。
