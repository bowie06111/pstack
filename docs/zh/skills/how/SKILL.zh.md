<!-- 中英对照译文。原文：skills/how/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/how/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/how/SKILL.md)

```yaml
name: how
description: "Use for \"how does X work\", code walkthroughs before changing something, and placement / ownership / layering questions (\"where should this live\", \"which package owns this\", \"is this the right layer\"). Explains subsystem architecture, runtime flow, onboarding mental models. Use why for motivation."
disable-model-invocation: true
```

> **说明（description）：** 用于“X 是怎么工作的”、改动之前的代码讲解，以及放置/归属/分层的问题（“这个应该放在哪”“哪个包拥有这个”“这一层对吗”）。讲解子系统架构、运行流程、帮助上手的心智模型。关于动机用 why。

# How｜How（怎么工作）

Explore the codebase to answer "how does X work?" questions. Produce architectural explanations at the level of a senior engineer onboarding onto a subsystem, enough to build a working mental model, not so much that it reads like annotated source code.

> 探索代码库来回答“X 是怎么工作的？”这类问题。产出的架构讲解要达到资深工程师上手一个子系统的程度：足以建立起可用的心智模型，又不至于读起来像带注释的源代码。

Every spawn is a Task subagent with `subagent_type: generalPurpose` and `readonly: true`. Explorers use the `how explorer` line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses) (default `grok-4.7-xhigh-fast`), and every explainer uses the `how explainer` line (default `claude-opus-5-5-xhigh`). Set `model` to that line's value, or to the default if the file or the line is missing. Leave `model` unset when the value is `auto` or `inherit-parent`. If your subagent tool rejects a slug, use the default and say so. If it rejects the default, use the closest valid slug of the same family from its error message or your harness's model list. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 每次派生都是一个 Task 子代理，`subagent_type: generalPurpose`，`readonly: true`。探索者使用 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的 `how explorer` 这一行（默认 `grok-4.7-xhigh-fast`），每个讲解者使用 `how explainer` 这一行（默认 `claude-opus-5-5-xhigh`）。把 `model` 设为该行的值，如果文件或这一行不存在，就用默认值。值为 `auto` 或 `inherit-parent` 时不设 `model`。如果你的子代理工具拒绝了某个模型名，就用默认值并说明。如果它连默认值也拒绝，就从它的错误信息或你运行环境的模型列表里，选同一家族中最接近的有效模型名。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

## 1. Assess complexity｜1. 评估复杂度

If the scope is ambiguous, state your interpretation and explore. The user can redirect.

> 如果范围有歧义，说出你的理解然后开始探索。用户可以纠正方向。

- **Simple** (a single module, a small utility, a narrow question such as "how does function X work"): no explorers. Spawn one explainer that explores and explains in one pass, built from `references/explainer-prompt.md` without the explorer-findings section. Go to step 4.

> - **简单**（单个模块、一个小工具、一个窄问题，比如“函数 X 是怎么工作的”）：不派探索者。派生一个讲解者，一次完成探索和讲解，提示词用 `references/explainer-prompt.md` 构建，去掉探索者发现那一节。跳到第 4 步。

- **Complex** (a subsystem spanning multiple files or services, a cross-cutting feature, a full architectural overview): go to step 2.

> - **复杂**（跨越多个文件或服务的子系统、一个跨领域的功能、一份完整的架构概览）：进入第 2 步。

When in doubt, take the simple path.

> 拿不准时，走简单路径。

**Other harnesses.** The spawns in this skill use Cursor's `Task` tool. In another harness, use its subagent tool: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Keep the prompt and the model. Drop parameters your tool doesn't have. If your harness has no subagent tool, as in Pi without an extension, run each role yourself, one after another.

> **其他运行环境。** 本技能里的派生使用 Cursor 的 `Task` 工具。在其他运行环境里，用它的子代理工具：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。保留提示词和模型。去掉你的工具没有的参数。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己依次扮演每个角色。

## 2. Explore (complex only)｜2. 探索（仅复杂情况）

Decompose the question into 2 to 4 angles, each a distinct slice of the subsystem. Spawn all explorers in a single message, each with `references/explorer-prompt.md` and its angle filled in.

> 把问题拆成 2 到 4 个角度，每个角度是子系统的一个不同切面。在一条消息里派生所有探索者，每个都用 `references/explorer-prompt.md` 并填好它的角度。

## 3. Synthesize (complex only)｜3. 综合（仅复杂情况）

Once all explorers have returned, spawn one explainer with `references/explainer-prompt.md` and every explorer's findings filled in.

> 所有探索者都返回后，派生一个讲解者，用 `references/explainer-prompt.md` 并填入每个探索者的发现。

## 4. Present｜4. 呈现

Present the explainer's output. Light edits for clarity or context from the conversation are fine. Do not substantially rewrite it.

> 呈现讲解者的输出。为了清晰或补充对话里的上下文做些轻微修改是可以的。不要大幅改写。

## Output format｜输出格式

The explanation uses the sections defined in `references/explainer-prompt.md`, dropping any that do not apply: Overview, Key Concepts, How It Works, Where Things Live, Gotchas.

> 讲解使用 `references/explainer-prompt.md` 里定义的各节，不适用的就去掉：概览、关键概念、工作原理、代码位置、注意事项。
