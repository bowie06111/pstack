<!-- 中英对照译文。原文：skills/arena/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/arena/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/arena/SKILL.md)

```yaml
name: arena
description: "Spawn N parallel candidates at the same task, pick a base, graft the strongest parts of the losers into it. Use for /arena, 'arena this', 'throw it in the arena', or when one attempt at a non-trivial artifact would lock in the wrong shape."
disable-model-invocation: true
```

> **说明（description）：** 派生 N 个并行的候选者做同一个任务，选一个底稿，把落选方案最强的部分嫁接进去。用于 /arena、“arena this”、“扔进竞技场”，或者当对一个非平凡产物只尝试一次就会锁定错误形状的时候。

# Arena｜Arena（竞技场）

Open a todolist with one entry per phase (Frame, Fan out, Cross-judge, Pick, Graft, Verify) before launching anything.

> 在启动任何东西之前，打开一个待办清单，每个阶段一项（框定、扇出、交叉评判、选底稿、嫁接、验证）。

## Phase A: Frame｜阶段 A：框定

Every candidate gets the same prompt, so the prompt is the contract.

> 每个候选者拿到的提示词都一样，所以提示词就是约定。

1. State the artifact each candidate is producing.

> 1. 说明每个候选者要产出的产物。

2. Derive the rubric, 3-6 concrete gradeable criteria for what success looks like on *this* task. It is the picker's tool in Phase D. Candidates only see the task.

> 2. 推导评分标准，3 到 6 条具体的、可打分的标准，说明在*这个*任务上成功是什么样子。它是阶段 D 里选择者的工具。候选者只看到任务。

3. Pick the runners. Use the `arena runners` line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses). If the file or that line is missing, default to one each on `claude-opus-5-5-xhigh` and `grok-4.7-xhigh-fast`. An `auto` or `inherit-parent` entry in this line or the cross-judge line means the parent model, so omit `model` for it. If your subagent tool rejects a configured entry, run that seat on its family's default and say so. Families go by prefix: `claude-*`, and `grok-*` with `cursor-grok-*` and `composer-*`. A refused `composer-*` entry runs on `grok-4.7-xhigh-fast`. With no family match, use `claude-opus-5-5-xhigh`. If it rejects a default, use the closest valid slug of the same family from its error message or your harness's model list. Spawn more runners when the arena covers multiple design directions. Use the same model N times when the work is generation-bound rather than judgment-sensitive. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 3. 选择执行者。使用 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的 `arena runners` 这一行。如果文件或这一行不存在，默认 `claude-opus-5-5-xhigh` 和 `grok-4.7-xhigh-fast` 各一个。这一行或交叉评委那一行里的 `auto` 或 `inherit-parent` 条目表示父模型，所以对它省略 `model`。如果你的子代理工具拒绝了某个配置的条目，就让那个位置运行它所属家族的默认模型，并说明。家族按前缀划分：`claude-*`；`grok-*` 与 `cursor-grok-*` 和 `composer-*` 同属一族。被拒绝的 `composer-*` 条目改用 `grok-4.7-xhigh-fast` 运行。没有匹配的家族时，用 `claude-opus-5-5-xhigh`。如果它拒绝了某个默认值，就从它的错误信息或你运行环境的模型列表里，选同一家族中最接近的有效模型名。当竞技场涵盖多个设计方向时，派生更多执行者。当工作受限于生成量而不是对判断敏感时，同一个模型用 N 次。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

4. Give each candidate its own output location (a git worktree where possible, otherwise `/tmp/arena-<slug>/candidate-<n>/`), per the **separate-before-serializing-shared-state** principle skill.

> 4. 按 **separate-before-serializing-shared-state** 原则技能，给每个候选者各自的输出位置（尽可能用 git worktree，否则用 `/tmp/arena-<slug>/candidate-<n>/`）。

## Phase B: Fan out｜阶段 B：扇出

Spawn all N subagents in one message with `run_in_background: true`, each with the task, the path to the shared grounding, its own output path, and instructions to produce the artifact and a short rationale naming the alternatives it considered and what it rejected.

> 在一条消息里派生全部 N 个子代理，`run_in_background: true`，每个都带上任务、共享基础材料的路径、它自己的输出路径，以及指示：产出产物，并附一份简短的理由，说出它考虑过的备选方案以及否决了什么。

