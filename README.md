# Skills

面向 [Cursor](https://cursor.com) 的 Agent Skills 集合。每个 skill 是一个独立目录，包含 `SKILL.md` 及可选参考文档。

## Skill 一览

### [markdown-agent](./markdown-agent/)（`@md-agent`）

扫描 Markdown 中的 ` ```agent ` 代码块，将产出写回块下方区间，完成后自动删除代码块。

## 如何使用

1. 将 skill 目录复制到 Cursor 的 skills 路径，或在项目中引用。
2. 在对话中通过 `@skill-name` 调用对应 skill。
3. 详见各目录下的 `SKILL.md`。

## 贡献

新增 skill 时：独立子目录 + 根文件 `SKILL.md`（含 YAML frontmatter），并在上方「Skill 一览」补充一行简介。
