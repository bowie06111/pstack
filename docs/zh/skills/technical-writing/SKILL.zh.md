<!-- 中英对照译文。原文：skills/technical-writing/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/technical-writing/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/technical-writing/SKILL.md)

```yaml
name: technical-writing
description: "Layered technical-writing standard: Diátaxis structure, Google developer style sentences, STE instruction rules, Global English syntax. Use for /technical-writing or when writing or reviewing docs, RFCs, readmes, PR descriptions, or commit messages."
disable-model-invocation: true
```

> **说明（description）：** 分层的技术写作标准：Diátaxis 结构、Google 开发者风格的句子、STE 的指令规则、Global English 的句法。用于 /technical-writing，或在编写或审查文档、RFC、README、PR 描述或提交信息时使用。

# Technical writing｜技术写作

Write so a tired engineer understands on the first read. Apply all four layers.

> 写得让一个疲惫的工程师第一遍就能看懂。四层全部应用。（译注：这套标准主要针对英文写作，但其中的道理大多同样适用于中文技术写作。）

Three rules sit above the layers:

> 有三条规则凌驾于各层之上：

- **Cut every word that does no work.** If the sentence survives without a word, the word goes. "In order to" is "to". "It is important to note that" is nothing.

> - **删掉每一个不干活的词。** 如果去掉某个词句子仍然成立，这个词就该删。“In order to”就是“to”。“It is important to note that（需要注意的是）”什么都不是。

- **Use the short, everyday word.** "Use", not "utilize". "Help", not "facilitate". "Do", not "perform". A long word has to buy its length with precision.

> - **用简短的日常词。** 用“use”，不用“utilize”。用“help”，不用“facilitate”。用“do”，不用“perform”。长词必须用精确性来换取它的长度。

- **When a rule makes a sentence worse, fix the sentence another way or leave it alone.** A sentence that follows every rule and sounds like a machine wrote it has failed.

> - **当一条规则让句子变得更糟时，换一种方式修句子，或者别动它。** 一个遵守了每条规则、却读起来像机器写的句子，是失败的。

The codebase is the word list. Write the real symbol, file, flag, or command name, not a synonym or a description of it.

> 代码库就是词汇表。写真实的符号、文件、参数或命令名，而不是它的同义词或对它的描述。

Don't invent jargon. Use the words a developer would say out loud: "move", "delete", "a budget that only decreases", not "evacuate", "ratchet", or "endgame". A named pattern is fine when the doc says what it means the first time. Propose a new offender and its replacement as an addition to `unslop`'s abstract-metaphor rule in your reply, with the diff. Don't edit that skill.

> 不要发明行话。用开发者会说出口的词：“移动”“删除”“只会减少的预算”，而不是“evacuate”“ratchet”或“endgame”。一个有名字的模式是可以的，只要文档在第一次出现时说明它的意思。如果发现了新的违规词及其替换词，就在回复里把它作为对 `unslop` 抽象比喻规则的补充提出来，附上 diff。不要编辑那个技能。

## Vary the rhythm｜变化节奏

A doc can obey every layer and still read machine-written: every sentence clipped short, no view anywhere, nothing specific.

> 一份文档可以遵守每一层规则，却仍然读起来像机器写的：每个句子都剪得很短，哪里都没有观点，没有任何具体的东西。

- Mix sentence lengths on purpose. Short sentences land a point. Longer ones carry a fact with its condition or consequence.

> - 有意混合句子长度。短句让一个要点落地。长句承载一个事实以及它的条件或后果。

- One thought per sentence does not mean one length per sentence. Split the sentence that carries two thoughts. Keep the long sentence that carries one.

> - 一句话一个意思，不等于每句话一样长。把承载两个意思的句子拆开。保留承载一个意思的长句。

- Have a view where the mode allows it. Explanation weighs trade-offs, so say what you make of them instead of listing pros and cons. Reference stays dry.

> - 在模式允许的地方要有观点。解释类文档要权衡取舍，所以说出你的看法，而不是罗列利弊。参考类文档保持干巴巴。

- Be specific over sterile. Not "schema changes can cause issues" but "a column rename fails the build".

> - 具体胜过枯燥。不是“改 schema 可能会引发问题”，而是“重命名一列会让构建失败”。

