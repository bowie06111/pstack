<!-- 中英对照译文。原文：skills/principle-guard-the-context-window/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-guard-the-context-window/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-guard-the-context-window/SKILL.md)

```yaml
name: principle-guard-the-context-window
description: "Apply when context is filling up: large outputs, long files, repeated reads, fan-out planning. Route bulk to subagents; keep summaries in the main thread, not raw payloads."
disable-model-invocation: true
```

> **说明（description）：** 在上下文快满时使用：大量输出、长文件、反复读取、规划扇出。把大块内容交给子代理；主线程里只保留摘要，而不是原始内容。

# Guard the Context Window｜守护上下文窗口

**Why:** Context overflow degrades reasoning quality, creates compression artifacts, and halts progress.

> **原因：** 上下文溢出会降低推理质量，造成压缩带来的失真，并让进展停滞。

**Pattern:**

> **做法：**

- **Size phases and cap scope.** Limit files per phase, set turn budgets, account for mechanism costs.

> - **划定阶段大小并限制范围。** 限制每个阶段的文件数，设定回合预算，把机制本身的开销算进去。
