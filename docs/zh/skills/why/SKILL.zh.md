<!-- 中英对照译文。原文：skills/why/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/why/SKILL.md)

```yaml
name: why
description: "Use for 'why does X work this way', 'why we picked Y', design rationale, regressions, postmortems, or data-backed thresholds. Discovers available MCPs and queries each evidence category (source control, issue tracker, long-form docs, real-time chat, infrastructure observability, error tracking, product analytics warehouse) in parallel, then returns a cited read on decisions and tradeoffs. Use how for runtime behavior."
disable-model-invocation: true
```

> **说明（description）：** 用于“X 为什么这样工作”“我们为什么选了 Y”、设计理由、回归、复盘，或有数据支撑的阈值。发现可用的 MCP，并行查询每一类证据（源代码管理、问题跟踪、长篇文档、实时聊天、基础设施可观测性、错误跟踪、产品分析数据仓库），然后返回一份带出处的、关于决定和取舍的解读。关于运行时行为用 how。

# Why｜Why（为什么）

Investigate the motivation and intent behind code. The companion `how` skill answers what the code does and how it works. `why` answers what forces led to its shape.

> 调查代码背后的动机和意图。配套的 `how` 技能回答代码做什么、怎么工作。`why` 回答是哪些力量促成了它现在的形状。

Each spawn below names a role line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses) and a default. Set `model` to that line's value, or to the default if the file or the line is missing. Leave `model` unset when the value is `auto` or `inherit-parent`. If your subagent tool rejects a slug, use the default and say so. If it rejects the default, use the closest valid slug of the same family from its error message or your harness's model list. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **poteto-mode** skill.

> 下面每一次派生都对应 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的一个角色行和一个默认值。把 `model` 设为该行的值，如果文件或这一行不存在，就用默认值。值为 `auto` 或 `inherit-parent` 时不设 `model`。如果你的子代理工具拒绝了某个模型名，就用默认值并说明。如果它连默认值也拒绝，就从它的错误信息或你运行环境的模型列表里，选同一家族中最接近的有效模型名。按 **poteto-mode** 技能的“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

Every spawn is `generalPurpose` with `readonly: false` (agent mode). **Do not use readonly/Ask mode.** It strips MCP access, which disables the MCP-backed investigators and the synthesizer's citation spot-checks. Investigators still shouldn't write anything.

> 每次派生都是 `generalPurpose`，`readonly: false`（代理模式）。**不要用只读/Ask 模式。** 它会去掉 MCP 访问，让依赖 MCP 的调查者和综合者的出处抽查都失效。调查者仍然不应该写任何东西。

## Operating Posture｜工作姿态

Operate as a **careful, cautious, and precise investigator**. Be honest about what you know vs what you're inferring. Read `references/epistemics.md` for the full confidence framework and phrasing guide. The synthesizer must follow it.

> 像一个**细心、谨慎、精确的调查者**那样工作。对你知道的和你推断的保持诚实。完整的置信度框架和措辞指南见 `references/epistemics.md`。综合者必须遵循它。

## Step 1. Understand the Target and the Question｜第 1 步：理解目标和问题

The **target** is usually a chunk of code, a pattern, a feature, or a named design decision. The **question** is usually a design rationale, a tradeoff, a motivating edge case, an external constraint, dead code, or a broad history sweep.

> **目标**通常是一段代码、一种模式、一个功能，或一个有名字的设计决定。**问题**通常是设计理由、一个取舍、一个促成改动的边界情况、一个外部约束、死代码，或者一次大范围的历史梳理。

If the target is vague ("why do we do it this way?" with no clear referent), make your best guess from conversation context (open files, recent edits, cursor location, what was just discussed). State your interpretation briefly so the user can redirect if you're off, then proceed.

> 如果目标很模糊（“我们为什么这么做？”却没有明确的指代对象），就根据对话上下文（打开的文件、最近的修改、光标位置、刚讨论过的内容）做出最好的猜测。简要说出你的理解，以便用户在你理解偏了时纠正，然后继续。

## Step 2. Establish the Code Anchor｜第 2 步：建立代码锚点

Before spawning investigators, build the seed context inline: file paths and line ranges, key symbols (functions, classes, constants), the last few commits touching the target, PR numbers from merge commits (pattern `(#1234)` in the subject line), and any linked ticket IDs. Run `git blame -L <start>,<end> <file>` for last-touch commits, `git log --follow -p -- <file>` for full history with patches through renames, `git log --oneline -20 -- <file>` for recent commits with PR numbers visible, and `git log -1 --format=%B <commit>` to extract PR numbers. Then pull the PR bodies and discussion for any substantive commit: `gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews`. This anchor goes to every investigator and to the synthesizer.

