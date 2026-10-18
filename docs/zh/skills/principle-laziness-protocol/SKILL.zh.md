<!-- 中英对照译文。原文：skills/principle-laziness-protocol/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-laziness-protocol/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-laziness-protocol/SKILL.md)

```yaml
name: principle-laziness-protocol
description: "Apply when refactoring, evaluating diff size, or tempted to add abstractions, layers, or signal threading. Bias toward deletion and the smallest change that solves the problem."
disable-model-invocation: true
```

> **说明（description）：** 在重构、评估 diff 大小，或想加抽象层、中间层、信号传递时使用。偏向删除，用解决问题的最小改动。

# Laziness Protocol｜懒惰协议

- **Consolidate decisions.** Do not repeat the same choice in several places. Put it behind one source of truth and pass the result as a simple flag.

> - **合并决策。** 不要在好几个地方重复同一个选择。把它放在单一事实来源后面，把结果作为一个简单的标记传下去。

- **Minimize the diff.** Make the smallest change that solves the problem. Fewer lines beat "elegant" boilerplate.

> - **让 diff 最小。** 做能解决问题的最小改动。行数少胜过“优雅”的样板代码。
