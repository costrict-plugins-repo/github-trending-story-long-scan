# Agent Note: 对齐封面提示词与平台导出说明

Status: implemented

## Problem

封面入口要求提示词全部使用英文，但文字层必须保留中文书名和笔名；风格库的通用修饰固定写 2:3，与入口的番茄 3:4 规则未交代替换关系。入口还把居中裁剪描述为不会切到文字，而现有 Step 6 已要求实际检查裁切结果。缺少书名或笔名时指定的 AskUserQuestion 也不是所有宿主都有的工具名。

## Decision

story-cover 的入口与风格库按 prose-gap-polish 五步逐份整理。提示词指令使用英文，书名和笔名保持作者原文；风格库的 2:3 是默认值，实际按 Step 1 的目标平台比例替换。导出说明保留现有居中裁剪命令和 Step 6 检查，不保证居中裁剪必然保住文字。缺必填信息时使用宿主可用的提问方式，保留原有补全要求，不增加确认门禁。现有 edits 示例使用 multipart 上传本地图片，该说明只限定本例，避免误称接口只能接受 multipart。

事实依据是本文件内的生成 / 编辑 / 导出命令、本机已安装 ImageGen skill 的输入与保存约定，以及 [OpenAI 图像编辑接口](https://developers.openai.com/api/reference/resources/images/methods/edit) 与 [图像提示词指南](https://developers.openai.com/cookbook/examples/multimodal/image-gen-models-prompting-guide)。模型默认值、base64 解码与已有尺寸请求保持。

## Alternatives considered

- **把风格库所有示例改为番茄 3:4**：示例直接可用于该平台，但会把其他平台的默认竖版比例一并改掉；保留示例并说明替换位置更符合既有多平台流程。
- **保留“居中裁剪不会切到”的保证**：说明简短，但提示词安全区不等于实际排版结果，且与现有质量检查重复冲突；导出后按原有检查确认文字完整。

## Consequences

收益是提示词语言、平台比例和导出检查衔接清楚，宿主缺少特定提问工具名时仍可补齐信息。代价是执行者仍需按平台替换默认比例，并查看导出的图片；本次不改命令、变量、接口参数、编号、模型默认值；发布包版本与指纹按现有规则更新。

## Verification

静态检查、工作流编号、文档预算、共享文件、决策笔记与作者报告守卫均通过。入口 frontmatter、所有标题、命令和示例代码围栏与原稿逐字相同。

## Coverage

| 文件 | 五步审查 | 结果 |
|---|---|---|
| SKILL.md | 1→5 完成 | 已修改，回查生成、编辑、导出命令与官方接口 |
| references/cover-styles.md | 1→5 完成 | 已修改，核对平台比例与提示词组合 |