> 在派生调查者之前，先就地构建种子上下文：文件路径和行号范围、关键符号（函数、类、常量）、最近几个触及目标的提交、合并提交里的 PR 编号（标题行里 `(#1234)` 这样的模式），以及任何关联的工单编号。运行 `git blame -L <start>,<end> <file>` 找最后修改的提交，`git log --follow -p -- <file>` 看跨越重命名的完整历史和补丁，`git log --oneline -20 -- <file>` 看能显示 PR 编号的最近提交，`git log -1 --format=%B <commit>` 提取 PR 编号。然后为每个有实质内容的提交拉取 PR 正文和讨论：`gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews`。这个锚点要交给每个调查者和综合者。

## Step 3. Spawn Parallel Investigators｜第 3 步：派生并行的调查者

**Default to the full parallel investigation.** The goal is a complete coverage map, not a minimal one. Document the null, don't skip the search.

> **默认做完整的并行调查。** 目标是一张完整的覆盖地图，而不是最小的。记录“没有结果”，不要跳过搜索。

### Discovery｜发现

Before spawning investigators, list the MCP servers connected to your harness. Use the available-tools map when present. In Cursor, you can also inspect the `mcps/` directory it exposes for enabled MCP servers. In other harnesses, MCP tools usually appear in your tool list with the server name in the tool name (for example `mcp__<server>__<tool>` in Claude Code).

> 在派生调查者之前，列出连接到你运行环境的 MCP 服务器。有可用工具映射时就用它。在 Cursor 里，你还可以检查它暴露的 `mcps/` 目录，看哪些 MCP 服务器已启用。在其他运行环境里，MCP 工具通常出现在你的工具列表里，工具名里带着服务器名（比如 Claude Code 里的 `mcp__<server>__<tool>`）。

Map each available MCP to one evidence category:

> 把每个可用的 MCP 对应到一个证据类别：

1. Source control history

> 1. 源代码版本历史

2. Issue / ticket tracker

> 2. 问题 / 工单跟踪

3. Long-form documents

> 3. 长篇文档

4. Real-time team chat

> 4. 实时团队聊天

5. Infrastructure observability

> 5. 基础设施可观测性

6. Error / exception tracking

> 6. 错误 / 异常跟踪

7. Product analytics warehouse

> 7. 产品分析数据仓库

Source control is always available through git and `gh`. For the other six, classify using the MCP name, server instructions, tool names, and resource descriptors. If an MCP could fit more than one category, choose the one matching its primary evidence. Record ambiguous cases in the coverage map.

> 源代码管理总是可以通过 git 和 `gh` 使用。对于其他六类，用 MCP 名称、服务器说明、工具名和资源描述来分类。如果某个 MCP 可以归入不止一个类别，就选与它主要证据相符的那一个。把有歧义的情况记在覆盖地图里。

Spawn one investigator per category that has a matching MCP, all in a single message so they run concurrently, each owning exactly one tool or MCP. Don't ask one agent to cover multiple MCPs. `model`: the `why investigators` line, default `grok-4.7-xhigh-fast`.

> 每个有匹配 MCP 的类别派生一个调查者，全部放在一条消息里让它们并发运行，每个恰好负责一个工具或 MCP。不要让一个代理覆盖多个 MCP。`model`：`why investigators` 这一行，默认 `grok-4.7-xhigh-fast`。

Each investigator's prompt is `references/investigator-prompt.md` filled in, plus:

> 每个调查者的提示词是填好的 `references/investigator-prompt.md`，再加上：

- the category playbook `references/sources/<source>.md` for the selected MCP, adapted from the examples in `references/source-playbook.md`

> - 所选 MCP 对应的类别操作手册 `references/sources/<source>.md`，根据 `references/source-playbook.md` 里的示例改编

- `references/sources/incident-postmortem.md` **if the target code looks defensive** (null checks, retry logic, timeout handling, rate limiting, feature flags, egress guards, OOM handlers)

> - `references/sources/incident-postmortem.md`，**如果目标代码看起来是防御性的**（空值检查、重试逻辑、超时处理、限流、功能开关、出口防护、内存溢出处理）

