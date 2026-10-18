<!-- 中英对照译文。原文：skills/show-me-your-work/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/show-me-your-work/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/show-me-your-work/SKILL.md)

```yaml
name: show-me-your-work
description: "Keep a reviewable decision trail for long-running or unattended work: a TSV log with one row per decision (what, why, evidence, result). Local by default; commit it when a reviewer needs the trail to trust the result. Use for /show-me-your-work, autonomous or multi-phase runs, or work a human reviews after stepping away."
disable-model-invocation: true
```

> **说明（description）：** 为长时间运行或无人值守的工作保留一份可审查的决策记录：一个 TSV 日志，每个决定一行（做了什么、为什么、证据、结果）。默认只放在本地；当审查者需要这份记录才能信任结果时再提交。用于 /show-me-your-work、自主或多阶段的运行，或者人离开后再回来审查的工作。

# Show me your work｜Show me your work（展示你的工作）

Keep one canonical log.

> 只保留一份规范的日志。

## The format｜格式

A single TSV file, one row per decision. Cells stay single-line. Evidence is a pointer, not prose.

> 一个 TSV 文件，每个决定一行。单元格保持单行。证据是一个指针，不是一段文字。

Copy `references/decision-log-template.tsv` (the header row) to start a clean log. Columns:

> 复制 `references/decision-log-template.tsv`（表头行）来开始一份干净的日志。各列：

- **ts.** ISO8601 timestamp.

> - **ts。** ISO8601 时间戳。

- **phase.** The phase or workstream.

> - **phase。** 阶段或工作流。

- **decision.** What was chosen or done, one line.

> - **decision。** 选择了或做了什么，一行。

- **why.** The reason in plain words. If a principle drove it, say it plainly, not as a jargon tag.

> - **why。** 用平实的话说的理由。如果是某条原则促成的，就直白地说出来，而不是贴一个术语标签。

- **evidence.** A link or path that proves it: commit SHA, PR number, `file:line`, or an artifact, trace, or screenshot path. Never a paragraph.

> - **evidence。** 能证明它的链接或路径：提交 SHA、PR 编号、`文件:行`，或者一个产物、trace 或截图的路径。永远不要写成一段话。

- **result.** The outcome or predicate state: `tests green`, `reverted`, `pixel-diff 0`, `INCONCLUSIVE`, `open`.

> - **result。** 结果或判定状态：`tests green`、`reverted`、`pixel-diff 0`、`INCONCLUSIVE`、`open`。

An example, plain-spoken so a reviewer reads it at a glance.

> 一个例子，用平实的话写，让审查者一眼就能读懂。

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	threw out a helper's work because its screenshots were blank	checked the real files instead of trusting its summary	worktree reset	reverted, tightened the instructions for next time
```

> 上面代码块的中文版：

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	先清点了工作量，大约 100 个组件，约 75 小时	想在开始长时间运行之前知道规模	commit 3a9f1c2	发现了 5 件需要先理清的事
2026-05-24T09:40:00Z	harness	在改任何东西之前，先给旧版本截了图	这样可以对比新旧，抓住任何视觉变化	scripts/snapshot.sh, baseline/	保存了 120 张参考截图
2026-05-24T11:15:00Z	widget	把组件样式迁过来，外观不变	保持改动小、结果完全一致	commit 7c21e0a, pixel-diff 0	看起来完全一样，测试通过
2026-05-24T12:30:00Z	widget	丢弃了一个帮手的工作，因为它的截图是空白的	检查了真实文件，而不是相信它的总结	worktree reset	已回滚，为下次收紧了指示
```

## Logging a row｜记录一行

Write each entry the way you'd tell a teammate what you did. Plain words, concrete actions, no AI speak or abstract jargon (the **unslop** skill applies to log text too).

> 像告诉队友你做了什么那样写每一条。平实的话、具体的动作，不要 AI 腔或抽象的术语（**unslop** 技能同样适用于日志文字）。

