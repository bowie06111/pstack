<!-- 中英对照译文。原文：skills/principle-fix-root-causes/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-fix-root-causes/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-fix-root-causes/SKILL.md)

```yaml
name: principle-fix-root-causes
description: "Apply when debugging. Trace each symptom back to the first place the system went wrong and fix it there. A guard is a fix only when the state it handles is legitimate."
disable-model-invocation: true
```

> **说明（description）：** 在调试时使用。把每个症状追溯到系统最先出错的地方，在那里修。只有当防护处理的状态本身是合法的，防护才算修复。

# Fix Root Causes｜修根因

When debugging, fix the fault, not the place where it surfaced. The symptom is where a wrong state became visible. The root cause is the first place the system did something it should not do, or failed to handle a state it should handle.

> 调试时，修复故障本身，而不是它暴露出来的地方。症状是错误状态变得可见的地方。根因是系统第一次做了不该做的事，或者没有处理一个本该处理的状态的地方。

**Why:** A symptom fix hides the evidence and leaves the fault free to surface somewhere else. Each one makes the system harder to reason about, because the code now carries a workaround for a bug that nobody named.

> **原因：** 只修症状会把证据藏起来，让故障可以在别处冒出来。每一次这样的修补都让系统更难推理，因为代码里多了一个针对没人点名的 bug 的变通办法。

**Pattern:**

> **做法：**

- Reproduce first.

> - 先复现。

- Ask "why" at each step until the answer is a wrong decision in the code, not a consequence of an earlier one.

> - 每一步都问“为什么”，直到答案是代码里的一个错误决定，而不是更早某个错误的后果。

- When stuck, instrument. Read the actual error and the actual state. Do not guess.

> - 卡住时，加插桩。读真实的错误和真实的状态。不要猜。

- Ask whether the state that triggered the symptom is legitimate. If the system may be in that state, the code that broke on it is the root cause, and handling the state there is the fix. If the state should never exist, a check where it broke hides the bug. Find what produced the state. Guards, retries, swallowed errors, clamped values, and discarded state are the usual ways to hide a bug.

> - 问一问触发症状的那个状态是否合法。如果系统可能处于那个状态，那么在它上面出错的代码就是根因，在那里处理这个状态就是修复。如果那个状态根本不该存在，在出错的地方加检查只会藏住 bug。去找是什么产生了这个状态。防护、重试、吞掉的错误、被截断的值和被丢弃的状态，是藏 bug 的常见手法。

- When something fails only after a restart, suspect persisted state first: config files, caches, lock files, serialized state.

> - 当某个东西只在重启后才失败时，先怀疑持久化的状态：配置文件、缓存、锁文件、序列化的状态。

- Check for the pattern, not just the instance. Find every place with the same fault and fix them all.

> - 检查这个模式，而不只是这一个实例。找出所有有同样故障的地方，全部修好。

- If a workaround needs a paragraph-long comment to justify it, the code is wrong. Fix the code, not the comment.

> - 如果一个变通办法需要一整段注释来解释它为什么合理，那代码就是错的。修代码，而不是修注释。

- If the deciding fact took a deep dig to find, propose the cheap, always-on signal that would have shown it at first look.

> - 如果关键事实要深挖才找得到，提议一个便宜的、一直开着的信号，让人第一眼就能看到它。
