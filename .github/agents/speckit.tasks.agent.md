---
description: 基于可用的设计工件为功能生成可操作的、依赖排序的 tasks.md。
handoffs: 
  - label: Analyze For Consistency
    agent: speckit.analyze
    prompt: Run a project analysis for consistency
    send: true
  - label: Implement Project
    agent: speckit.implement
    prompt: Start the implementation in phases
    send: true
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 概述

1. **设置**：从仓库根目录运行 `.specify/scripts/powershell/check-prerequisites.ps1 -Json` 并解析 FEATURE_DIR 和 AVAILABLE_DOCS 列表。所有路径必须是绝对路径。对于参数中的单引号（如 "I'm Groot"），使用转义语法：例如 'I'\''m Groot'（或者如果可能的话使用双引号："I'm Groot"）。

2. **加载设计文档**：从 FEATURE_DIR 读取：
   - **必需**：plan.md（技术栈、库、结构）、spec.md（带优先级的用户故事）
   - **可选**：data-model.md（实体）、contracts/（API 端点）、research.md（决策）、quickstart.md（测试场景）
   - 注意：并非所有项目都有所有文档。根据可用的内容生成任务。

3. **执行任务生成工作流**：
   - 加载 plan.md 并提取技术栈、库、项目结构
   - 加载 spec.md 并提取带优先级的用户故事（P1、P2、P3 等）
   - 如果 data-model.md 存在：提取实体并映射到用户故事
   - 如果 contracts/ 存在：将端点映射到用户故事
   - 如果 research.md 存在：提取用于设置任务的决策
   - 生成按用户故事组织的任务（参见下面的任务生成规则）
   - 生成显示用户故事完成顺序的依赖关系图
   - 为每个用户故事创建并行执行示例
   - 验证任务完整性（每个用户故事都有所需的所有任务，可独立测试）

4. **生成 tasks.md**：使用 `.specify/templates/tasks-template.md` 作为结构，填充：
   - 来自 plan.md 的正确功能名称
   - 阶段 1：设置任务（项目初始化）
   - 阶段 2：基础任务（所有用户故事的阻塞先决条件）
   - 阶段 3+：每个用户故事一个阶段（按 spec.md 的优先级顺序）
   - 每个阶段包括：故事目标、独立测试标准、测试（如果请求）、实施任务
   - 最终阶段：完善和跨领域关注点
   - 所有任务必须遵循严格的检查清单格式（参见下面的任务生成规则）
   - 每个任务的清晰文件路径
   - 显示故事完成顺序的依赖关系部分
   - 每个故事的并行执行示例
   - 实施策略部分（MVP 优先，增量交付）

5. **报告**：输出生成的 tasks.md 路径和摘要：
   - 总任务数
   - 每个用户故事的任务数
   - 识别的并行机会
   - 每个故事的独立测试标准
   - 建议的 MVP 范围（通常只是用户故事 1）
   - 格式验证：确认所有任务都遵循检查清单格式（复选框、ID、标签、文件路径）

任务生成上下文：$ARGUMENTS

tasks.md 应该是立即可执行的 - 每个任务必须足够具体，以便 LLM 无需额外上下文即可完成。

## 任务生成规则

**关键**：任务必须按用户故事组织，以实现独立的实施和测试。

**测试是可选的**：仅在功能规范中明确请求或用户请求 TDD 方法时生成测试任务。

### Checklist Format (REQUIRED)

Every task MUST strictly follow this format:

```text
- [ ] [TaskID] [P?] [Story?] Description with file path
```

**Format Components**:

1. **Checkbox**: ALWAYS start with `- [ ]` (markdown checkbox)
2. **Task ID**: Sequential number (T001, T002, T003...) in execution order
3. **[P] marker**: Include ONLY if task is parallelizable (different files, no dependencies on incomplete tasks)
4. **[Story] label**: REQUIRED for user story phase tasks only
   - Format: [US1], [US2], [US3], etc. (maps to user stories from spec.md)
   - Setup phase: NO story label
   - Foundational phase: NO story label  
   - User Story phases: MUST have story label
   - Polish phase: NO story label
5. **Description**: Clear action with exact file path

**Examples**:

- ✅ CORRECT: `- [ ] T001 Create project structure per implementation plan`
- ✅ CORRECT: `- [ ] T005 [P] Implement authentication middleware in src/middleware/auth.py`
- ✅ CORRECT: `- [ ] T012 [P] [US1] Create User model in src/models/user.py`
- ✅ CORRECT: `- [ ] T014 [US1] Implement UserService in src/services/user_service.py`
- ❌ WRONG: `- [ ] Create User model` (missing ID and Story label)
- ❌ WRONG: `T001 [US1] Create model` (missing checkbox)
- ❌ WRONG: `- [ ] [US1] Create User model` (missing Task ID)
- ❌ WRONG: `- [ ] T001 [US1] Create model` (missing file path)

### Task Organization

1. **From User Stories (spec.md)** - PRIMARY ORGANIZATION:
   - Each user story (P1, P2, P3...) gets its own phase
   - Map all related components to their story:
     - Models needed for that story
     - Services needed for that story
     - Endpoints/UI needed for that story
     - If tests requested: Tests specific to that story
   - Mark story dependencies (most stories should be independent)

2. **From Contracts**:
   - Map each contract/endpoint → to the user story it serves
   - If tests requested: Each contract → contract test task [P] before implementation in that story's phase

3. **From Data Model**:
   - Map each entity to the user story(ies) that need it
   - If entity serves multiple stories: Put in earliest story or Setup phase
   - Relationships → service layer tasks in appropriate story phase

4. **From Setup/Infrastructure**:
   - Shared infrastructure → Setup phase (Phase 1)
   - Foundational/blocking tasks → Foundational phase (Phase 2)
   - Story-specific setup → within that story's phase

### Phase Structure

- **Phase 1**: Setup (project initialization)
- **Phase 2**: Foundational (blocking prerequisites - MUST complete before user stories)
- **Phase 3+**: User Stories in priority order (P1, P2, P3...)
  - Within each story: Tests (if requested) → Models → Services → Endpoints → Integration
  - Each phase should be a complete, independently testable increment
- **Final Phase**: Polish & Cross-Cutting Concerns
