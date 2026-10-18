<!-- 中英对照译文。原文：skills/figure-it-out/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/figure-it-out/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/figure-it-out/SKILL.md)

```yaml
name: figure-it-out
description: "Design an auditable playbook when no narrower one fits: a large migration, an ambitious multi-part change, or work a human reviews after stepping away. Scales rigor to the task, runs a hypothesis loop, and logs decisions. Use for /figure-it-out or 'figure it out'."
disable-model-invocation: true
```

> **说明（description）：** 当没有更窄的剧本适用时，设计一个可审计的剧本：大规模迁移、雄心勃勃的多部分改动，或者人离开后再回来审查的工作。让严谨程度与任务相称，运行假设循环，并记录决策。用于 /figure-it-out 或“figure it out”。

# Figure it out｜想办法搞定（Figure it out）

When the task matches no playbook, design one. The deliverable before any code is the workflow itself: a sequence of phases that scales rigor to the task, runs the scientific method, and leaves a decision trail a human can audit after stepping away.

> 当任务不匹配任何剧本时，就设计一个。在写任何代码之前，交付物就是工作流本身：一系列让严谨程度与任务相称、运行科学方法、并留下人离开后能审计的决策记录的阶段。

## Start｜开始

Open a todolist whose first item is to read the Principles section of the **poteto-mode** skill. Then add the phases below as todos.

> 打开一个待办清单，第一项是阅读 **poteto-mode** 技能的“原则”一节。然后把下面的各个阶段作为待办项加进去。

## Phase A: Frame｜阶段 A：框定

Ground first, then commit. Don't start the run until you can state:

> 先打基础，再承诺。在你能说清以下几点之前，不要开始运行：

- The definition of done as a falsifiable predicate (the **prove-it-works** principle skill).

> - 完成的定义，写成一个可证伪的判定（**prove-it-works** 原则技能）。

- Scope, quantified: rough units and effort, plus the blockers grounding surfaced.

> - 范围，量化：大致的单元数和工作量，以及打基础时发现的阻塞项。

- The rigor level, biased high. One-way doors and high blast radius get more. Reversible low-stakes steps get less. Rigor is gates and artifacts, not "try harder".

> - 严谨程度，偏高为宜。单向门和影响范围大的事项要更严谨。可逆、低风险的步骤可以放松。严谨体现在关卡和产物上，而不是“更努力一点”。

Present the framing and tradeoffs before committing to a long run. Reversible work proceeds (the **never-block-on-the-human** principle skill), but a multi-hour run earns one checkpoint.

> 在承诺一次长时间运行之前，先展示框定和取舍。可逆的工作直接进行（**never-block-on-the-human** 原则技能），但一次要跑好几个小时的运行值得设一个检查点。

## Phase B: Design the workflow｜阶段 B：设计工作流

Decompose into atomic, independently-landable units. Sequence riskiest-unknown-first. Scaffold and verification come before features (the **foundational-thinking** principle skill).

> 拆成原子的、可以独立落地的单元。把风险最大的未知项排在最前面。脚手架和验证先于功能（**foundational-thinking** 原则技能）。

- Build the verification harness before the work, with the baseline captured from the pre-change state, so the check reads as "old value vs new value".

> - 在做工作之前先搭好验证工具，并从改动前的状态记录基线，这样检查读起来就是“旧值对比新值”。

- For one-way-door design decisions, run the **architect** skill (it runs **arena**). Skip it for mechanical work whose shape is already concrete. A second arena over a settled design is over-engineering (the **laziness-protocol** principle skill).

> - 对于单向门的设计决策，运行 **architect** 技能（它会运行 **arena**）。对于形状已经很具体的机械性工作就跳过。对一个已经定下来的设计再跑一次 arena 是过度设计（**laziness-protocol** 原则技能）。

- Decide what fans out. Parallelize only across seams, and give each worker its own worktree or branch (the **separate-before-serializing-shared-state** principle skill). Don't over-fan. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> - 决定哪些要扇出。只在接缝处并行，并给每个工作者自己的 worktree 或分支（**separate-before-serializing-shared-state** 原则技能）。不要过度扇出。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

- Write the designed phase list down. That list is what the human reviews.

> - 把设计好的阶段列表写下来。这份列表就是人要审查的东西。

Then execute the design. Add its steps to the todolist as concrete items between the Phase C and Phase D entries. Run each under the Phase C loop discipline, and weave the Phase D log through them, a row as each step lands, rather than saving the whole trail for the end.

> 然后执行这个设计。把它的步骤作为具体项加到待办清单里，放在阶段 C 和阶段 D 之间。每一步都按阶段 C 的循环纪律运行，并把阶段 D 的日志穿插其中，每一步落地就记一行，而不是把整份记录攒到最后。

## Phase C: Run the loop｜阶段 C：运行循环

Each unit is an experiment. State the hypothesis, make the smallest change, measure against the predicate on the real artifact, keep it if it advanced, revert it if it didn't. Apply the **sequence-verifiable-units** principle skill, verifying each unit before starting the next instead of batching checks at the end.

> 每个单元都是一次实验。说出假设，做最小的改动，在真实产物上对照判定进行测量，有进展就保留，没有就回滚。应用 **sequence-verifiable-units** 原则技能，每个单元验证后再开始下一个，而不是把检查攒到最后一起做。

- Verify by inspecting the artifact, never a self-report. When something passes too easily, suspect the observation method before the system.

> - 通过检查产物来验证，永远不要靠自我报告。当某个东西过得太容易时，先怀疑观察方法，再怀疑系统。

- Pair delegated work with a judge. If a worker games the gate, reset and harden the contract. If the gate itself is wrong, fix the gate in its own change rather than routing around it.

> - 委派出去的工作要配一个评判者。如果工作者钻关卡的空子，就重置并加固约定。如果关卡本身错了，就在单独的改动里修好关卡，而不是绕过它。

- A verdict is VERIFIED, NOT VERIFIED, or INCONCLUSIVE. Inconclusive is not a pass. Don't hide a negative.

> - 结论只有 VERIFIED（已验证）、NOT VERIFIED（未验证）或 INCONCLUSIVE（无法判断）。无法判断不算通过。不要隐藏负面结果。

## Phase D: Keep the audit trail｜阶段 D：保留审计记录

Log the run via the **show-me-your-work** skill. figure-it-out's work is usually ambitious enough to commit the trail so the reviewer can read it in the PR. The trail plus the diff is what lets the human come back and trust the work.

> 通过 **show-me-your-work** 技能记录这次运行。figure-it-out 处理的工作通常足够有野心，值得把记录提交上去，让审查者能在 PR 里读到。记录加上 diff，正是让人回来后能信任这项工作的东西。

## Phase E: Verify and hand back｜阶段 E：验证并交回

Check the whole against the Phase A predicate on the real product, not just the harness. Encode any recurring correction as a gate, a lint rule, a check, or a script (the **encode-lessons-in-structure** principle skill).

> 在真实的产品上，而不只是在验证工具上，对照阶段 A 的判定检查整体。把任何反复出现的纠正编码成关卡、lint 规则、检查或脚本（**encode-lessons-in-structure** 原则技能）。

**Reply:** the playbook you designed, the rigor level and why, the decision-trail path, what's verified against the predicate, and what's still open.

> **回复：** 你设计的剧本、严谨程度及原因、决策记录的路径、对照判定已验证了什么，以及还有什么没解决。
