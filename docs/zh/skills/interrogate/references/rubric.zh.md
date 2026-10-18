<!-- 中英对照译文。原文：skills/interrogate/references/rubric.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/interrogate/references/rubric.md](https://github.com/bowie06111/pstack/blob/main/skills/interrogate/references/rubric.md)

# Review Rubric｜审查评分标准

Review through whichever lenses are relevant. Not every lens applies to every change.

> 用相关的视角来审查。不是每个视角都适用于每个改动。

## Correctness｜正确性

Does the code actually do what the intent says it should?

> 代码真的做到了意图所说的事情吗？

- Edge cases: empty inputs, nil/undefined, boundary values, concurrent access

> - 边界情况：空输入、nil/undefined、边界值、并发访问

- Error handling: are errors caught, propagated, or silently swallowed?

> - 错误处理：错误是被捕获、向上传递，还是被悄悄吞掉了？

- Off-by-one, type coercion, integer overflow, string encoding

> - 差一错误、类型强转、整数溢出、字符串编码

- State management: race conditions, stale closures, dangling references

> - 状态管理：竞态条件、过期的闭包、悬空引用

- Do both the happy path and the sad path work?

> - 正常路径和异常路径都能工作吗？

- Idempotency: what happens if this operation runs twice, or if a previous run crashed halfway? If the answer is "it depends on what state was left behind," a reconciliation step is missing.

> - 幂等性：这个操作跑两次会怎样，或者上一次运行半路崩溃了会怎样？如果答案是“取决于留下了什么状态”，就缺少一个对账步骤。

- Concurrency: if multiple actors can touch the same mutable state (files, branches, shared data), is access serialized structurally (locks, sequential phases, exclusive ownership), or by conventions that won't hold?

> - 并发：如果多个参与者能碰同一份可变状态（文件、分支、共享数据），访问是用结构手段串行化的（锁、顺序阶段、独占所有权），还是靠守不住的约定？

When you find a potential bug, trace the execution path. Don't just flag "this could be nil". Show the call chain that makes it nil.

> 当你发现一个潜在的 bug 时，追踪执行路径。不要只是标一句“这里可能是 nil”。展示让它变成 nil 的调用链。

## Root Causes vs. Symptoms｜根因还是症状

Is the code fixing the actual problem or papering over a symptom?

> 代码是在修真正的问题，还是在掩盖一个症状？

Answering this often requires looking beyond the changed files. Read the surrounding code (callers, callees, type definitions, sibling modules) and understand the architecture the change lives in. Use the tools available to you (Read, Grep, Glob) to follow the call chain, read the types, and understand why the code exists before judging whether the change addresses the right layer.

> 回答这个问题经常需要看改动文件之外的东西。读周围的代码（调用方、被调用方、类型定义、同级模块），理解改动所在的架构。用你手头的工具（Read、Grep、Glob）顺着调用链走、读类型、弄清代码为什么存在，然后再判断这个改动是否处理在了正确的层次上。

- Guard clauses that mask a deeper invariant violation

> - 掩盖了更深层不变量被违反的守卫语句

- Retry logic that hides a broken contract

> - 藏起了一个被破坏的约定的重试逻辑

- Type casts that silence a modeling error

> - 让建模错误闭嘴的类型强转

- A workaround: why is it needed? What would a proper fix look like?

> - 一个变通办法：为什么需要它？正确的修复应该是什么样？

- A fix in module A that should really be a fix in module B's contract

> - 一个在模块 A 里的修复，其实应该是对模块 B 约定的修复

- Instructions where structure would be better: if the fix is a comment saying "don't do X" or a convention someone has to remember, ask whether it could instead be a type constraint, a lint rule, or a runtime check that makes the wrong thing impossible

> - 本该用结构的地方用了指示：如果修复是一条写着“不要做 X”的注释，或者一个需要有人记住的约定，就问一问它能不能改成类型约束、lint 规则或运行时检查，让错误的做法变得不可能

## Structural Integrity｜结构完整性

Does the code fit well into the system it's part of?

> 代码和它所属的系统契合吗？

- Boundary discipline: is validation at system boundaries, or scattered through business logic? Validate data once where it enters the system, then trust it internally.

> - 边界纪律：校验是在系统边界上，还是散落在业务逻辑里？数据进入系统时校验一次，然后在内部信任它。

- Abstraction level: is the code mixing high-level orchestration with low-level detail?

> - 抽象层次：代码是不是把高层编排和底层细节混在了一起？

- Coupling: does this change introduce dependencies that will make future changes harder?

> - 耦合：这个改动是否引入了会让将来的改动更困难的依赖？

- Data model fit: do the data structures match the actual access patterns?

> - 数据模型的契合：数据结构和实际的访问模式匹配吗？

- Bolted-on vs. integrated: if the new requirement had been known from the start, would the code look like this, or does the change feel patched onto the existing design?

> - 硬接上去的还是融为一体的：如果一开始就知道这个新需求，代码会是这个样子吗，还是这个改动感觉像是补丁打在了现有设计上？

- Legacy dual-paths: does the change introduce a new API while keeping the old one alive? If there are no external consumers, migrate callers and delete the old path in the same wave. Don't leave compatibility layers that will become permanent.

> - 遗留的双路径：这个改动是否引入了新 API，同时还让旧的继续存在？如果没有外部使用者，就在同一波里迁移调用方并删除旧路径。不要留下会变成永久的兼容层。

Don't penalize simple code for lacking abstraction. Premature abstraction is worse than duplication.

> 不要因为简单的代码缺少抽象就扣分。过早的抽象比重复更糟。

## Verification｜验证

Can you tell that this code works from reading it?

> 光读代码，你能看出它是能用的吗？

- Are there tests? Do they test behavior or implementation details?

> - 有测试吗？它们测的是行为还是实现细节？

- Are there assertions/invariants that would catch regressions?

> - 有能抓住回归的断言/不变量吗？

- If this is a bug fix: is there a test for the bug?

> - 如果这是 bug 修复：有针对这个 bug 的测试吗？

- If this touches an integration boundary: is the full path tested?

> - 如果这触及集成边界：完整的路径测试过了吗？

- Check the real thing, not a proxy. If the code checks liveness via file mtime or cached state instead of reading the actual value, that's a verification gap.

> - 检查真实的东西，而不是代理指标。如果代码通过文件修改时间或缓存的状态来检查存活，而不是读取真实的值，那就是一个验证缺口。

- For delegated or async work: does the code verify actual output artifacts, or does it trust self-reports and summaries?

> - 对于委派出去的或异步的工作：代码是验证了真实的输出产物，还是相信了自我报告和总结？

## Complexity Budget｜复杂度预算

Is the complexity justified by what the code accomplishes?

> 代码所完成的事情配得上它的复杂度吗？

- Code that could be simpler without losing correctness or clarity

> - 在不损失正确性或清晰度的前提下本可以更简单的代码

- Abstractions that serve only one call site

> - 只服务于一个调用点的抽象

- Configuration or parameterization for cases that don't exist yet

> - 为还不存在的情况准备的配置或参数化

- Dead code, unused imports, vestigial parameters

> - 死代码、未使用的导入、残留的参数

- Over-engineering: "just in case" code paths with no current callers

> - 过度设计：目前没有调用方的“以防万一”的代码路径

- Obsolete compatibility paths kept alive for transitional stability that's no longer needed. If the migration is done, delete the scaffolding

> - 为了已不再需要的过渡期稳定而保留的过时兼容路径。迁移完成了，就删掉脚手架

- Does the user experience justify the complexity? Every feature, control, and option should earn its place. Half-finished features are worse than missing ones.

> - 用户体验配得上这份复杂度吗？每个功能、控件和选项都应该有存在的理由。做了一半的功能比没有更糟。

Simpler is better unless simpler is wrong.

> 越简单越好，除非简单是错的。

## Security｜安全

For each security finding, trace the input path through the code and show it.

> 对每个安全发现，追踪输入在代码里的路径并展示出来。

- User input flowing to dangerous sinks (SQL, shell, eval, innerHTML) without sanitization

> - 用户输入未经净化就流向危险的去处（SQL、shell、eval、innerHTML）

- Authentication/authorization gaps in new endpoints

> - 新端点上的认证/授权缺口

- Secrets in code, logs, or error messages

> - 代码、日志或错误信息里的密钥

- TOCTOU (time-of-check-time-of-use) in security-critical paths

> - 安全关键路径上的 TOCTOU（检查时与使用时不一致）问题
