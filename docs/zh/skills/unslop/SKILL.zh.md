<!-- 中英对照译文。原文：skills/unslop/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/unslop/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/unslop/SKILL.md)

```yaml
name: unslop
description: Cut AI tells from any writing.
disable-model-invocation: true
```

> **说明（description）：** 从任何文字里去掉 AI 的痕迹。

# Unslop｜Unslop（去 AI 腔）

Edit text to remove AI patterns. Scan for the patterns below, then rewrite. Preserve meaning and match the intended tone.

> 编辑文字，去掉 AI 的写作模式。扫描下面这些模式，然后改写。保留原意，贴合预期的语气。

## Patterns to detect and fix｜要检测和修复的模式

Rule numbers are stable ids that other skills cite. A removed rule leaves a gap.

> 规则编号是固定的 id，其他技能会引用它们。删掉的规则会留下空号。（译注：这些规则主要针对英文写作，但大部分道理同样适用于中文。）

### Content｜内容

3. **Superficial -ing phrases.** "highlighting...", "ensuring...", "reflecting...", "showcasing...", "fostering...". Delete or expand with real sources.

> 3. **浮于表面的 -ing 短语。** “highlighting...（凸显了……）”“ensuring...（确保了……）”“reflecting...（反映出……）”“showcasing...（展示了……）”“fostering...（促进了……）”。删掉，或者用真实的出处展开。

5. **Vague attributions.** "Experts believe", "Industry reports suggest", "Some critics argue". Name the source or delete.

> 5. **模糊的归属。** “专家认为”“行业报告表明”“一些批评者认为”。说出出处，或者删掉。

### Language｜用词

7. **AI vocabulary.** Additionally, crucial, delve, enduring, enhance, fostering, garner, interplay, intricate, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore, vibrant. Replace with plain words.

> 7. **AI 词汇。** Additionally、crucial、delve、enduring、enhance、fostering、garner、interplay、intricate、landscape（抽象用法）、pivotal、showcase、tapestry（抽象用法）、testament、underscore、vibrant。换成平实的词。

8. **Fancy ways to say "is".** "serves as", "stands as", "boasts", "features". Say "is" or "has".

> 8. **花哨地表达“是”。** “serves as（充当）”“stands as（作为）”“boasts（拥有）”“features（具备）”。就说“是”或“有”。

9. **"Not just X, but Y."** State the point directly instead.

> 9. **“不只是 X，更是 Y。”** 直接说出要点。

10. **Rule of three.** Don't force ideas into groups of three. Use the natural number.

> 10. **三连排比。** 不要硬把想法凑成三个一组。用自然的数量。

11. **Synonym cycling.** Protagonist, main character, central figure, hero all in one paragraph. Pick one, repeat it.

> 11. **同义词轮换。** 一段话里主角、主人公、中心人物、英雄轮着用。选一个，重复用它。

12. **False ranges.** "from X to Y" where X and Y aren't on a meaningful scale. List topics directly.

> 12. **虚假的范围。** “从 X 到 Y”，而 X 和 Y 并不在一个有意义的尺度上。直接列出话题。

### Style｜文风

13. **Em dash overuse.** Avoid em dashes entirely. Use periods or commas only (no parentheses, no en dashes, no hyphen-as-dash substitutes). If a thought needs separation, end the sentence or use a comma.

> 13. **滥用长破折号。** 完全不用长破折号。只用句号或逗号（不用括号，不用短破折号，也不用连字符冒充破折号）。如果一个想法需要隔开，就结束这句话或者用逗号。

14. **Colon overuse.** Colons are fine before a list or example, not as mid-sentence connectors. "If you're coming from traditional automation: instead of registering event handlers, you describe conditions" adds nothing with the colon. Rewrite to let the point stand on its own without comparison framing. "Describing when the scheduler should fire works best as plain English." Same meaning, no crutch punctuation.

> 14. **滥用冒号。** 冒号放在列表或例子前面可以，但不能当作句子中间的连接词。“如果你之前用的是传统自动化：不用注册事件处理器，而是描述条件”，这里的冒号什么都没增加。改写成让要点自己站得住、不需要对比框架的句子。“描述调度器什么时候应该触发，用平实的语言最好。”意思一样，不靠拐杖式的标点。

