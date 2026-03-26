---
name: code-review
description: 审查当前变更的代码，检查潜在问题和改进建议
triggers:
  - 当用户说 "审查代码" 时
  - 当用户说 "review code" 时
  - 当用户说 "检查代码质量" 时
  - 当用户请求 "/code-review" 时
---

# Code Review Skill - 代码审查

对当前未提交的代码变更进行全面审查，识别潜在问题并提供改进建议。

## 功能说明

此 skill 会执行以下检查：

1. **代码质量检查**
   - 代码风格一致性
   - 命名规范
   - 代码复杂度

2. **安全检查**
   - SQL 注入风险
   - XSS 漏洞
   - 敏感信息泄露

3. **最佳实践检查**
   - 错误处理
   - 日志记录
   - 性能问题

4. **文档检查**
   - 注释完整性
   - 函数/方法说明

## 使用方法

```
/code-review
```

或直接说 "审查代码"。

## 执行步骤

### 步骤 1：获取变更列表

```bash
git status
git diff
```

获取所有未提交的变更内容。

### 步骤 2：分析变更文件

对每个变更的文件进行分析：

- 识别文件类型（JS/TS/Python/等）
- 解析变更的代码块
- 检查变更的影响范围

### 步骤 3：执行检查

按照检查清单逐项检查：

| 检查项 | 说明 |
|--------|------|
| 安全漏洞 | 检查常见安全问题 |
| 代码异味 | 识别代码坏味道 |
| 性能问题 | 查找性能瓶颈 |
| 可维护性 | 评估代码可读性 |

### 步骤 4：生成报告

输出结构化的审查报告：

```
📋 Code Review Report
======================

Files Changed: 3
Lines Added: 150
Lines Removed: 30

✅ Passed Checks:
- No SQL injection vulnerabilities
- Proper error handling

⚠️ Warnings:
- [file.ts:42] Consider using const instead of let
- [utils.js:15] Missing error handling for async function

❌ Issues Found:
- [api.js:88] Potential XSS vulnerability in user input

💡 Suggestions:
- Add unit tests for new functions
- Consider extracting repeated logic into a helper function
```

## 输出格式

### 严重程度级别

| 级别 | 图标 | 说明 |
|------|------|------|
| Critical | ❌ | 必须修复的严重问题 |
| Warning | ⚠️ | 建议修复的问题 |
| Info | ℹ️ | 可选的改进建议 |
| Pass | ✅ | 检查通过 |

## 配置选项

可以在项目根目录创建 `.codereviewrc` 文件来自定义检查规则：

```json
{
  "rules": {
    "security": true,
    "performance": true,
    "style": false,
    "documentation": true
  },
  "severity": "warning",
  "exclude": ["*.test.js", "*.spec.ts"]
}
```

## 注意事项

- 仅审查已修改的代码，不会审查整个项目
- 对于大型变更，建议分批提交和审查
- 审查结果仅供参考，最终决定权在开发者

## 与其他工具集成

- 可以与 ESLint、Prettier 等工具配合使用
- 支持 Git hooks 自动执行
- 可集成到 CI/CD 流程中
