---
description: 从交互式或提供的原则输入创建或更新项目章程，确保所有依赖模板保持同步。
handoffs: 
  - label: Build Specification
    agent: speckit.specify
      prompt: 基于更新后的章程实现功能规范。我想要构建...
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 概述

您正在更新 `.specify/memory/constitution.md` 中的项目章程。此文件是一个模板，包含方括号中的占位符标记（例如 `[PROJECT_NAME]`、`[PRINCIPLE_1_NAME]`）。您的工作是（a）收集/派生具体值，（b）精确填充模板，以及（c）将任何修改传播到依赖工件。

遵循此执行流程：

1. 加载现有章程模板 `.specify/memory/constitution.md`。
   - 识别所有形如 `[ALL_CAPS_IDENTIFIER]` 的占位符标记。
   **重要**：用户可能需要比模板中更多或更少的原则。如果用户指定了数量，请遵循该数量并相应更新文档。

2. 收集/推导占位符的具体值：
   - 如果会话中提供了值，则使用它。
   - 否则从仓库上下文（README、文档、嵌入的先前章程版本）中推断。
   - 关于治理日期：`RATIFICATION_DATE` 是最初采纳日期（如果未知请询问或标记 TODO），如果进行了更改则 `LAST_AMENDED_DATE` 设为今天，否则保持之前的日期。
   - `CONSTITUTION_VERSION` 必须根据语义化版本规则递增：
     - MAJOR：不向后兼容的治理/原则删除或重定义。
     - MINOR：新增原则/部分或对指南进行实质性扩展。
     - PATCH：澄清、措辞、拼写修正或非语义性精修。
   - 如果版本提升类型不明确，请在最终确定前提出理由。

3. 起草更新后的章程内容：
   - 用具体文本替换每个占位符（除非项目有意保留某些模板槽并给出明确理由，否则不应留下方括号标记）。
   - 保留标题层级；替换后的注释可移除（除非仍有说明价值）。
   - 确保每个原则部分包含：简洁的名称行、一段话（或项目符号）说明不可协商的规则，以及在不明显时的明确理由。
   - 确保治理部分列出修订程序、版本策略和合规审查预期。

4. 一致性传播检查清单（将先前的检查清单转换为主动验证）：
   - 阅读 `.specify/templates/plan-template.md`，确保任何“章程检查”或规则与更新后的原则一致。
   - 阅读 `.specify/templates/spec-template.md` 以检查范围/需求对齐——如果章程添加或删除了强制性部分或约束，请更新模板。
   - 阅读 `.specify/templates/tasks-template.md`，确保任务分类反映新增或删除的以原则为驱动的任务类型（例如可观测性、版本管理、测试规范）。
   - 阅读 `.specify/templates/commands/*.md` 中的每个命令文件（包括本文件），以验证是否存在过时引用（例如仅针对特定代理的名称），在需要通用指南时将其修正。
   - 阅读任何运行时指南文档（例如 `README.md`、`docs/quickstart.md` 或存在的代理特定指南文件），并更新对已更改原则的引用。

5. 生成同步影响报告（在更新后将其作为 HTML 注释置于章程文件顶部）：
   - 版本变更：旧 → 新
   - 被修改的原则列表（如重命名则列出旧标题 → 新标题）
   - 新增部分
   - 删除部分
   - 需要更新的模板（✅ 已更新 / ⚠ 待处理）及其路径
   - 如有故意暂缓的占位符，列出后续 TODO。

6. 在最终输出前进行验证：
   - 不留未解释的方括号标记。
   - 版本行与报告一致。
   - 日期为 ISO 格式 YYYY-MM-DD。
   - 原则为声明式、可测试且避免模糊语言（如将 "should" 在适当情况下替换为 MUST/SHOULD 并附理由）。

7. 将完成的章程写回 `.specify/memory/constitution.md`（覆盖写入）。

8. 向用户输出最终摘要，其中包括：
   - 新版本号及提升理由。
   - 标记为人工后续处理的任何文件。
   - 建议的提交信息（例如：`docs: amend constitution to vX.Y.Z (principle additions + governance update)`）。

格式与样式要求：

- 使用与模板完全一致的 Markdown 标题层级（不要降级或升级标题层级）。
- 对较长的理由行进行折行以保持可读性（理想 <100 字），但不要为了硬性换行而破坏句意。
- 在章节之间保留单个空行。
- 避免行尾多余空白。

If the user supplies partial updates (e.g., only one principle revision), still perform validation and version decision steps.

If critical info missing (e.g., ratification date truly unknown), insert `TODO(<FIELD_NAME>): explanation` and include in the Sync Impact Report under deferred items.

Do not create a new template; always operate on the existing `.specify/memory/constitution.md` file.