15. **Boldface overuse.** Don't bold every proper noun or acronym.

> 15. **滥用加粗。** 不要把每个专有名词或缩写都加粗。

16. **Inline-header lists.** The tell is a bold label and colon that restates the line: "**Performance:** Performance improved...". Convert those to prose. A bold lead-in that ends in a period, names the item, and is followed by genuinely new detail ("**Schema in TypeScript.** Tables live in one file.") is fine, not a tell.

> 16. **行内标题式的列表。** 痕迹是一个加粗的标签加冒号，然后复述这一行：“**性能：** 性能提升了……”。把这些改成正常的句子。一个以句号结尾、点出条目名称、后面跟着真正新信息的加粗开头（“**Schema 用 TypeScript 写。** 表都放在一个文件里。”）是可以的，不算痕迹。

17. **Title case headings.** Use sentence case.

> 17. **标题每个词首字母大写。** 用句首大写。

18. **Decorative emojis.** Remove from headings and bullets.

> 18. **装饰性的表情符号。** 从标题和要点里去掉。

19. **Curly quotes.** Use straight quotes.

> 19. **弯引号。** 用直引号。

### Communication artifacts｜聊天机器人的痕迹

20. **Chatbot phrases.** "I hope this helps!", "Let me know if...", "Of course!", "Certainly!", "Found the smoking gun!" Remove.

> 20. **聊天机器人用语。** “希望这对你有帮助！”“如果……请告诉我”“当然！”“没问题！”“找到确凿证据了！”删掉。

22. **Sycophantic tone.** "Great question! You're absolutely right!" Respond directly.

> 22. **谄媚的语气。** “好问题！你说得完全正确！”直接回应。

### Filler｜填充语

23. **Filler phrases.** "In order to" becomes "To". "Due to the fact that" becomes "Because". Delete "It is important to note that".

> 23. **填充短语。** “In order to”改成“To”。“Due to the fact that”改成“Because”。删掉“It is important to note that（需要注意的是）”。

24. **Excessive hedging.** "could potentially possibly be argued that it might" becomes "may".

> 24. **过度的保留语气。** “could potentially possibly be argued that it might（或许可能可以认为它也许）”改成“may（可能）”。

25. **Generic conclusions.** "The future looks bright." State specific plans or facts.

> 25. **空泛的结尾。** “未来一片光明。”说出具体的计划或事实。

### Jargon｜行话

