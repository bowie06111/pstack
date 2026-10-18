<!-- 中英对照译文。原文：skills/interrogate/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/interrogate/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/interrogate/SKILL.md)

```yaml
name: interrogate
description: "Use for \"interrogate\", \"adversarial review\", \"multi-model review\", \"challenge this\", \"stress test this code\", \"find blind spots\", or \"tear this apart\". Multiple LLM reviewers challenge changes from independent angles."
disable-model-invocation: true
```

> **说明（description）：** 用于“interrogate”“对抗性审查”“多模型审查”“挑战这个”“压力测试这段代码”“找盲点”或“把它拆了”。多个大模型审查者从独立的角度挑战改动。

# Interrogate｜Interrogate（审问）

Spawn one reviewer per configured model to adversarially review code changes. The adversarial signal comes from model diversity, not assigned personas. The deliverable is a synthesized verdict. Do NOT auto-apply changes.

> 为每个配置的模型派生一个审查者，对代码改动做对抗性审查。对抗信号来自模型的多样性，而不是指派的角色。交付物是一个综合后的结论。不要自动应用改动。

## Step 1, Scope｜第 1 步：范围

Identify what to review from context:

> 从上下文确定要审查什么：

- If the user points at specific files or a diff, use that.

> - 如果用户指出了具体的文件或 diff，就用那个。

- If on a feature branch, run `git diff main...HEAD` (or the appropriate base branch) for the full changeset.

> - 如果在功能分支上，运行 `git diff main...HEAD`（或合适的基础分支）得到完整的改动集。

- If the user's message references recent work, gather the relevant files.

> - 如果用户的消息提到了最近的工作，就收集相关的文件。

Package the diff or file contents with any surrounding context files the reviewers need to understand the code.

> 把 diff 或文件内容，连同审查者理解代码所需的周边上下文文件一起打包。

## Step 2, Intent｜第 2 步：意图

Before spawning reviewers, state the intent in one clear paragraph from the user's message, commit messages, the PR description if one exists, and the code. If you're unsure about the intent, ask the user before proceeding.

> 在派生审查者之前，根据用户的消息、提交信息、PR 描述（如果有的话）和代码，用清楚的一段话说明意图。如果你对意图拿不准，先问用户再继续。

## Step 3, Spawn Reviewers｜第 3 步：派生审查者

Launch all reviewers in a single message with the Task tool, one per entry on the `interrogate reviewers` line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses), labeled Reviewer A, B, and so on to match the entry count. If the file or that line is missing, use the table defaults.

> 用 Task 工具在一条消息里启动所有审查者，pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里 `interrogate reviewers` 这一行的每个条目一个，按条目数量标为审查者 A、B，依此类推。如果文件或这一行不存在，用表格里的默认值。

**Other harnesses.** The spawns in this skill use Cursor's `Task` tool. In another harness, use its subagent tool: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Keep the prompt and the model. Drop parameters your tool doesn't have. If your harness has no subagent tool, as in Pi without an extension, run each reviewer yourself, one after another.

> **其他运行环境。** 本技能里的派生使用 Cursor 的 `Task` 工具。在其他运行环境里，用它的子代理工具：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。保留提示词和模型。去掉你的工具没有的参数。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己依次扮演每个审查者。

| Subagent | Default model |
|----------|---------------|
| Reviewer A | `claude-opus-5-5-xhigh` |
| Reviewer B | `grok-4.7-xhigh-fast` |

> | 子代理 | 默认模型 |
> |----------|---------------|
> | 审查者 A | `claude-opus-5-5-xhigh` |
> | 审查者 B | `grok-4.7-xhigh-fast` |

Each reviewer gets `subagent_type: generalPurpose`, `readonly: true`, and `model` set to its configured entry, or its table default when there is no configured line. Omit `model` for an `auto` or `inherit-parent` entry, so that reviewer runs on the parent model.

> 每个审查者都用 `subagent_type: generalPurpose`、`readonly: true`，`model` 设为它的配置条目，没有配置行时设为表格默认值。条目是 `auto` 或 `inherit-parent` 时省略 `model`，让那个审查者在父模型上运行。

If your subagent tool rejects a configured entry, run that reviewer on its family's table default and say so. Families go by prefix: `claude-*`, and `grok-*` with `cursor-grok-*` and `composer-*`. A refused `composer-*` entry runs on Reviewer B's default. With no family match, use Reviewer A's default. If it rejects a table default, spawn with the closest equivalent from the valid slugs in its error message or your harness's model list (prefer the same family and reasoning tier), and open a separate PR to update the default table. Do not block the review on the slug issue. Never treat an alias entry as a rejected slug or apply either fallback to it.

