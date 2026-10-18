<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/bug-fix.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/bug-fix.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/bug-fix.md)

### Bug fix｜修 bug

**You own this task. Plan, review, verify.** Delegate investigation and the fix to subagents, stay in the lead. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> **这个任务由你负责。规划、审查、验证。** 把调查和修复委派给子代理，自己保持主导。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

Be scientific. Every shipped line traces to runtime evidence. Belt-and-suspenders that "might help" is a hypothesis, not a fix. It does not ship. When evidence refutes a hypothesis, revert what it motivated. The smallest change the evidence justifies ships, nothing more.

> 要科学。每一行发布的代码都要能追溯到运行时证据。“也许有用”的双保险是假设，不是修复。它不能发布。当证据否定了一个假设，就回滚由它引出的改动。只发布证据所能支持的最小改动，不多一点。

1. Reproduce it yourself on the matching surface via the control skill (Non-negotiables), even when a debug or instrumentation protocol says to ask the user to reproduce. Ask the user only with a stated, specific reason the control surface cannot reach the target, and only after driving it as far as it goes. If it won't reproduce directly, synthesize the trigger, tighten conditions, or instrument until it fires.

> 1. 通过控制技能在对应的界面上自己复现（见“不可妥协的规则”），即使某个调试或插桩协议说要让用户来复现。只有在能说出控制界面够不到目标的具体理由、并且已经尽可能地操作过之后，才请用户帮忙。如果无法直接复现，就合成触发条件、收紧条件或加插桩，直到它出现。

2. Binary-search the cause. Form the candidate hypotheses, then rule them out until one survives. Seed them with `how` over the affected subsystem and the **why** skill for regression history. Each pass, take the split that cuts the most remaining problem space, get runtime evidence, eliminate. When program state is unclear, add instrumentation or logging and read it as the code runs. Don't guess. Drive a long or stubborn hunt with Cursor's `/loop` command. Confirm the surviving *mechanism* with runtime evidence before the step-3 architect/interrogate fan-out.

> 2. 二分查找原因。列出候选假设，逐个排除，直到只剩一个。用 `how` 梳理受影响的子系统，用 **why** 技能查回归历史，作为假设的来源。每一轮选能切掉最多剩余问题空间的那一刀，拿到运行时证据，排除。程序状态不清楚时，加插桩或日志，在代码运行时读它。不要猜。漫长或顽固的排查用 Cursor 的 `/loop` 命令推进。在第 3 步的 architect/interrogate 扇出之前，用运行时证据确认存活下来的*机制*。

3. Plan the fix. If it crosses a function boundary, `architect` first. Delegate implementation to a subagent using your configured bug-fix model (default `grok-4.7-xhigh-fast`) with a specific scope.

> 3. 规划修复。如果跨越函数边界，先用 `architect`。用你配置的 bug-fix 模型（默认 `grok-4.7-xhigh-fast`）把实现委派给子代理，给出明确的范围。

4. Verify on the same surface. The original repro now passes. "Inconclusive" or wrong-surface is not a pass. Flag it. Unit tests show branch behavior, not bug absence.

> 4. 在同一个界面上验证。原来的复现现在通过了。“无法判断”或测错了界面都不算通过。要标出来。单元测试显示的是分支行为，不能证明 bug 不存在。

5. Stage the commits so the failing repro lands before the fix in git history. See the **tdd** skill for the failing-test-first cadence when the bug has a cheap local test path. Skip it when the test would be expensive, integration-heavy, or unclear.
   This is the canonical **sequence-verifiable-units** principle skill, the failing test first and the fix on top.

> 5. 安排提交顺序，让失败的复现在 git 历史里先于修复落地。bug 有便宜的本地测试路径时，按 **tdd** 技能先写失败测试的节奏来。测试成本高、依赖大量集成或不清楚时，就跳过。
>    这是 **sequence-verifiable-units** 原则技能的标准示范：先有失败的测试，修复叠在上面。

6. Run **Opening a PR**.

> 6. 运行 **Opening a PR**。

**Reply:** what was broken, root cause, fix, how you verified. Paste failing-then-passing repro output verbatim.

> **回复：** 什么坏了、根因、修复、你如何验证的。原样粘贴先失败后通过的复现输出。
