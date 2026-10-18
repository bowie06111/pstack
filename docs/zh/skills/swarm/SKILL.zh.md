<!-- 中英对照译文。原文：skills/swarm/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/swarm/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/swarm/SKILL.md)

```yaml
name: swarm
description: "Fan out N parallel workers, drain them, and return one report. Use for /swarm, 'swarm this', or parallel coverage, races, gauntlets, and exploration."
disable-model-invocation: true
```

> **说明（description）：** 扇出 N 个并行工作者，等他们全部完成，返回一份报告。用于 /swarm、“swarm this”，或并行覆盖、竞速、挑战赛和探索。

# Swarm｜Swarm（蜂群）

Fan out N parallel cloud workers. They may cover separate slices, race the same brief, or mix both. The parent waits, aggregates, and returns one report.

> 扇出 N 个并行的云端工作者。他们可以覆盖不同的切片、对同一份任务说明竞速，或者两者混合。父代理等待、汇总，并返回一份报告。

## Start｜开始

Open a todolist with one entry per phase before launching anything.

> 在启动任何东西之前，打开一个待办清单，每个阶段一项。

1. Frame

> 1. 框定

2. Fan out

> 2. 扇出

3. Aggregate

> 3. 汇总

4. Report

> 4. 报告

## Phase A: Frame｜阶段 A：框定

1. State the done predicate and the artifact or report the swarm must return.

> 1. 说明完成判定，以及 swarm 必须返回的产物或报告。

2. Choose the shape. Partition into slices, race N workers on identical briefs, or mix both. For a race or mixed shape, declare `first pass`, `rank all`, or `best-of` before spawning.

> 2. 选择形状。分成切片、让 N 个工作者对相同的任务说明竞速，或者两者混合。对于竞速或混合形状，在派生之前声明用 `first pass`（第一个通过）、`rank all`（全部排名）还是 `best-of`（取最好）。

3. Set N from the user or derive it from the shape. N is total workers, not the cloud concurrency limit.

> 3. N 由用户给定，或者根据形状推出。N 是工作者总数，不是云端并发上限。

4. Pick the worker model from the `swarm workers` line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses). If the file or that line is missing, use `grok-4.7-xhigh-fast`. For `auto` or `inherit-parent`, omit `model` so the workers run on the parent model. If your subagent tool rejects a slug, use the default and say so. If it rejects the default, use the closest valid slug of the same family from its error message or your harness's model list. For a model race, name each arm's model up front. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 4. 从 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的 `swarm workers` 这一行选择工作者模型。如果文件或这一行不存在，就用 `grok-4.7-xhigh-fast`。值为 `auto` 或 `inherit-parent` 时省略 `model`，让工作者在父模型上运行。如果你的子代理工具拒绝了某个模型名，就用默认值并说明。如果它连默认值也拒绝，就从它的错误信息或你运行环境的模型列表里，选同一家族中最接近的有效模型名。对于模型竞速，事先为每一路指定模型。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

5. Give each worker its own writable output when it writes. When workers verify or measure commits, each brief names the exact SHAs. A measurement brief also names the method (sample count, what one sample is, order). The worker records both in its result.

> 5. 工作者需要写入时，给每个工作者各自可写的输出位置。当工作者要验证或测量提交时，每份任务说明都写明确切的 SHA。测量类的任务说明还要写明方法（采样次数、一次采样是什么、顺序）。工作者在结果里把两者都记录下来。

## Phase B: Fan out｜阶段 B：扇出

Spawn all N workers in one message with `subagent_type: generalPurpose`, `environment: "cloud"`, `run_in_background: true`, and the step 4 model, left unset for `auto` or `inherit-parent`. Use `environment: "local"` only when the worker needs access to something on the user's computer.

> 在一条消息里派生全部 N 个工作者，`subagent_type: generalPurpose`、`environment: "cloud"`、`run_in_background: true`，以及第 4 步选定的模型（`auto` 或 `inherit-parent` 时不设）。只有当工作者需要访问用户电脑上的东西时，才用 `environment: "local"`。

When a worker must start from a non-default pushed branch, pass `cloud_base_branch`.

> 当工作者必须从一个非默认的已推送分支开始时，传 `cloud_base_branch`。

**Other harnesses.** These parameters belong to Cursor's `Task` tool, and `environment: "cloud"` runs a Cursor cloud agent. In another harness, use its subagent tool: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Workers run locally there, so give each one its own worktree or output path. Keep the brief and the model. Drop parameters your tool doesn't have. If your harness has no subagent tool, as in Pi without an extension, run the workers yourself, one after another.

> **其他运行环境。** 这些参数属于 Cursor 的 `Task` 工具，`environment: "cloud"` 运行的是 Cursor 云端代理。在其他运行环境里，用它的子代理工具：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。工作者在那里是本地运行的，所以给每个工作者自己的 worktree 或输出路径。保留任务说明和模型。去掉你的工具没有的参数。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己依次运行各个工作者。

Every brief stands alone. Include the goal, scope, exact slice or race arm, how to verify, and what to report. Reports use `PASS`, `ISSUES`, or `BLOCKED` with evidence. A worker that can prove a defect reports `ISSUES` and lists every issue it can prove, not only the first.

> 每份任务说明都要能独立成立。写明目标、范围、确切的切片或竞速路线、如何验证，以及报告什么。报告用 `PASS`、`ISSUES` 或 `BLOCKED`，并附上证据。能证明缺陷的工作者报告 `ISSUES`，并列出它能证明的每一个问题，而不只是第一个。

If a worker drops out, proceed with N-1 and note it.

> 如果有工作者掉线，就用 N-1 个继续，并记下来。

## Phase C: Aggregate｜阶段 C：汇总

Read the terminal results. Drop a result that does not record the SHAs and method its brief names, and respawn that worker once. After a second miss, record a gap. A gap does not count as a pass. For coverage, every required slice needs a result. For a race, apply the selection rule declared up front. Use first pass, rank all, or best-of. Do not paste raw worker dumps.

> 读取终态结果。没有记录任务说明所要求的 SHA 和方法的结果，丢弃，并重新派生那个工作者一次。第二次还不合格，就记为缺口。缺口不算通过。对于覆盖型，每个必需的切片都要有结果。对于竞速型，应用事先声明的选择规则：第一个通过、全部排名或取最好。不要贴工作者的原始输出。

Keep a compact result table, one-line evidenced issues, and explicit gaps or dropouts.

> 保留一张紧凑的结果表、每个有证据的问题各一行，以及明确的缺口或掉线情况。

## Phase D: Report｜阶段 D：报告

Return one consolidated in-chat report with the table, issue one-liners, gaps or dropouts, and the race rule when used.

> 在聊天里返回一份合并后的报告，包括结果表、每个问题一行、缺口或掉线情况，以及用到时的竞速规则。
