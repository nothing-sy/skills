---
name: hello
description: 示例 skill，演示 skill 的基本结构
user-invocable: true
---

# Hello Skill

这是一个示例 skill，演示 skill 的基本结构。

## 功能说明

当用户调用 `/hello` 时，输出问候语。

## 使用方法

```
/hello [name]
```

## 执行指令

1. 如果用户提供了名字，输出 "Hello, {name}!"
2. 如果没有提供名字，输出 "Hello, World!"
