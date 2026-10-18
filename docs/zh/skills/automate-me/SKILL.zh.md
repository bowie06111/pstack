<!-- 中英对照译文。原文：skills/automate-me/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/automate-me/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/automate-me/SKILL.md)

```yaml
name: automate-me
description: "Use for \"automate me\", \"create/update/refresh my -mode skill\", \"turn/capture my preferences or working style into a skill\", or wanting agents to follow how the user works. Drafts or revises a personal -mode skill via create-skill + unslop, optionally pulling fresh evidence from recent transcripts."
disable-model-invocation: true
```

> **说明（description）：** 用于“automate me”“创建/更新/刷新我的 -mode 技能”“把我的偏好或工作风格变成/记录成一个技能”，或者希望代理按用户的方式工作。通过 create-skill 加 unslop 起草或修订一个个人的 -mode 技能，可选地从最近的会话记录中提取新证据。

# Automate me｜Automate me（把我自动化）

A guided flow for turning the user's working conventions into a skill agents will follow. The output is one `-mode` skill tailored to them (e.g. `jay-mode`, `priya-mode`).

> 一个引导式的流程，把用户的工作习惯变成代理会遵循的技能。产出是一个为他们量身定制的 `-mode` 技能（例如 `jay-mode`、`priya-mode`）。

This skill orchestrates three others: an inline mining pass (see step 1), your harness's skill-authoring skill (authoring), and the **unslop** skill (prose discipline). It sequences them. It doesn't replace them.

> 这个技能编排了另外三样东西：一次内联的挖掘（见第 1 步）、你运行环境的技能编写技能（负责编写），以及 **unslop** 技能（文字纪律）。它负责安排它们的顺序。它不取代它们。

The skill-authoring skill is `create-skill` in Cursor (built in) or Anthropic's `skill-creator`. If you have neither, follow the Agent Skills format at agentskills.io. "`create-skill`" below means whichever of these you use.

> 技能编写技能在 Cursor 里是 `create-skill`（内置），其他地方是 Anthropic 的 `skill-creator`。两者都没有的话，遵循 agentskills.io 上的 Agent Skills 格式。下面说的“`create-skill`”指的是你用的那一个。

## Flow｜流程

### 0. Check for an existing skill｜0. 检查是否已有技能

Look recursively for `*-mode/SKILL.md` matching the user's handle in your harness's skill folders. In the project: `.cursor/skills/` (Cursor), `.claude/skills/` (Claude Code), `.pi/skills/` (Pi), or `.agents/skills/` (Codex, OpenCode, and others). For the user: `~/.cursor/skills/`, `~/.claude/skills/`, `~/.pi/agent/skills/`, `~/.codex/skills/`, `~/.config/opencode/skills/`, or `~/.agents/skills/`. Mode skills can live in a personal category directory (`<skills>/<handle>/`), not only at the top level. If one exists, confirm intent with your structured-question tool (`AskQuestion` in Cursor, `AskUserQuestion` in Claude Code, `question` in OpenCode) unless they already said "update my skill" or similar:

> 在你运行环境的技能文件夹里，递归查找与用户名对应的 `*-mode/SKILL.md`。项目级：`.cursor/skills/`（Cursor）、`.claude/skills/`（Claude Code）、`.pi/skills/`（Pi）或 `.agents/skills/`（Codex、OpenCode 等）。用户级：`~/.cursor/skills/`、`~/.claude/skills/`、`~/.pi/agent/skills/`、`~/.codex/skills/`、`~/.config/opencode/skills/` 或 `~/.agents/skills/`。模式技能可能放在个人分类目录里（`<skills>/<handle>/`），而不只是顶层。如果已经存在一个，用你的结构化提问工具（Cursor 里是 `AskQuestion`，Claude Code 里是 `AskUserQuestion`，OpenCode 里是 `question`）确认意图，除非用户已经说了“更新我的技能”之类的话：

- Update the existing skill (default for repeat runs)

> - 更新现有技能（重复运行时的默认选项）

- Start fresh (rare, ask why before doing it)

> - 重新开始（很少见，做之前问一下原因）

Update mode changes the rest of the flow:

> 更新模式会改变流程的其余部分：

- Step 1 mines only history since the skill was last edited (`git log -1 --format=%cI <path>`).

