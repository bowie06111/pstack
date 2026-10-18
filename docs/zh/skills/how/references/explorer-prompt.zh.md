<!-- 中英对照译文。原文：skills/how/references/explorer-prompt.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/how/references/explorer-prompt.md](https://github.com/bowie06111/pstack/blob/main/skills/how/references/explorer-prompt.md)

# Explorer Prompt Template｜探索者提示词模板

Build each explorer subagent's prompt from this template. Fill in the placeholders.

> 用这个模板构建每个探索者子代理的提示词。填好占位符。

---

Other explorers are investigating different slices of the same subsystem in parallel. Don't try to cover everything. Focus on your assigned angle and go deep.

> 其他探索者正在并行调查同一个子系统的不同切面。不要试图面面俱到。专注于分配给你的角度，挖深。

## Question｜问题

> {QUESTION}

> > {问题}

## Your Exploration Angle｜你的探索角度

{EXPLORATION_ANGLE}

> {探索角度}

## Output｜输出

### Components Found
The key types, services, classes, and abstractions. For each: name, file path, and a one-sentence description of what it does.

> ### 找到的组件
> 关键的类型、服务、类和抽象。每个都写：名字、文件路径，以及一句话说明它做什么。

### Flow
The execution flow step by step. For each step: what function/method runs, what file it's in, what it does, what it calls next. Include the data that flows between steps.

> ### 流程
> 一步一步的执行流程。每一步都写：运行的是哪个函数/方法、它在哪个文件里、它做什么、它接下来调用什么。包括在步骤之间流动的数据。

### Files Read
Every file you read during exploration, so the explainer can reference them.

> ### 读过的文件
> 探索过程中你读过的每一个文件，以便讲解者引用。

### Boundaries
Where this subsystem connects to other parts of the codebase. The inputs and outputs.

> ### 边界
> 这个子系统与代码库其他部分连接的地方。输入和输出。

### Non-Obvious Things
Anything surprising, historically motivated, or easy to get wrong. Things that look like they should work one way but work another.

> ### 不明显的地方
> 任何出人意料的、有历史原因的或容易弄错的东西。看起来应该这样工作、实际上却那样工作的地方。

### Open Questions
Anything you couldn't fully trace or understand. Be honest about gaps.

> ### 待解问题
> 任何你没能完全追踪或理解的东西。坦诚说明缺口。
