<!-- 中英对照译文。原文：skills/principle-separate-before-serializing-shared-state/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-separate-before-serializing-shared-state/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-separate-before-serializing-shared-state/SKILL.md)

```yaml
name: principle-separate-before-serializing-shared-state
description: "Apply when concurrent actors might write to the same file, branch, key, or state object. Eliminate the sharing first; serialize structurally only when one shared writer is a real invariant."
disable-model-invocation: true
```

> **说明（description）：** 在并发参与者可能写同一个文件、分支、键或状态对象时使用。先消除共享；只有当“单一共享写入者”是真正的不变量时，才用结构手段串行化。

# Separate Before Serializing Shared State｜先分离再串行化共享状态

When concurrent actors might share mutable state, first ask whether they need the same mutable object. If not, eliminate the sharing. When sharing is real, enforce serialization structurally: lockfiles, sequential phases, exclusive ownership. Instructions and conventions are not concurrency control.

> 当并发参与者可能共享可变状态时，先问它们是否需要同一个可变对象。如果不需要，就消除共享。当共享是真实需要的，就用结构手段强制串行化：锁文件、顺序阶段、独占所有权。指示和约定不算并发控制。

**Why:** Concurrent writes to shared state create race conditions that are intermittent, hard to reproduce, and expensive to debug.

> **原因：** 对共享状态的并发写入会产生竞态条件，它们时有时无、难以复现，调试代价很高。

**Pattern:**

> **做法：**

1. **Identify shared mutable state** (files both read and write, branches both push to, APIs both define and consume).

> 1. **找出共享的可变状态**（双方都读写的文件、双方都推送的分支、双方都定义和使用的 API）。

2. **Default: eliminate the shared write target.** Ask: do these actors need one canonical object, or are they publishing independent facts? Give each actor its own owned file, key, branch, or state directory, and merge only at the read/reporting boundary. Two workers writing their own `lastX` field into one `state.json` is still shared mutation. `indexer-state.json` + `metrics-state.json` is not.

> 2. **默认：消除共享的写入目标。** 问：这些参与者需要一个规范的对象，还是在发布各自独立的事实？给每个参与者自己拥有的文件、键、分支或状态目录，只在读取/报告的边界上合并。两个工作者把各自的 `lastX` 字段写进同一个 `state.json`，仍然是共享修改。`indexer-state.json` 加 `metrics-state.json` 就不是。

3. **Only when one shared write target is a real invariant, serialize access structurally** (lockfiles, sequential phases, single-writer actor, or atomic compare-and-swap). Treat "we need a lock" as a design smell to check, not as the default answer.

> 3. **只有当单一共享写入目标是真正的不变量时，才用结构手段串行化访问**（锁文件、顺序阶段、单一写入者，或原子的比较并交换）。把“我们需要一把锁”当作一个需要检查的设计异味，而不是默认答案。