If a candidate produces no output, proceed with N-1 and note the dropout.

> 如果某个候选者没有产出，就用 N-1 个继续，并记下掉线情况。

## Phase C: Cross-judge｜阶段 C：交叉评判

After all Phase B candidates complete, choose one model from the `arena cross-judge pool` line in the pstack settings file. If the file or that line is missing, choose from `claude-opus-5-5-xhigh` and `grok-4.7-xhigh-fast`. Prefer a different model family from the parent's. Spawn one readonly judge subagent on that model. It sees the rubric and the candidates by path label, scores each criterion, and recommends a base with rationale. It runs in parallel with your reading in Phase D. Don't spawn it while candidates are still writing.

> 阶段 B 的所有候选者都完成后，从 pstack 设置文件里的 `arena cross-judge pool` 这一行选一个模型。如果文件或这一行不存在，就在 `claude-opus-5-5-xhigh` 和 `grok-4.7-xhigh-fast` 之间选。优先选和父代理不同的模型家族。在那个模型上派生一个只读的评委子代理。它通过路径标签看到评分标准和各个候选方案，对每条标准打分，并推荐一个底稿及理由。它和你在阶段 D 的阅读并行进行。在候选者还在写的时候不要派生它。

## Phase D: Pick a base｜阶段 D：选底稿

Read every candidate end to end before picking. Score each against the rubric criterion by criterion, not on holistic feel, and compare with the cross-judge. Agreement on the base confirms the pick. Disagreement means one of you is biased or the rubric was ambiguous. Read both rationales before deciding.

> 选之前，把每个候选方案从头到尾读一遍。对照评分标准逐条打分，而不是凭整体感觉，然后和交叉评委的结果比较。在底稿上意见一致，就确认了选择。意见不一致，说明你们之中有一方有偏见，或者评分标准有歧义。决定之前把两份理由都读一遍。

Pick the base a future maintainer can extend most easily without breaking invariants. When two feel tied, prefer the cleaner boundary or smaller API, per the Laziness Protocol.

> 选一个将来的维护者最容易扩展、又不会破坏不变量的底稿。两个感觉不相上下时，按懒惰协议，选边界更干净或 API 更小的那个。

## Phase E: Graft｜阶段 E：嫁接

Walk each losing candidate once more for what is worth porting into the base, usually one or two things per candidate, not most of it. Fold each graft in by hand, per the **redesign-from-first-principles** principle skill, so the result stays coherent under one mental model. Don't paste mechanically.

> 再把每个落选的候选方案过一遍，看有什么值得移植到底稿里，通常每个候选方案一两样东西，而不是大部分内容。按 **redesign-from-first-principles** 原则技能，手工把每一处嫁接融合进去，让结果在同一个心智模型下保持连贯。不要机械地粘贴。

If the candidates converge on one shape, that is strong agreement. Note it and ship the consensus shape. No graft is needed. If they wildly diverge, Phase A was under-specified. Reframe and re-run rather than averaging the divergence.

> 如果候选方案收敛到同一个形状，那就是强烈的共识。记下来，发布这个共识形状。不需要嫁接。如果它们分歧极大，说明阶段 A 规定得不够。重新框定并重跑，而不是把分歧取平均。

## Phase F: Verify｜阶段 F：验证

Verify the synthesized artifact like any other output, per the **prove-it-works** principle skill. If verification finds a problem the arena missed, either Phase A was wrong (re-frame and re-run) or a candidate caught it and you missed the graft (go back to Phase E). Don't paper over it.

> 按 **prove-it-works** 原则技能，像验证任何其他产出一样验证综合后的产物。如果验证发现了竞技场漏掉的问题，要么是阶段 A 错了（重新框定并重跑），要么是某个候选方案其实抓到了而你漏掉了嫁接（回到阶段 E）。不要掩盖它。

## Outputs｜输出

One synthesized artifact, with a short synthesis note alongside naming the base and why, the cross-judge's verdict, each graft with its source candidate, what was rejected and why, any convergence or dropouts, and the verification result.

> 一个综合后的产物，旁边附一份简短的综合说明：底稿是哪个以及原因、交叉评委的结论、每一处嫁接及其来源候选方案、否决了什么以及原因、任何收敛或掉线情况，以及验证结果。