Use the helper `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`. It stamps `ts`, writes the header on first use, strips stray tabs/newlines, and prefixes any cell starting with `=`, `+`, `-`, or `@` with a single quote. A bare `printf` appending a row works too, but mind those same bytes if cells come from generated or user-supplied text.

> 用辅助脚本 `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`。它会打上 `ts`，第一次使用时写入表头，去掉多余的制表符和换行，并在任何以 `=`、`+`、`-` 或 `@` 开头的单元格前加一个单引号。直接用 `printf` 追加一行也行，但如果单元格内容来自生成的或用户提供的文字，要注意同样这几个字符。

Log decision points and checkpoints, not every action: a fork chosen, a unit completed with its verification result, a pivot or revert with its trigger, a blocker surfaced, a gate fixed. For loop runs, one row per iteration. Skip the trivial and self-evident.

> 记录决策点和检查点，而不是每一个动作：选择了某条分岔、完成了一个单元及其验证结果、一次转向或回滚及其触发原因、出现了一个阻塞项、修好了一道关卡。对于循环运行，每轮一行。跳过琐碎的和不言自明的。

A run is one agent conversation, including its later turns and any summary of it. A pickup, a replacement agent, or a new chat starts a new run. When a run adds to a log that already has rows, its first row has phase `start`, and so does its first row after another run's `start` row. So a run that comes back to a log in a later turn first reads the log's last rows to see whether another run wrote since. A `start` row names the `ts` range of the rows before it that this run did not write, and its evidence names this run, such as its agent id. Use phase `start` for nothing else.

> 一次运行就是一个代理对话，包括它之后的回合以及对它的任何总结。一次接手、一个替代的代理或一个新对话，开始的是一次新的运行。当一次运行往一份已经有内容的日志里追加时，它的第一行的 phase 是 `start`；在另一次运行的 `start` 行之后它写的第一行也是如此。所以一次在之后的回合回到日志的运行，要先读日志最后几行，看看期间有没有别的运行写过。`start` 行要写明它之前那些不是本次运行写的行的 `ts` 范围，它的 evidence 写明本次运行，比如它的代理 id。phase `start` 不用于其他任何用途。

## Where it lives｜放在哪里

By default the log is a working artifact, not committed. Keep it at `decisions.tsv` in the work dir, or `.audit/<task-slug>.tsv` when several efforts run at once, and leave it out of git.

> 默认情况下，日志是一个工作产物，不提交。把它放在工作目录里的 `decisions.tsv`，或者同时有几项工作在进行时放在 `.audit/<task-slug>.tsv`，并把它排除在 git 之外。

Commit it only when the work is ambitious enough that a reviewer needs the trail to trust the result.

> 只有当工作足够有野心、审查者需要这份记录才能信任结果时，才提交它。

## Rules｜规则

- Append-only. A wrong call gets a new row that supersedes it. Never edit or delete history.

> - 只追加。做错的决定用新的一行来取代它。永远不要编辑或删除历史。

- Prefer evidence produced by committed scripts over hand-made one-offs (the **encode-lessons-in-structure** principle skill).

> - 优先用已提交的脚本产出的证据，而不是手工的一次性产物（**encode-lessons-in-structure** 原则技能）。

## Audit the log against the transcript｜对照会话记录审计日志

