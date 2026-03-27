---
name: git
description: 提交本地代码并推送到远端。当用户调用 /git 时执行，自动暂存更改、拉取远端代码、检查冲突、提交并推送。
user-invocable: true
---

# Git 提交推送 Skill

自动完成代码提交和推送的完整流程。

## 工作流程

执行以下步骤时，如果一切正常（无冲突），自动执行，不再询问用户。

### 1. 检查工作区状态

首先检查当前分支和文件状态：
```bash
git status
git branch --show-current
```

如果没有更改，提示用户并结束。

### 2. 暂存所有更改

```bash
git add -A
```

### 3. 拉取远端代码

```bash
git pull origin <current-branch>
```

### 4. 检查冲突

- **有冲突**: 提示用户存在冲突，列出冲突文件，告知用户需要手动解决冲突后自行提交。**任务到此结束，不再继续执行后续步骤。**
- **无冲突**: 继续下一步

### 5. 生成 Commit Message

根据更改内容生成符合 Conventional Commits 规范的提交信息：

格式: `<type>(<scope>): <description>`

常用 type:
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更改
- `style`: 代码格式（不影响逻辑）
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建/工具/依赖更新
- `perf`: 性能优化

通过 `git diff --cached` 分析更改，生成合适的 commit message。

### 6. 提交代码

```bash
git commit -m "<commit-message>"
```

### 7. 推送到远端

```bash
git push origin <current-branch>
```

## 冲突处理

当检测到冲突时，输出格式：

```
检测到冲突，请手动解决以下文件中的冲突：
- <file1>
- <file2>

解决冲突后，请自行执行：
1. git add <resolved-files>
2. git commit
3. git push
```

**重要**: 冲突发生时，不要尝试自动解决，不要继续执行后续步骤。

## 注意事项

- 不要使用 `--force` 推送
- 不要使用 `git reset --hard` 等破坏性命令
- 确保 commit message 简洁明了，用中文或英文保持一致
