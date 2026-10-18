<!-- 中英对照译文。原文：skills/recall/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/recall/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/recall/SKILL.md)

```yaml
name: recall
description: "Reconstruct your recent working context from your own chat history, live state, and the shared record (user reports, prior fixes, incidents), then hand back a tight current-state brief. Use for 'recall my work on X', 'catch me up', 'what have I been working on', 'where did I leave off', before starting or resuming work."
disable-model-invocation: true
```

> **说明（description）：** 从你自己的对话历史、实时状态和共享记录（用户报告、之前的修复、事故）中重建你最近的工作上下文，然后交回一份紧凑的现状简报。用于“回忆我在 X 上的工作”“让我跟上进度”“我最近在做什么”“我上次做到哪了”，在开始或恢复工作之前使用。

# Recall｜Recall（回忆）

**Before you start or resume work, you rebuild the user's recent working context and hand back a tight capsule of where things stand now and what to do next.**

> **在你开始或恢复工作之前，你要重建用户最近的工作上下文，并交回一份紧凑的“胶囊”，说明现在的情况和下一步该做什么。**

Keep it tight and on-topic. Read only what the in-scope threads need, then stop.

> 保持紧凑、紧扣主题。只读范围内的线索需要的内容，然后停下。

Your context lives in two records. Your own chat history holds what you did and decided. The shared record holds everything that happened around the same code under other names: the symptoms users keep reporting, the fixes that shipped and got reverted, the errors still firing in prod. That second record is what the **why** skill searches, across source control, the issue tracker, chat and issue channels, long-form docs, and error tracking. A feature with a long bug tail keeps most of its story there, so don't reconstruct it from your transcripts alone.

> 你的上下文存在于两份记录里。你自己的对话历史记录着你做了什么、决定了什么。共享记录则包含了在其他名义下围绕同一段代码发生的一切：用户反复报告的症状、发布了又被回滚的修复、生产环境里仍在触发的错误。第二份记录就是 **why** 技能所搜索的东西，横跨源代码管理、问题跟踪、聊天和问题频道、长篇文档以及错误跟踪。一个有着长长 bug 尾巴的功能，大部分故事都在那里，所以不要只靠你的对话记录来重建它。

Transcripts live where your harness keeps them. Each file is JSONL. Find the active workspace's directory for your harness. If you can't tell which harness you are, check which of these exist for this workspace:

> 对话记录存放在你的运行环境保存它们的地方。每个文件都是 JSONL。找到你运行环境里当前工作区对应的目录。如果你分不清自己在哪个运行环境里，就检查这个工作区下面哪些路径存在：

- **Cursor:** `~/.cursor/projects/<slug>/agent-transcripts/<uuid>/<uuid>.jsonl`. `<slug>` is the workspace path with the leading slash dropped and each "/" turned into "-" (`/Users/you/proj` becomes `Users-you-proj`).

> - **Cursor：** `~/.cursor/projects/<slug>/agent-transcripts/<uuid>/<uuid>.jsonl`。`<slug>` 是去掉开头斜杠、把每个“/”换成“-”的工作区路径（`/Users/you/proj` 变成 `Users-you-proj`）。

- **Claude Code:** `~/.claude/projects/<slug>/<session-id>.jsonl`. `<slug>` is the workspace path with every character that isn't a letter or digit turned into "-" (`/Users/you/proj` becomes `-Users-you-proj`).

> - **Claude Code：** `~/.claude/projects/<slug>/<session-id>.jsonl`。`<slug>` 是把工作区路径里所有非字母数字的字符换成“-”（`/Users/you/proj` 变成 `-Users-you-proj`）。

- **Pi:** `~/.pi/agent/sessions/--<slug>--/<timestamp>_<id>.jsonl`. `<slug>` is the workspace path with the leading slash dropped and each "/" turned into "-" (`--Users-you-proj--`).

> - **Pi：** `~/.pi/agent/sessions/--<slug>--/<timestamp>_<id>.jsonl`。`<slug>` 是去掉开头斜杠、把每个“/”换成“-”的工作区路径（`--Users-you-proj--`）。

- **Codex:** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`, not grouped by workspace. Keep only files whose first line has `payload.cwd` equal to the workspace path.

> - **Codex：** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`，不按工作区分组。只保留第一行 `payload.cwd` 等于工作区路径的文件。

- **OpenCode:** `~/.local/share/opencode/storage/`. Sessions are JSON files under `session/<project-id>/`, with their messages under `message/` and `part/`.

> - **OpenCode：** `~/.local/share/opencode/storage/`。会话是 `session/<project-id>/` 下的 JSON 文件，它们的消息在 `message/` 和 `part/` 下。

1. Classify, then route. One specific prior chat to resume is the `session-pickup` playbook, not this. Turning habits into a durable skill is `automate-me`. A human-readable summary of your work is a different task. Recall loads working context across recent chats before you act. If the user already gave you a full state capsule (paths, branch, the change), use it and skip the mining.

> 1. 先分类，再分派。要恢复某一个具体的之前对话，属于 `session-pickup` 剧本，不是这个技能。把习惯变成持久的技能，属于 `automate-me`。给人看的工作总结是另一项任务。Recall 是在你行动之前加载最近多个对话里的工作上下文。如果用户已经给了你一份完整的状态胶囊（路径、分支、改动），就直接用它，跳过挖掘。

