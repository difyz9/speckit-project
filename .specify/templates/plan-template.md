# 实施计划： [FEATURE]

**分支**：`[###-feature-name]` | **日期**： [DATE] | **规范**： [link]
**输入**： 来自 `/specs/[###-feature-name]/spec.md` 的功能规范

**说明**：此模板由 `/speckit.plan` 命令填充。执行工作流请参见 `.specify/templates/commands/plan.md`。

## 摘要

[从功能规范中提取：主要需求 + 来自研究的技术方案]

## 技术上下文

<!--
  请在本节中替换为项目的技术细节。
  本处结构仅为指导迭代过程而给出的建议性布局。
-->

**语言/版本**： [例如 Python 3.11、Swift 5.9、Rust 1.75 或 NEEDS CLARIFICATION]  
**主要依赖**： [例如 FastAPI、UIKit、LLVM 或 NEEDS CLARIFICATION]  
**存储**： [如适用，例如 PostgreSQL、CoreData、文件或 N/A]  
**测试**： [例如 pytest、XCTest、cargo test 或 NEEDS CLARIFICATION]  
**目标平台**： [例如 Linux 服务器、iOS 15+、WASM 或 NEEDS CLARIFICATION]
**项目类型**： [single/web/mobile - 决定源码结构]  
**性能目标**： [领域相关，例如 1000 req/s、10k lines/sec、60 fps 或 NEEDS CLARIFICATION]  
**约束**： [领域相关，例如 <200ms p95、<100MB 内存、离线可用或 NEEDS CLARIFICATION]  
**规模/范围**： [领域相关，例如 10k 用户、1M 行代码、50 个屏幕或 NEEDS CLARIFICATION]

## 章程检查

*门控：在第 0 阶段研究之前必须通过。设计阶段（第 1 阶段）后需重新检查。*

[门控根据章程文件确定]

## 项目结构

### 文档（本功能）

```text
specs/[###-feature]/
├── plan.md              # 本文件（/speckit.plan 命令输出）
├── research.md          # 第 0 阶段输出（/speckit.plan 命令）
├── data-model.md        # 第 1 阶段输出（/speckit.plan 命令）
├── quickstart.md        # 第 1 阶段输出（/speckit.plan 命令）
├── contracts/           # 第 1 阶段输出（/speckit.plan 命令）
└── tasks.md             # 第 2 阶段输出（/speckit.tasks 命令 - 不是由 /speckit.plan 创建）
```

### 源代码（仓库根）
<!--
  请替换以下占位树为该功能的具体布局。
  删除未使用的选项并用实际路径扩展所选结构（例如 apps/admin、packages/something）。交付的计划中不得包含选项标签。
-->

```text
# [如果未使用则删除] 选项 1：单一项目（默认）
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [如果未使用则删除] 选项 2：Web 应用（当检测到 "frontend" + "backend" 时）
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [如果未使用则删除] 选项 3：移动端 + API（当检测到 "iOS/Android" 时）
api/
└── [同上端对应的 backend 结构]

ios/ 或 android/
└── [平台特定结构：功能模块、UI 流程、平台测试]
```

**结构决策**： [记录所选结构并引用上文中列出的实际目录]

## 复杂性跟踪

> **仅在章程检查存在必须证明理由的违规时填写**

| 违规 | 需要说明 | 被拒绝的更简单替代方案 |
|-----------|------------|-------------------------------------|
| [例如，第 4 个项目] | [当前需求] | [为何 3 个项目不足] |
| [例如，仓库模式] | [具体问题] | [为何直接访问数据库不足] |