> - 第 1 步只挖掘技能上次编辑之后的历史（`git log -1 --format=%cI <path>`）。

- Step 2 asks what's changed or missing, not what to capture from zero.

> - 第 2 步问的是哪些变了或缺了，而不是从零开始要记录什么。

- Step 4 edits the existing file in place. Preserve sections the user hasn't contradicted. Revise ones with new evidence. Add new sections only for genuinely new rules.

> - 第 4 步原地编辑现有文件。保留用户没有反驳的章节。修订有新证据的章节。只为真正新的规则添加新章节。

### 1. Mine their history｜1. 挖掘他们的历史

Locate the active workspace's transcripts before fanning out. Use only that workspace's directory. Don't glob across every project's directory. That crosses workspace boundaries and reads private chats from unrelated projects. Where they live depends on your harness:

> 在扇出之前，先找到当前工作区的会话记录。只用那个工作区的目录。不要在所有项目的目录里通配查找。那会越过工作区边界，读到无关项目的私人对话。会话记录放在哪里取决于你的运行环境：

- **Cursor:** the system prompt names the workspace's `agent-transcripts/` directory under `~/.cursor/projects/`.

> - **Cursor：** 系统提示指明了工作区在 `~/.cursor/projects/` 下的 `agent-transcripts/` 目录。

- **Claude Code:** `~/.claude/projects/<slug>/*.jsonl`, where `<slug>` is the workspace path with every character that isn't a letter or digit turned into "-".

> - **Claude Code：** `~/.claude/projects/<slug>/*.jsonl`，其中 `<slug>` 是把工作区路径里所有非字母数字的字符换成“-”。

- **Pi:** `~/.pi/agent/sessions/--<slug>--/*.jsonl`, where `<slug>` is the workspace path with the leading slash dropped and each "/" turned into "-".

> - **Pi：** `~/.pi/agent/sessions/--<slug>--/*.jsonl`，其中 `<slug>` 是去掉开头斜杠、把每个“/”换成“-”的工作区路径。

