<!-- 中英对照译文。原文：skills/reflect/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/reflect/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/reflect/SKILL.md)

```yaml
name: reflect
description: Spawn three parallel review subagents over the active transcript, surface learnings, and route each to a concrete edit on an existing skill. Use when the user says reflect.
disable-model-invocation: true
```

> **说明（description）：** 在当前会话记录上派生三个并行的审查子代理，提炼经验，并把每条经验分派为对某个现有技能的具体修改。在用户说 reflect 时使用。

# Reflect｜Reflect（复盘）

Mine the current conversation for durable learnings, then route them into skill edits.

> 从当前对话里挖掘持久的经验，然后把它们分派成技能修改。

Invoke when the user says "reflect" or "/reflect". Skip when the conversation is trivial, off-topic, or already covered by an existing skill the parent followed correctly. One-offs are not learnings.

> 在用户说“reflect”或“/reflect”时调用。当对话很琐碎、跑题，或者已经被父代理正确遵循的某个现有技能覆盖时，跳过。一次性的事情不算经验。

## 1. Locate the active transcript｜1. 找到当前的会话记录

Before fanning out, find this conversation's transcript in the active workspace's session directory. Do not glob across every project's directory. That crosses workspace boundaries and reads private chats from unrelated projects.

> 在扇出之前，在当前工作区的会话目录里找到这个对话的会话记录。不要在所有项目的目录里通配查找。那会越过工作区边界，读到无关项目的私人对话。

Where the transcript lives depends on your harness:

> 会话记录放在哪里取决于你的运行环境：

- **Cursor:** the `agent-transcripts/` directory the system prompt names.

> - **Cursor：** 系统提示指明的 `agent-transcripts/` 目录。

  ```bash
  ls -t <agent-transcripts>/*.jsonl <agent-transcripts>/*/*.jsonl <agent-transcripts>/*/subagents/*.jsonl 2>/dev/null | head -10
  ```

  That covers the legacy flat (`<id>.jsonl`), current nested (`<id>/<id>.jsonl`), and subagent (`<parent>/subagents/<child>.jsonl`) layouts. For each candidate, read the first JSONL line. Take the file whose `message.content[0].text` contains the conversation's opening user prompt.

>   这覆盖了旧的扁平结构（`<id>.jsonl`）、现在的嵌套结构（`<id>/<id>.jsonl`）和子代理结构（`<parent>/subagents/<child>.jsonl`）。对每个候选，读第一行 JSONL。选 `message.content[0].text` 里包含这个对话开头用户提示词的那个文件。

- **Claude Code:** `~/.claude/projects/<slug>/<session-id>.jsonl`, where `<slug>` is the workspace path with every character that isn't a letter or digit turned into "-".

> - **Claude Code：** `~/.claude/projects/<slug>/<session-id>.jsonl`，其中 `<slug>` 是把工作区路径里所有非字母数字的字符换成“-”。

- **Pi:** `~/.pi/agent/sessions/--<slug>--/*.jsonl`, where `<slug>` is the workspace path with the leading slash dropped and each "/" turned into "-".

> - **Pi：** `~/.pi/agent/sessions/--<slug>--/*.jsonl`，其中 `<slug>` 是去掉开头斜杠、把每个“/”换成“-”的工作区路径。

- **Codex:** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`. Keep only files whose first line has `payload.cwd` equal to the workspace path.

> - **Codex：** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`。只保留第一行 `payload.cwd` 等于工作区路径的文件。

- **Other harnesses:** check the harness's session directory for this workspace.

> - **其他运行环境：** 检查该运行环境为这个工作区保存会话的目录。

Outside Cursor, take the newest candidates first and confirm one by finding the conversation's opening user prompt in its first user message. If none matches, pass a tight digest of the session instead.

> 在 Cursor 之外，先看最新的候选，通过在其第一条用户消息里找到这个对话开头的用户提示词来确认。如果都对不上，就改为传入一份紧凑的会话摘要。

## 2. Spawn three reviewers in parallel｜2. 并行派生三个审查者

One message, three `Task` calls, `subagent_type: generalPurpose`, agent mode (`readonly: false`). Reviewers need MCP access to look up the tickets, chat threads, and observability traces the transcript references, and readonly strips MCPs.

> 一条消息，三个 `Task` 调用，`subagent_type: generalPurpose`，代理模式（`readonly: false`）。审查者需要 MCP 权限来查会话记录引用的工单、聊天讨论串和可观测性 trace，而只读模式会去掉 MCP。

**Other harnesses.** The spawns in this skill use Cursor's `Task` tool. In another harness, use its subagent tool: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Keep the prompt and the model. Drop parameters your tool doesn't have. If your harness has no subagent tool, as in Pi without an extension, run each role yourself, one after another.

> **其他运行环境。** 本技能里的派生使用 Cursor 的 `Task` 工具。在其他运行环境里，用它的子代理工具：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。保留提示词和模型。去掉你的工具没有的参数。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己依次扮演每个角色。

Each reviewer and the synthesizer name a role line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses) and a default. Set `model` to that line's value, or to the default if the file or the line is missing. Leave `model` unset when the value is `auto` or `inherit-parent`. If your subagent tool rejects a slug, use the default and say so. If it rejects the default, use the closest valid slug of the same family from its error message or your harness's model list. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 每个审查者和综合者都对应 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的一个角色行和一个默认值。把 `model` 设为该行的值，如果文件或这一行不存在，就用默认值。值为 `auto` 或 `inherit-parent` 时不设 `model`。如果你的子代理工具拒绝了某个模型名，就用默认值并说明。如果它连默认值也拒绝，就从它的错误信息或你运行环境的模型列表里，选同一家族中最接近的有效模型名。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

