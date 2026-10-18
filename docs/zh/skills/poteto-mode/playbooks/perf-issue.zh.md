<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/perf-issue.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/perf-issue.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/perf-issue.md)

### Perf issue｜性能问题

**You own the measurement story. Plan, review, verify the numbers.** Tie every fix to a measurement, don't read source instead of measuring.

> **测量这条线索由你负责。规划、审查、验证数字。** 每个修复都要对应一次测量，不要用读源码代替测量。

1. Capture a baseline trace via the matching control skill. Vet the baseline, and each later number, with the **benchmark-checklist** skill.

> 1. 通过对应的控制技能抓取一份基线 trace。用 **benchmark-checklist** 技能审查基线以及之后的每一个数字。

2. `how` to ground hypotheses. Don't claim a perf ceiling without running it first.
   Try the performance mantras in order, cheapest first:
   1. Don't do it. Stop work whose result nothing uses rather than cheapening it.
   2. Do it, but don't do it again.
   3. Do it less.
   4. Do it later.
   5. Do it when they're not looking.
   6. Do it concurrently.
   7. Do it cheaper.

> 2. 用 `how` 为假设打基础。没有先跑一遍，不要声称存在性能上限。
>    按顺序尝试性能口诀，最便宜的先来：
>    1. 别做。结果没人用的工作直接停掉，而不是让它变便宜。
>    2. 做，但别重复做。
>    3. 少做。
>    4. 晚点做。
>    5. 趁没人看时做。
>    6. 并发做。
>    7. 更便宜地做。

   When an earlier mantra meets the target, stop.

>    前面的口诀已经达到目标时，就停下。

3. Plan the fix from the trace. If it crosses a function boundary, `architect` first. Delegate implementation to a subagent using your configured perf-issue model (default `grok-4.7-xhigh-fast`). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section. Review the diff. Capture a post-fix trace.
   Apply the **sequence-verifiable-units** principle skill, verifying each attempt before trying the next.

> 3. 根据 trace 规划修复。如果跨越函数边界，先用 `architect`。用你配置的 perf-issue 模型（默认 `grok-4.7-xhigh-fast`）把实现委派给子代理。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。审查 diff。抓取一份修复后的 trace。
>    应用 **sequence-verifiable-units** 原则技能，每次尝试验证后再试下一个。

4. Parse and compare the artifacts (JSON to sqlite, diff). "Inconclusive" or wrong-surface is not a pass. Flag it.

> 4. 解析并比较产物（JSON 转 sqlite，做 diff）。“无法判断”或测错了界面都不算通过。要标出来。

5. Cite the measurement in the PR.

> 5. 在 PR 里引用测量结果。

6. Run **Opening a PR**.

> 6. 运行 **Opening a PR**。

For sustained improvement against a metric rather than a one-off fix, use the Hillclimb playbook (`playbooks/hillclimb.md`).

> 如果要针对一个指标持续改进而不是一次性修复，使用 Hillclimb 剧本（`playbooks/hillclimb.md`）。

**Reply:** baseline number, post-fix number, delta, artifact path.

> **回复：** 基线数字、修复后数字、差值、产物路径。