## Pick the mode first (Diátaxis)｜先选模式（Diátaxis）

One document, one mode. Two questions pick it: does the content inform action (doing) or understanding (thinking), and does it serve learning or work?

> 一份文档，一种模式。两个问题决定选哪种：内容是指导行动（做）还是帮助理解（想）？它服务于学习还是工作？

- Action + learning: **tutorial**.

> - 行动 + 学习：**教程**。

- Action + work: **how-to**.

> - 行动 + 工作：**操作指南**。

- Understanding + work: **reference**.

> - 理解 + 工作：**参考**。

- Understanding + learning: **explanation**.

> - 理解 + 学习：**解释**。

Use the compass on a whole document or on one sentence.

> 这个罗盘既可以用在整份文档上，也可以用在单个句子上。

**Tutorial: learning by doing.** The learner's success is your job, not theirs. Open by saying what the learner will build, not what they will "learn". Every step produces a visible result, early and often. Tell them what they should see: the expected output, the prompt change, the log line. Cut explanation to one clause and a link. Stay concrete. Write as "we", in commands: "First, do x. Now, do y."

> **教程：在做中学。** 学习者的成功是你的责任，不是他们的。开头说学习者要构建什么，而不是他们要“学”什么。每一步都要产生可见的结果，早出现、常出现。告诉他们应该看到什么：预期的输出、提示符的变化、那一行日志。解释压缩到一个从句加一个链接。保持具体。用“我们”，用命令句：“首先，做 x。现在，做 y。”

**How-to: steps to a goal.** Solve a problem a person has, not an operation the machine can perform. Assume competence and skip teaching. Action only: no digressions, no background, no completeness for its own sake. Link those instead. Allow forks and judgment: "If you want x, do y." Name the guide by the task: "How to calibrate the radar array", not "Radar array calibration".

> **操作指南：通往目标的步骤。** 解决一个人遇到的问题，而不是机器能执行的某个操作。假定读者有能力，跳过教学。只写行动：不离题、不讲背景、不为了完整而完整。那些内容用链接。允许分岔和判断：“如果你想要 x，就做 y。”用任务来命名指南：“如何校准雷达阵列”，而不是“雷达阵列校准”。

**Reference: facts for lookup.** Only describe. No instruction, no persuasion, no opinion. Be dry, complete, and sure. State facts, options, limits, and errors with no hedging. Mirror the structure of the thing described, so code and docs can be navigated together. Put material where readers expect it. Generate from code where possible.

> **参考：供查阅的事实。** 只做描述。没有指示，没有说服，没有观点。干巴巴、完整、确定。陈述事实、选项、限制和错误，不加保留语气。镜像所描述对象的结构，让代码和文档可以对照着浏览。把内容放在读者预期的位置。能从代码生成的就从代码生成。

**Explanation: understanding and why.** One bounded topic, readable away from the product. Each title should tolerate an implicit "About..." in front. Anchor on a real why question. Give context: design decisions, history, constraints, alternatives. Opinion is allowed here and nowhere else.

> **解释：理解与为什么。** 一个有边界的话题，离开产品也能读。每个标题前面都应该能默默加上一个“关于……”。围绕一个真实的“为什么”问题展开。给出背景：设计决定、历史、约束、备选方案。观点在这里可以有，其他地方都不行。

Don't mix modes: no reference tables inside a tutorial, no tutorial hand-holding inside reference, no arguing inside a how-to. Split and link instead.

> 不要混合模式：教程里不放参考表格，参考里不手把手教，操作指南里不争论。拆开并加链接。

## Write sentences to the reader (Google developer style)｜对着读者写句子（Google 开发者风格）

- Talk to the reader as "you", in the present tense. "Will" only for things that genuinely happen later.

> 用“你”对读者说话，用现在时。只有确实之后才会发生的事才用“将会”。

- Say who does what: "the compiler checks", not "is checked". Passive is fine only when the actor is unknown or beside the point.

> - 说清谁做什么：“编译器检查”，而不是“被检查”。只有当动作的发出者未知或无关紧要时，才用被动。

- Write instructions as commands: "Click Submit." State facts plainly. Never "should be done".

> - 把指示写成命令句：“点击提交。”平实地陈述事实。永远不要说“应该被完成”。

- Put the condition before the instruction: "To delete the document, click Delete."