- **Codex:** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`. Keep only files whose first line has `payload.cwd` equal to the workspace path.

> - **Codex：** `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`。只保留第一行 `payload.cwd` 等于工作区路径的文件。

- **Other harnesses:** the harness's session directory for this workspace.

> - **其他运行环境：** 该运行环境为这个工作区保存会话的目录。

Survey recent agent conversations within that scope for recurring patterns. Run multiple parallel subagents across slices of history (e.g. last 2-4 weeks, split into 3 slices so each has enough material). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill. Each slice mining subagent reads transcripts from the workspace-scoped path the parent provides, looks for the signals below, and returns a short structured list of patterns it saw with evidence pointers. Default signals worth hunting:

> 在这个范围内浏览最近的代理对话，寻找反复出现的模式。在历史的不同切片上运行多个并行子代理（例如最近 2 到 4 周，分成 3 片，让每片都有足够的材料）。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。每个负责一个切片的挖掘子代理，从父代理提供的、限定在工作区内的路径读取会话记录，寻找下面这些信号，并返回一份简短的结构化列表，列出它看到的模式以及证据指针。默认值得寻找的信号：

- Response preferences (length, tone, format, "dumb it down" corrections)

> - 回复偏好（长度、语气、格式、“说得简单点”之类的纠正）

- Delegation habits (subagents, models, specialized workflows, parallelism)

> - 委派习惯（子代理、模型、专门的工作流、并行）

- Verification posture (what "done" means, unit tests vs live repro, reviewers)

> - 验证姿态（“完成”意味着什么、单元测试还是实地复现、审查者）

- Code and prose discipline (style, principles cited, lint/format tools)

> - 代码和文字纪律（风格、引用的原则、lint/格式化工具）

- Process conventions (worktrees, commits, PRs, review/merge tooling)

> - 流程约定（worktree、提交、PR、评审/合并工具）

- Meta preferences (fixing skills mid-task, proposing new ones)

> - 元偏好（任务中途修技能、提议新技能）

Cross-check across slices before elevating a signal. Patterns seen in 2+ slices are high-confidence. Lone signals are weak and usually get dropped.

> 在把一个信号提升之前，跨切片交叉核对。在 2 个或更多切片里都出现的模式置信度高。只出现一次的信号很弱，通常会被去掉。

### 2. Ask the user directly｜2. 直接问用户

Mining misses intent that hasn't come up yet. Use your structured-question tool (structured multi-choice: `AskQuestion` in Cursor, `AskUserQuestion` in Claude Code, `question` in OpenCode) rather than asking the user to type from scratch. If your harness has none, ask in chat with numbered options.

> 挖掘会漏掉还没出现过的意图。用你的结构化提问工具（结构化的多选：Cursor 里是 `AskQuestion`，Claude Code 里是 `AskUserQuestion`，OpenCode 里是 `question`），而不是让用户从头打字。如果你的运行环境没有这种工具，就在聊天里用带编号的选项提问。

Shape: one or two questions with 4-6 options each, multi-select (`allow_multiple: true` in Cursor) for category questions. Start broad ("Which areas matter most?"), then follow up on selected areas with specific options. After the structured rounds, one free-form chat question catches anything the options missed.

> 形式：一两个问题，每个 4 到 6 个选项，类别类的问题用多选（Cursor 里是 `allow_multiple: true`）。先问宽泛的（“哪些方面最重要？”），再针对选中的方面给出具体的选项跟进。结构化的几轮之后，用一个自由回答的聊天问题，兜住选项漏掉的东西。

Don't dump 20 questions.

> 不要一口气抛出 20 个问题。

### 3. Cluster findings｜3. 把发现归类

Group the combined signals into sections. Common ones (use only what applies):

> 把汇总的信号分成几个章节。常见的有（只用适用的）：

- **Response style**: length, tone, format.

> - **回复风格**：长度、语气、格式。

- **Autonomy**: how much to do without asking, MCP tool use.

> - **自主程度**：不问就做多少事、MCP 工具的使用。

- **Understand first**: which skills to reach for when scoping or investigating a change.

> - **先理解**：界定或调查一个改动时用哪些技能。

- **Subagents**: default, parallelism, model-to-task, specialized workflows.

> - **子代理**：默认做法、并行、模型与任务的对应、专门的工作流。

- **Prose / code discipline**: principles, lint tools, style guides.

> - **文字 / 代码纪律**：原则、lint 工具、风格指南。

- **Review and verify**: repro posture, verification skills, live-testing tools.

> - **审查和验证**：复现的姿态、验证技能、实地测试工具。

- **Process**: git worktrees, commits, PRs, review/merge tooling.

> - **流程**：git worktree、提交、PR、评审/合并工具。

- **Skills**: skill-authoring habits, fix-the-skill-first, proposing new skills.

> - **技能**：编写技能的习惯、先修技能、提议新技能。

The **poteto-mode** skill shows the shape. Read it for granularity. Don't copy its content. The user's rules are not the same as poteto-mode's.

> **poteto-mode** 技能展示了这种形状。读它来把握粒度。不要照抄它的内容。用户的规则和 poteto-mode 的规则不一样。

### 4. Draft the skill｜4. 起草技能

Use your skill-authoring skill (`create-skill`) to author the skill. Placement:

> 用你的技能编写技能（`create-skill`）来编写这个技能。放置位置：

- Path: preserve an existing mode skill's category. For a new mode, use `<skills>/<handle>/<handle>-mode/SKILL.md` when the repo has an established personal category for that handle. Otherwise default to `<skills>/<handle>-mode/SKILL.md` in the project, or the user-level skill folder if the user prefers a personal skill. `<skills>` is your harness's skill folder from step 0.

> - 路径：保留现有模式技能的分类。对于新的模式，如果仓库里已经有该用户名的个人分类，就用 `<skills>/<handle>/<handle>-mode/SKILL.md`。否则默认放在项目里的 `<skills>/<handle>-mode/SKILL.md`；如果用户更想要个人技能，就放在用户级的技能文件夹。`<skills>` 是第 0 步里你运行环境的技能文件夹。

- Handle: the user's first name or chosen identifier.

> - 名字：用户的名字或他们选择的标识。

- Frontmatter `description`: trigger on their name + `/<handle>-mode` + "work in their style", not on generic keywords like "write code" or "review PR".

> - frontmatter 的 `description`：在他们的名字、`/<handle>-mode` 和“按他们的风格工作”上触发，而不是在“写代码”或“审查 PR”这样的通用关键词上触发。

- Frontmatter formatting: follow `create-skill`'s YAML rules. Keep `description` as one YAML scalar. Quote it or use `description: >-` with indented continuation lines when punctuation or wrapping requires it.

> - frontmatter 的格式：遵循 `create-skill` 的 YAML 规则。`description` 保持为一个 YAML 标量。当标点或换行需要时，给它加引号，或者用 `description: >-` 加缩进的续行。

- Frontmatter `disable-model-invocation: true` by default. Opt out only if the user explicitly wants their mode to apply on every turn.

> - frontmatter 默认设 `disable-model-invocation: true`。只有当用户明确希望他们的模式在每一回合都生效时才取消。

### 5. Iterate on prose｜5. 打磨文字

Apply the **unslop** skill and `create-skill`'s writing guidelines to every line.

> 对每一行应用 **unslop** 技能和 `create-skill` 的写作指南。

Show the draft to the user and take feedback. Expect multiple iterations. Cut ruthlessly. A mode skill is not a manual.

> 把草稿给用户看，听取反馈。预计要迭代好几轮。大刀阔斧地删。模式技能不是使用手册。

### 6. Land it｜6. 落地

Work in a worktree off main. Commit and open a PR. Don't push to main directly.

> 在从 main 拉出的 worktree 里工作。提交并开 PR。不要直接推送到 main。

## Guardrails｜护栏

- **Don't overfit to one conversation.** A preference stated once and contradicted another time is noise. Require multiple instances before codifying it.

> - **不要过度拟合某一次对话。** 说过一次、另一次又被推翻的偏好是噪音。要有多次实例才把它写成规则。

- **Don't be clever.** Restating other skills' contents, inventing metaphors, or writing "poetic" prose for an agent reader is cost without benefit. Keep it operational.

> - **不要耍聪明。** 复述其他技能的内容、发明比喻，或者给代理读者写“诗意”的文字，只有成本没有收益。保持可操作。

- **Reference, don't inline.** Other skills the user relies on should appear as path references, not pasted excerpts. Same for any principle docs they maintain elsewhere.

> - **引用，不要内联。** 用户依赖的其他技能应该以路径引用的形式出现，而不是贴摘录。他们在别处维护的原则文档也一样。

- **Keep sections minimal.** Only add a section if the user has a specific, non-default rule there. "Communicate clearly" is not a section. "Short paragraphs. Tables when comparing options. Bullets only when items are genuinely parallel." is.

> - **章节保持最少。** 只有当用户在某方面有具体的、非默认的规则时，才加那一节。“沟通要清楚”不算一节。“段落要短。比较选项时用表格。只有条目确实并列时才用要点。”才算。

- **Name conventions generic.** Use "the user" or "the human" in imperatives, not the author's first name.

> - **命名保持通用。** 在祈使句里用“用户”或“人”，而不是作者的名字。

- **Don't force symmetry.** If a user has no process rules worth writing down, skip the Process section entirely.

> - **不要强求对称。** 如果用户没有值得写下来的流程规则，就整个跳过“流程”一节。

## Evaluation｜评估

A `-mode` skill is subjective output. A `create-skill`-style test/iterate benchmark loop isn't useful here. Vibe-check with the user: does it read like them? Did it miss anything? Then ship.

> `-mode` 技能是主观的产出。`create-skill` 那种测试/迭代的基准循环在这里没什么用。和用户一起凭感觉检查：读起来像他们吗？有没有漏掉什么？然后发布。

Run a description-optimization loop only if the skill's trigger accuracy turns out to be a problem in practice.

> 只有当技能的触发准确率在实际使用中成了问题时，才运行描述优化循环。

## When not to use｜什么时候不要用

- User wants a task-specific skill (not working conventions): `create-skill` alone, no mining required.

> - 用户想要一个针对特定任务的技能（而不是工作习惯）：单独用 `create-skill`，不需要挖掘。

- User wants to capture one narrow workflow (e.g. "how I write commit messages"). That's a regular skill, not a mode skill.

> - 用户想记录一个很窄的工作流（例如“我怎么写提交信息”）。那是一个普通技能，不是模式技能。
