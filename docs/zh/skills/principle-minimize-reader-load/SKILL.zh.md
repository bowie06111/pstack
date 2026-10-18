<!-- 中英对照译文。原文：skills/principle-minimize-reader-load/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-minimize-reader-load/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-minimize-reader-load/SKILL.md)

```yaml
name: principle-minimize-reader-load
description: "Apply when reviewing or shaping code that's hard to trace. Count layers between question and answer, and hidden state in the reader's head; collapse one-caller wrappers and shrink mutable scope."
disable-model-invocation: true
```

> **说明（description）：** 在审查或调整难以追踪的代码时使用。数一数从问题到答案之间有几层，以及读者脑子里要记住多少隐藏状态；合并只有一个调用方的包装层，缩小可变范围。

# Minimize Reader Load｜降低阅读负担

Maintainability is the work a reader must do to understand code. Track two axes: **layers to trace** (indirections between the question and the answer) and **state to hold** (hidden or mutable context the reader must keep in their head). LOC, cyclomatic complexity, and "clean architecture" are proxies. Reader load is the thing that matters. The two axes are independent. A flat file with 50 globals can be as hard to reason about as a 6-layer adapter stack. Guard both. This is the human analog of [Guard the Context Window](../principle-guard-the-context-window/SKILL.zh.md). Working memory is finite for readers too.

> 可维护性就是读者为了理解代码必须做的功。跟踪两个维度：**要追踪的层数**（问题和答案之间的间接层）和**要记住的状态**（读者必须记在脑子里的隐藏或可变的上下文）。代码行数、圈复杂度和“整洁架构”都只是代理指标。阅读负担才是真正重要的东西。这两个维度相互独立。一个有 50 个全局变量的扁平文件，可能和一个 6 层的适配器栈一样难以推理。两个都要守住。这是 [守护上下文窗口](../principle-guard-the-context-window/SKILL.zh.md) 在人身上的对应物。读者的工作记忆也是有限的。

- **Collapse layers** that cost more than they save: wrappers with one caller, adapters with no second implementation, speculative indirection that was never needed. Inline them.

> - **合并得不偿失的层：** 只有一个调用方的包装层、没有第二种实现的适配器、从没被需要过的投机性间接层。把它们内联掉。

- **Make adjacent layers change the abstraction.** A layer that repeats the same methods and arguments adds reader load without compression. Collapse pass-through layers.

> - **让相邻的层改变抽象。** 一个重复同样方法和参数的层，只增加阅读负担而没有压缩任何东西。合并只做转发的层。

- **Demand interface compression.** A broad interface that hides little complexity makes readers learn both the surface and the implementation. Prefer boundaries that hide meaningful decisions.

> - **要求接口有压缩作用。** 一个宽泛却几乎没隐藏什么复杂度的接口，会让读者既要学接口又要学实现。优先选择能隐藏有意义决策的边界。

- **Shrink state scope:** prefer pure functions (returns over mutations), locals over fields, fields over module state, and module state over globals. Derive instead of sync.

> - **缩小状态范围：** 优先用纯函数（返回值优于修改），局部变量优于字段，字段优于模块状态，模块状态优于全局变量。推导而不是同步。

- **Name the invariant at the boundary,** not in every consumer, so the reader learns it once.

> - **在边界上说明不变量，** 而不是在每个使用方那里说，这样读者只需学一次。

- Before adding a layer or a piece of state, ask: does it reduce reader load elsewhere by at least as much?

> - 在增加一层或一块状态之前，问：它在别处减少的阅读负担，至少和它增加的一样多吗？

**The test:** can a new reader answer "where does X come from?" and "what can change X?" in under 30 seconds? If not, cut layers or cut state.

> **检验方法：** 一个新读者能否在 30 秒内回答“X 从哪里来？”和“什么能改变 X？”如果不能，就减层数或减状态。
