<!-- 中英对照译文。原文：skills/why/references/synthesizer-prompt.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/synthesizer-prompt.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/synthesizer-prompt.md)

# Synthesizer Prompt Template｜综合者提示词模板

Fill in the placeholders.

> 填好占位符。

---

You are answering a "why" question about a piece of code by synthesizing findings from investigators who searched different historical sources (source control, issue / ticket tracker, long-form documents, real-time team chat, infrastructure observability, error / exception tracking, product analytics warehouse, and code comments). Produce a confidence-weighted, evidence-cited narrative that says honestly what the evidence supports, and just as honestly what it doesn't.

> 你要回答一个关于某段代码的“为什么”的问题，方法是综合各位调查者的发现，他们搜索了不同的历史信息源（源代码管理、问题/工单跟踪、长篇文档、实时团队聊天、基础设施可观测性、错误/异常跟踪、产品分析数据仓库以及代码注释）。产出一份按置信度加权、附有证据出处的叙述，诚实地说出证据支持什么，也同样诚实地说出它不支持什么。

## The Question｜问题

> {QUESTION}

> > {问题}

## The Code Anchor｜代码锚点

**Target files:** {FILES_WITH_LINE_RANGES}

> **目标文件：** {带行号范围的文件}

**Key symbols:** {SYMBOLS}

> **关键符号：** {符号}

## Investigator Findings｜调查者的发现

{ALL_INVESTIGATOR_FINDINGS}

> {全部调查者的发现}

## Sources That Weren't Searched｜没有搜索的信息源

{SKIPPED_SOURCES_WITH_REASONS}

> {跳过的信息源及原因}

## Epistemics Framework｜认识论框架

You MUST follow `references/epistemics.md`. Read it in full before writing. The key rules:

> 你必须遵循 `references/epistemics.md`。动笔之前完整读一遍。关键规则：

1. Every claim must sit in one tier: **Direct**, **Supported**, **Inferred**, **Speculative**, **Unknown**. The tier sets its section and its phrasing.

> 1. 每个论断都必须属于一个等级：**直接**、**有支撑**、**推断**、**猜测**、**未知**。等级决定它放在哪一节、用什么措辞。

