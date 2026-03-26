---
name: demo
description: 一个演示 skill，展示如何编写 agent skill 的基本结构
triggers:
  - 当用户说 "演示 skill" 时
  - 当用户说 "show demo" 时
---

# Demo Skill - 演示 Skill

这是一个演示性质的 skill，用于展示如何编写 Claude Code Agent Skill 的基本结构和最佳实践。

## 功能说明

此 skill 会：

1. 打印欢迎信息
2. 显示当前工作目录的文件列表
3. 展示 Git 状态（如果是在 Git 仓库中）
4. 提供项目概览

## 使用方法

在 Claude Code 中输入：

```
/demo
```

或者直接说 "演示 skill"。

## 执行步骤

### 步骤 1：显示欢迎信息

向用户打招呼，说明即将执行的演示操作。

### 步骤 2：列出目录内容

使用 `ls -la` 命令列出当前目录的文件和文件夹。

### 步骤 3：检查 Git 状态

如果当前目录是 Git 仓库，显示：
- 当前分支
- 工作区状态
- 最近的提交记录

### 步骤 4：生成项目概览

根据目录结构，总结项目的类型和主要组成部分。

## 输出示例

```
🎉 Demo Skill 演示

📁 当前目录: /path/to/project

📄 文件列表:
- README.md
- src/
- package.json

🌿 Git 信息:
- 分支: main
- 状态: clean
- 最近提交: abc123 - Initial commit

📊 项目概览:
这是一个 Node.js 项目，包含源代码目录 src/
```

## 注意事项

- 如果目录中没有文件，会显示空目录提示
- 如果不是 Git 仓库，会跳过 Git 相关步骤
- 对于大型目录，只显示前 20 个文件

## 自定义建议

你可以基于此模板创建自己的 skill：

1. 复制此文件到新的目录
2. 修改 `name`、`description` 和 `triggers`
3. 更新执行步骤和功能说明
4. 添加你需要的具体操作
