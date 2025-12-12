# Spec-Kit 命令快速参考指南

## 📚 命令总览

| 命令 | 用途一句话 | 使用时机 |
|------|----------|---------|
| `/speckit.constitution` | 定义项目的核心原则和开发规范 | 项目开始时（可选） |
| `/speckit.specify` | 将功能需求转化为清晰的规范文档 | 开发新功能的第一步 |
| `/speckit.clarify` | 解决规范中的模糊和歧义问题 | 规范化后、规划前（可选） |
| `/speckit.plan` | 制定功能的技术实现方案 | 规范明确后 |
| `/speckit.tasks` | 将技术方案分解为可执行的任务清单 | 技术方案完成后 |
| `/speckit.analyze` | 检查规范、计划、任务的一致性 | 任务分解后、实现前（可选） |
| `/speckit.checklist` | 生成需求质量验证清单 | 任何阶段验证质量 |
| `/speckit.implement` | 按任务清单逐步实现功能代码 | 所有准备工作完成后 |

---

## 🎯 核心工作流（必需）

### 1️⃣ `/speckit.specify` - 规范化
**用途**：将功能需求转化为清晰的规范文档

**何时使用**：开发新功能的第一步

**输入示例**：
```
/speckit.specify

开发一个用户注册功能。用户可以通过邮箱和密码注册账号，
系统需要验证邮箱格式，密码强度至少8位包含字母和数字。
```

**输出**：
- `spec.md` - 功能规范文档
- `checklists/requirements.md` - 质量检查清单

---

### 2️⃣ `/speckit.plan` - 技术规划
**用途**：制定功能的技术实现方案

**何时使用**：规范明确后

**输入示例**：
```
/speckit.plan
```

**输出**：
- `plan.md` - 技术实现计划
- `data-model.md` - 数据模型
- `contracts/` - API 接口定义
- `research.md` - 技术决策

---

### 3️⃣ `/speckit.tasks` - 任务分解
**用途**：将技术方案分解为可执行的任务清单

**何时使用**：技术方案完成后

**输入示例**：
```
/speckit.tasks
```

**输出**：
- `tasks.md` - 详细任务清单

---

### 4️⃣ `/speckit.implement` - 实现
**用途**：按任务清单逐步实现功能代码

**何时使用**：所有准备工作完成后

**输入示例**：
```
/speckit.implement
```

**效果**：
- 按阶段执行任务
- 遵循 TDD 原则
- 自动跟踪进度

---

## 🔧 辅助命令（可选）

### 🅰️ `/speckit.constitution` - 项目原则
**用途**：定义项目的核心原则和开发规范

**何时使用**：项目开始时（可选）

**输入示例**：
```
/speckit.constitution

我们的项目遵循：
1. 测试驱动开发（TDD）是强制的
2. 代码必须有详细的文档注释
3. 使用 TypeScript + React
```

**输出**：
- `.specify/memory/constitution.md` - 项目宪章

---

### 🅱️ `/speckit.clarify` - 需求澄清
**用途**：解决规范中的模糊和歧义问题

**何时使用**：规范化后、规划前（可选）

**输入示例**：
```
/speckit.clarify
```

**效果**：
- 识别需求中的不明确之处
- 提出结构化问题
- 更新规范文档

---

### 🅲 `/speckit.analyze` - 一致性分析
**用途**：检查规范、计划、任务的一致性

**何时使用**：任务分解后、实现前（可选）

**输入示例**：
```
/speckit.analyze
```

**输出**：
- 一致性分析报告
- 问题清单和修复建议

---

### 🅳 `/speckit.checklist` - 质量检查
**用途**：生成需求质量验证清单

**何时使用**：任何阶段验证质量

**输入示例**：
```
/speckit.checklist

为登录功能创建安全检查清单
```

**输出**：
- `checklists/[名称].md` - 定制化检查清单

---

## 🚀 完整流程示例

```bash
# 步骤 0（可选）：定义项目原则
/speckit.constitution
我们的项目遵循 TDD、RESTful API 设计

# 步骤 1：创建功能规范 ✅ 必需
/speckit.specify
开发待办事项管理功能...

# 步骤 2（可选）：澄清需求
/speckit.clarify

# 步骤 3：制定技术方案 ✅ 必需
/speckit.plan

# 步骤 4（可选）：生成检查清单
/speckit.checklist
创建 UX 检查清单

# 步骤 5：分解任务 ✅ 必需
/speckit.tasks

# 步骤 6（可选）：一致性检查
/speckit.analyze

# 步骤 7：开始实现 ✅ 必需
/speckit.implement
```

---

## 💡 使用技巧

### 1. 最小工作流（4 步）
如果时间紧张，只需执行核心 4 步：
1. `/speckit.specify` - 规范化
2. `/speckit.plan` - 技术规划
3. `/speckit.tasks` - 任务分解
4. `/speckit.implement` - 实现

### 2. 质量优先工作流（7 步）
如果追求高质量，建议完整流程：
1. `/speckit.constitution` - 定义原则（可选）
2. `/speckit.specify` - 规范化
3. `/speckit.clarify` - 澄清需求（可选）
4. `/speckit.plan` - 技术规划
5. `/speckit.tasks` - 任务分解
6. `/speckit.analyze` - 一致性检查（可选）
7. `/speckit.implement` - 实现

### 3. 灵活使用
- **检查清单** (`/speckit.checklist`) 可以在任何阶段使用
- **澄清** (`/speckit.clarify`) 可以多次运行
- **分析** (`/speckit.analyze`) 建议在实现前运行

---

## 📊 命令优先级

### 🔴 必需（核心流程）
1. `/speckit.specify` - 规范化
2. `/speckit.plan` - 技术规划
3. `/speckit.tasks` - 任务分解
4. `/speckit.implement` - 实现

### 🟡 推荐（提升质量）
5. `/speckit.clarify` - 需求澄清
6. `/speckit.analyze` - 一致性分析

### 🟢 可选（按需使用）
7. `/speckit.constitution` - 项目原则
8. `/speckit.checklist` - 质量检查

---

## 🎯 何时使用每个命令？

| 场景 | 推荐命令 |
|------|---------|
| 开始新项目 | `/speckit.constitution` |
| 开发新功能 | `/speckit.specify` → `/speckit.plan` → `/speckit.tasks` → `/speckit.implement` |
| 需求不清楚 | `/speckit.clarify` |
| 担心质量 | `/speckit.analyze` + `/speckit.checklist` |
| 快速原型 | `/speckit.specify` → `/speckit.implement`（跳过中间步骤） |
| 团队协作 | 完整流程（所有命令） |

---

**快速查找命令用途，高效使用 Spec-Kit！** 🚀