26. **Abstract metaphor nouns.** Substrate (base), wedge, wedge in (add), vector (way, method), locus, vantage, nexus, primitive as a noun, harness as a metaphor, surface as in "API surface", bedrock, scaffolding as a metaphor, modality, paradigm, gold-plating (more than the job needs), ratchet as a metaphor (the mechanism's real name, or a limit that only tightens), evacuate for moving code (move out), endgame (the last phase), north star, flywheel. These read as technical but usually have a plainer concrete word. Pick the concrete word.

> 26. **抽象的比喻名词。** Substrate（改用 base，基础）、wedge、wedge in（改用 add，加入）、vector（改用 way、method，方法）、locus、vantage、nexus、作名词用的 primitive、作比喻用的 harness、“API surface”里的 surface、bedrock、作比喻用的 scaffolding、modality、paradigm、gold-plating（超出工作所需的部分）、作比喻用的 ratchet（用机制的真名，或者“只会收紧的限制”）、用 evacuate 表示移动代码（改用 move out，移出）、endgame（最后阶段）、north star、flywheel。这些词读起来像技术术语，但通常都有一个更平实、更具体的词。选那个具体的词。

### Plain speech｜说人话

27. **Say what it does, not how it feels.** "the database stays close at hand", "SQL you can read", "types that follow your schema" name a feeling. The fix names the mechanism or a number: "`.toSQL()` returns the exact string sent to the database", "a column rename fails the build". Ask what the sentence tells the reader to do or know, then write that. If you can't restate it as a concrete instruction, fact, or number, cut it. One more check: if the sentence could appear unchanged in another project's docs, it says nothing about this one. Cut it.

> 27. **说它做了什么，而不是它给人什么感觉。** “数据库就在手边”“你能读懂的 SQL”“跟随你的 schema 的类型”，说的都是一种感觉。修正的方法是说出机制或数字：“`.toSQL()` 返回发给数据库的确切字符串”“重命名一列会让构建失败”。问一问这句话要告诉读者做什么或知道什么，然后就写那个。如果你没法把它重述成一条具体的指示、事实或数字，就删掉它。再检查一遍：如果这句话原封不动地放进另一个项目的文档里也说得通，那它对这个项目什么都没说。删掉它。

28. **Shorten or split dense sentences.** If the reader has to backtrack to parse a sentence, break it in two or drop clauses. One idea per sentence.

> 28. **缩短或拆分冗长的句子。** 如果读者得回头重读才能理解一句话，就把它拆成两句，或者去掉一些从句。一句话一个意思。

29. **Active voice.** Prefer it. Catch "is/are/was/were + past participle" and name the actor: "queries are validated" becomes "the compiler validates queries", "the file is parsed by the loader" becomes "the loader parses the file". Passive is fine only when the actor is unknown or genuinely doesn't matter.

> 29. **主动语态。** 优先用它。找出“is/are/was/were + 过去分词”的结构，说出动作的发出者：“queries are validated（查询被校验）”改成“the compiler validates queries（编译器校验查询）”，“the file is parsed by the loader（文件被加载器解析）”改成“the loader parses the file（加载器解析文件）”。只有当发出者未知或确实无关紧要时，才用被动。

30. **Cut adverbs, or use a stronger verb.** "runs quickly" becomes "is fast" or the number, and "significantly improves" becomes the measured delta. An adverb propping up a weak verb means the verb is wrong.

> 30. **删掉副词，或者用更有力的动词。** “runs quickly（运行得很快）”改成“is fast（快）”或者直接给数字，“significantly improves（显著提升）”改成测出来的差值。需要副词来撑着的弱动词，说明动词选错了。

31. **Prefer the plain word.** "utilize" and "leverage" become "use", "facilitate" becomes "help", "numerous" becomes "many", "in the event that" becomes "if". The fancier synonym is rarely clearer.

> 31. **优先用平实的词。** “utilize”和“leverage”改成“use（用）”，“facilitate”改成“help（帮助）”，“numerous”改成“many（很多）”，“in the event that”改成“if（如果）”。更花哨的同义词很少更清楚。

32. **Mannered prose.** Metaphor or flourish where a literal phrase exists: aphorisms ("wire it or delete it"), rhetorical fragments for effect, personified code ("the plan holds it"), figurative verbs ("rides along", "stands on"), stock framing phrases. "A dial worth turning" becomes "a parameter worth varying". Say what you mean. Rule 26 covers the metaphor nouns.

> 32. **矫揉造作的文字。** 在有字面说法的地方用比喻或修饰：格言式的话（“要么接上，要么删掉”）、为了效果而用的修辞性残句、拟人化的代码（“计划把它握在手里”）、比喻性的动词（“搭着顺风车”“站在……之上”）、套路化的框架短语。“值得一转的旋钮”改成“值得调整的参数”。说你想说的意思。比喻名词见第 26 条。

33. **Over-compression.** Dropped articles, verbless fragments, symbol-speak, and abbreviations that make the reader decode instead of read. "Parser rejects bad date → exit 2, no write" becomes "The parser rejects a bad date, exits with code 2, and writes nothing." Write whole sentences with their articles and verbs, and spell out arrows and abbreviations.

> 33. **过度压缩。** 省掉冠词、没有动词的残句、用符号代替话语，以及让读者去破译而不是阅读的缩写。“Parser rejects bad date → exit 2, no write（解析器拒绝错误日期 → 退出 2，不写）”改成“The parser rejects a bad date, exits with code 2, and writes nothing.（解析器会拒绝错误的日期，以代码 2 退出，并且不写入任何东西。）”写完整的句子，带上冠词和动词，把箭头和缩写都写成文字。
