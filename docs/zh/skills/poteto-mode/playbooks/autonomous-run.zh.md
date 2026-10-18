<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/autonomous-run.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/autonomous-run.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/autonomous-run.md)

### Autonomous run｜自主运行

**You own the exit condition. Define done, then drive to it without stopping.**

> **退出条件由你负责。先定义什么叫完成，然后不停地推进到那里。**

1. State the exit condition as a checkable predicate before the first iteration (tests green, repro fixed, all N PRs merged, pixel-diff zero).

> 1. 在第一轮之前，把退出条件写成一个可检查的判定（测试全绿、复现已修好、全部 N 个 PR 已合并、像素差异为零）。

2. Pick the wake mechanism using your harness's `/loop` command (built into Cursor and Claude Code, not a pstack skill. Without one, see **Harness** in SKILL.md). An event to watch (CI, a merge, a ref advancing) gets a watcher subagent that wakes you on the event, with a long time-based heartbeat as fallback. No event gets a fixed-interval heartbeat sized to when the result is worth re-checking. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 2. 用你运行环境的 `/loop` 命令选择唤醒机制（Cursor 和 Claude Code 内置，不是 pstack 技能。没有的话，见 SKILL.md 里的 **运行环境**）。有事件可以盯（CI、一次合并、某个引用前进）的，派一个监视子代理在事件发生时唤醒你，再配一个间隔较长的定时心跳作为后备。没有事件的，用固定间隔的心跳，间隔按结果值得重新检查的时机来定。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

3. Each iteration makes the smallest change the evidence justifies, verifies it against the predicate, commits if it advanced, discards changes that didn't help. Belt-and-suspenders that "might help" gets reverted, not left to ride.
   Sequence the work via the **sequence-verifiable-units** principle skill, verifying each unit before the next instead of batching checks at the end.

> 3. 每一轮只做证据所能支持的最小改动，对照判定验证，有进展就提交，没帮助的改动丢弃。“也许有用”的双保险要回滚，不要留着搭便车。
>    按 **sequence-verifiable-units** 原则技能安排工作顺序，每个单元验证后再做下一个，而不是把检查攒到最后一起做。

4. Mid-run discoveries are yours. Address broken skills, related bugs, flaky verifiers, review noise, tooling failures, orphaned follow-ups, and fixable drift yourself via poteto-mode. Put out-of-band fixes in their own PR. Do not park reversible work for the human or use `AskQuestion`. Surface only irreversible actions, genuine product or preference calls no experiment can settle, or a real dead end. Keep the predicate as the main drive, and return to it after each side fix.

> 4. 运行中途的发现归你处理。坏掉的技能、相关的 bug、不稳定的验证器、评审噪音、工具故障、被遗忘的后续事项和可修复的偏差，都通过 poteto-mode 自己处理。主线之外的修复单独开 PR。不要把可逆的工作搁置下来留给人，也不要用 `AskQuestion`。只上报不可逆的操作、没有实验能解决的真正的产品或偏好决定，或者真正的死路。把判定作为主驱动力，每次处理完支线修复都回到它上面。

5. Checkpoint every iteration via the **show-me-your-work** skill, a row for what changed and whether the predicate moved.

> 5. 每一轮都通过 **show-me-your-work** 技能记录检查点，一行写改了什么、判定有没有推进。

6. Stop when the predicate is met. A plateau is not a stop, so keep going and pivot your approach to push past it. Surface a genuine dead end rather than spinning, and never relax the predicate to declare victory.

> 6. 判定满足时停下。平台期不是停下的理由，所以要继续，换一种思路突破它。遇到真正的死路就上报，不要原地打转，也永远不要放宽判定来宣布胜利。

**Reply:** the exit condition, iterations run, what landed, what was discarded, final predicate state.

> **回复：** 退出条件、跑了几轮、落地了什么、丢弃了什么、判定的最终状态。