- the code anchor and the user's original question

> - 代码锚点和用户的原始问题

**Other harnesses.** The spawns in this skill use Cursor's `Task` tool. In another harness, use its subagent tool: `Agent` in Claude Code (`subagent_type: general-purpose`), `task` in OpenCode (`subagent_type: general`), `spawn_agent` in Codex. Keep the prompt and the model. Drop parameters your tool doesn't have. If your harness has no subagent tool, as in Pi without an extension, run each investigator yourself, one after another.

> **其他运行环境。** 本技能里的派生使用 Cursor 的 `Task` 工具。在其他运行环境里，用它的子代理工具：Claude Code 里是 `Agent`（`subagent_type: general-purpose`），OpenCode 里是 `task`（`subagent_type: general`），Codex 里是 `spawn_agent`。保留提示词和模型。去掉你的工具没有的参数。如果你的运行环境没有子代理工具，比如没装扩展的 Pi，就自己依次扮演每个调查者。

### Investigator roster, one per available evidence category｜调查者名单，每个可用的证据类别一个

Each category below is uniquely positioned to surface one shape of "why", shown after "Best at". Use it to know what to expect back, to name a gap when a category returns empty, and, only in the rare provably-irrelevant case, to justify a skip.

> 下面每个类别都特别擅长揭示某一种形态的“为什么”，写在“最擅长”后面。用它来预判会得到什么、在某个类别返回空结果时说出缺口，以及只在极少数可证明无关的情况下为跳过提供理由。

1. **Source control** (git history, `gh` for PRs, code comments, tests). Always spawn, the only guaranteed source. Best at *implementation-time rationale captured during review*.

> 1. **源代码管理**（git 历史、用 `gh` 看 PR、代码注释、测试）。总是要派生，这是唯一有保证的信息源。最擅长*实现时在评审中记录下来的理由*。

2. **Issue / ticket tracker** (e.g. Linear, Jira, GitHub Issues, Plane, Shortcut MCP). Best at *the product or business forcing function*. Strongest when the why is external to engineering.

> 2. **问题 / 工单跟踪**（例如 Linear、Jira、GitHub Issues、Plane、Shortcut 的 MCP）。最擅长*产品或业务上的推动力*。当“为什么”来自工程之外时最有力。

3. **Long-form documents** (e.g. Notion, Confluence, Google Docs, Coda MCP). Best at *long-form design rationale*, written out before it becomes code.

> 3. **长篇文档**（例如 Notion、Confluence、Google Docs、Coda 的 MCP）。最擅长*长篇的设计理由*，在它变成代码之前就写好了。

4. **Real-time team chat** (e.g. Slack, Discord, Microsoft Teams, Mattermost MCP). Best at *real-time deliberation that never reached a doc*. Especially important when the source control, ticket, and doc paper trail is thin.

> 4. **实时团队聊天**（例如 Slack、Discord、Microsoft Teams、Mattermost 的 MCP）。最擅长*从没写进文档的实时讨论*。当源代码管理、工单和文档留下的书面记录很少时尤其重要。

5. **Infrastructure observability** (e.g. Datadog, New Relic, Honeycomb, Grafana, Splunk MCP). Best at *infrastructure and runtime reality that motivated the code*. Strongest when the target reacts to an infra signal (timeouts, retries, rate limits, circuit breakers).

> 5. **基础设施可观测性**（例如 Datadog、New Relic、Honeycomb、Grafana、Splunk 的 MCP）。最擅长*促成这段代码的基础设施和运行时现实*。当目标是在对基础设施信号做出反应时（超时、重试、限流、熔断）最有力。

6. **Error / exception tracking** (e.g. Sentry, Rollbar, Bugsnag, Airbrake MCP). Best at *the specific exceptions and error trajectories that motivated defensive or corrective code*. Strongest for catch blocks, null guards, type checks, retries, and other defenses.

> 6. **错误 / 异常跟踪**（例如 Sentry、Rollbar、Bugsnag、Airbrake 的 MCP）。最擅长*促成防御性或纠正性代码的具体异常和错误走势*。对 catch 块、空值防护、类型检查、重试和其他防御手段最有力。

7. **Product analytics warehouse** (e.g. Databricks, Snowflake, BigQuery, ClickHouse, dbt, Redshift MCP). Best at *product and data reality that shaped the code*. Strongest for flag-gated code, experiment-driven ships, data migrations, and "where did this number come from" questions.

