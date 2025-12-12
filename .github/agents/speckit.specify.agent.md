---
description: 从自然语言功能描述创建或更新功能规范。
handoffs: 
   - label: Build Technical Plan
      agent: speckit.plan
      prompt: 为规范创建技术计划。示例：我正在使用...
   - label: Clarify Spec Requirements
      agent: speckit.clarify
      prompt: 澄清规范中的需求
     send: true
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 概述

用户在触发消息中 `/speckit.specify` 后键入的文本**就是**功能描述。假设您在此对话中始终可以使用它，即使下面显示的是 `$ARGUMENTS` 字面量。除非用户提供了空命令，否则不要要求用户重复。

给定该功能描述，请执行以下操作：

1. **生成简洁的短分支名**（2-4 个词）：
    - 分析功能描述并提取最有意义的关键词
    - 创建一个 2–4 词的短名以捕捉功能要点
    - 尽量使用“动词-名词”格式（例如，"add-user-auth"、"fix-payment-bug"）
    - 保留技术术语和缩写（OAuth2、API、JWT 等）
    - 保持简洁且具有可读性，便于一眼识别功能
    - 示例：
       - "I want to add user authentication" → "user-auth"
       - "Implement OAuth2 integration for the API" → "oauth2-api-integration"
       - "Create a dashboard for analytics" → "analytics-dashboard"
       - "Fix payment processing timeout bug" → "fix-payment-timeout"

2. 在创建新分支前检查现有分支：

   a. 首先拉取远程分支以确保信息是最新的：

      ```bash
      git fetch --all --prune
      ```

   b. 在所有来源中查找与短名匹配的最大特性编号：
      - 远程分支：`git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - 本地分支：`git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - 规范目录：查找匹配 `specs/[0-9]+-<short-name>` 的目录

   c. 确定下一个可用编号：
      - 从上述三个来源提取所有编号
      - 找到最大的编号 N
      - 使用 N+1 作为新分支编号

   d. 使用计算出的编号和短名运行脚本 `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS"`：
      - 传递 `--number N+1` 和 `--short-name "your-short-name"` 以及功能描述
      - Bash 示例：`.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" --json --number 5 --short-name "user-auth" "Add user authentication"`
      - PowerShell 示例：`.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" -Json -Number 5 -ShortName "user-auth" "Add user authentication"`

   **重要提示**：
   - 必须检查三个来源（远程分支、本地分支、规范目录）以找到最高编号
   - 仅匹配与短名完全一致的分支/目录模式
   - 如果未发现任何与该短名匹配的分支/目录，则从编号 1 开始
   - 每个功能仅运行此脚本一次
   - JSON 输出会在终端中提供 —— 始终参考它以获取实际路径和内容
   - JSON 输出将包含 `BRANCH_NAME` 和 `SPEC_FILE` 路径
   - 对于包含单引号的参数（如 "I'm Groot"），请使用转义语法，例如：'I'\''m Groot'（或尽量使用双引号："I'm Groot"）

3. 加载 `.specify/templates/spec-template.md` 以了解所需章节结构。

4. 遵循以下执行流程：

    1. 从输入解析用户描述
       如果为空：报错 "No feature description provided"
    2. 从描述中提取关键概念
       识别：参与者（actors）、动作、数据、约束条件
    3. 对于不明确的方面：
       - 基于上下文和行业惯例做出合理假设
       - 仅在以下情况才使用 [NEEDS CLARIFICATION: specific question] 标记：
         - 该选择会显著影响功能范围或用户体验
         - 存在多种合理解释且会导致不同后果
         - 没有合理的默认值可用
       - **限制：最多 3 个 [NEEDS CLARIFICATION] 标记**
       - 按影响排序澄清优先级：范围 > 安全/隐私 > 用户体验 > 技术细节
    4. 填写“用户场景与测试”部分
       如果无法确定主要用户流程：报错 "Cannot determine user scenarios"
    5. 生成功能需求
       每条需求都必须可测试
       对未指定的细节使用合理默认值（并在假设部分记录）
    6. 定义成功标准
       创建可衡量且与技术无关的结果
       包括定量指标（时间、性能、容量）和定性衡量（用户满意度、任务完成率）
       每条标准必须可在不依赖实现细节的情况下验证
    7. 确定关键实体（若涉及数据）
    8. 返回：SUCCESS（规格已准备好进入计划阶段）

5. 使用模板结构将规范写入 `SPEC_FILE`，用从功能描述（参数）中得出的具体信息替换占位符，同时保留章节顺序和标题。


