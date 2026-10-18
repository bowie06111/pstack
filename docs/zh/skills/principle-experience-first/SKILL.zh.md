<!-- 中英对照译文。原文：skills/principle-experience-first/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-experience-first/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-experience-first/SKILL.md)

```yaml
name: principle-experience-first
description: "Apply when product, UX, or feature-scope tradeoffs come up. Choose user delight over implementation convenience; ship fewer polished features over more rough ones."
disable-model-invocation: true
```

> **说明（description）：** 在出现产品、用户体验或功能范围的取舍时使用。选择让用户满意，而不是实现方便；宁可发布更少但打磨好的功能，也不要更多粗糙的功能。

# Experience First｜体验优先

When implementation convenience conflicts with user delight, choose delight.

> 当实现的方便与用户的满意相冲突时，选择满意。

- Every feature, control, and option must be justified

> - 每个功能、控件和选项都必须有存在的理由

- Ship less, ship better (polished experience with three features beats rough one with ten)

> - 少发布，发布得更好（三个功能的打磨好的体验，胜过十个功能的粗糙体验）

- Prototype before committing (design decisions are cheaper in throwaway HTML than production code)

> - 定下来之前先做原型（设计决策在一次性的 HTML 里比在生产代码里便宜）

- Get the details right (transitions, alignment, spacing, feedback, error states)

> - 把细节做对（过渡、对齐、间距、反馈、错误状态）

- Tighten the core loop (every feature should serve the central workflow or get out of the way)

> - 收紧核心循环（每个功能都应该服务于核心工作流，否则就别挡路）

The user is whoever consumes the work. For a UI that is the end user. For a library or an internal API it is the colleague who imports it. The engineer who maintains the code next is a user too. Weigh their experience the same way, and explain impact from their perspective.

> 用户是任何使用这项工作的人。对于 UI，是终端用户。对于库或内部 API，是引入它的同事。下一个维护这段代码的工程师也是用户。用同样的方式权衡他们的体验，并从他们的角度解释影响。

Foundations should serve the experience. Foundational thinking governs the *sequence* of work. This principle governs the *target*.

> 基础应该服务于体验。基础思维决定工作的*顺序*。这条原则决定*目标*。
