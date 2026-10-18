<!-- 中英对照译文。原文：skills/principle-make-operations-idempotent/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-make-operations-idempotent/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-make-operations-idempotent/SKILL.md)

```yaml
name: principle-make-operations-idempotent
description: "Apply when designing commands, lifecycle steps, or processing loops that run amid crashes, restarts, and retries. Converge to the same end state regardless of partial prior runs."
disable-model-invocation: true
```

> **说明（description）：** 在设计会在崩溃、重启和重试中运行的命令、生命周期步骤或处理循环时使用。不管之前的运行做到哪一步，都收敛到同一个最终状态。

# Make Operations Idempotent｜操作幂等

Design operations so they converge to the correct state regardless of how many times they run or where the previous run stopped. Every state-mutating operation should answer: "What happens if this runs twice? What happens if the previous run crashed halfway?"

> 设计操作时，让它们不管运行多少次、上一次运行停在哪里，都能收敛到正确的状态。每个修改状态的操作都应该回答：“这个跑两次会怎样？如果上一次运行半路崩溃了会怎样？”

**Why:** Commands, lifecycle operations, and processing loops run where crashes, restarts, and retries are normal. If partial state changes the next run's outcome, every restart becomes a debugging session.

> **原因：** 命令、生命周期操作和处理循环运行的环境里，崩溃、重启和重试是常态。如果残留的部分状态会改变下一次运行的结果，每次重启都会变成一次调试。

**The idea:** derive each step from the state that exists, not from what the previous run meant to do. Read the current state, compare it to the goal, and apply only the difference. A step written this way is safe to repeat, because a repeat finds nothing left to do.

> **思路：** 每一步都从现存的状态推导出来，而不是从上一次运行本来想做什么推导出来。读取当前状态，与目标比较，只应用差异部分。这样写的步骤可以安全地重复，因为重复时会发现已经没什么要做的了。

Anything a run leaves behind, such as a lock, a temporary file, a half-written record, or a claimed job, must be recognizable as stale by the next run. Judge staleness by evidence that stays true wherever the previous run could have stopped, not only in the common case. A marker that the next run cannot judge turns every crash into manual cleanup. For example, write to a temporary path and rename it into place, or compare artifacts by content instead of by creation order.

> 运行留下的任何东西，比如一把锁、一个临时文件、一条写了一半的记录或一个被认领的任务，都必须能被下一次运行识别为过期。判断是否过期所依据的证据，要在上一次运行可能停下的任何位置都成立，而不只是在常见情况下成立。下一次运行无法判断的标记，会把每次崩溃都变成手工清理。比如，先写到临时路径再重命名到位，或者按内容而不是按创建顺序来比较产物。

**The test:**

> **检验方法：**

1. What happens if this runs twice in a row?

> 1. 连续跑两次会怎样？

2. What happens if the previous run crashed at every possible point?

> 2. 如果上一次运行在每一个可能的位置崩溃，会怎样？

3. Does re-execution converge to the same end state?

> 3. 重新执行是否会收敛到同一个最终状态？

If any answer is "it depends on what state was left behind," the operation needs a reconciliation step.

> 如果有任何一个答案是“取决于留下了什么状态”，这个操作就需要一个对账步骤。
