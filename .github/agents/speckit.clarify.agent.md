---
description: 通过最多提出 5 个高度针对性的澄清问题，识别当前功能规范中的不明确区域，并将答案编码回规范中。
handoffs: 
  - label: 构建技术计划
    agent: speckit.plan
    prompt: 为规范创建计划。我正在构建...
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 大纲

目标：检测并减少当前功能规范中的模糊性或缺失的决策点，并直接在规范文件中记录澄清。

注意：此澄清工作流预期在调用 `/speckit.plan` **之前**运行（并完成）。如果用户明确表示他们跳过澄清（例如，探索性尖峰），您可以继续，但必须警告下游返工风险增加。

执行步骤：

1. 从仓库根目录**仅运行一次**命令 `.specify/scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly`（等同于 `--json --paths-only` / `-Json -PathsOnly` 模式）。解析最小 JSON 有效载荷字段：
   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - （可选）捕获 `IMPL_PLAN`、`TASKS` 以便后续链式流程使用。
   - 若 JSON 解析失败，则中止并提示用户重新运行 `/speckit.specify` 或检查特性分支环境。
   - 对于类似 "I'm Groot" 的参数中的单引号，使用转义语法：例如 'I'\''m Groot'（或尽量使用双引号："I'm Groot"）。

2. Load the current spec file. Perform a structured ambiguity & coverage scan using this taxonomy. For each category, mark status: Clear / Partial / Missing. Produce an internal coverage map used for prioritization (do not output raw map unless no questions will be asked).

   Functional Scope & Behavior:
   - Core user goals & success criteria
   - Explicit out-of-scope declarations
   - User roles / personas differentiation

   Domain & Data Model:
   - Entities, attributes, relationships
   - Identity & uniqueness rules
   - Lifecycle/state transitions
   - Data volume / scale assumptions

   Interaction & UX Flow:
   - Critical user journeys / sequences
   - Error/empty/loading states
   - Accessibility or localization notes

   Non-Functional Quality Attributes:
   - Performance (latency, throughput targets)
   - Scalability (horizontal/vertical, limits)
   - Reliability & availability (uptime, recovery expectations)
   - Observability (logging, metrics, tracing signals)
   - Security & privacy (authN/Z, data protection, threat assumptions)
   - Compliance / regulatory constraints (if any)

   Integration & External Dependencies:
   - External services/APIs and failure modes
   - Data import/export formats
   - Protocol/versioning assumptions

   Edge Cases & Failure Handling:
   - Negative scenarios
   - Rate limiting / throttling
   - Conflict resolution (e.g., concurrent edits)

   Constraints & Tradeoffs:
   - Technical constraints (language, storage, hosting)
   - Explicit tradeoffs or rejected alternatives

   Terminology & Consistency:
   - Canonical glossary terms
   - Avoided synonyms / deprecated terms

   Completion Signals:
   - Acceptance criteria testability
   - Measurable Definition of Done style indicators

   Misc / Placeholders:
   - TODO markers / unresolved decisions
   - Ambiguous adjectives ("robust", "intuitive") lacking quantification

   For each category with Partial or Missing status, add a candidate question opportunity unless:
   - Clarification would not materially change implementation or validation strategy
   - Information is better deferred to planning phase (note internally)

3. 在内部生成一个优先队列的候选澄清问题列表（最多 5 个）。不要一次性输出所有问题。适用以下约束：
    - 整个会话中最多 10 个问题（包含所有交互）
    - 每个问题必须可通过以下任一方式回答：
       - 简短的多选项选择（2–5 个互斥选项），或
       - 一个词或短语的答案（明确限制为 "答案 ≤5 个词"）
    - 仅包含那些其回答会实质性影响架构、数据建模、任务分解、测试设计、UX 行为、可运维性或合规验证的问题
    - 确保类别覆盖的平衡：优先覆盖影响最大且未解决的类别；避免在单一高影响区域未解决时询问两个低影响问题
    - 排除已回答的问题、琐碎的样式偏好或计划层面的执行细节（除非阻塞正确性）
    - 优先提出能降低下游返工风险或防止不一致验收测试的澄清
    - 如果未解决的类别超过 5 个，则按（影响 * 不确定性）启发式选择前 5 个

