# Agent Note: Windows 后台宿主隐藏写作守卫拉起的 Git 控制台窗口

Status: implemented
Date: 2026-10-26
Related: https://github.com/zenstory-ai/oh-story-claudecode/issues/463

## Problem

OpenCode 桌面版在 Windows 后台服务进程中执行写作守卫时，插件的项目根定位与压缩前统计会同步拉起 Git；共享守卫核在提交检查时也会拉起 Git。这三处子进程没有要求 Windows 隐藏控制台窗口，导致写正文、修改正文或压缩上下文时反复闪出控制台，干扰正常写作。

## Decision

- OpenCode 插件的 `projectRoot()` 与 `tryGit()` 调用 Git 时传入 `windowsHide: true`。
- 共享 `story_hook_core.js` 的 `stagedMarkdownWarnings()` 调用 Git 时也传入 `windowsHide: true`，再由共享资产同步器更新 OpenCode、ZCode 与 Antigravity 的部署副本。
- 保留 PR #464 贡献者 SakarPaisen 的原始提交和三个运行时选项修改，合入当前 main；不重复覆盖已有补丁。
- 在现有 OpenCode 插件行为测试中代理并转发真实 `child_process` 调用，分别执行项目根定位、压缩前 Git 统计和 staged warning 检查，断言实际 options 含隐藏窗口开关，避免把源码字符串存在性误当成行为覆盖。

- 按当前 main 的锁基线将 `story-setup` 包更新到 `1.1.41`；`story` 的 exact companion 随之指向该版本，其生成的 `INSTALL.md` 字节变化，因此包更新到 `1.0.26`。使用仓库工作流固定的中央控制器 `c584f3caac2f47168781f7165b38200f1bda2350` 重建摘要并校验基线；不改源码产品版本，也不触发发布。

## Alternatives considered

- **只合入现有贡献补丁，不补自动化回归**——最强理由：改动只有三个布尔选项，且贡献者已在 Windows 真机验证。否：后续重构 options 时可静默删掉该行为，macOS/Linux CI 也无法从外观发现回归；薄代理能跨平台锁住传参契约。
- **在所有仓库脚本的子进程调用上一律增加隐藏窗口选项**——最强理由：可统一消除未来 Windows 闪窗。否：本事故只涉及部署后运行在 GUI 后台宿主中的写作守卫；扩大到开发脚本和浏览器工具会引入无关 diff，且没有对应用户症状。

## Consequences

- **收益**：Windows 桌面宿主中的三条已知 Git 路径不再创建可见控制台，且 options 契约有可执行回归保护。
- **代价**：测试会短暂代理 Node 内置 `child_process` 的两个同步方法，但仍转发真实调用，并在结束时恢复；生产逻辑在非 Windows 平台收到一个无效果的兼容选项。
- **验证上限**：跨平台 CI 只能证明实际传参和原有返回行为不变；控制台是否肉眼闪现仍以 Windows 桌面版真机复测为最终外观证据。
