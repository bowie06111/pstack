<!-- 中英对照译文。原文：skills/poteto-help/references/prompting.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-help/references/prompting.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-help/references/prompting.md)

# Word the prompt｜怎么写提示词

A prompt states the intent and the check for done. The playbook supplies the steps, so a few plain sentences beat a spec.

> 提示词说明意图和完成的检验标准。步骤由剧本提供，所以几句平实的话胜过一份规格。

## Put in｜要写的

- The goal. Say what is wrong or what the user wants.

> - 目标。说出哪里有问题，或者用户想要什么。

- The done check. It can pass or fail. "Make it better" and a duration are not checks.

> - 完成的检验。它必须能通过或失败。“弄好一点”和一段时长都不是检验。

- The proof to show. Ask for the real command output, a video of the flow, the stored value, or a before and after number.

> - 要展示的证明。要求真实的命令输出、流程的录屏、存储的值，或前后对比的数字。

- What the user already knows. A symptom, a repro step, a log, or a link saves the agent a search.

> - 用户已经知道的东西。一个症状、一个复现步骤、一段日志或一个链接，能替代理省一次搜索。

- The real constraints. "repro first", "don't change any code yet", "zero behavior change", and "let me review before proceeding" each change what the agent does.

> - 真实的约束。“先复现”“先别改代码”“行为零变化”和“让我审查后再继续”，每一句都会改变代理的做法。

## Leave out｜不要写的

- The how. Say what to achieve, and leave the agent room to find a better way.

> - 怎么做。说要达成什么，给代理留出找到更好办法的空间。

- A list of skills or steps. A hand-written order drops or reorders steps the playbook keeps. Name a skill only to override one choice.

> - 一串技能或步骤。手写的顺序会漏掉或打乱剧本本来会保留的步骤。只在要覆盖某一个选择时才点名技能。

- The user's theory of the cause, until the agent restates the problem. A stated guess narrows the search.

> - 用户对原因的猜测，等代理复述完问题再说。说出口的猜测会缩小搜索范围。

## Load the context first｜先加载上下文

- For a noisy report, ask the agent to restate the underlying issue in its own words and in plain English before it does anything else. A misreading shows up before any code exists.

> - 面对一份嘈杂的报告，在做任何其他事之前，先让代理用自己的话、用平实的语言复述背后的问题。误解会在任何代码出现之前暴露出来。

- In a fresh chat, `/recall` earlier work on the topic. Old chats hold context that the new agent lacks.

> - 在新对话里，用 `/recall` 找回这个话题之前的工作。旧对话里有新代理缺少的上下文。

- Before a change to unfamiliar code, ask `/how` for the mechanics and `/why` for the reasons. An agent with no traced model fixes the symptom at the first plausible spot.

> - 在改动不熟悉的代码之前，用 `/how` 问机制，用 `/why` 问原因。没有追踪出模型的代理，会在第一个看似合理的地方修症状。

- Ask `/teach` to make the case for a choice, as in "convince me it fixes the cause and not the symptom". A case is easier to check than a summary.

> - 让 `/teach` 为某个选择做出论证，比如“说服我它修的是原因而不是症状”。论证比总结更容易核查。

## Design before the plan｜先设计，后计划

- Never take the first design. Ask for prototypes of a few options, with screenshots or videos for UI, and pick from the evidence.

> - 永远不要采纳第一个设计。要几个选项的原型，UI 的话附上截图或视频，根据证据来选。

- Let prototypes answer the open questions. Don't review an abstract plan adversarially, because reviewers invent risks that never happen.

> - 让原型回答开放问题。不要对一个抽象的计划做对抗审查，因为审查者会编造永远不会发生的风险。

- For a shared package or API, ask for the README or a tutorial first, then work back to the code. The doc becomes the target the agent checks itself against.

> - 对于共享的包或 API，先要 README 或教程，再倒推到代码。文档成为代理自我核对的目标。

- Ask for the plan only after the design is settled. Each step of the plan ends in a check.

> - 只在设计定下来之后才要计划。计划的每一步都以一次检查结束。

## Follow up short｜跟进要简短

- "do it", "continue", and "keep going until done" are whole prompts once the chat holds the task.

> - 一旦对话里已经有了任务，“do it”“continue”和“keep going until done”本身就是完整的提示词。

- Start with "new task" when the subject changes. Otherwise the mode treats the message as the next step.

> - 换话题时以“new task”开头。否则模式会把这条消息当作下一步。

## Before stepping away｜离开之前

- Say "im going to bed" or "im stepping away" so the agent stops asking.

> - 说“im going to bed”或“im stepping away”，让代理不再提问。

- Write done as checks every iteration can run, and give `/loop` that predicate.

> - 把“完成”写成每一轮都能运行的检查，并把这个判定交给 `/loop`。

- Ask for a fresh worktree off a named base.

> - 要求基于一个指定的基础分支开一个新的 worktree。

- Pre-answer what the agent would stop for, such as "don't ask me before committing".

> - 提前回答代理会停下来问的事情，比如“提交前不用问我”。

- Ask for a decision log to audit later.

> - 要求记一份决策日志，留待之后审计。

- Give an exit: "if you're truly stuck after a few hours, stop and write up why".

> - 给一个出口：“如果几个小时后真的卡住了，就停下并写明原因”。

## Steer in one line｜一句话纠偏

- Restate the goal: "i said the goal is to repro. i did not ask for a fix yet."

> - 重申目标：“我说了目标是复现。我还没要你修。”

- Name the principle: "apply prove it works. show me the real output, not the build log."

> - 点名原则：“apply prove it works。给我看真实的输出，不是构建日志。”

- A principle name works because the agent already read the rule. Its reply names the decision the rule changed.

> - 原则名之所以管用，是因为代理已经读过那条规则。它的回复会说出这条规则改变了哪个决定。