At the end of the run, before handing back, check the log told the truth. Read this run's transcript from the active workspace's session directory: in Cursor, the `agent-transcripts/` directory the system prompt names. In Claude Code, `~/.claude/projects/<slug>/` (the workspace path with every character that isn't a letter or digit turned into "-"). In Pi, `~/.pi/agent/sessions/--<slug>--/`. In Codex, the `~/.codex/sessions/` file whose first line has `payload.cwd` equal to the workspace path. Don't glob across every project's directory. That reads unrelated private chats. Walk this run's rows against what actually happened. Each stretch of them begins at one of this run's `start` rows, or at the first row if this run created the log, and ends at the next `start` row of another run:

> 在运行结束、交回之前，检查日志是否说了实话。从当前工作区的会话目录读取本次运行的会话记录：在 Cursor 里是系统提示指明的 `agent-transcripts/` 目录。在 Claude Code 里是 `~/.claude/projects/<slug>/`（把工作区路径里所有非字母数字的字符换成“-”）。在 Pi 里是 `~/.pi/agent/sessions/--<slug>--/`。在 Codex 里是第一行 `payload.cwd` 等于工作区路径的那个 `~/.codex/sessions/` 文件。不要在所有项目的目录里通配查找。那会读到无关的私人对话。把本次运行写的各行和实际发生的事对照一遍。每一段都从本次运行的某个 `start` 行开始（如果日志是本次运行创建的，就从第一行开始），到另一次运行的下一个 `start` 行结束：

- Check that every row maps to a real decision or action.

> - 检查每一行都对应一个真实的决定或动作。

- Check that each row's evidence resolves and shows what the row claims.

> - 检查每一行的证据都能找到，并且确实显示了这一行所声称的内容。

- A fork, pivot, or abandoned approach that shaped the work but isn't logged is a gap. Add it.

> - 影响了工作、却没被记录的分岔、转向或被放弃的做法，就是缺口。补上它。

Correct the log, not the story. The audit never edits or removes a row, even an invented one. When a row records neither a real decision nor a real action, or its claim or evidence is wrong, add a row that supersedes it with what actually happened and a pointer that resolves. This audit does not check rows outside this run's stretches. If this run's own work shows one of them is wrong, supersede it like any wrong call.

> 修正的是日志，而不是故事。审计永远不编辑或删除任何一行，即使是编造出来的那行。当某一行既不记录真实的决定也不记录真实的动作，或者它的说法或证据是错的，就加一行来取代它，写明实际发生了什么，并附上一个能找到的指针。这次审计不检查本次运行那几段之外的行。如果本次运行自己的工作表明其中某一行是错的，就像对待任何错误的决定那样取代它。

## Cross-model review of the trail｜对记录做跨模型审查

Before handing back, spawn a subagent on a different model family from the one that did the work. Self-review is not a substitute. The subagent reads the audit trail and the run's transcript, then flags what the user should pay attention to. Not a redo of the work, a scan for what's suboptimal or risky. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 交回之前，在与完成工作的模型不同家族的模型上派生一个子代理。自我审查不能替代它。这个子代理读审计记录和本次运行的会话记录，然后标出用户应该注意的地方。不是重做工作，而是扫一遍哪里不够好或有风险。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

- Decisions logged with weak or absent evidence.

> - 证据薄弱或缺失的决定。

- Verification steps skipped or claimed without proof in the transcript.

> - 被跳过的验证步骤，或者在会话记录里没有证明却声称做过的验证。

- Choices that look risky in hindsight (premature, scope-creeping, papering over a symptom).

> - 事后看来有风险的选择（过早、范围蔓延、掩盖症状）。

- Gaps the user would otherwise miss on a casual skim.

> - 用户随便浏览一下就会错过的缺口。

Every reply for a run that produced a trail ends with an "Attention" section. Lead with the reviewer's model on its own line (`reviewed by <model>`), then list each flag pointing to specific rows or moments. "No flags" is a valid value. The model name is not.

> 每一个产出了记录的运行的回复，都以一节“Attention（需要注意）”结尾。第一行单独写审查者的模型（`reviewed by <模型>`），然后列出每一个标记，指向具体的行或时刻。“No flags（没有标记）”是有效的内容。模型名不能省。

## Reviewing the trail｜审阅记录

Read top to bottom, follow the evidence pointers, spot-check. GitHub renders a committed TSV as a table. `column -s$'\t' -t decisions.tsv` renders it in a terminal.

> 从上往下读，顺着证据指针走，抽查。GitHub 会把提交的 TSV 渲染成表格。`column -s$'\t' -t decisions.tsv` 可以在终端里把它渲染出来。

## Composing this skill｜与其他技能组合

Other skills route their audit trail here instead of inventing one. Reference it by name and let it own the format. Don't restate the columns.

> 其他技能把它们的审计记录交给这个技能，而不是自己另搞一套。按名字引用它，让它负责格式。不要复述各列。
