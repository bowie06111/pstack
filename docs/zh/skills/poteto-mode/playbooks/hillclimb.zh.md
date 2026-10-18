<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/hillclimb.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/hillclimb.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/hillclimb.md)

### Hillclimb｜爬坡

**You own the metric and the experiment's integrity. Supervise and review. Delegate the attempts.** For sustained, iterative improvement of one measurable thing against a target. A one-off fix is Bug fix or Perf issue. This is the loop.

> **指标和实验的完整性由你负责。监督和审查。把尝试委派出去。** 用于针对一个目标，持续、迭代地改进一个可测量的东西。一次性修复属于 Bug fix 或 Perf issue。这个是循环。

Core discipline: one change, one measurement, keep or revert. Never stack untested changes, and never claim a win from code inspection (the **prove-it-works** principle skill).

> 核心纪律：一次改动，一次测量，保留或回滚。永远不要叠加未经测试的改动，也永远不要凭读代码宣称取得了改进（**prove-it-works** 原则技能）。

1. Ground the workload and architecture before choosing the metric. Run the **how** skill over the target, name the realistic workload dimensions that can move the result (data size, history, state, concurrency), and select a case that reproduces the user's complaint. If no case reproduces it, fix the repro instead of hillclimbing. Then fix one metric, the direction that counts as better, and a checkable stop predicate that pairs a target with a floor on attempts so a lucky early win can't end the run (the example "at least 50% better than baseline and at least 10 iterations" is this shape). Use the user's numbers when given, otherwise agree them.

> 1. 在选指标之前，先弄清工作负载和架构。用 **how** 技能梳理目标，说出能影响结果的真实工作负载维度（数据量、历史、状态、并发），并选一个能复现用户抱怨的场景。如果没有场景能复现，就去修复现，而不是爬坡。然后确定一个指标、什么方向算更好，以及一个可检查的停止判定，判定要把目标和最少尝试次数配在一起，这样早期侥幸的一次改进不会结束运行（例子“比基线至少好 50%，并且至少 10 轮”就是这种形式）。用户给了数字就用用户的，否则和用户商定。

2. Build the measurement harness, prove its sensitivity, then freeze it (the **build-the-lever** principle skill). Run contrasting realistic workloads and confirm the target case reproduces the symptom while easier cases separate as expected. If the harness cannot distinguish them, revise the workload or metric. Vet the harness with the **benchmark-checklist** skill before you freeze it, and make it print its error count and a count of the work done. Once frozen, one repeatable command emits the metric, sampled enough to clear the noise (median of N, not a single run). Record the baseline metric and a green run of the regression gate (the tests that must keep passing) before any change.

> 2. 构建测量工具，证明它的灵敏度，然后冻结它（**build-the-lever** 原则技能）。跑几组对比鲜明的真实工作负载，确认目标场景能复现症状，而较容易的场景如预期那样区分开来。如果工具分不出它们，就修改工作负载或指标。冻结之前，用 **benchmark-checklist** 技能审查这个工具，并让它打印错误数和完成的工作量。冻结后，一条可重复的命令就能输出指标，采样次数足以压过噪音（取 N 次的中位数，而不是单次运行）。在任何改动之前，记录基线指标，以及一次回归关卡（必须保持通过的那些测试）的绿色运行。

3. Open the decision log via the **show-me-your-work** skill. A `decision.tsv`, one row per attempt: id, hypothesis, change, before, after, delta, tests, verdict (kept or reverted), note. Read it before each attempt. Keep it out of the tree (gitignored).

> 3. 通过 **show-me-your-work** 技能打开决策日志。一个 `decision.tsv`，每次尝试一行：编号、假设、改动、之前、之后、差值、测试、结论（保留或回滚）、备注。每次尝试前读它。不要放进代码树（加入 gitignore）。

4. Ground each hypothesis in the architecture model from step 1, so it names a specific mechanism ("defer X off the boot path because it blocks first paint"), not "try memoizing something". For a perf metric, order hypotheses by the performance mantras in step 2 of the Perf issue playbook (`playbooks/perf-issue.md`). Borrow only their order, not that step's stop rule.

