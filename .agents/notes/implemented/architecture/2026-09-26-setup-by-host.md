# Agent Note: story-setup 按宿主拆分，入口只读当前宿主那一份

Status: implemented
Date: 2026-09-26
Related: [每次调用加载压到 35K](2026-09-26-per-call-load-ceiling-35k.md)、[OpenCode 版本门](../bug-fix/2026-09-24-opencode-version-gate-fail-closed.md)、[后续修正：换宿主重部署与单个项目标记](../bug-fix/2026-09-26-scan-setup-review-fixes.md)

## Problem

作者第一次部署时，`skills/story-setup/SKILL.md` 去空白约 40K 一次读完，其中约 25K 是另外七个宿主的部署清单、算法、校验和报告提示；只用一个宿主的作者真正需要约 12K。流程也按「维护者视角」排：全新项目先让作者从 8 个宿主里选（当前宿主本可以从运行环境判断），接着追问部署位置；OpenCode 选模型时问题里直接写 chapter-extractor、consistency-checker 这些内部 agent 名，作者看不懂。

## Decision

- 入口 `SKILL.md` 只留通用流程，按作者第一次部署的时间线排：判断当前宿主 → 确认项目根 → 按宿主部署 → 验收 → 安装报告。路径安全检查、清理自嵌套残留、部署标记、模板占位符、AGENTS.md 合并策略、重新部署规则留在入口。
- 各宿主专属内容拆到 `references/deploy-{target_cli}.md` 共 8 份（claude-code、opencode、codex、antigravity、zcode、openclaw、reasonix、generic），每份含本端部署清单（保留 `Source path`/`Target path` 表头）、部署算法、「验证」和「安装报告必须提示」。Phase 2 Step 1 用一张 `target_cli → 文件` 表路由，多端时每端各读一份，不读其他宿主的文件。
- 宿主判断顺序：已部署项目以 sentinel 的 `target_cli` 为准；否则看自身运行环境（系统提示、工具名、调用语法）和正在执行的 SKILL.md 安装路径；再不行看项目标记，恰好一个就用。只有都定不下来才问「你现在是在哪个软件里跟我对话？」，选项是产品名加一句白话说明。当前宿主定下来而项目里还有其他宿主的标记时，只问一句要不要一起更新。
- 项目根默认是当前工作目录，只有当前目录是主目录、磁盘根、系统目录或 skill 包自身时才问。
- OpenCode 选模型的三级问题改用角色名（拆书助手、资料检索员、校对员 / 写手、人物设计师、资料研究员 / 总指挥），内部 agent 名只在「技术备注」列和部署明细里出现；写手（narrative-writer）与总指挥同在高端一级（与模型分级表一致），「请勿使用低端模型，会影响正文质量」的提醒也放在高端这级。
- 字数（去空白）：入口 10,101；宿主文件 claude-code 5,390、opencode 7,860、codex 4,032、antigravity 6,433、zcode 4,453、openclaw 1,978、reasonix 2,179、generic 1,617。入口 + 最大一份（OpenCode）为 17,961。
- 守卫跟着内容走：`check-story-setup-deployment.sh` 新增 TS2a（8 份文件存在、入口逐一路由、每份含清单/验证/报告提示三节与清单表头、恰好 8 份、入口不再「让用户选择目标环境」「确认部署位置」、自检行要求部署文件），原先对 SKILL.md 的锚点按归属改指各宿主文件，自复制探测器扫入口加 8 份文件，并新增 skills-only 三端不得串用他端 AGENTS 模板、Reasonix 建 `.agents/skills` 链接、Claude 重启标记等断言；`check-{opencode,codex,zcode,antigravity}-adapter.sh` 的 story-setup 锚点改指对应宿主文件并补路由断言；OpenCode 另加「先缓存模型再覆盖」和「问作者的话里不出现内部 agent 名」两条；`check-current-skill-contracts.py` 的 fallback 路径禁令扩到 `skills/story-setup/references`。

## Alternatives considered

- **只删文字、不拆文件**：最强理由是单文件最好维护，守卫不用改锚点。不采用：40K 里大半是别的宿主的必要步骤（版本门、hooks 互斥、symlink 迁移同意），删不动；只要作者只用一个宿主，这些就是纯负担。
- **按「hooks 端 / skills-only 端」拆成两三份**：最强理由是 OpenClaw、Reasonix、generic 三份高度相似，合并能少一些重复。不采用：三端的 AGENTS 模板、symlink 与报告提示各不相同，合在一起又回到「读别人的步骤」；重复的只有三行清单，代价小。
- **完全不问、只靠探测**：最强理由是作者零打扰。不采用：网页版 AI 或自建 Agent 里模型未必知道自己是谁，项目里也可能同时有多个宿主的标记，猜错会装错一整套文件，此时问一句白话代价更小。

## Consequences

- **收益**：第一次部署只读约 12–18K（原 40K）；能判断宿主时不再问作者选宿主、不再问部署位置；选模型的问题作者看得懂。每个宿主的部署步骤集中在一份文件里，改一个宿主不必在 550 行里找散落的段落。
- **代价**：跨宿主共享的规则（路径安全、AGENTS.md 合并）留在入口，宿主文件与入口互相指名引用，改名要两边同步；三份 skills-only 文件有少量重复的清单行。宿主判断依赖模型对自身运行环境的认知，判断错时靠作者在报告里发现；多端时仍要读多份文件。
- **行为变化**：部署步骤、校验与升级提示逐条保留；变化只有三处——能判断宿主时不再询问、默认不再询问部署位置、选模型问题改用角色名。顺带修正了原 OpenClaw / Reasonix / generic 算法里「安装报告提示项见 Phase 3 第 N 步」的错位编号（原文都差一位），改为指向本文件的「安装报告必须提示」。
- **已处理的待办**：写手曾在逐级提问和配置摘要里归中端、在分级表里归高端，现已统一到高端；重新部署时写手缓存的是旧中档模型（按本次分级落在低端或中端），会在高端这级或安装报告里提示作者重选。
