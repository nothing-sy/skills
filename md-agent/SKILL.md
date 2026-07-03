---
name: md-agent
description: >-
  Processes reserved agent prompts embedded in Markdown files via ```agent fenced
  code blocks and applies in-place edits below each block in the same .md file.
  Use when the user explicitly invokes this skill, mentions @md-agent, or asks to
  execute agent prompts inside a Markdown document.
disable-model-invocation: true
---

# MD Agent

处理 Markdown 文件中以 ` ```agent ` 围栏代码块预留的 Agent 任务，将结果**就地写回代码块下方**，完成后**自动删除**整个代码块，仅保留产出内容。

## 快速开始

1. 用户显式调用本 skill（`@md-agent`）并指定目标 `.md` 文件（或使用当前打开文件）。
2. 扫描文件中所有 ` ```agent ` 代码块（排除 ` ```agent-skip `）。
3. **一次性**处理全部待处理块，**从上到下**依次执行。
4. 每条指令成功后：更新代码块下方区间内容，并删除该 ` ```agent ... ``` ` 块。

语法细节见 [schema.md](schema.md)，完整示例见 [examples.md](examples.md)。

## 指令识别

**待处理：** 围栏开行为 ` ```agent `（允许行尾空格），闭行为独立一行的 ` ``` `。

**排除：** 围栏开行为 ` ```agent-skip ` 的块，扫描时忽略，不删除、不产出。

**块内内容：** 全部为自然语言指令（中文为主），支持块内自然换行，无需续行缩进规则。

**失败记录：** 代码块紧下方可存在 `<!-- @agent-error: 简要原因 -->`（处理成功后一并删除）。

## 执行清单

按顺序执行，不可跳步：

```
- [ ] 1. 读取用户指定的 .md 文件（或当前打开文件）
- [ ] 2. 扫描所有 ```agent 代码块，排除 ```agent-skip
- [ ] 3. 按「指令选择」确定本轮待处理块列表
- [ ] 4. 对列表中每个块依次：确定下方读写区间，读取引用的源码/配置文件
- [ ] 5. 生成或更新代码块下方区间内容，就地写回 MD
- [ ] 6. 成功后删除该 ```agent ... ``` 块及紧邻的 @agent-error 注释（若有）
- [ ] 7. 失败则保留代码块，在块下方追加或更新 @agent-error 注释
- [ ] 8. 向用户汇报：处理了哪些指令、改了什么、是否有失败项
```

## 指令选择

| 条件 | 行为 |
|------|------|
| 用户指定行号 | 只处理包含该行的 ` ```agent ` 块 |
| 用户明确要求只处理一条 | 从上到下取第一个 ` ```agent ` 块 |
| **默认** | **一次性处理文件中全部** ` ```agent ` 块，**从上到下**顺序执行 |

## 写回规则（section-below）

产出始终落在代码块**下方**的区间内，完成后删除代码块本身。

### 区间边界

- **起点：** ` ```agent ` 块闭合围栏 ` ``` ` 的下一行。
- **终点**（先遇到即停）：
  1. 下一条 ` ```agent ` / ` ```agent-skip ` 块
  2. 同级或更高级 Markdown 标题（`#` 数量 ≤ 锚点标题级别；无锚点标题时遇任意标题即停）
  3. 文件末尾
- **锚点标题：** 向上最近的一个 Markdown 标题。

## 完成与失败

**成功：**

1. 按指令更新代码块下方区间内容。
2. 删除整个 ` ```agent ... ``` ` 块。
3. 若存在 `<!-- @agent-error: ... -->` 注释，一并删除。

仅保留生成的 Markdown 正文。` ```agent ` 块是写给 Agent 的临时提示，不是文档终稿的一部分。

**失败：**

1. **保留** ` ```agent ` 代码块。
2. 在代码块下方追加或更新：`<!-- @agent-error: 简要原因 -->`。
3. 不修改下方已有产出（若有）。

**手动跳过（可选）：** 将围栏开行改为 ` ```agent-skip `，永久忽略。

## 安全与边界

- 指令若要求执行 shell 或修改非 MD 源码，**须先向用户确认**；默认只改 MD 就地内容。
- 批量处理时，未完成轮次的其他 ` ```agent ` 块不得误删或破坏。
- 不把页面级细节写入项目的 `README_NEW.md` / `PLAN.md`，除非指令明确要求且符合该项目文档边界。
- 文件编码保持 UTF-8 without BOM。
- 仅修改目标 MD 及指令明确引用的关联文件；不主动改无关文件。

## 附加资源

- 语法与边界参考：[schema.md](schema.md)
- 可复制示例：[examples.md](examples.md)
