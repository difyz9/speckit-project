---
description: 通过处理和执行 tasks.md 中定义的所有任务来执行实施计划
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 概述

1. 从仓库根目录运行 `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` 并解析 FEATURE_DIR 和 AVAILABLE_DOCS 列表。所有路径必须是绝对路径。对于参数中的单引号（如 "I'm Groot"），使用转义语法：例如 'I'\''m Groot'（或者如果可能的话使用双引号："I'm Groot"）。

2. **检查检查表状态**（如果存在 FEATURE_DIR/checklists/）：
    - 扫描 `checklists/` 目录下的所有检查表文件
    - 对每个检查表统计：
       - 总项数：匹配 `- [ ]`、`- [X]` 或 `- [x]` 的所有行
       - 已完成项：匹配 `- [X]` 或 `- [x]` 的行
       - 未完成项：匹配 `- [ ]` 的行
    - 生成状态表：

       ```text
       | Checklist | Total | Completed | Incomplete | Status |
       |-----------|-------|-----------|------------|--------|
       | ux.md     | 12    | 12        | 0          | ✓ PASS |
       | test.md   | 8     | 5         | 3          | ✗ FAIL |
       | security.md | 6   | 6         | 0          | ✓ PASS |
       ```

    - 计算整体状态：
       - **PASS**：所有检查表的未完成项为 0
       - **FAIL**：一个或多个检查表存在未完成项

    - **如果有任何检查表未完成**：
       - 显示包含未完成项计数的表格
       - **停止执行** 并询问："Some checklists are incomplete. Do you want to proceed with implementation anyway? (yes/no)"
       - 在继续之前等待用户响应
       - 若用户回答 "no"、"wait" 或 "stop"，则中止执行
       - 若用户回答 "yes"、"proceed" 或 "continue"，则继续执行步骤 3

    - **如果所有检查表已完成**：
       - 显示所有检查表通过的表格
       - 自动继续执行步骤 3

3. 加载并分析实现上下文：
   - **必需**：阅读 `tasks.md` 以获取完整任务列表和执行计划
   - **必需**：阅读 `plan.md` 以了解技术栈、架构和文件结构
   - **如存在**：阅读 `data-model.md` 以了解实体和关系
   - **如存在**：查看 `contracts/` 获取 API 规范和测试要求
   - **如存在**：阅读 `research.md` 获取技术决策与约束
   - **如存在**：阅读 `quickstart.md` 了解集成场景

