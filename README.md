# Spec‑Kit 中文版 🇨🇳

简洁、结构化的规范驱动开发工具包，配合 GitHub Copilot 高效完成“规范 → 计划 → 任务 → 实施”的全流程。

## 目录
- 介绍与优势
- 快速开始
- 工作流与命令
- 项目结构
- 使用示例
- 系统要求
- 贡献与许可
- 相关链接

## 🌟 介绍与优势

Spec‑Kit 通过规范驱动开发（Spec‑Driven Development, SDD）将需求与实现有机衔接：
- 结构化流程：规范 → 计划 → 任务 → 实施
- 技术无关：先明确“是什么/为什么”，再决定“怎么做”
- 迭代细化：分阶段沉淀，不一次性“生成一切”
- Copilot 友好：命令与模板针对 AI 协作优化

## 🚀 快速开始

### 1. 安装 Specify CLI

选择持久化安装（推荐）：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### 2. 初始化项目

在你的项目目录中运行：

```bash
# 创建新项目
specify init my-project --ai copilot

# 或在现有项目中初始化
specify init .  --ai copilot
# 或使用 --here 标志
specify init --here --ai copilot
```

## 🧭 工作流与命令

初始化完成后，在 VS Code 中使用 GitHub Copilot 时，你将可以使用以下 **斜杠命令**：

### 核心命令

| 命令 | 用途 | 说明 |
|------|------|------|
| `/speckit.constitution` | 建立项目原则 | 设定不可协商的项目规则与质量门槛 |
| `/speckit.specify` | 定义需求 | 关注“是什么/为什么”，生成规范文档 |
| `/speckit.plan` | 技术规划 | 关注“怎么做”，输出技术方案与架构 |
| `/speckit.tasks` | 生成任务列表 | 将实施计划分解为可执行的任务 |
| `/speckit.implement` | 执行实施 | 根据计划执行所有任务并构建功能 |

### 辅助命令

| 命令 | 用途 |
|------|------|
| `/speckit.clarify` | 澄清不明确的需求 |
| `/speckit.analyze` | 跨文件一致性与覆盖度分析 |
| `/speckit.checklist` | 生成“需求的单元测试”式质量清单 |

## 📚 使用示例

### 步骤 1：建立项目原则

```
/speckit.constitution 创建关注代码质量、测试标准、用户体验一致性和性能要求的原则
```

### 步骤 2：创建功能规范

```
/speckit.specify 构建一个照片管理应用，可以将照片组织到不同的相册中。相册按日期分组，可以在主页面上通过拖放重新组织。
```

**重要提示**：在这个阶段，尽可能明确地说明你想构建**什么**以及**为什么**，但不要关注技术栈。

### 步骤 3：创建技术实施计划

```
/speckit.plan 应用使用 Vite，尽量减少库的数量。尽可能使用原生 HTML、CSS 和 JavaScript。图片不上传到任何地方，元数据存储在本地存储中。
```

### 步骤 4：分解任务

```
/speckit.tasks
```

这将生成一个详细的任务列表，包含：
- 按用户故事组织的任务
- 依赖关系管理
- 可并行执行的标记
- 具体的文件路径

### 步骤 5：执行实施

```
/speckit.implement
```

Copilot 将按照正确的顺序执行任务，遵循依赖关系和并行执行标记。

## 🎯 关键优势（摘要）

1. 结构化开发：从需求到实施的清晰流程
2. 技术无关：先定义需求，再选择技术栈
3. 迭代细化：多步骤细化，减少返工
4. Copilot 深度集成：专门优化的工作流程

## 📁 项目结构

初始化后，你的项目将包含：

```
.specify/
├── memory/
│   └── constitution.md        # 项目原则
├── scripts/                   # 辅助脚本
├── specs/                     # 功能规范
│   └── 001-feature-name/
│       ├── spec.md           # 功能规范
│       ├── plan.md           # 实施计划
│       └── tasks.md          # 任务分解
└── templates/                 # 模板文件
```

## 🔧 系统要求

- **操作系统**: Linux/macOS/Windows
- **GitHub Copilot**: 在 VS Code 中安装并激活
- **Python**: 3.11+
- **uv**: 用于包管理
- **Git**: 版本控制

## 📖 更多资源