> - 把条件放在指示前面：“要删除文档，点击删除。”

- Put the common case first. Exceptions after.

> - 先写常见情况。例外放在后面。

- Sound like a knowledgeable friend. No buzzwords, no figurative language, no "please" in instructions, and never "simply", "easy", or "quickly" in a procedure. If it were simple, the reader would not be here.

> - 听起来像一个懂行的朋友。不用时髦词，不用比喻性的语言，指示里不说“请”，步骤里永远不说“简单地”“轻松”或“很快”。如果真那么简单，读者就不会在这里了。

- Don't pre-announce ("we will soon support...") and don't start consecutive sentences with the same phrase.

> - 不要预告（“我们很快将支持……”），也不要让连续的句子以同一个短语开头。

- Link with words that say where the link goes: the page title or a short description. Never "click here". Prefer a sentence of context on the page over a link off it.

> - 链接文字要说明链接通往哪里：页面标题或简短描述。永远不要写“点击这里”。宁可在页面上写一句上下文，也不要链接出去。

- Headings carry the point, not just the topic ("Pick the mode first", not "Modes"). Sentence case. A task heading is a bare verb phrase ("Create an instance"). A concept heading is a noun phrase. One h1 per page, no skipped levels.

> - 标题承载要点，而不只是话题（“先选模式”，而不是“模式”）。用句首大写。任务类标题是一个光秃秃的动词短语（“创建一个实例”）。概念类标题是一个名词短语。每页一个 h1，不跳级。

- Numbered lists for sequences, bullets for everything else. Introduce a list with a complete sentence. Keep items parallel.

> - 有顺序的用编号列表，其他一律用要点。用一个完整的句子引出列表。条目保持并列。

- Code goes in code font. UI elements go in bold. Use serial commas. Drop "etc." and say up front that a list is partial.

> - 代码用代码字体。界面元素用粗体。用连续逗号。不要写“等等”，而是事先说明这个列表不完整。

## Make statements load one at a time (STE rules)｜让陈述一次加载一个（STE 规则）

- One instruction per sentence. One thought per sentence everywhere else.

> - 每句话一条指示。其他地方每句话一个意思。

- Split instructions longer than about 20 words and other sentences longer than about 25.

> - 超过约 20 个词的指示、超过约 25 个词的其他句子，都要拆开。

- Put the warning or condition before the step it guards: "If hot oil touches your skin, injuries can occur."

> - 把警告或条件放在它所保护的步骤前面：“如果热油碰到皮肤，可能会受伤。”

- Keep "the" and "a": "Remove backup file" reads two ways. "Remove the backup file" reads one.

> - 保留“the”和“a”：“Remove backup file”有两种读法。“Remove the backup file”只有一种。

- Give each word one meaning and one job, then keep it. If "check" means inspect, don't also use it for restrain.

> - 每个词只给一个意思、一个用途，然后坚持下去。如果“check”的意思是检查，就不要再用它表示约束。

- Pick one word per action and stick to it: "start", not "start" here and "initiate" there.

> - 每个动作选一个词并坚持用：用“start”，而不是这里用“start”、那里用“initiate”。

- Write procedures as direct commands, never as narration and never in the passive: "Install the component", not "the component must be installed".

> - 把步骤写成直接的命令，永远不要写成叙述，也永远不要用被动：“安装这个组件”，而不是“这个组件必须被安装”。

- Avoid "-ing" words where you can.

> - 能避免就避免“-ing”形式的词。

## Leave no sentence open to two readings (Global English)｜不让任何句子有两种读法（Global English）

- Keep words like "only" and "not" next to the word they change: "only fails on growth" and "fails only on growth" say different things.

> - 把“only”和“not”这样的词放在它所修饰的词旁边：“only fails on growth”和“fails only on growth”说的是不同的事。

- Break up long noun strings: "the proto import budget check script" becomes "the script that checks the proto-import budget".

> - 拆开长长的名词串：“the proto import budget check script”改成“检查 proto 导入预算的那个脚本”。

- Make every "it", "they", and "this" point at one obvious thing. Repeat the noun when in doubt. Never use "this" or "which" to point at a whole clause.

> - 让每个“它”“它们”和“这个”都指向一个明显的东西。拿不准时就重复那个名词。永远不要用“这”或“which”去指代一整个从句。