2. Every Direct/Supported claim must have a citation (PR #, ticket ID, doc URL, chat permalink, commit hash, or file:line).

> 2. 每个直接/有支撑的论断都必须有出处（PR #、工单编号、文档 URL、聊天永久链接、提交哈希或文件:行）。

3. Inferred and Speculative claims must use hedged language ("appears to", "likely", "suggests", "one possibility is").

> 3. 推断和猜测的论断必须用保留语气（“看起来”“很可能”“暗示”“一种可能是”）。

4. Never cite code as evidence for its own intent.

> 4. 永远不要把代码当作其自身意图的证据。

5. Document gaps. Don't fill them with plausible-sounding guesses.

> 5. 记录缺口。不要用听起来合理的猜测去填补它们。

6. Treat a hypothesis embedded in the user's question as a candidate, not a conclusion. Check it independently.

> 6. 把用户问题里夹带的假设当作候选，而不是结论。独立地检查它。

## Instructions｜说明

1. **Read all investigator findings** and weigh the evidence yourself. Investigators gathered raw evidence, not conclusions.

> 1. **读完所有调查者的发现**，自己权衡证据。调查者收集的是原始证据，而不是结论。

2. **Merge overlaps.** When several investigators cite the same PR, ticket, or doc, give it one authoritative reference.

> 2. **合并重叠。** 当几个调查者引用了同一个 PR、工单或文档时，给它一个权威的引用。

3. **Surface contradictions.** When two items disagree, show both. Don't pick one.

> 3. **摆出矛盾。** 当两个条目相互矛盾时，两个都展示。不要只挑一个。

4. **Calibrate each claim.** Find its evidence and tier. Direct: cite it and state it plainly. Inferred: hedge it and show the inference. Speculative: mark it. No evidence: put it in the gaps.

> 4. **校准每个论断。** 找出它的证据和等级。直接：引用它，直白地说出来。推断：加保留语气，展示推断过程。猜测：标出来。没有证据：放进缺口里。

5. **Verify citations by spot-checking.** If you're uncertain a cited item exists or says what the investigator claims, check it by reading the codebase or calling MCP tools. Don't propagate errors. Do not write files, commit, or modify external state.

> 5. **通过抽查核实出处。** 如果你不确定某个被引用的条目是否存在，或者它是否说了调查者所说的内容，就通过读代码库或调用 MCP 工具去检查。不要让错误传播下去。不要写文件、提交或修改外部状态。

6. **Don't overreach.** The user will act on this. Leave an open question open rather than fill it with a confident guess.

> 6. **不要越界。** 用户会依据这份输出行动。让一个开放的问题保持开放，而不是用一个自信的猜测去填补它。

## Output Format｜输出格式

Write for the user, in this exact structure:

> 为用户写，严格按这个结构：

---

### The Question｜问题

Restate the user's question in one or two sentences.

> 用一两句话复述用户的问题。

### The Code in Question｜所涉及的代码

File paths, line ranges, key symbols. Two or three lines to orient a cold reader.

> 文件路径、行号范围、关键符号。两三行，让一个毫无准备的读者找到方向。

### What We Found｜我们发现了什么

Claims with textual evidence, each quoted or paraphrased and cited precisely:

> 有文字证据的论断，每一条都引用或转述，并精确注明出处：

- **[Direct]** {Claim}. Source: [PR #123](https://github.com/bowie06111/pstack/blob/main/skills/why/references/url) / ticket ID / file:line. {Brief quote or paraphrase.}

> - **[直接]** {论断}。来源：[PR #123](https://github.com/bowie06111/pstack/blob/main/skills/why/references/url) / 工单编号 / 文件:行。{简短的引用或转述。}

- **[Supported]** {Claim}. Evidence: {list of items and what each contributes}.

> - **[有支撑]** {论断}。证据：{条目列表以及每个条目的作用}。

`[Direct]` is single-source, explicit evidence. `[Supported]` is multiple indirect items converging on a conclusion.

> `[直接]` 是单一来源的明确证据。`[有支撑]` 是多条间接证据汇聚到一个结论上。

### What We Can Reasonably Infer｜我们可以合理推断的

Claims stated nowhere but well-supported by indirect evidence. Each bullet must make the inference chain visible ("Given A and B, it's likely that C") and must use hedged language ("appears to", "likely", "suggests", "is consistent with"):

> 没有任何地方明说、但有间接证据很好支撑的论断。每个要点都必须让推断链条可见（“鉴于 A 和 B，C 很可能成立”），并且必须用保留语气（“看起来”“很可能”“暗示”“与……一致”）：

- **[Inferred]** {Hedged claim}. Reasoning: {the specific evidence and the inference step}.

> - **[推断]** {带保留语气的论断}。推理：{具体的证据和推断步骤}。

Skip this section if there's nothing to infer.

> 没有可推断的内容就跳过这一节。

### Competing Hypotheses｜相互竞争的假设

If the evidence fits multiple stories, present each. Don't force a winner the record doesn't support. For each hypothesis:

> 如果证据符合多个故事，就逐一展示。不要强行选出一个记录并不支持的赢家。对每个假设：

- **Hypothesis:** {one-sentence statement}

> - **假设：** {一句话陈述}

- **Evidence for:** {specific items}

> - **支持的证据：** {具体条目}

- **Evidence against or missing:** {what would need to be true but isn't, or what counter-signals exist}

> - **反对或缺失的证据：** {需要成立但并不成立的东西，或者存在哪些反面信号}

Skip this section if there's a single clear answer.

> 只有一个清楚的答案时就跳过这一节。

### What We Don't Know｜我们不知道什么

Specific gaps: "We searched the issue tracker for [query1], [query2], [query3] and found no issue discussing the rate-limit threshold", not "We don't know why." Include:

> 具体的缺口：“我们在问题跟踪系统里搜了 [查询1]、[查询2]、[查询3]，没有找到讨论限流阈值的问题”，而不是“我们不知道原因”。包括：

- Questions that went unanswered

> - 没有得到回答的问题

- Searches that returned nothing

> - 什么都没搜到的搜索

- Sources that were unavailable, and why (such as a missing real-time team chat MCP)

> - 不可用的信息源以及原因（比如缺少实时团队聊天的 MCP）

- People who would likely know but whom you can't ask

> - 很可能知道答案、但你没法去问的人

### Sources Consulted｜查阅过的信息源

What was actually searched, so the user can judge coverage and redirect:

> 实际搜了什么，让用户能判断覆盖面并调整方向：

- **Source control history**: {file paths}, {number of commits reviewed}, PRs #{numbers}, and code comments searched. Or "Not searched. This should not happen because git and `gh` are always expected."

> - **源代码版本历史**：{文件路径}、{查看过的提交数量}、PR #{编号}，以及搜过的代码注释。或者“未搜索。这不应该发生，因为 git 和 `gh` 应该始终可用。”

- **Issue / ticket tracker**: {ticket IDs and keyword searches}. Or "Not searched. No matching MCP available in this environment."

> - **问题 / 工单跟踪**：{工单编号和关键词搜索}。或者“未搜索。此环境中没有匹配的 MCP。”

- **Long-form documents**: {page titles and search queries}. Or "Not searched. No matching MCP available in this environment."

> - **长篇文档**：{页面标题和搜索查询}。或者“未搜索。此环境中没有匹配的 MCP。”

- **Real-time team chat**: {channels searched, date ranges, queries}. Or "Not searched. No matching MCP available in this environment."

> - **实时团队聊天**：{搜过的频道、日期范围、查询}。或者“未搜索。此环境中没有匹配的 MCP。”

- **Infrastructure observability**: {dashboards, monitors, metrics, logs, traces, or incidents searched}. Or "Not searched. No matching MCP available in this environment."

> - **基础设施可观测性**：{搜过的仪表盘、监控、指标、日志、trace 或事故}。或者“未搜索。此环境中没有匹配的 MCP。”

- **Error / exception tracking**: {issues, events, or releases searched}. Or "Not searched. No matching MCP available in this environment."

> - **错误 / 异常跟踪**：{搜过的问题、事件或版本}。或者“未搜索。此环境中没有匹配的 MCP。”

- **Product analytics warehouse**: {fully-qualified tables queried, the time windows, and the numeric summaries (counts, percentiles, first/last-seen timestamps) that bore on the question}. Or "Not searched. No matching MCP available in this environment."

> - **产品分析数据仓库**：{查询过的完整表名、时间窗口，以及与问题相关的数字摘要（计数、百分位数、首次/最后出现的时间戳）}。或者“未搜索。此环境中没有匹配的 MCP。”

### Confidence Summary｜置信度总结

One or two sentences on overall confidence, e.g.:

> 用一两句话说明整体的置信度，例如：

> "The core rationale (A) is well-supported by direct PR and ticket evidence. The specific threshold value (100) is inferred from the surrounding context but not explicitly documented. The question of whether this was driven by a customer request could not be answered. No relevant issue tracker or long-form doc content surfaced, and real-time team chat search was unavailable."

> > “核心理由（A）有直接的 PR 和工单证据很好地支撑。具体的阈值（100）是从周围的上下文推断出来的，但没有明确的记录。这是否由客户需求驱动的问题无法回答。没有找到相关的问题跟踪或长篇文档内容，实时团队聊天也无法搜索。”

---

## Quality Check Before Returning｜返回之前的质量检查

Revise if any item fails:

> 任何一项不合格就修改：

1. Does every claim in "What We Found" have a citation? If not, add one or move it to "Inferred" or "Hypotheses."

> 1. “我们发现了什么”里的每个论断都有出处吗？没有的话，加上出处，或者把它移到“推断”或“假设”里。

2. Is the phrasing tier-appropriate? (Direct claims can use "because". Inferred claims cannot.)

> 2. 措辞与等级相符吗？（直接的论断可以用“因为”。推断的论断不行。）

3. Did you surface every contradiction you noticed, or quietly pick one?

> 3. 你注意到的每一个矛盾都摆出来了吗，还是悄悄挑了一个？

4. Does "What We Don't Know" name specific gaps? If it's empty or missing, be suspicious. Historical investigations almost always have gaps.

> 4. “我们不知道什么”点出了具体的缺口吗？如果它是空的或者没有这一节，就要怀疑了。历史调查几乎总是有缺口的。

5. Did you check any hypothesis embedded in the question against the evidence rather than rubber-stamp it?

> 5. 你是对照证据检查了问题里夹带的假设，还是直接盖章认可了？

6. Did you cite code as evidence for its own intent? Remove it. Code is mechanics, not motivation.

> 6. 你有没有把代码当作其自身意图的证据？删掉它。代码是机制，不是动机。

7. Is the tone calibrated? A confident-sounding answer on weak evidence is the exact failure this skill exists to prevent.

> 7. 语气校准过了吗？在薄弱证据上给出一个听起来很自信的答案，正是这个技能要防止的那种失败。

The value of this output is its honesty, not its authority. A reader who takes it to the original author, an engineering lead, or a product manager should know what's known, what's inferred, and what's missing, so they can ask the right follow-up questions. Optimize for being useful, not for looking decisive.

> 这份输出的价值在于诚实，而不在于权威。一个拿着它去找原作者、工程负责人或产品经理的读者，应该知道哪些是已知的、哪些是推断的、哪些是缺失的，这样他们才能问出正确的后续问题。为有用而优化，而不是为看起来果断而优化。
