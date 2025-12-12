# Spec-Kit 中文版 🇨🇳

> GitHub Spec-Kit 的完整中文汉化版本 | 规范驱动开发工具包

[![GitHub](https://img.shields.io/badge/GitHub-spec--kit-blue)](https://github.com/github/spec-kit)
[![Language](https://img.shields.io/badge/语言-中文-red)](README.md)
[![Status](https://img.shields.io/badge/状态-稳定-green)](README.md)

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
