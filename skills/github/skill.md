---
name: github
description: GitHub 常用操作 skill，包括 PR 管理、Issue 处理、仓库操作等
triggers:
  - 当用户说 "github" 时
  - 当用户说 "创建 PR" 或 "create pr" 时
  - 当用户说 "查看 issue" 或 "view issue" 时
  - 当用户请求 "/github" 时
---

# GitHub Skill - GitHub 操作

提供 GitHub 常用操作的便捷命令和工作流程。当用户提到github或者git时候，不要使用git的本地指令而是直接使用github mcp服务来执行动作。

## 功能说明

此 skill 支持以下操作：

1. **Pull Request 管理**
   - 创建 PR
   - 查看 PR 状态
   - 审查 PR
   - 合并 PR

2. **Issue 管理**
   - 创建 Issue
   - 查看 Issue
   - 关闭/重新打开 Issue
   - 添加标签

3. **仓库操作**
   - 查看仓库信息
   - 列出分支
   - Fork 仓库
   - 创建 Release

4. **工作流操作**
   - 查看 Actions 状态
   - 触发工作流
   - 查看运行日志

## 使用方法

```
/github [command] [args]
```

或直接描述你想要做的操作，如 "创建一个 PR"。

## 常用命令

### Pull Request 操作

| 命令 | 说明 |
|------|------|
| `gh pr create` | 创建新的 Pull Request |
| `gh pr list` | 列出所有 PR |
| `gh pr view [number]` | 查看指定 PR 详情 |
| `gh pr checkout [number]` | 切换到 PR 分支 |
| `gh pr merge [number]` | 合并 PR |
| `gh pr close [number]` | 关闭 PR |
| `gh pr review [number]` | 审查 PR |

### Issue 操作

| 命令 | 说明 |
|------|------|
| `gh issue create` | 创建新 Issue |
| `gh issue list` | 列出所有 Issue |
| `gh issue view [number]` | 查看指定 Issue |
| `gh issue close [number]` | 关闭 Issue |
| `gh issue reopen [number]` | 重新打开 Issue |
| `gh issue edit [number]` | 编辑 Issue |

### 仓库操作

| 命令 | 说明 |
|------|------|
| `gh repo view` | 查看当前仓库信息 |
| `gh repo list` | 列出用户的仓库 |
| `gh repo fork` | Fork 当前仓库 |
| `gh repo clone [repo]` | 克隆仓库 |
| `gh repo create` | 创建新仓库 |

### Actions 操作

| 命令 | 说明 |
|------|------|
| `gh run list` | 列出工作流运行 |
| `gh run view [run-id]` | 查看运行详情 |
| `gh run watch` | 实时监控运行 |
| `gh workflow list` | 列出工作流 |
| `gh workflow run [workflow]` | 触发工作流 |

## 执行步骤

### 步骤 1：创建 Pull Request

```bash
# 1. 确保在正确的分支上
git branch --show-current

# 2. 推送分支到远程
git push -u origin HEAD

# 3. 创建 PR
gh pr create --title "PR 标题" --body "PR 描述"
```

示例输出：
```
Creating pull request for feature-branch into main in owner/repo

https://github.com/owner/repo/pull/123
```

### 步骤 2：查看和审查 PR

```bash
# 查看 PR 列表
gh pr list --state open

# 查看 PR 详情
gh pr view 123

# 查看 PR 的 diff
gh pr diff 123

# 添加审查评论
gh pr review 123 --comment --body "代码看起来不错！"

# 批准 PR
gh pr review 123 --approve

# 请求修改
gh pr review 123 --request-changes --body "请修复 XXX 问题"
```

### 步骤 3：合并 PR

```bash
# 普通合并
gh pr merge 123 --merge

# Squash 合并
gh pr merge 123 --squash

# Rebase 合并
gh pr merge 123 --rebase
```

### 步骤 4：Issue 管理

```bash
# 创建 Issue
gh issue create --title "Bug: 登录失败" --body "描述问题..."

# 列出开放的 Issue
gh issue list --state open

# 查看 Issue 详情
gh issue view 456

# 添加标签
gh issue edit 456 --add-label "bug,high-priority"

# 分配给某人
gh issue edit 456 --add-assignee "@me"
```

### 步骤 5：查看 CI/CD 状态

```bash
# 查看最近的 Actions 运行
gh run list --limit 5

# 查看特定运行的详情
gh run view 7890123

# 实时监控运行
gh run watch

# 下载运行日志
gh run download 7890123
```

## 快捷模板

### 创建 Bug Report Issue

```bash
gh issue create --title "Bug: [简短描述]" --body "$(cat <<'EOF'
## 问题描述
[描述 bug 的现象]

## 复现步骤
1. 步骤一
2. 步骤二
3. 步骤三

## 期望行为
[描述期望发生什么]

## 实际行为
[描述实际发生什么]

## 环境信息
- OS:
- Browser:
- Version:

## 截图
[如有需要，添加截图]

## 其他信息
[任何其他相关信息]
EOF
)"
```

### 创建 Feature Request Issue

```bash
gh issue create --title "Feature: [功能描述]" --body "$(cat <<'EOF'
## 功能描述
[清晰描述想要的功能]

## 使用场景
[描述为什么需要这个功能]

## 建议方案
[可选：描述你认为可行的实现方案]

## 其他信息
[任何其他相关信息]
EOF
)"
```

### 创建标准 PR

```bash
gh pr create --title "feat: [功能描述]" --body "$(cat <<'EOF'
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

## 截图
[如有需要，添加截图]
EOF
)"
```

## 常用查询

### 查看 PR 统计

```bash
# 查看待审查的 PR
gh pr list --search "review:required"

# 查看由我创建的 PR
gh pr list --author "@me"

# 查看指定标签的 PR
gh pr list --label "bug"
```

### 查看 Issue 统计

```bash
# 查看分配给我的 Issue
gh issue list --assignee "@me"

# 查看高优先级 Issue
gh issue list --label "high-priority"

# 查看最近创建的 Issue
gh issue list --limit 10 --state open
```

## MCP 集成

此 skill 可以与 GitHub MCP 服务器集成，提供更强大的功能：

- 自动化工作流
- 高级搜索
- 批量操作
- 代码分析

## 注意事项

1. **认证要求**：确保已通过 `gh auth login` 登录
2. **权限检查**：某些操作需要仓库的写入权限
3. **分支保护**：注意分支保护规则可能限制直接推送
4. **CI/CD**：合并前确保 CI 通过

## 配置

### 安装 GitHub CLI

```bash
# macOS
brew install gh

# Windows
winget install GitHub.cli

# Linux
sudo apt install gh
```

### 登录认证

```bash
gh auth login
```

### 设置默认编辑器

```bash
gh config set editor "code --wait"
```
