# Agent Skills 项目

这是一个专门用于编写和管理 Claude Code Agent Skills 的项目。

## 什么是 Agent Skill？

Agent Skill 是一种可复用的功能模块，允许 Claude Code 执行特定的任务。通过 skill，你可以：

- 封装常用的操作流程
- 创建自定义命令（通过 `/skill-name` 调用）
- 扩展 Claude Code 的能力

## 项目结构

```
skills/
├── README.md           # 项目说明文档
├── settings.json       # MCP 服务器配置
├── skills/             # skill 存放目录
│   ├── demo/           # 演示 skill
│   │   └── skill.md
│   ├── code-review/    # 代码审查 skill
│   │   └── skill.md
│   └── github/         # GitHub 操作 skill
│       └── skill.md
└── .claude/            # Claude Code 配置目录
```

## 如何创建一个新的 Skill

### 1. 创建 Skill 目录

在 `skills/` 目录下创建一个新的文件夹，以 skill 名称命名：

```bash
mkdir skills/my-skill
```

### 2. 编写 Skill 定义

在 skill 目录中创建 `skill.md` 文件：

```markdown
---
name: my-skill
description: 简短描述这个 skill 的功能
triggers:
  - 当用户说 "xxx" 时触发
  - 当代码中包含 xxx 时触发
---

# Skill 名称

详细描述这个 skill 应该做什么...

## 执行步骤

1. 第一步做什么
2. 第二步做什么
3. ...

## 注意事项

- 注意点 1
- 注意点 2
```

### 3. Skill 配置说明

| 字段 | 说明 |
|------|------|
| `name` | Skill 名称，用于 `/skill-name` 调用 |
| `description` | 简短描述，显示在 skill 列表中 |
| `triggers` | 触发条件列表，定义何时自动激活此 skill |

## 内置 Skills 参考

Claude Code 提供了一些内置 skills：

- `/commit` - 创建 Git 提交
- `/review-pr` - 审查 Pull Request
- `/test` - 运行测试
- `/babysit-prs` - 监控 PR 状态

## 项目 Skills

本项目包含以下自定义 skills：

| Skill | 说明 |
|-------|------|
| `demo` | 演示 skill 的基本结构 |
| `code-review` | 代码审查，检查潜在问题和改进建议 |
| `github` | GitHub 常用操作，包括 PR、Issue、仓库管理等 |

## MCP 服务器配置

项目包含 `settings.json` 文件，配置了以下 MCP 服务器：

### GitHub MCP

提供 GitHub API 集成，支持：
- 仓库操作
- Issue 和 PR 管理
- 代码搜索
- 工作流管理

**配置方式：**

1. 设置 GitHub Personal Access Token 环境变量：
   ```bash
   export GITHUB_TOKEN=your_personal_access_token
   ```

2. 确保 Token 具有以下权限：
   - `repo` - 完整的仓库访问
   - `workflow` - 工作流操作
   - `read:org` - 读取组织信息

**获取 Token：**
1. 访问 https://github.com/settings/tokens
2. 点击 "Generate new token (classic)"
3. 选择所需权限
4. 生成并保存 Token

## 最佳实践

1. **单一职责**：每个 skill 应该只做一件事
2. **清晰命名**：使用描述性的名称，让用户一目了然
3. **详细文档**：在 skill.md 中提供完整的使用说明
4. **错误处理**：考虑边界情况和错误场景
5. **可测试性**：确保 skill 可以被独立测试

## 示例

查看 `skills/demo/` 目录中的演示 skill，了解如何编写一个完整的 skill。

## 贡献指南

1. Fork 本项目
2. 创建新的 skill 分支
3. 提交 Pull Request

## 许可证

MIT License
