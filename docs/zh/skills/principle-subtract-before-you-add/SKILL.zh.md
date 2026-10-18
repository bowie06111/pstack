<!-- 中英对照译文。原文：skills/principle-subtract-before-you-add/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-subtract-before-you-add/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-subtract-before-you-add/SKILL.md)

```yaml
name: principle-subtract-before-you-add
description: "Apply when sequencing an addition, refactor, or rewrite. Remove dead code, redundant validators, and stub references first, then build on the simpler base."
disable-model-invocation: true
```

> **说明（description）：** 在安排新增、重构或重写的顺序时使用。先删掉死代码、多余的校验和空壳引用，再在更简单的基础上构建。

# Subtract Before You Add｜先减后加

When evolving a system, remove complexity first, then build. Default to subtraction.

> 演进一个系统时，先去掉复杂度，再构建。默认做减法。

Make simplification a continual investment. Leave the design slightly simpler and more capable behind the same or smaller surface than you found it.

> 把简化当作持续的投入。离开时，让设计在相同或更小的接口背后，比你接手时稍微简单一点、能力强一点。

- Sequence removal before construction

> - 先删除，再构建

- Cut before you polish (get to the minimum before investing in quality)

> - 先砍，再打磨（先做到最小，再投入质量）

- Design for observed usage, not speculative edge cases

> - 为观察到的使用情况设计，而不是为投机性的边界情况

- No speculative validators, parsers, or guards beyond what the spec demands

> - 不要写规格要求之外的投机性校验器、解析器或防护

- Simplify prompts (remove redundant instructions, excessive templates)

> - 简化提示词（删掉多余的指示和过多的模板）

- When a reference has no novel content, delete it rather than leaving a stub

> - 当一个引用没有新内容时，删掉它，而不是留一个空壳
