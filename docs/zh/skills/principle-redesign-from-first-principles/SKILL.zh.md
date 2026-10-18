<!-- 中英对照译文。原文：skills/principle-redesign-from-first-principles/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-redesign-from-first-principles/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-redesign-from-first-principles/SKILL.md)

```yaml
name: principle-redesign-from-first-principles
description: "Apply when integrating a new requirement into an existing design."
disable-model-invocation: true
```

> **说明（description）：** 在把新需求整合进现有设计时使用。

# Redesign From First Principles｜从第一性原理重新设计

When integrating a change, don't bolt it onto the existing design. Redesign as if the requirement had been a foundational assumption from the start.

> 整合一个改动时，不要把它硬接到现有设计上。就当这个需求从一开始就是基础前提，来重新设计。

- Read all affected files and understand the current design

> - 读所有受影响的文件，理解当前的设计

- Ask: "if we were writing this from scratch with this new requirement, what would we build?"

> - 问：“如果我们带着这个新需求从头写，会做成什么样？”

- Propagate the change through every reference: types, docs, examples, rationale sections

> - 把改动传播到每一处引用：类型、文档、示例、理由说明

- Think about the whole redesign, then deliver it incrementally

> - 整体想清楚重新设计，然后分步交付

This is the method for preserving option value when integrating changes into an existing design.

> 这是把改动整合进现有设计时保留选择余地的方法。
