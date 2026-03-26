---
name: github
description: GitHub 远程操作 skill，使用 GitHub MCP 服务管理 PR、Issue、仓库等
user-invocable: true
---

# GitHub Skill - GitHub 远程操作

提供 GitHub 远程操作的便捷工作流程。

## 重要规则

**所有远程 GitHub 操作必须使用 GitHub MCP 服务，禁止使用 `gh` CLI 或本地 git 命令进行远程操作。**

| 场景 | 使用工具 |
|------|----------|
| 查看/创建/更新 PR | `mcp__github__*` 工具 |
| 查看/创建/更新 Issue | `mcp__github__*` 工具 |
| 查看/创建/更新远程文件 | `mcp__github__*` 工具 |
| 查看仓库信息、分支、提交 | `mcp__github__*` 工具 |
| 本地分支切换、暂存、提交 | `git` 本地命令 |

## 可用的 GitHub MCP 工具

### Pull Request 操作

| MCP 工具 | 功能 |
|----------|------|
| `mcp__github__create_pull_request` | 创建 PR |
| `mcp__github__get_pull_request` | 获取 PR 详情 |
| `mcp__github__list_pull_requests` | 列出 PR |
| `mcp__github__merge_pull_request` | 合并 PR |
| `mcp__github__get_pull_request_files` | 获取 PR 文件变更 |
| `mcp__github__get_pull_request_reviews` | 获取 PR 审查 |
| `mcp__github__get_pull_request_comments` | 获取 PR 评论 |
| `mcp__github__get_pull_request_status` | 获取 PR 状态检查 |
| `mcp__github__create_pull_request_review` | 创建 PR 审查 |
| `mcp__github__update_pull_request_branch` | 更新 PR 分支 |

### Issue 操作

| MCP 工具 | 功能 |
|----------|------|
| `mcp__github__create_issue` | 创建 Issue |
| `mcp__github__get_issue` | 获取 Issue 详情 |
| `mcp__github__list_issues` | 列出 Issue |
| `mcp__github__update_issue` | 更新 Issue |
| `mcp__github__add_issue_comment` | 添加 Issue 评论 |

### 仓库操作

| MCP 工具 | 功能 |
|----------|------|
| `mcp__github__get_file_contents` | 获取文件内容 |
| `mcp__github__create_or_update_file` | 创建或更新文件 |
| `mcp__github__push_files` | 批量推送文件 |
| `mcp__github__create_repository` | 创建仓库 |
| `mcp__github__fork_repository` | Fork 仓库 |
| `mcp__github__create_branch` | 创建分支 |
| `mcp__github__list_commits` | 列出提交 |

### 搜索操作

| MCP 工具 | 功能 |
|----------|------|
| `mcp__github__search_code` | 搜索代码 |
| `mcp__github__search_issues` | 搜索 Issue/PR |
| `mcp__github__search_repositories` | 搜索仓库 |
| `mcp__github__search_users` | 搜索用户 |

## 执行步骤

### 步骤 1：创建 Pull Request

使用 `mcp__github__create_pull_request`：

```
owner: 仓库所有者
repo: 仓库名称
title: PR 标题
head: 源分支
base: 目标分支
body: PR 描述
```

### 步骤 2：查看和审查 PR

- 查看 PR 详情：`mcp__github__get_pull_request`
- 查看 PR 文件变更：`mcp__github__get_pull_request_files`
- 查看 PR 状态检查：`mcp__github__get_pull_request_status`
- 创建审查：`mcp__github__create_pull_request_review`

### 步骤 3：合并 PR

使用 `mcp__github__merge_pull_request`：

```
owner: 仓库所有者
repo: 仓库名称
pull_number: PR 编号
merge_method: "merge" | "squash" | "rebase"
```

### 步骤 4：Issue 管理

- 创建 Issue：`mcp__github__create_issue`
- 查看 Issue：`mcp__github__get_issue`
- 列出 Issue：`mcp__github__list_issues`
- 更新 Issue：`mcp__github__update_issue`（可关闭/重新打开/添加标签）
- 添加评论：`mcp__github__add_issue_comment`

### 步骤 5：推送文件到远程

**单个文件**：使用 `mcp__github__create_or_update_file`

```
owner: 仓库所有者
repo: 仓库名称
path: 文件路径
content: 文件内容
message: 提交信息
branch: 分支名
sha: 文件 SHA（更新现有文件时必需）
```

**多个文件**：使用 `mcp__github__push_files`

```
owner: 仓库所有者
repo: 仓库名称
branch: 分支名
files: [{ path, content }, ...]
message: 提交信息
```

## 快捷模板

### 创建 Bug Report Issue

```
title: "Bug: [简短描述]"
body: |
  ## 问题描述
  [描述 bug 的现象]

  ## 复现步骤
  1. 步骤一
  2. 步骤二

  ## 期望行为
  [描述期望发生什么]

  ## 实际行为
  [描述实际发生什么]

  ## 环境信息
  - OS:
  - Browser:
  - Version:
```

### 创建 Feature Request Issue

```
title: "Feature: [功能描述]"
body: |
  ## 功能描述
  [清晰描述想要的功能]

  ## 使用场景
  [描述为什么需要这个功能]

  ## 建议方案
  [可选：描述你认为可行的实现方案]
```

### 创建标准 PR

```
title: "feat: [功能描述]"
body: |
  ## 变更说明
  [描述这个 PR 做了什么]

  ## 变更类型
  - [ ] Bug fix
  - [ ] New feature
  - [ ] Breaking change
  - [ ] Documentation update

  ## 测试计划
  - [ ] 测试项 1
  - [ ] 测试项 2

  ## 相关 Issue
  Closes #123
```

## 注意事项

1. **必须使用 MCP**：所有远程操作必须使用 GitHub MCP 工具
2. **文件更新需要 SHA**：更新已存在的文件时，必须先获取文件 SHA
3. **权限检查**：确保 GitHub Token 有足够的权限
4. **分支保护**：注意分支保护规则可能限制直接推送

## 本地操作（非远程）

以下操作仍可使用本地 git 命令：

```bash
# 查看当前分支
git branch --show-current

# 切换分支
git checkout <branch>

# 暂存更改
git add <files>

# 本地提交
git commit -m "message"

# 查看状态
git status

# 查看差异
git diff
```

**推送代码到远程时，必须使用 GitHub MCP 的 `create_or_update_file` 或 `push_files`。**