- [完整文档](https://github.github.io/spec-kit/)
- [详细方法论](https://github.com/github/spec-kit/blob/main/spec-driven.md)
- [视频教程](https://www.youtube.com/watch?v=a9eR1xsfvHg)

通过这种规范驱动的开发方式，你可以充分发挥 GitHub Copilot 的能力，更系统、更高质量地构建软件！🚀

---

## 📖 简介

**Spec-Kit** 是 GitHub 开源的规范驱动开发工具包，与 Cursor 等 AI 编码工具深度集成，帮助开发者从需求到实现的全流程开发。

**本项目特点**：
- 🇨🇳 **完整中文化** - 所有命令和模板已汉化
- 📋 **开头一句话** - 每个命令都有简洁的用途说明
- 🚀 **开箱即用** - 克隆即可使用
- 📚 **文档完善** - 详细的中文使用指南

---

## ✨ 核心功能

### 🎯 四阶段核心工作流

| 阶段 | 命令 | 用途 |
|------|------|------|
| 1️⃣ | `/speckit.specify` | 将功能需求转化为清晰的规范文档 |
| 2️⃣ | `/speckit.plan` | 制定功能的技术实现方案 |
| 3️⃣ | `/speckit.tasks` | 将技术方案分解为可执行的任务清单 |
| 4️⃣ | `/speckit.implement` | 按任务清单逐步实现功能代码 |

### 🔧 辅助命令

| 命令 | 用途 | 使用时机 |
|------|------|---------|
| `/speckit.constitution` | 定义项目的核心原则和开发规范 | 项目开始时（可选） |
| `/speckit.clarify` | 解决规范中的模糊和歧义问题 | 规范化后（可选） |
| `/speckit.analyze` | 检查规范、计划、任务的一致性 | 实现前（可选） |
| `/speckit.checklist` | 生成需求质量验证清单 | 任何阶段 |

---

## 🚀 快速开始

### 方式一：使用此模板创建新项目

```bash
# 1. 在 GitHub 上点击 "Use this template" 创建新仓库

# 2. 克隆您的新仓库
git clone https://github.com/your-username/your-project.git
cd your-project

# 3. 开始使用（在 Cursor 中）
/speckit.specify
开发一个用户注册功能...
```

### 方式二：在现有项目中使用

```bash
# 1. 确保已安装 Spec-Kit CLI
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 2. 在项目中初始化
cd your-existing-project
specify init --here --ai cursor --force

# 3. 复制中文化文件
# 从本模板复制 .cursor/commands/ 和 .specify/templates/ 到您的项目
```

---

## 📚 使用示例

### 完整开发流程

```bash
# 步骤 1：创建功能规范
/speckit.specify
开发一个待办事项管理功能。用户可以创建、查看、标记完成、删除待办事项。

# 步骤 2：制定技术方案
/speckit.plan

# 步骤 3：分解任务
/speckit.tasks

# 步骤 4：开始实现
/speckit.implement
```

### 最小工作流（4步）

```
指定 → 规划 → 任务 → 实现
```

### 完整工作流（含质量检查）

```
原则 → 指定 → 澄清 → 规划 → 任务 → 分析 → 实现
```

---

## 📁 项目结构

```
.
├── .cursor/
│   └── commands/          # 8个汉化的命令文件
│       ├── speckit.constitution.md
│       ├── speckit.specify.md
│       ├── speckit.clarify.md
│       ├── speckit.plan.md
│       ├── speckit.tasks.md
│       ├── speckit.implement.md
│       ├── speckit.analyze.md
│       └── speckit.checklist.md
│
├── .specify/
│   ├── memory/
│   │   └── constitution.md    # 项目宪章模板
│   ├── scripts/               # 自动化脚本
│   └── templates/             # 5个汉化的文档模板
│
├── README.md                  # 本文档
└── 命令快速参考.md             # 命令速查表
```

---

## 🎯 汉化说明

本项目遵循以下汉化原则：

✅ **已汉化**
- 所有命令的 `description` 和执行说明
- 所有模板的章节标题和注释
- 每个命令开头的"命令用途"说明

✅ **保持英文**
- 命令文件名（如 `speckit.specify.md`）
- 命令触发词（如 `/speckit.specify`）
- 技术标识符（变量名、路径等）

**原因**：确保工具稳定运行的同时提供最佳中文体验

---

## 💡 核心优势

| 对比项 | 传统开发 | Spec-Kit |
|--------|---------|----------|
| 需求管理 | 口头沟通，易误解 | 结构化文档，清晰明确 |
| 开发流程 | 直接编码，后期问题多 | 先规范后实现，减少返工 |
| 测试覆盖 | 后期补充，覆盖率低 | 强制 TDD，测试先行 |
| 文档维护 | 文档与代码脱节 | 规范与实现同步 |
| 团队协作 | 依赖个人理解 | 基于统一规范 |

---

## 🌟 特色功能

### 1. 开头一句话用途说明
每个命令文档都以简洁的方式告诉您它的用途：

```markdown
## 📋 命令用途

**将功能需求转化为清晰的规范文档**
```

### 2. 快速参考表
查看 [命令快速参考.md](./命令快速参考.md) 获取：
- 所有命令总览
- 使用时机说明
- 完整工作流示例
- 使用技巧

---

## 📋 系统要求

- **Python**: 3.11+
- **包管理器**: uv
- **AI 工具**: Cursor（推荐）或其他兼容工具
- **Git**: 版本控制

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

如果您发现翻译不准确或有改进建议，请：
1. Fork 本仓库
2. 创建您的特性分支
3. 提交更改
4. 发起 Pull Request

---

## 📄 许可证

本项目基于原 [github/spec-kit](https://github.com/github/spec-kit) 项目。

汉化工作遵循原项目的许可证。

---

## 🔗 相关链接

- [Spec-Kit 原项目](https://github.com/github/spec-kit)
- [Cursor 官网](https://cursor.sh)
- [uv 包管理器](https://github.com/astral-sh/uv)

---

## ⭐ 如果有帮助，请给个 Star！

如果这个汉化版本对您有帮助，请点击右上角的 ⭐ Star 支持我们！

---

**开始使用 Spec-Kit 中文版，体验规范驱动开发的强大威力！** 🚀