| Lens | Role line | Default `model` | Prompt template |
|---|---|---|---|
| Judgment | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/judgment-reviewer.md` |
| Tooling | `reflect tooling` | `grok-4.7-xhigh-fast` | `references/tooling-reviewer.md` |
| Divergent | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/divergent-reviewer.md` |

> | 视角 | 角色行 | 默认 `model` | 提示词模板 |
> |---|---|---|---|
> | 判断 | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/judgment-reviewer.md` |
> | 工具 | `reflect tooling` | `grok-4.7-xhigh-fast` | `references/tooling-reviewer.md` |
> | 发散 | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/divergent-reviewer.md` |

Pass each template verbatim, substituting the transcript path or digest where marked. Reviewers return findings in the `Task` response body.

> 原样传入每个模板，在标记处替换成会话记录路径或摘要。审查者在 `Task` 的返回正文里给出发现。

## 3. Synthesize｜3. 综合

One `Task` call, `subagent_type: generalPurpose`, `model` from the `reflect judgment, divergent, synthesizer` line (default `claude-opus-5-5-xhigh`), agent mode (`readonly: false`). It spot-verifies citations through MCP, and readonly strips MCPs. Pass `references/synthesizer.md` verbatim, with each reviewer's full output inlined where marked. It returns an Accepted / Rejected / Backlog list.

> 一个 `Task` 调用，`subagent_type: generalPurpose`，`model` 取自 `reflect judgment, divergent, synthesizer` 这一行（默认 `claude-opus-5-5-xhigh`），代理模式（`readonly: false`）。它会通过 MCP 抽查引用，而只读模式会去掉 MCP。原样传入 `references/synthesizer.md`，在标记处内联每个审查者的完整输出。它返回一份 已接受 / 已否决 / 待办 列表。

## 4. Structural enforcement check｜4. 结构机制检查

Move any Accepted item that a lint rule, script, metadata flag, or runtime check would enforce more reliably to Backlog. See the **encode-lessons-in-structure** principle skill.

> 任何由 lint 规则、脚本、元数据标记或运行时检查来执行会更可靠的“已接受”条目，都移到“待办”。见 **encode-lessons-in-structure** 原则技能。

## 5. Apply｜5. 应用

Present the synthesizer's full Accepted / Rejected / Backlog output and wait for explicit approval before applying any Accepted edit. The user picks the subset and may redirect routings. Skill changes affect every future agent in the org. Do not auto-apply.

> 展示综合者完整的 已接受 / 已否决 / 待办 输出，在应用任何“已接受”的修改之前，等待明确的批准。用户挑选其中一部分，也可以改变分派。技能的改动会影响组织里将来的每一个代理。不要自动应用。

File each Backlog item to your team's devex or backlog tracker without waiting. Only the Accepted list waits for approval.

> 把每个“待办”条目直接提交到团队的开发体验或待办跟踪系统，不用等待。只有“已接受”列表需要等待批准。

Follow each approved row's Routing exactly:

> 严格按照每个被批准行的“分派”来做：

- Trivial existing-skill edit (a one-line bullet, a tightened sentence, a stale fact corrected): the parent does it directly.

> - 琐碎的现有技能修改（一行要点、一句更紧凑的话、纠正一个过时的事实）：父代理直接做。

- Substantive existing-skill edit (a new section, a new pattern table, more than ~10 lines): hand to your harness's skill-authoring skill and run its draft / test / iterate loop. That is `create-skill` in Cursor (built in) or Anthropic's `skill-creator`. If you have neither, follow the Agent Skills format at agentskills.io. "`create-skill`" below means whichever of these you use.

> - 实质性的现有技能修改（新的一节、新的模式表、超过约 10 行）：交给你运行环境的技能编写技能，跑它的起草 / 测试 / 迭代循环。Cursor 里是 `create-skill`（内置），其他地方是 Anthropic 的 `skill-creator`。两者都没有的话，遵循 agentskills.io 上的 Agent Skills 格式。下面说的“`create-skill`”指的是你用的那一个。

- `tune description: <skill path>` (the skill exists but didn't trigger when it should have): hand to `create-skill` and run its description-optimization loop.

> - `tune description: <技能路径>`（技能存在，但在本该触发时没有触发）：交给 `create-skill`，跑它的描述优化循环。

- `new skill via create-skill: <kebab-name>`: hand creation to `create-skill`. Do not invent the shape ad hoc.

> - `new skill via create-skill: <短横线名称>`：把创建交给 `create-skill`。不要临时编造形状。

If your environment ships a SKILL.md validator, run it on every touched skill before declaring done.

> 如果你的环境带有 SKILL.md 校验器，在宣布完成之前对每个改动过的技能运行它。

## 6. Summarize for the user｜6. 给用户总结

Short list, no preamble:

> 简短的列表，没有开场白：

- Edits applied: `<skill path>`. What changed, one line each.

> - 已应用的修改：`<技能路径>`。改了什么，每条一行。

- New skills created: `<skill path>`. One line each (rare).

> - 新建的技能：`<技能路径>`。每个一行（很少见）。

- Backlog filed to the devex tracker: `<issue title>` (`<tags>`). One line each.

> - 已提交到开发体验跟踪系统的待办：`<问题标题>`（`<标签>`）。每条一行。

- Dropped: one line per rejected finding + reason from the synthesizer.

> - 已丢弃：每个被否决的发现一行，附上综合者给出的理由。
