# Agent Note: 对齐 CDP 文档的复用与重建分支

Status: implemented

## Problem

browser-cdp 文档把 `CDP_STATUS=ready` 一律解释为直接复用、不得 setup，但启动脚本在显式传入 `--reset` 或 `--profile` 时会跳过复用，继续重建调试实例。文档还把结束常规 Chrome 写成首次启动必然发生，未交代来源 profile 必须存在，并把需要询问用户绑定到并非所有宿主都有的 AskUserQuestion 工具。

## Decision

现有命令、字段、选项与退出码保持，复用和重建按脚本实际条件说明：普通连接遇到 ready 时直接复用；明确请求 reset 或 profile 时按重建处理，沿用已有的 Chrome 进程结束同意要求。文档补充 Chrome 至少使用过一次、来源 profile 存在的前提，并以宿主可用提问方式表达原有询问要求。重复解释已合并，已有的按 debug 实例 PID 清理边界保留，不新增通用安全规范。OpenCode 的超时说明按前台命令场景表述，不再断言它一律没有后台执行工具。

依据是 `setup-cdp-chrome.js` 的 `main`、`ensureConsentToKill`、`verifyPortOwnedByLaunch`，以及本机 `agent-browser 0.38.2` 的 `--help` 和 `eval --help`。启动器的普通与 reset 两种 dry-run 均返回 0，未启动或结束 Chrome。

## Alternatives considered

- **所有 ready 状态都继续复用**：路径最短，但登录过期后 reset、切换 profile 都会被文档错误地跳过，与脚本现有分支相反。
- **改脚本配合文档的一律复用规则**：可减少文档条件，但会取消脚本已支持的显式重建语义；本次维护文档，以实际实现为准。

## Consequences

收益是首次连接、复用和重建的说明与当前脚本一致，缺 profile 时有明确排查方向。代价是文档保留必要分支；执行仍依赖当前平台的 Chrome 与进程查询工具。本次不改启动器、浏览器操作命令、现有同意条件或发布配置。

## Verification

静态检查、工作流编号、文档预算、共享文件、决策笔记、作者报告与 scan/browser 源码策略守卫均通过。frontmatter、标题和可执行命令、示例的代码围栏与原稿逐字相同。

## Coverage

`skills/browser-cdp/SKILL.md` 已完整通读并依次完成合句、去元说明、消歧义、脚本回查、分支逻辑五步，已落地修改。命令、字段、编号、示例围栏保持。
