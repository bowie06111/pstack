<!-- 中英对照译文。原文：skills/architect/references/runner-prompt.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/architect/references/runner-prompt.md](https://github.com/bowie06111/pstack/blob/main/skills/architect/references/runner-prompt.md)

# Architect runner prompt｜Architect 执行者提示词

The orchestrator passes this file through to every parallel candidate runner during Phase B and fills in the variable inputs around it: the task, the Phase A grounding artifacts, the isolated working directory, and the path to write outputs. The working directory is a git worktree when available, otherwise a per-runner subdirectory under the sketch dir. What matters is independence between candidates.

> 在阶段 B，编排者会把这个文件原样传给每一个并行的候选方案执行者，并在它周围填上可变的输入：任务、阶段 A 的基础材料、隔离的工作目录，以及写入输出的路径。有 git worktree 时工作目录就是 worktree，否则是草图目录下每个执行者各自的子目录。重要的是候选方案之间相互独立。

You are producing one candidate design in architect's parallel exploration. Read the **architect** skill in full first. That's the workflow you're inside. Output a candidate design package: type sketch, function signatures, module map, and prose rationale shaped per [`rationale-template.md`](rationale-template.zh.md).

> 你正在 architect 的并行探索中产出一个候选设计。先完整阅读 **architect** 技能。那就是你所处的工作流。输出一个候选设计包：类型草图、函数签名、模块地图，以及按 [`rationale-template.md`](rationale-template.zh.md) 格式写的文字理由。

Apply the following discipline. The orchestrator compares candidates on these axes to pick a base.

> 遵守下面的纪律。编排者会在这些维度上比较候选方案来选底稿。

- Caller's usage first. Write the README-style usage and two or three real call sites before the types, then derive the type sketch from them. The usage is the spec. The two must agree, so reconcile the sketch to the usage, not the reverse.

> - 调用方的用法优先。在写类型之前，先写 README 式的用法和两三个真实的调用点，然后从中推导出类型草图。用法就是规格。两者必须一致，所以让草图向用法看齐，而不是反过来。

- Data structures first. Get the core types right and the code becomes obvious. Trace each dominant access pattern through the proposed structure. If the answer is "we'll add a map / index / cache later," the structure is wrong.

> - 数据结构优先。把核心类型做对，代码就会变得显而易见。让每种主要的访问模式在提议的结构里走一遍。如果答案是“我们以后再加一个映射/索引/缓存”，那结构就是错的。

- Interface depth. Compare the capability hidden behind the public surface relative to the size of that surface. Prefer a simple interface that pulls complexity into the callee, even when the implementation becomes less simple. Do not put transport or wire types on the public API. Parse into domain types behind the interface.

> - 接口深度。比较公开接口背后隐藏的能力相对于接口本身的大小。优先选择把复杂度拉进被调用方的简单接口，即使实现因此变得不那么简单。不要把传输或线路格式的类型放到公开 API 上。在接口背后解析成领域类型。

- Shared state: if two actors might both write, ask "what happens?" If the answer isn't "nothing," default to per-actor state with a merge at the read boundary, per the **separate-before-serializing-shared-state** principle skill.

> - 共享状态：如果两个参与者都可能写入，就问“会发生什么？”如果答案不是“什么都不会发生”，就按 **separate-before-serializing-shared-state** 原则技能，默认每个参与者各有自己的状态，在读取的边界上合并。

- Make boundaries visible. `not implemented` errors for bodies, `// TODO` pseudocode for tricky logic, doc comments stating intent and invariants. A reader should trace data from input to output by reading types and signatures alone.

> - 让边界可见。函数体用 `not implemented` 错误，复杂的逻辑用 `// TODO` 伪代码，用文档注释说明意图和不变量。读者应该只读类型和签名就能追踪数据从输入到输出的路径。

- Encode invariants in types: hard-to-misuse types > runtime checks > prose comments, per the **encode-lessons-in-structure** principle skill.

> - 把不变量编码进类型：难以误用的类型 > 运行时检查 > 文字注释，按 **encode-lessons-in-structure** 原则技能。

- Validate at boundaries, trust types inside, per the **boundary-discipline** principle skill. Business logic as pure functions. The shell stays thin.

> - 在边界校验，内部信任类型，按 **boundary-discipline** 原则技能。业务逻辑写成纯函数。外壳保持薄。

- Single source of truth per invariant. Derive instead of sync.

> - 每个不变量只有一个事实来源。推导而不是同步。

- Idempotent state transitions where applicable, per the **make-operations-idempotent** principle skill. Ask what happens if the operation runs twice or crashes halfway.

> - 适用时让状态转换幂等，按 **make-operations-idempotent** 原则技能。问一问操作跑两次或半路崩溃会怎样。

- Short call chains. If tracing the flow needs more than three files, flatten the hierarchy, per the **laziness-protocol** and **minimize-reader-load** principle skills.

> - 调用链要短。如果追踪流程需要超过三个文件，就把层级压平，按 **laziness-protocol** 和 **minimize-reader-load** 原则技能。

You are one of the parallel runners, each on a different model. Produce the best design your model can make. Don't hedge against the others. Differences between candidates are the signal used to pick a base and graft. Converging on a safe-looking middle defeats the exploration.

> 你是并行执行者之一，每个执行者用的是不同的模型。拿出你的模型能做出的最好设计。不要为了和别人对冲而留余地。候选方案之间的差异正是用来选底稿和嫁接的信号。都向一个看起来稳妥的中间方案靠拢，会让探索失去意义。