> 如果你的子代理工具拒绝了某个配置的条目，就让那个审查者运行它所属家族的表格默认值，并说明。家族按前缀划分：`claude-*`；`grok-*` 与 `cursor-grok-*` 和 `composer-*` 同属一族。被拒绝的 `composer-*` 条目改用审查者 B 的默认值。没有匹配的家族时，用审查者 A 的默认值。如果它拒绝了某个表格默认值，就从它错误信息里的有效模型名或你运行环境的模型列表里，选最接近的等价模型来派生（优先同一家族和推理等级），并单独开一个 PR 更新默认值表格。不要因为模型名的问题卡住审查。永远不要把别名条目当作被拒绝的模型名，也不要对它应用任何一种回退。

Read `references/reviewer-prompt.md` and fill in the same template for every reviewer with the stated intent, the diff or file contents, the rubric from `references/rubric.md`, and the code-quality lens from `references/code-quality-review.md`. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 读取 `references/reviewer-prompt.md`，为每个审查者填写同一个模板：说明的意图、diff 或文件内容、`references/rubric.md` 里的评分标准，以及 `references/code-quality-review.md` 里的代码质量视角。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

## Step 4, Synthesize｜第 4 步：综合

As results come back, build a unified picture. Parse every reviewer's findings. Merge findings that describe the same issue differently, and note which models raised each one. Findings two or more models raised independently are the highest signal. Read a lone model's finding, but weight it accordingly. Note disagreements. If one model flags something and another explicitly says the opposite, that is context for the verdict.

> 结果陆续返回时，构建一幅统一的图景。解析每个审查者的发现。合并用不同方式描述同一个问题的发现，并记下每个发现是哪些模型提出的。两个或更多模型独立提出的发现是最高信号。只有一个模型提出的发现也要读，但相应地调整权重。记下分歧。如果一个模型标出了某个问题，而另一个模型明确说了相反的话，这就是结论的背景。

## Step 5, Lead Judgment｜第 5 步：主审判断

You are the lead reviewer, a pragmatic senior engineer, not a neutral aggregator. Read `references/lead-judgment.md` for the full framework. Put every finding in one of four buckets, with the model(s) that raised it and a one-line rationale.

> 你是主审，一位务实的资深工程师，而不是中立的汇总者。完整的框架见 `references/lead-judgment.md`。把每个发现放进四个桶之一，附上提出它的模型和一行理由。

- **Act on**. Real issues affecting correctness, security, or maintainability given the actual goals. They would block a real PR.

> - **Act on（要处理）**。在实际目标下影响正确性、安全性或可维护性的真实问题。它们会让一个真实的 PR 无法通过。

- **Consider**. Legitimate, but you're not sure they outweigh the cost of addressing them now. Worth the user's attention.

> - **Consider（可考虑）**。合理，但你不确定它们是否值得现在付出处理的代价。值得用户关注。

- **Noted**. Technically valid but not actionable. Context-dependent, premature optimization, or low-impact at this stage.

> - **Noted（备注）**。技术上成立，但不可执行。取决于具体情况、属于过早优化，或者在这个阶段影响很小。

- **Dismissed**. Wrong, nitpicky, or missing context, with a brief reason.

> - **Dismissed（驳回）**。错误的、吹毛求疵的或缺少语境的，附上简短理由。

## Output Format｜输出格式

### Intent
> [The stated intent paragraph from Step 2]

> ### 意图
> > [第 2 步里说明意图的那一段]

### Reviewers｜审查者

- Reviewer [label]: [model name], [N findings] (one bullet per reviewer)

> - 审查者 [标签]：[模型名]，[N 个发现]（每个审查者一个要点）

### Act On
[Each: description, which models raised it, why it matters.]

> ### 要处理
> [每一条：描述、哪些模型提出的、为什么重要。]

### Consider
[Each: description, which models raised it, the tradeoff.]

> ### 可考虑
> [每一条：描述、哪些模型提出的、取舍。]

### Noted
[Brief list.]

> ### 备注
> [简短列表。]

### Dismissed
[Each with a brief rationale.]

> ### 驳回
> [每一条附上简短理由。]

### Agreement Map
[Where models agreed and diverged, and what that pattern tells us.]

> ### 一致性地图
> [模型在哪里一致、在哪里分歧，以及这种模式告诉了我们什么。]
