<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/eval.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/eval.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/eval.md)

### Eval｜评测

**You own the experiment design. Plan, blind, run, synthesize.**

> **实验设计由你负责。规划、盲化、运行、综合。**

**Non-negotiables for blinding:**

> **盲化的不可妥协规则：**

- No `eval`, `test`, `judge`, `experiment`, `rubric`, `score`, `compare`, `benchmark`, `candidate`, or `arena` in any directory, file, or prompt the candidate sees.

> - 候选者能看到的任何目录、文件或提示词里，都不能出现 `eval`、`test`、`judge`、`experiment`、`rubric`、`score`、`compare`、`benchmark`、`candidate` 或 `arena`。

- The candidate prompt looks like an organic user request. State the goal, not the meta.

> - 给候选者的提示词要像一个自然的用户请求。说目标，不说元信息。

- No chain-eliciting cues. Don't ask the candidate to list which skills, principles, or files they applied. Ask for design notes generally and grade chain-following from code shape, not self-report.

> - 不要有引导它暴露执行链条的提示。不要让候选者列出它用了哪些技能、原则或文件。只泛泛地要设计说明，并根据代码的形状来评判它是否遵循了链条，而不是根据它的自我报告。

- Sanitize directory and slug names. Use project-shaped names a user might pick.

> - 净化目录名和 slug 名。用用户可能会起的、像项目名的名字。

- Don't tell the candidate other candidates exist.

> - 不要告诉候选者还有其他候选者。

- The judge can know it's judging but sees outputs by sanitized label only, never by model name.

> - 评委可以知道自己在评判，但只能通过净化后的标签看到产出，永远看不到模型名。

- Comparing two variants: one judge scores both sets in a single pass on one scale, blind to which set each came from.

> - 比较两个变体时：由一个评委一次性在同一个尺度上给两组打分，并且不知道每组来自哪个变体。

**Steps:**

> **步骤：**

1. **Frame.** State what variant is under test and what behavior counts as success. Write the rubric (3-6 concrete criteria) for the judge only. Hold it back from candidates.

> 1. **框定。** 说明要测试的是哪个变体，什么样的行为算成功。只为评委写评分标准（3 到 6 条具体标准）。不要让候选者看到。

2. **Set up sanitized environments.** Per-candidate working dir with the variant in place. Plant any context an organic task would have: a project skeleton, the skills the candidate would naturally read.

> 2. **搭建净化后的环境。** 每个候选者一个工作目录，变体已就位。放入一个自然任务会有的所有上下文：项目骨架、候选者自然会去读的技能。

3. **Author one organic prompt.** What a user would type. No leakage of what's being measured.

> 3. **写一个自然的提示词。** 就是用户会输入的那种。不泄露要测量什么。

4. **Spawn N parallel candidates** on different models per the **arena** skill's Phase B. Each works in its own sanitized dir. Same prompt to each. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 4. **按 **arena** 技能的阶段 B，在不同模型上派生 N 个并行候选者。** 每个都在自己净化后的目录里工作。给每个同样的提示词。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

5. **Spawn one blinded judge** on a different model family per the **arena** skill's Phase C. Judge sees outputs by sanitized label and the rubric, never a model name.

> 5. **按 **arena** 技能的阶段 C，在另一个模型家族上派生一个盲评委。** 评委只看到净化后标签下的产出和评分标准，永远看不到模型名。

6. **Verify the chain from transcripts, not self-report.** Read each candidate's local transcript in the active workspace's session directory (in Cursor, the `agent-transcripts/` directory the system prompt names. Other harnesses: see **Harness** in SKILL.md). Do not glob across every project's directory. That crosses workspace boundaries and reads private chats from unrelated projects. Look at which files each candidate actually opened. Grade chain-following from the files it really read plus the shape of the code, never from the candidate's own claims.

> 6. **根据对话记录而不是自我报告来核实执行链条。** 读每个候选者在当前工作区会话目录里的本地对话记录（在 Cursor 里是系统提示指明的 `agent-transcripts/` 目录。其他运行环境：见 SKILL.md 里的 **运行环境**）。不要在所有项目的目录里通配查找。那会越过工作区边界，读到无关项目的私人对话。看每个候选者实际打开了哪些文件。根据它真正读过的文件加上代码的形状来评判它是否遵循了链条，永远不要根据候选者自己的说法。

7. **Read every candidate output yourself** end to end. Compare to the judge's verdict. Disagreement means a model is biased or the rubric is ambiguous. Synthesize.

> 7. **自己从头到尾读每个候选者的产出。** 和评委的结论比较。有分歧就说明某个模型有偏见，或者评分标准有歧义。综合得出结论。

**Reply:** variant under test, rubric, per-candidate notes, judge's verdict, your synthesis, and a recommendation for whether to promote the variant.

> **回复：** 被测变体、评分标准、每个候选者的说明、评委的结论、你的综合判断，以及是否推广该变体的建议。