> 4. 每个假设都以第 1 步得到的架构模型为依据，让它说出一个具体的机制（“把 X 移出启动路径，因为它阻塞了首次绘制”），而不是“试着缓存点什么”。对于性能指标，按 Perf issue 剧本（`playbooks/perf-issue.md`）第 2 步的性能口诀给假设排序。只借用它们的顺序，不借用那一步的停止规则。

5. Loop, one hypothesis per iteration:
   - Hand the change to a subagent using your configured hillclimb model (default `grok-4.7-xhigh-fast`) with a tight scope. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section. Supervise and review the diff rather than typing it (the **guard-the-context-window** principle skill). When several independent hypotheses are live, fan them to parallel subagents, each in its own worktree (the **separate-before-serializing-shared-state** principle skill).
   - Measure before and after with the frozen harness, and run the regression gate.
   - Accept only when the metric moves past noise and the gate stays green. Otherwise revert the change in full. A tweak that "might help" is not kept.
   - One commit per accepted fix, staging only the files you changed (`git add <files>`, never `-A`). Log the row either way, kept or reverted.
   Each iteration ends in a check before the next begins (the **sequence-verifiable-units** principle skill). If the run is unattended, borrow only the wake mechanism from the Autonomous run playbook (`playbooks/autonomous-run.md`), not its stop rule.

> 5. 循环，每轮一个假设：
>    - 用你配置的 hillclimb 模型（默认 `grok-4.7-xhigh-fast`）把改动交给子代理，范围要收紧。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。监督和审查 diff，而不是自己去敲（**guard-the-context-window** 原则技能）。同时有几个独立的假设时，把它们扇出给并行的子代理，每个在自己的 worktree 里（**separate-before-serializing-shared-state** 原则技能）。
>    - 用冻结的工具测量前后，并跑回归关卡。
>    - 只有当指标的变化超过噪音、并且关卡保持绿色时才接受。否则完整回滚这个改动。“也许有用”的小调整不保留。
>    - 每个被接受的修复一次提交，只暂存你改过的文件（`git add <files>`，永远不要 `-A`）。不管保留还是回滚，都记下这一行。
>    每一轮都以一次检查结束，然后再开始下一轮（**sequence-verifiable-units** 原则技能）。如果运行无人值守，只借用 Autonomous run 剧本（`playbooks/autonomous-run.md`）的唤醒机制，不借用它的停止规则。

6. Push past the first plateau. On a stall, several rejects in a row, pivot category, combine near-misses, re-read the source, or try something more radical before concluding the hill is climbed. Correctness and simplicity outrank the number. Revert a win that breaks behavior, and keep a simplification that holds the number (the **laziness-protocol** principle skill).

> 6. 突破第一个平台期。停滞时（连续几次被拒绝），换个类别，把差一点成功的组合起来，重读源码，或者在断定已经爬到顶之前试试更激进的做法。正确性和简洁性比数字更重要。破坏了行为的改进要回滚，守住数字的简化要保留（**laziness-protocol** 原则技能）。

7. Stop when the predicate is met, or when the remaining ideas are marginal and not worth their cost. Don't relax the predicate to meet it, and don't quit while cheap untried hypotheses remain. If you are stuck, surface it instead of spinning.

> 7. 判定满足时停下，或者剩下的想法收益微小、不值得付出成本时停下。不要放宽判定来满足它，也不要在还有便宜的、没试过的假设时放弃。如果卡住了，就上报，不要原地打转。

8. Run **Opening a PR** with the accepted commits stacked in the order they landed.

> 8. 运行 **Opening a PR**，被接受的提交按落地顺序堆叠。

**Reply:** the metric and target, baseline to final with the percent delta, iterations run (kept vs reverted), each accepted fix on one line, the `decision.tsv` path, and the best idea you would try next if pushed further.

> **回复：** 指标和目标、从基线到最终的值及百分比变化、跑了几轮（保留和回滚各多少）、每个被接受的修复各一行、`decision.tsv` 的路径，以及如果继续推进你下一个会尝试的最好想法。