4. 交互式顺序问答循环：
    - 每次仅呈现 **一个** 问题。
    - 对于多选问题：
       - **分析所有选项**，并基于以下因素确定 **最合适的选项**：
          - 项目类型的最佳实践
          - 相似实现中的常见模式
          - 风险降低（安全、性能、可维护性）
          - 与规范中可见的明确项目目标或约束的一致性
       - 将你 **推荐的选项显著呈现** 在顶部并给出简短理由（1-2 句）说明为何这是最佳选择
       - 格式示例：`**Recommended:** Option [X] - <reasoning>`
       - 然后以 Markdown 表格展示所有选项：

       | Option | Description |
       |--------|-------------|
       | A | <Option A description> |
       | B | <Option B description> |
       | C | <Option C description> (如需可扩展到 D/E，最多 5 个选项) |
       | Short | 提供不同的简短答案（<=5 个词）（仅当允许自由格式时包含） |

       - 在表格之后加入：`You can reply with the option letter (e.g., "A"), accept the recommendation by saying "yes" or "recommended", or provide your own short answer.`
    - 对于简短回答类型（无离散选项）：
       - 基于最佳实践和上下文提供你的 **建议答案**。
       - 格式示例：`**Suggested:** <your proposed answer> - <brief reasoning>`
       - 然后输出：`Format: Short answer (<=5 words). You can accept the suggestion by saying "yes" or "suggested", or provide your own answer.`
    - 在用户回答后：
       - 若用户回复 "yes"、"recommended" 或 "suggested"，则将之前的推荐/建议作为最终答案
       - 否则，验证答案是否匹配某个选项或符合 <=5 个词的约束
       - 若回答含糊，要求快速澄清（该澄清仍计为当前问题，不计为新问题），然后再继续
       - 一旦满意，将答案记录在工作内存中（暂不写入磁盘），并继续下一个排队问题
    - 停止提问的条件：
       - 所有关键歧义提前解决（剩余队列项变为非必要），或
       - 用户发出完成信号（"done"、"good"、"no more"），或
       - 已提出 5 个问题
    - 切勿提前泄露后续队列中的问题
    - 若起始时没有有效问题，立即报告无需正式澄清的关键歧义

5. 每个被接受的答案后的集成（增量更新方法）：
    - 保持规范的内存表示（会话开始时加载一次）以及原始文件内容
    - 对于本次会话中的第一个集成答案：
       - 确保存在 `## Clarifications` 节（如缺失，则在规范模板的最高级上下文/概述部分之后创建）
       - 在其下为今天创建（若不存在）一个 `### Session YYYY-MM-DD` 子标题
    - 在接受后立即追加一行要点：`- Q: <question> → A: <final answer>`
    - 然后将澄清立即应用到最合适的章节：
       - 功能性歧义 → 更新或新增 Functional Requirements 下的要点
       - 用户交互 / 角色区分 → 更新 User Stories 或 Actors 小节（若存在），加入澄清的角色、约束或场景
       - 数据结构 / 实体 → 更新 Data Model（添加字段、类型、关系），保留顺序并简要记录新增约束
       - 非功能约束 → 在 Non-Functional / Quality Attributes 中添加或修改可衡量的标准（将模糊形容词转换为指标或明确目标）
       - 边界情况 / 负面流程 → 在 Edge Cases / Error Handling 下新增要点（或根据模板创建该小节）
       - 术语冲突 → 在规范中统一术语；仅在必要时保留原有称呼并注明 `(formerly referred to as "X")`
    - 若澄清使之前的模糊语句失效，应替换该语句而非追加；不应保留相互矛盾的废弃文本
    - 在每次集成后保存规范文件以降低上下文丢失风险（原子性覆盖写入）
    - 保持格式：不要重新排序无关章节；保持标题层级结构
    - 每次插入的澄清应简洁且可测试（避免叙述性偏移）

6. Validation (performed after EACH write plus final pass):
   - Clarifications session contains exactly one bullet per accepted answer (no duplicates).
   - Total asked (accepted) questions ≤ 5.
   - Updated sections contain no lingering vague placeholders the new answer was meant to resolve.
   - No contradictory earlier statement remains (scan for now-invalid alternative choices removed).
   - Markdown structure valid; only allowed new headings: `## Clarifications`, `### Session YYYY-MM-DD`.
   - Terminology consistency: same canonical term used across all updated sections.

7. Write the updated spec back to `FEATURE_SPEC`.

8. Report completion (after questioning loop ends or early termination):
   - Number of questions asked & answered.
   - Path to updated spec.
   - Sections touched (list names).
   - Coverage summary table listing each taxonomy category with Status: Resolved (was Partial/Missing and addressed), Deferred (exceeds question quota or better suited for planning), Clear (already sufficient), Outstanding (still Partial/Missing but low impact).
   - If any Outstanding or Deferred remain, recommend whether to proceed to `/speckit.plan` or run `/speckit.clarify` again later post-plan.
   - Suggested next command.

Behavior rules:

- If no meaningful ambiguities found (or all potential questions would be low-impact), respond: "No critical ambiguities detected worth formal clarification." and suggest proceeding.
- If spec file missing, instruct user to run `/speckit.specify` first (do not create a new spec here).
- Never exceed 5 total asked questions (clarification retries for a single question do not count as new questions).
- Avoid speculative tech stack questions unless the absence blocks functional clarity.
- Respect user early termination signals ("stop", "done", "proceed").
- If no questions asked due to full coverage, output a compact coverage summary (all categories Clear) then suggest advancing.
- If quota reached with unresolved high-impact categories remaining, explicitly flag them under Deferred with rationale.

Context for prioritization: $ARGUMENTS