4. **项目设置校验**：
   - **必需**：根据实际项目设置创建/校验忽略文件（ignore 文件）：

   **检测与创建逻辑**：
   - 运行以下命令以判断仓库是否为 git 仓库（若是则创建/校验 .gitignore）：

      ```sh
      git rev-parse --git-dir 2>/dev/null
      ```

   - 若存在 Dockerfile* 或 plan.md 中包含 Docker → 创建/校验 `.dockerignore`
   - 若存在 `.eslintrc*` → 创建/校验 `.eslintignore`
   - 若存在 `eslint.config.*` → 确保配置中的 `ignores` 条目覆盖所需模式
   - 若存在 `.prettierrc*` → 创建/校验 `.prettierignore`
   - 若存在 `.npmrc` 或 `package.json` → 在需要发布时创建/校验 `.npmignore`
   - 若存在 Terraform 文件（*.tf）→ 创建/校验 `.terraformignore`
   - 若存在 Helm chart → 创建/校验 `.helmignore`

   **若忽略文件已存在**：验证其包含关键模式，仅追加缺失的关键模式
   **若忽略文件缺失**：根据检测到的技术栈创建包含完整模式集的忽略文件

   **按技术栈的常见模式**（基于 plan.md 的技术栈）：
   - **Node.js/JavaScript/TypeScript**：`node_modules/`、`dist/`、`build/`、`*.log`、`.env*`
   - **Python**：`__pycache__/`、`*.pyc`、`.venv/`、`venv/`、`dist/`、`*.egg-info/`
   - **Java**：`target/`、`*.class`、`*.jar`、`.gradle/`、`build/`
   - **C#/.NET**：`bin/`、`obj/`、`*.user`、`*.suo`、`packages/`
   - **Go**：`*.exe`、`*.test`、`vendor/`、`*.out`
   - **Ruby**：`.bundle/`、`log/`、`tmp/`、`*.gem`、`vendor/bundle/`
   - **PHP**：`vendor/`、`*.log`、`*.cache`、`*.env`
   - **Rust**：`target/`、`debug/`、`release/`、`*.rs.bk`、`*.rlib`、`*.prof*`、`.idea/`、`*.log`、`.env*`
   - **Kotlin**：`build/`、`out/`、`.gradle/`、`.idea/`、`*.class`、`*.jar`、`*.iml`、`*.log`、`.env*`
   - **C++**：`build/`、`bin/`、`obj/`、`out/`、`*.o`、`*.so`、`*.a`、`*.exe`、`*.dll`、`.idea/`、`*.log`、`.env*`
   - **C**：`build/`、`bin/`、`obj/`、`out/`、`*.o`、`*.a`、`*.so`、`*.exe`、`Makefile`、`config.log`、`.idea/`、`*.log`、`.env*`
   - **Swift**：`.build/`、`DerivedData/`、`*.swiftpm/`、`Packages/`
   - **R**：`.Rproj.user/`、`.Rhistory`、`.RData`、`.Ruserdata`、`*.Rproj`、`packrat/`、`renv/`
   - **通用**：`.DS_Store`、`Thumbs.db`、`*.tmp`、`*.swp`、`.vscode/`、`.idea/`

   **工具相关的特定模式**：
   - **Docker**：`node_modules/`、`.git/`、`Dockerfile*`、`.dockerignore`、`*.log*`、`.env*`、`coverage/`
   - **ESLint**：`node_modules/`、`dist/`、`build/`、`coverage/`、`*.min.js`
   - **Prettier**：`node_modules/`、`dist/`、`build/`、`coverage/`、`package-lock.json`、`yarn.lock`、`pnpm-lock.yaml`
   - **Terraform**：`.terraform/`、`*.tfstate*`、`*.tfvars`、`.terraform.lock.hcl`
   - **Kubernetes/k8s**：`*.secret.yaml`、`secrets/`、`.kube/`、`kubeconfig*`、`*.key`、`*.crt`

5. 解析 `tasks.md` 结构并提取：
   - **任务阶段**：Setup、Tests、Core、Integration、Polish
   - **任务依赖**：顺序执行与并行执行规则
   - **任务详情**：ID、描述、文件路径、并行标记 [P]
   - **执行流程**：顺序与依赖要求

6. 根据任务计划执行实现：
   - **按阶段执行**：完成每个阶段后再进入下一个阶段
   - **遵守依赖**：按顺序运行顺序任务，标记为并行 [P] 的任务可以同时运行
   - **遵循 TDD 思想**：在对应实现任务之前先执行测试任务
   - **基于文件的协调**：影响相同文件的任务必须按顺序执行
   - **验证检查点**：在继续前验证每个阶段的完成情况

7. 实施执行规则：
   - **先初始化**：初始化项目结构、依赖和配置
   - **先测试后代码**：如需为 contracts、实体或集成场景编写测试，请先完成测试任务
   - **核心开发**：实现模型、服务、CLI 命令、接口端点
   - **集成工作**：数据库连接、中间件、日志、外部服务集成
   - **打磨与验证**：单元测试、性能优化、文档完善

8. 进度跟踪与错误处理：
   - 在每个已完成任务后报告进度
   - 若任何非并行任务失败，则停止执行
   - 对于并行任务 [P]：继续执行成功的任务，并报告失败的任务
   - 提供带上下文的清晰错误信息以便排查
   - 如果无法继续实现，提出后续可行步骤建议
   - **重要**：已完成的任务请在任务文件中将其标记为 `[X]`。

9. 完成验证：
   - 验证所有必需任务均已完成
   - 检查已实现的功能是否与原始规范一致
   - 验证测试通过且覆盖率满足要求
   - 确认实现遵循技术计划
   - 提交包含已完成工作摘要的最终状态报告

Note: This command assumes a complete task breakdown exists in tasks.md. If tasks are incomplete or missing, suggest running `/speckit.tasks` first to regenerate the task list.
