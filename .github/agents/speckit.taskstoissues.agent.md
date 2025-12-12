---
description: 将现有任务转换为可操作的、按依赖关系排序的 GitHub issue，基于可用的设计工件为功能创建问题。
tools: ['github/github-mcp-server/issue_write']
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 概述

1. 从仓库根目录运行 `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` 并解析 `FEATURE_DIR` 和 `AVAILABLE_DOCS` 列表。所有路径必须为绝对路径。对于参数中的单引号（如 "I'm Groot"），使用转义语法：例如 'I'\''m Groot'（或在可能的情况下使用双引号："I'm Groot"）。
2. 从执行脚本的输出中提取 **tasks** 的路径。
3. 通过运行以下命令获取 Git 远程：

```bash
git config --get remote.origin.url
```

> [!注意]
> 仅当远程为 GitHub URL 时才继续下面的步骤

4. 对于任务列表中的每个任务，使用 GitHub MCP 服务在与 Git 远程对应的仓库中创建新的 issue。

> [!注意]
> 在任何情况下都不要在与远程 URL 不匹配的仓库中创建 issues。
