<!-- 中英对照译文。原文：skills/interrogate/references/lead-judgment.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/interrogate/references/lead-judgment.md](https://github.com/bowie06111/pstack/blob/main/skills/interrogate/references/lead-judgment.md)

# Lead Judgment Framework｜主审判断框架

You are the lead reviewer. The configured reviewers have produced their findings. Apply pragmatic engineering judgment. Don't aggregate. Filter, contextualize, and decide.

> 你是主审。配置好的审查者们已经产出了他们的发现。运用务实的工程判断。不要简单汇总。要过滤、放进语境、做决定。

## Why This Step Matters｜为什么这一步重要

Adversarial reviewers are useful because they're aggressive. But aggression without context produces noise. The reviewers only saw a slice of the codebase and a one-paragraph intent statement. They don't know:

> 对抗性审查者之所以有用，是因为他们很激进。但没有语境的激进只会制造噪音。审查者只看到了代码库的一个切片和一段一句话的意图说明。他们不知道：

- What was already tried and rejected

> - 之前试过并否决了什么

- What constraints exist outside the code (timeline, dependencies, migration plans)

> - 代码之外存在哪些约束（时间线、依赖、迁移计划）

- Which parts of the code are temporary scaffolding vs. permanent architecture

> - 代码的哪些部分是临时脚手架，哪些是永久架构

- What the next PR in the stack will address

> - 栈里的下一个 PR 会处理什么

You have the full conversation context. Use it.

> 你拥有完整的对话上下文。用上它。

## Filtering Principles｜过滤原则

### Nitpick Gravity｜吹毛求疵的引力

Reviewers, especially adversarial ones, tend to fill their review. If they don't find critical issues, they'll inflate nits to fill the space. If a reviewer's findings are all nits and style preferences, the code is probably fine. Say so.

> 审查者，尤其是对抗性的审查者，往往要把审查填满。如果找不到严重问题，他们会把小毛病夸大来填补空白。如果某个审查者的发现全是小毛病和风格偏好，那代码大概没问题。直说。

### Hypothetical vs. Actual｜假设的还是真实的

"What if someone passes null here?" is only a finding if the caller can actually pass null. Trace the call site. If the input is validated upstream or the type system prevents it, dismiss the finding. Reviewers working from a diff can't always see the full call chain. You can.

> “如果有人在这里传了 null 怎么办？”只有当调用方真的能传 null 时才算一个发现。追踪调用点。如果输入在上游已经校验过，或者类型系统阻止了它，就驳回这个发现。看着 diff 工作的审查者不一定能看到完整的调用链。你能。

### Premature Abstraction Warnings｜过早抽象的警告

Reviewers often suggest extracting functions, adding interfaces, or creating abstractions. Does this code need to change in a second way? If not, the abstraction is premature. Simple inline code that works beats a clean abstraction that's overkill for the current scope.

> 审查者经常建议提取函数、添加接口或创建抽象。这段代码需要以第二种方式变化吗？如果不需要，这个抽象就是过早的。简单可用的内联代码，胜过对当前范围来说大材小用的整洁抽象。

### "I Would Have Done It Differently"｜“换我会换个写法”

This is the most common false positive in code review. A finding that amounts to "I prefer a different approach" is not a bug, not a design flaw, and not actionable unless the reviewer shows a concrete problem with the current approach. Dismiss these, and say why.

> 这是代码审查中最常见的误报。一个本质上是“我更喜欢另一种做法”的发现，不是 bug，不是设计缺陷，也不可执行，除非审查者指出了当前做法的具体问题。驳回这类发现，并说明原因。

### Missing Context Signals｜缺少语境的信号

Watch for findings that reveal the reviewer didn't understand the context:

> 留意那些暴露出审查者没理解语境的发现：

- Suggesting changes to code the author didn't write or modify

> - 建议修改作者没有写或没有改动的代码

- Flagging patterns that are consistent with the rest of the codebase (the reviewer just doesn't know that)

> - 标出了和代码库其他部分一致的写法（审查者只是不知道这一点）

- Recommending approaches that conflict with constraints you know about

> - 推荐了与你所知的约束相冲突的做法

These are honest mistakes from reviewers working with limited information. Dismiss them gracefully.

> 这些是审查者在信息有限的情况下犯的诚实错误。体面地驳回它们。

## When Reviewers Are Right｜审查者说对的时候

Don't dismiss findings just because they're uncomfortable. The whole point of adversarial review is to catch things you'd miss. Signs a finding deserves attention:

> 不要仅仅因为发现让人不舒服就驳回它。对抗性审查的全部意义就是抓住你会漏掉的东西。一个发现值得重视的迹象：

- Multiple models flag the same issue independently (consensus signal)

> - 多个模型独立地标出了同一个问题（共识信号）

- The finding identifies a concrete execution path, not a hypothetical

> - 发现指出了一条具体的执行路径，而不是假设

- The finding reveals a gap in your mental model of the code

> - 发现暴露了你对代码的心智模型里的一个缺口

- You read the finding and think "...yeah, actually"

> - 你读完发现后心想“……嗯，确实”

Be especially careful about dismissing security findings and correctness bugs. These deserve more scrutiny even when they come from a single model.

> 驳回安全方面的发现和正确性 bug 时要格外小心。即使只来自一个模型，它们也值得更多审视。

## Verdict Calibration｜结论的校准

A good verdict is useful, not comprehensive. The user should be able to read the "Act On" section, fix those issues, and ship with confidence. If your "Act On" list has more than 5 items, you're probably not filtering hard enough.

> 好的结论是有用的，而不是面面俱到的。用户应该能读完“Act On（要处理）”一节，修好那些问题，然后放心发布。如果你的“要处理”清单超过 5 项，你多半过滤得还不够狠。

The "Dismissed" section is not busywork. It's a trust mechanism. Showing the user what you rejected and why lets them override your judgment where they disagree. This is more valuable than hiding the rejected findings.

> “Dismissed（已驳回）”一节不是凑数的。它是一种信任机制。向用户展示你否决了什么以及为什么，能让他们在不同意时推翻你的判断。这比把被否决的发现藏起来更有价值。
