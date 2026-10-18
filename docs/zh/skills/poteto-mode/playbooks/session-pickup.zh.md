<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/session-pickup.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/session-pickup.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/session-pickup.md)

### Session pickup｜会话接手

**You own the resume point. Read the prior trail, don't redo it.**

> **恢复点由你负责。读之前的记录，不要重做。**

1. Locate the prior trail. A local transcript in the active workspace's session directory (in Cursor, the `agent-transcripts/` directory the system prompt names. Other harnesses: see **Harness** in SKILL.md. Do not glob across every project's directory, that crosses workspace boundaries and reads private chats from unrelated projects), a cloud-agent URL, or a pushed branch. Read the metadata overview and last messages first, then scan back for the decision points. Parse a long transcript in a subagent and keep the reduced timeline in the main thread (the **principle-guard-the-context-window** skill). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 1. 找到之前的记录。可能是当前工作区会话目录里的本地对话记录（在 Cursor 里是系统提示指明的 `agent-transcripts/` 目录。其他运行环境：见 SKILL.md 里的 **运行环境**。不要在所有项目的目录里通配查找，那会越过工作区边界，读到无关项目的私人对话），也可能是一个云端代理链接或一个已推送的分支。先读元数据概览和最后几条消息，再往回扫找决策点。在子代理里解析长对话记录，主线程只保留精简后的时间线（**principle-guard-the-context-window** 技能）。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

2. Reconstruct operational state. The branch and worktree, what already landed (`git log`, `git diff` against the base), the open todos, the decisions made. The prior trail is authoritative input. Resist the bias to re-derive it.

> 2. 重建运行状态。分支和 worktree、已经落地的内容（`git log`、对照基础分支 `git diff`）、未完成的待办、已做出的决定。之前的记录是权威输入。克制住重新推导一遍的冲动。

3. Diff done vs pending. Compare what shipped against what was planned, name the resume point, do not re-run the prior repro or redo completed work. A "let me verify from scratch" pass means you're treating the trail as untrustworthy when it's authoritative.

> 3. 比较已完成和待完成的部分。对照计划看已发布的内容，说出恢复点，不要重跑之前的复现，也不要重做已完成的工作。“让我从头验证一遍”意味着你把一份权威的记录当成了不可信的。

4. Route the remaining work to the matching playbook and pick the verdict: continue the execution, ship a finished recommendation, ratify or override a prior conclusion, or postmortem a failed run. The pickup playbook ends here. The routed playbook owns the rest.

> 4. 把剩余工作分派到对应的剧本，并选定结论：继续执行、交付一个已完成的建议、认可或推翻之前的结论，或者对一次失败的运行做复盘。接手剧本到此结束。剩下的由被分派的剧本负责。

5. Verify the inherited claims against the original goal on the real artifact (the **principle-prove-it-works** skill). A passing prior self-report is not the proof.

> 5. 在真实产物上，对照最初的目标验证继承来的论断（**principle-prove-it-works** 技能）。之前一份显示通过的自我报告不算证明。

**Reply:** where the prior agent stopped, what you inherited vs redid (ideally nothing redone), the resume point, and the outcome.

> **回复：** 之前的代理停在哪里、你继承了什么和重做了什么（理想情况是什么都没重做）、恢复点，以及结果。