> 7. **产品分析数据仓库**（例如 Databricks、Snowflake、BigQuery、ClickHouse、dbt、Redshift 的 MCP）。最擅长*塑造了这段代码的产品和数据现实*。对受功能开关控制的代码、由实验驱动的发布、数据迁移，以及“这个数字是从哪来的”这类问题最有力。

### When to skip an investigator｜什么时候可以跳过一个调查者

Only with an **explicit, written justification** that goes in the final Sources Consulted. Two valid reasons:

> 只有在有**明确的、书面的理由**时才行，这个理由要写进最终的“查阅过的信息源”。两个有效的理由：

- **No MCP is available for that category** in this environment. Flag it as a gap, not a choice: "Real-time team chat skipped. No matching MCP available, so the conversational record was not searchable."

> - **这个环境里没有该类别的 MCP。** 把它标为缺口，而不是一个选择：“跳过了实时团队聊天。没有匹配的 MCP，所以对话记录无法搜索。”

- **The source is provably irrelevant**, not just "probably irrelevant." A high bar: "Error / exception tracking skipped. Target is a build-time script with no runtime code path."

> - **该信息源可以证明无关**，而不只是“大概无关”。门槛很高：“跳过了错误 / 异常跟踪。目标是一个构建时脚本，没有运行时代码路径。”

If your scope assessment suggests a single-commit trivial target where the PR description already contains the complete answer, you may answer inline **only after** confirming all seven available category searches would be redundant. Say so explicitly. This should be rare.

> 如果你的范围评估表明，目标是一个只涉及单个提交的琐碎目标，而且 PR 描述里已经包含了完整的答案，你可以直接就地回答，但**只有在**确认全部七类可用的搜索都是多余的之后才行。要明确说明这一点。这种情况应该很少见。

## Step 4. Synthesize｜第 4 步：综合

Spawn one synthesizer. `model`: the `why synthesizer` line, default `claude-opus-5-5-xhigh`. It gets:

> 派生一个综合者。`model`：`why synthesizer` 这一行，默认 `claude-opus-5-5-xhigh`。它会拿到：

1. The investigator findings, including any null results and any categories skipped with justification

> 1. 调查者的发现，包括任何空结果，以及任何附有理由被跳过的类别

2. The code anchor

> 2. 代码锚点

3. The user's original question

> 3. 用户的原始问题

4. The epistemics framework from `references/epistemics.md`

> 4. `references/epistemics.md` 里的认识论框架

5. The synthesizer prompt template from `references/synthesizer-prompt.md`

> 5. `references/synthesizer-prompt.md` 里的综合者提示词模板

## Step 5. Present｜第 5 步：呈现

Present the synthesizer's output to the user. You may lightly edit for clarity or add context from the conversation, but **do not rewrite the confidence language**.

> 把综合者的输出呈现给用户。你可以为了清晰做些轻微修改，或者补充对话里的上下文，但**不要改写置信度用语**。

## Output Format｜输出格式

The structure is the one in `references/synthesizer-prompt.md`: The Question, The Code in Question, What We Found, What We Can Reasonably Infer, Competing Hypotheses, What We Don't Know, Sources Consulted, Confidence Summary. Adapt as needed, but keep the confidence separation intact, and keep Sources Consulted as one line per investigator, including the ones that returned nothing or were skipped, with the reason.

> 结构就是 `references/synthesizer-prompt.md` 里的那个：问题、所涉及的代码、我们发现了什么、我们可以合理推断的、相互竞争的假设、我们不知道什么、查阅过的信息源、置信度总结。可以按需调整，但要保持置信度的区分，并且让“查阅过的信息源”每个调查者一行，包括什么都没找到或被跳过的那些，并附上原因。

After Sources Consulted, if the user's `why` question is a precursor to changing this code, convert the lineage findings into a Preserve / Change / Avoid / Risk constraint set for planning the change.

> 在“查阅过的信息源”之后，如果用户的 `why` 问题是改动这段代码的前奏，就把这些来龙去脉的发现转换成一组 保留 / 改变 / 避免 / 风险 的约束，用于规划这次改动。

## Common Failure Modes to Avoid｜要避免的常见失败模式

- **Recency bias**. Assuming the most recent commit is authoritative. The current shape is often the accretion of many earlier decisions. Trace back.

> - **近因偏差**。假设最近的提交是权威的。现在的形状往往是许多更早决定的累积。往回追溯。