2. Lock the scope before searching. Pin the window ("recent" is a real range, default the last 7 days), the topic if named, and the workspace (default the active one. Never read another project's transcripts without being asked). State the scope back. Never quietly turn "all" into "recent N".

> 2. 搜索之前先锁定范围。确定时间窗口（“最近”是一个真实的范围，默认最近 7 天）、话题（如果点名了的话）以及工作区（默认当前工作区。没被要求时，永远不要读其他项目的对话记录）。把范围复述一遍。永远不要悄悄把“全部”变成“最近 N 个”。

3. Fan out across your chat history. Spawn parallel subagents on a fast, cheap model, each taking a slice of the corpus. Tell every subagent to order candidates by real modification time (`ls -t`) and never by UUID name, grep the topic first and then read only the matching chats and only their relevant regions, and skip the current chat plus obvious noise (subagent, eval, and test chats). Each returns the same schema, one block per chat: topic, the user's goal, decisions, open threads, struggles and corrections, and artifacts (PRs, tickets, branches), each citing the chat UUID. For one or two chats, skip the fan-out and search directly. The raw transcripts stay in the subagents. The main thread gets only their findings. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 3. 在你的对话历史上扇出。派生几个使用快速便宜模型的并行子代理，每个负责语料的一片。告诉每个子代理：按真实的修改时间（`ls -t`）给候选排序，永远不要按 UUID 名字排序；先 grep 话题，然后只读匹配的对话、只读其中相关的部分；跳过当前对话和明显的噪音（子代理、评测和测试对话）。每个子代理都返回同样的格式，每个对话一块：话题、用户的目标、决定、未完成的线索、困难和纠正，以及产物（PR、工单、分支），每一项都注明对话 UUID。只有一两个对话时，跳过扇出，直接搜索。原始对话记录留在子代理那里。主线程只拿到它们的发现。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

4. Sweep the shared record whenever the topic names a feature, file, subsystem, area, or bug. This is the default, not a judgment call, and "my work on X" does not exempt it. Hand it to the **why** skill's source investigators, but steer their question from "why was this built this way" to "what's the current state, what's been tried and didn't hold, and what are users still reporting". Reuse its per-source playbooks, run the investigators in parallel with the chat-history mining, and inherit its posture: one investigator per source, null results are findings, skip an unavailable MCP and say so. Fold what comes back into the brief. Skip this step only for pure activity recall with no named target ("what did I do this week"), where your own history and live state are the entire answer.

> 4. 只要话题点名了某个功能、文件、子系统、领域或 bug，就清查共享记录。这是默认动作，不需要判断，而且“我在 X 上的工作”也不能免除这一步。把它交给 **why** 技能的各信息源调查者，但把他们的问题从“这个为什么这样构建”改成“现在是什么状态、试过什么但没站住脚、用户还在报告什么”。复用它针对各信息源的操作手册，让调查者和对话历史的挖掘并行运行，并继承它的做法：每个信息源一个调查者，空结果也是发现，跳过不可用的 MCP 并说明。把返回的内容整合进简报。只有在纯粹回忆活动、没有点名目标时（“我这周做了什么”）才跳过这一步，那时你自己的历史和实时状态就是全部答案。

5. Verify against live state. Take the PRs, branches, and tickets that the mining and the sweep surfaced and check them with `git` and `gh`. When the answer hinges on what an agent actually did (the tools it ran, files it read, errors it hit), read the full transcript, not just a trimmed local copy.

> 5. 对照实时状态核实。拿挖掘和清查找出的 PR、分支和工单，用 `git` 和 `gh` 检查。当答案取决于某个代理实际做了什么（它运行的工具、读过的文件、遇到的错误）时，读完整的对话记录，而不只是一份裁剪过的本地副本。

6. Write the brief to the contract below. Group by thread. Stay on the named topic.

> 6. 按下面的约定写简报。按线索分组。紧扣点名的话题。

## Output contract｜输出约定

Lead with the capsule, then the thread status, then the problems, then the next move. Deeper detail goes below or gets cut.

> 先写胶囊，然后是线索状态，然后是问题，然后是下一步。更深的细节放在下面，或者删掉。

- **Capsule.** At most 5 bullets. What this work is and where it stands overall.

> - **胶囊。** 最多 5 个要点。这项工作是什么，整体处于什么状态。

- **Threads.** One line each, prefixed with exactly one status tag: `[merged #N]`, `[open PR #N]`, `[in flight <branch>]`, `[verified, uncommitted]`, `[reverted #N]`, or `[planned, not started]`. A thread with no tag is not done yet, so tag it.

> - **线索。** 每条一行，前面恰好加一个状态标签：`[merged #N]`（已合并）、`[open PR #N]`（PR 未合并）、`[in flight <branch>]`（进行中）、`[verified, uncommitted]`（已验证未提交）、`[reverted #N]`（已回滚）或 `[planned, not started]`（已计划未开始）。没有标签的线索就是还没完成，所以要给它加上标签。

- **Problems.** At most 5, the recurring ones. Include the symptoms users keep reporting and any fix that shipped and was reverted, so the next attempt starts where the last one failed.

> - **问题。** 最多 5 个，反复出现的那些。包括用户反复报告的症状，以及任何发布了又被回滚的修复，这样下一次尝试就能从上一次失败的地方开始。

- **Next move.** The single most useful next action, concrete.

> - **下一步。** 唯一一个最有用的下一步行动，要具体。

An adjacent feature or ticket stays out unless it blocks this one. When the capsule and thread lines outgrow a screen, cut detail before you cut threads. Write the brief through the **unslop** skill, cite chat findings by UUID and shared-record findings by their source (PR #, ticket ID, chat permalink, error-tracker issue), and sanitize private context before any public output.

> 相邻的功能或工单不写，除非它阻塞了这一个。当胶囊和线索行超过一屏时，先删细节，再删线索。通过 **unslop** 技能来写简报，对话里的发现按 UUID 注明出处，共享记录里的发现按来源注明（PR #、工单编号、聊天永久链接、错误跟踪的问题），在任何公开输出之前清除私密的上下文。

**Reply:** the brief, to the contract above.

> **回复：** 按上面的约定写成的简报。
