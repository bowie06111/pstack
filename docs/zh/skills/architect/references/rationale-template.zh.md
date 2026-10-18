<!-- 中英对照译文。原文：skills/architect/references/rationale-template.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/architect/references/rationale-template.md](https://github.com/bowie06111/pstack/blob/main/skills/architect/references/rationale-template.md)

# Rationale template｜设计理由模板

The prose that ships alongside the type sketch. One page. Sentence-case headings, no boilerplate. Replace the italic notes with actual content.

> 和类型草图一起交付的文字说明。一页。标题用句首大写，不要套话。把斜体的说明替换成实际内容。

## Problem｜问题

*One paragraph. What we're trying to do, and what about the existing system or constraints makes the shape non-obvious. If [Phase A](../SKILL.zh.md#phase-a-ground-the-problem) surfaced constraints the design must honor (existing types to interop with, callers we can't break, invariants that crossed our boundary), name them here so the reader sees the same constraints you saw.*

> *一段话。我们要做什么，以及现有系统或约束的哪些方面让这个形状并不显而易见。如果 [阶段 A](../SKILL.zh.md#phase-a-ground-the-problem) 发现了设计必须遵守的约束（要互操作的现有类型、不能弄坏的调用方、跨越了我们边界的不变量），在这里写出来，让读者看到和你一样的约束。*

## Usage (caller's view)｜用法（调用方视角）

*Write this first, before the type sketch. Show the README or quickstart the consumer reads, plus two or three realistic call sites in their own code. What they import, what they call, what comes back. The type sketch in [Shape](#shape) is derived from this. The two must agree. When they diverge, reconcile the sketch to the usage, not the reverse. The caller's experience is the spec. The types serve it.*

> *先写这一节，在类型草图之前。展示使用者会读的 README 或快速入门，再加上他们自己代码里两三个真实的调用点。他们导入什么、调用什么、得到什么。[形状](#shape) 里的类型草图由此推导出来。两者必须一致。出现分歧时，让草图向用法看齐，而不是反过来。调用方的体验就是规格。类型为它服务。*

## Shape｜形状

*The recommended architecture. Data structures first. Then how data flows through the signatures. Name the load-bearing decisions. State which invariants are encoded in types, where validation lives, and what the system deliberately does not do. Judge interface depth explicitly. State what complexity the public surface hides, what remains exposed to callers, and why the interface is no larger than needed. Cite the principle behind each decision (e.g., `per boundary-discipline`). Don't restate it.*

> *推荐的架构。先写数据结构。然后写数据如何流经各个函数签名。说出起关键作用的决定。说明哪些不变量编码在类型里、校验放在哪里，以及系统刻意不做什么。明确评判接口深度。说明公开接口隐藏了什么复杂度、还有什么暴露给调用方，以及为什么接口没有比需要的更大。为每个决定注明背后的原则（例如 `per boundary-discipline`）。不要复述原则。*

## Synthesis decision｜综合决定

*Filled in by [arena](../../arena/SKILL.zh.md). Records which candidate became the base and why, what was adapted from each of the others, and what was rejected and why.*

> *由 [arena](../../arena/SKILL.zh.md) 填写。记录哪个候选方案成了底稿以及原因、从其他每个方案里采纳了什么，以及否决了什么和原因。*

## Tradeoffs accepted｜接受的取舍

*One bullet per tradeoff the chosen shape makes. Form: "we accept X in exchange for Y." Name anything a future reader might mistake for an oversight, including things that look like premature optimization or premature simplification.*

> *所选形状做出的每个取舍一个要点。格式：“我们接受 X，以换取 Y。”说出任何将来的读者可能误以为是疏忽的东西，包括看起来像过早优化或过早简化的地方。*

## Alternatives considered｜考虑过的备选方案

*Required. Name at least one concrete alternative shape, with one line on why it lost. Judge each alternative on interface depth, not implementation simplicity alone. Name the complexity it exposes to callers and the complexity it hides. Two or three alternatives belong here when the design space had real contenders. One is fine when the constraints forced the answer, with the conclusion phrased as "this was the only viable shape because..." Avoid listing flavors of the same shape. This section covers design alternatives the chosen shape considered and rejected, not other runner candidates.*

> *必填。至少说出一个具体的备选形状，用一行说明它为什么落选。评判每个备选方案要看接口深度，而不只是实现是否简单。说出它暴露给调用方的复杂度和它隐藏的复杂度。当设计空间里有真正的竞争者时，这里应该放两三个备选方案。当约束逼出了唯一答案时，一个也行，结论写成“这是唯一可行的形状，因为……”。不要列出同一种形状的不同变体。这一节讲的是所选形状考虑过并否决的设计方案，而不是其他执行者的候选方案。*

## Open questions and risks｜待解问题和风险

*Things you noticed during the sketch that the human needs to weigh in on, and risks worth flagging before implementation starts. Phrase as questions, not assertions, so the human's answer is the resolution rather than a comment.*

> *画草图时你注意到的、需要人来权衡的事情，以及在开始实现之前值得提出的风险。写成问题而不是断言，这样人的回答就是结论，而不是一条评论。*

## Next implementation step｜下一步实现

*The first thing to build against the sketch. One sentence. What you'd start writing immediately after synthesis (or after Phase D sign-off, if a checkpoint was opted into).*

> *按草图要构建的第一件事。一句话。综合之后（如果选择了检查点，就是阶段 D 签字之后）你会立刻开始写的东西。*