6. **规范质量验证**：在写入初始规范后，根据质量标准对其进行验证：

   a. **创建规范质量检查表**：在 `FEATURE_DIR/checklists/requirements.md` 生成一个核对清单文件，使用检查表模板结构并包含如下验证项：

      ```markdown
      # 规范质量检查表： [FEATURE NAME]
      
      **目的**：在进入计划阶段前验证规范的完整性和质量
      **创建时间**： [DATE]
      **功能**： [链接到 spec.md]
      
      ## 内容质量
      
      - [ ] 无实现细节（语言、框架、API）
      - [ ] 聚焦于用户价值和业务需求
      - [ ] 面向非技术干系人编写
      - [ ] 所有必填章节已完成
      
      ## 需求完整性
      
      - [ ] 无 [NEEDS CLARIFICATION] 标记残留
      - [ ] 需求可测试且无歧义
      - [ ] 成功标准可衡量
      - [ ] 成功标准与技术无关（无实现细节）
      - [ ] 所有验收场景已定义
      - [ ] 已识别边界情况
      - [ ] 范围已明确界定
      - [ ] 已识别依赖与假设
      
      ## 功能准备就绪度
      
      - [ ] 所有功能需求具有清晰的验收标准
      - [ ] 用户场景覆盖主要流程
      - [ ] 功能满足成功标准中定义的可衡量结果
      - [ ] 规范中没有泄露实现细节
      
      ## 备注
      
      - 标记为未完成的项在运行 `/speckit.clarify` 或 `/speckit.plan` 之前需要更新规范
      ```

   b. **运行验证检查**：根据每个检查项审查规范：
      - 对每项判断通过或失败
      - 记录发现的具体问题（引用相关规范段落）

   c. **处理验证结果**：

      - **如果所有项通过**：将检查表标记为完成并继续下一步

      - **如果有项失败（不包括 [NEEDS CLARIFICATION]）**：
        1. 列出失败项及具体问题
        2. 更新规范以处理每个问题
        3. 重新运行验证，直到所有项通过（最多 3 次迭代）
        4. 若 3 次迭代后仍未通过，将剩余问题记录在检查表备注中并警告用户

      - **如果仍有 [NEEDS CLARIFICATION] 标记**：
        1. 从规范中提取所有 [NEEDS CLARIFICATION: ...] 标记
        2. **数量限制**：如果超过 3 个标记，仅保留最关键的 3 个（按范围/安全/UX 影响排序），其余做合理猜测
        3. 对于每个需澄清的问题（最多 3 个），以以下格式向用户呈现选项：

           ```markdown
           ## 问题 [N]： [主题]
           
           **上下文**： [引用相关规范段落]
           
           **我们需要知道的是**： [来自 NEEDS CLARIFICATION 标记的具体问题]
           
           **建议答案**：
           
           | Option | Answer | Implications |
           |--------|--------|--------------|
           | A      | [第一个建议答案] | [对功能的影响] |
           | B      | [第二个建议答案] | [对功能的影响] |
           | C      | [第三个建议答案] | [对功能的影响] |
           | Custom | 提供自定义答案 | [说明如何提供自定义输入] |
           
           **你的选择**： _[等待用户回复]_
           ```

        4. **关键 - 表格格式**：确保 Markdown 表格格式正确：
           - 管道符两侧使用一致间隔
           - 每个单元格内容两侧留空格：`| Content |` 而非 `|Content|`
           - 表头分隔行至少使用 3 个短横线：`|--------|`
           - 在 Markdown 预览中检查表格渲染是否正常
        5. 问题按顺序编号（Q1、Q2、Q3，最多 3 个）
        6. 在等待回复前一次性呈现所有问题
        7. 等待用户为所有问题给出选择（例如："Q1: A, Q2: Custom - [细节], Q3: B"）
        8. 将规范中的每个 [NEEDS CLARIFICATION] 标记替换为用户选择或提供的答案
        9. 在所有澄清完成后重新运行验证

   d. **更新检查表**：在每次验证迭代后，更新检查表文件以反映当前的通过/失败状态

7. Report completion with branch name, spec file path, checklist results, and readiness for the next phase (`/speckit.clarify` or `/speckit.plan`).

**NOTE:** The script creates and checks out the new branch and initializes the spec file before writing.

## General Guidelines

## Quick Guidelines

- Focus on **WHAT** users need and **WHY**.
- Avoid HOW to implement (no tech stack, APIs, code structure).
- Written for business stakeholders, not developers.
- DO NOT create any checklists that are embedded in the spec. That will be a separate command.

### Section Requirements

- **Mandatory sections**: Must be completed for every feature
- **Optional sections**: Include only when relevant to the feature
- When a section doesn't apply, remove it entirely (don't leave as "N/A")

### For AI Generation

When creating this spec from a user prompt:

1. **Make informed guesses**: Use context, industry standards, and common patterns to fill gaps
2. **Document assumptions**: Record reasonable defaults in the Assumptions section
3. **Limit clarifications**: Maximum 3 [NEEDS CLARIFICATION] markers - use only for critical decisions that:
   - Significantly impact feature scope or user experience
   - Have multiple reasonable interpretations with different implications
   - Lack any reasonable default
4. **Prioritize clarifications**: scope > security/privacy > user experience > technical details
5. **Think like a tester**: Every vague requirement should fail the "testable and unambiguous" checklist item
6. **Common areas needing clarification** (only if no reasonable default exists):
   - Feature scope and boundaries (include/exclude specific use cases)
   - User types and permissions (if multiple conflicting interpretations possible)
   - Security/compliance requirements (when legally/financially significant)

**Examples of reasonable defaults** (don't ask about these):

- Data retention: Industry-standard practices for the domain
- Performance targets: Standard web/mobile app expectations unless specified
- Error handling: User-friendly messages with appropriate fallbacks
- Authentication method: Standard session-based or OAuth2 for web apps
- Integration patterns: RESTful APIs unless specified otherwise

### Success Criteria Guidelines

Success criteria must be:

1. **Measurable**: Include specific metrics (time, percentage, count, rate)
2. **Technology-agnostic**: No mention of frameworks, languages, databases, or tools
3. **User-focused**: Describe outcomes from user/business perspective, not system internals
4. **Verifiable**: Can be tested/validated without knowing implementation details

**Good examples**:

- "Users can complete checkout in under 3 minutes"
- "System supports 10,000 concurrent users"
- "95% of searches return results in under 1 second"
- "Task completion rate improves by 40%"

**Bad examples** (implementation-focused):

- "API response time is under 200ms" (too technical, use "Users see results instantly")
- "Database can handle 1000 TPS" (implementation detail, use user-facing metric)
- "React components render efficiently" (framework-specific)
- "Redis cache hit rate above 80%" (technology-specific)
