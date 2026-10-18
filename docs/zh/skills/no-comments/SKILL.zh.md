<!-- 中英对照译文。原文：skills/no-comments/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/no-comments/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/no-comments/SKILL.md)

```yaml
name: no-comments
description: "Spawn Comment Sicko, fix accepted findings, and offer encodings for claimed constraints."
disable-model-invocation: true
```

> **说明（description）：** 派生 Comment Sicko，修复被接受的发现，并为声称的约束提供编码方案。

# No comments｜不要注释

Spawn Comment Sicko. Act on accepted findings.

> 派生 Comment Sicko。根据被接受的发现采取行动。

Defer to Comment Sicko's fresh perspective.

> 尊重 Comment Sicko 的新鲜视角。

## Scope｜范围

Use the caller's files or diff. Otherwise use the current diff against the base branch, default `main`, including the working tree.

> 使用调用方给的文件或 diff。否则使用当前相对基础分支（默认 `main`）的 diff，包括工作区里的改动。

## Steps｜步骤

1. Spawn `Task` with `subagent_type: "comment-sicko"`. Pass the scope. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill. Do not restate its rules. If your harness doesn't have comment-sicko registered as a subagent (any harness other than Cursor with the pstack plugin), spawn a general subagent instead: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Its prompt is [`references/comment-sicko.md`](references/comment-sicko.zh.md) verbatim, then the scope. If your harness has no subagent tool, as in Pi without an extension, run that prompt yourself in a fresh pass over the scope.

> 1. 派生 `Task`，`subagent_type: "comment-sicko"`。传入范围。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。不要复述它的规则。如果你的运行环境没有把 comment-sicko 注册为子代理（除了装了 pstack 插件的 Cursor 之外的任何运行环境），就改为派生一个通用子代理：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。它的提示词是原样的 [`references/comment-sicko.md`](references/comment-sicko.zh.md)，后面接上范围。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己用那段提示词对范围重新过一遍。

2. Inspect its report and diff. Reject application-code edits, scope escapes, exception-protected deletions, misstated `MUST KILL` reasons, and flags that treat kept intentional code as guilty. Reshape flags on our-code surprises stay actionable. Do not restore those comments. A keep survives only with proof it is about something we cannot change. Audit missed scoped lint and TypeScript suppressions. Correctness or safety suppressions stay actionable `MUST KILL`s. Restore deletions only with exact exceptions and scoped proof. Before accepting thin `IMPORTANT` or `do not remove` kills or keeps, run `/how` or `/why` on their symbol. If a kill is ambiguous, do not restore. If a keep is refuted or still ambiguous, delete it. Revert and rerun one rejected report with the failure named. Reject a second, report it open, and fail `/no-comments`.

> 2. 检查它的报告和 diff。拒绝以下内容：对应用代码的修改、越出范围、删除了受例外保护的注释、说错了理由的 `MUST KILL`，以及把有意保留的代码当成罪魁的标记。针对我们自己代码里意外之处的重塑标记仍然可以执行。不要恢复那些注释。只有证明它说的是我们无法改变的东西，保留才成立。核查范围内被漏掉的 lint 和 TypeScript 抑制。涉及正确性或安全的抑制仍然是可执行的 `MUST KILL`。只有在有确切的例外和范围内的证明时，才恢复被删除的内容。在接受理由单薄的 `IMPORTANT` 或 `do not remove` 的删除或保留之前，对它的符号运行 `/how` 或 `/why`。如果删除有歧义，不要恢复。如果保留被驳倒或仍有歧义，删掉它。回滚一份被拒绝的报告，说明失败原因后重跑一次。第二次再被拒绝，就报告为未解决，并让 `/no-comments` 失败。

3. Fix trivial accepted flags directly by deleting a dead path, dropping a parameter, or using the real API. If any fix needs a shape, run `/architect` once for the accepted set and surrounding code. Stop at the sketch. Architect shapes. Step 4 implements.

> 3. 直接修复琐碎的被接受标记：删掉一条死路径、去掉一个参数，或者改用真正的 API。如果有任何修复需要定形状，就对被接受的那一组和周围代码运行一次 `/architect`。停在草图阶段。Architect 负责定形状。第 4 步负责实现。

4. Implement the smallest root-cause fix in scope. Remove every named workaround. If the root cause is out of scope, land the smallest in-scope fix and report the rest open. The **principle-fix-root-causes** and **principle-redesign-from-first-principles** skills guide intent only. Neither authorizes widening the fence nor fixing instances outside it. Never bolt on symptom guards.

> 4. 在范围内实施最小的根因修复。去掉每一个被点名的变通办法。如果根因在范围之外，就落地最小的范围内修复，其余的报告为未解决。**principle-fix-root-causes** 和 **principle-redesign-from-first-principles** 技能只用来指导意图。两者都不授权扩大边界，也不授权修复边界外的实例。永远不要加上只治症状的防护。

5. Constraint comments say `do not remove`, `do not change wording`, or `talk to X before changing`. Leave keeps about things we cannot change. Offer the cheapest in-scope type, runtime, test, or CI lint. Wait for interactive approval. Unattended and eval require caller pre-approval. If approved, encode then delete. Otherwise delete, report the constraint open, and sketch out-of-scope work.

> 5. 约束类注释会说 `do not remove`、`do not change wording` 或 `talk to X before changing`。关于我们无法改变的东西的保留就留着。提供范围内最便宜的类型、运行时、测试或 CI lint 方案。等待交互式的批准。无人值守和评测需要调用方预先批准。如果获得批准，就先编码再删除。否则就删除，把约束报告为未解决，并勾画出范围外的工作。

6. Report the deletion count, restored comments, reruns, architect sketch, fixes, encoding offers, encodings, unenforced constraints, and other open work.

> 6. 报告删除数量、恢复的注释、重跑次数、architect 草图、修复、编码方案、已做的编码、未被强制执行的约束，以及其他未完成的工作。