- Don't drop verbs: "Phase 1 moves the converters and Phase 2 the runtime" leaves Phase 2 without one. Give it one.

> - 不要省略动词：“第 1 阶段移动转换器，第 2 阶段运行时”让第 2 阶段没有动词。给它一个。

- Keep the small words that show structure, such as "that" in "Ensure that the switch is off". Never trade clarity for word count.

> - 保留表示结构的小词，比如“Ensure that the switch is off”里的“that”。永远不要拿清晰度换字数。

- Repeat the article in a series when it prevents a misread: "the client and the host", not "the client and host", when they are two things.

> - 当能防止误读时，在并列结构里重复冠词：当它们是两样东西时，写“the client and the host”，而不是“the client and host”。

- Say which parts "and" or "or" joins when a sentence can group two ways. "Both...and", "either...or", and "if...then" are free disambiguators.

> - 当一个句子有两种分组方式时，说清“和”或“或”连接的是哪几部分。“既……又……”“要么……要么……”和“如果……那么……”是免费的消歧工具。

- Use periods, not semicolons. Replace an em dash with a new sentence.

> - 用句号，不用分号。用一个新句子代替长破折号。

- Make text in parentheses a full grammatical unit or its own sentence. Never form plurals with "(s)".

> - 括号里的文字要么是完整的语法单位，要么自成一句。永远不要用“(s)”构成复数。

- No slashes: write "a, b, or both" instead of "a/b" or "and/or".

> - 不用斜杠：写“a、b 或两者”，而不是“a/b”或“and/or”。

- Call each thing by one name, everywhere. A doc that says "the gate", "the ratchet", and "the budget check" for one thing teaches three things. Don't churn what didn't change.

> - 每样东西只用一个名字，处处如此。一份文档把同一样东西叫作“关卡”“棘轮”和“预算检查”，就是在教三样东西。没变的东西不要换说法。

- Skip idioms, colloquialisms, Latin abbreviations, and metaphors.

> - 不用习语、口语、拉丁缩写和比喻。

## Voice and repo specifics｜语气和仓库的具体要求

- Apply the **unslop** skill to every doc this skill touches. That skill owns the slop-pattern catalog: AI vocabulary, filler, hedging, formatting tells.

> - 对这个技能触及的每份文档应用 **unslop** 技能。那个技能负责 AI 垃圾模式的目录：AI 词汇、填充语、保留语气、格式上的痕迹。

- PR descriptions and commit messages are writing too. Every layer except Diátaxis applies to them. A PR body is a briefing that a reviewer can read in under a minute. Do not paste swarm logs, SHA lists, or metric tables. Link them.

> - PR 描述和提交信息也是写作。除了 Diátaxis，每一层都适用于它们。PR 正文是一份审查者一分钟内就能读完的简报。不要贴 swarm 日志、SHA 列表或指标表格。给链接。

- Product UI strings are not documentation. Use your product's copy guidelines for those.

> - 产品界面上的文字不是文档。那些用你产品的文案指南。

- Indent code snippets with tabs. Write real paths. Make every count or tree claim true at the commit that lands it, and include the command that regenerates it.

> - 代码片段用 Tab 缩进。写真实的路径。让每一个关于数量或目录树的说法在落地它的那个提交上都是真的，并附上重新生成它的命令。

## Worked example｜示例

Before:

> 修改前：

> Configuration of the proto import ratchet budget script parameters is performed via budget.json. Note that it's important to remember that running with --write, which updates the committed budget to reflect the current count, should only be done when lowering it. If exceeded, CI fails.

> > proto 导入棘轮预算脚本参数的配置是通过 budget.json 进行的。需要注意的是，重要的是要记住，带 --write 运行（它会更新已提交的预算以反映当前计数）应该只在降低预算时进行。如果超出，CI 会失败。

After:

> 修改后：

> `budget.mjs` reads the committed budget from `budget.json` and counts the files that import protos. If the count exceeds the budget, CI fails. Run `budget.mjs --write` only to lower the budget.

> > `budget.mjs` 从 `budget.json` 读取已提交的预算，并统计导入 proto 的文件数。如果数量超过预算，CI 就会失败。只有在要降低预算时才运行 `budget.mjs --write`。
