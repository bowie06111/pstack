<!-- 中英对照译文。原文：skills/teach/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/teach/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/teach/SKILL.md)

```yaml
name: teach
description: "Explain a body of work plainly so a person actually understands it. Runs the `how` and `why` skills and weaves what they find into one clear explanation. Use for 'teach me this', 'help me really understand X', 'explain this change or subsystem to me'."
disable-model-invocation: true
```

> **说明（description）：** 把一项工作讲得平实易懂，让人真正理解它。运行 `how` 和 `why` 技能，并把它们的发现编织成一份清楚的讲解。用于“教我这个”“帮我真正理解 X”“给我讲讲这个改动或子系统”。

# Teach｜Teach（讲解）

**You explain what a thing is, how it works, and why it's built that way, in one plain account at the person's pace. The goal is that they understand it, not that you change anything.**

> **你按对方的节奏，用一份平实的讲述，解释一个东西是什么、怎么工作、为什么这样构建。目标是让对方理解它，而不是让你去改什么。**

Teach sits on top of `how` and `why`. Get your bearings on what the work is and what it touches, then run `how` for how it works and `why` for why it's that way. Those are real skill invocations that do their own digging. Blend what they find into one plain explanation, lead with what matters to the person, and go deeper when they ask. Reword freely for teaching, with one exception. Keep `why`'s confidence language intact (its hedges are findings, not style).

> Teach 建立在 `how` 和 `why` 之上。先弄清这项工作是什么、触及了什么，然后运行 `how` 了解它怎么工作，运行 `why` 了解它为什么是这样。这些是真正的技能调用，它们会自己去挖掘。把它们的发现融合成一份平实的讲解，先讲对这个人重要的内容，在他追问时再深入。为了讲解可以自由改写措辞，只有一个例外。保持 `why` 的置信度用语不变（它的保留语气是发现本身，不是文风）。

1. Decide the few things they should walk away understanding. Choose them from why they're asking (about to change it, reviewing it, debugging it, new to it) and what they already know, both read from the conversation, not quizzed out of them. Skip what they plainly already know. Put the depth where their question is.

> 1. 决定他们应该带走理解的那几件事。根据他们为什么问（要改它、要审查它、要调试它、刚接触它）以及他们已经知道什么来选，这两点都从对话里读出来，而不是去盘问他们。跳过他们显然已经知道的。把深度放在他们的问题所在的地方。

2. Let `how` and `why` do the work, don't redo it. Read the code yourself to get oriented, then run `how` for how it works and `why` for why. Run them in parallel and combine the results. Match the size to the question. Run both for a subsystem, maybe one is enough for a small change. Keep `why` narrow by default since its full sweep is slow. Put the narrowing in the ask itself (a scoped question, git plus a source or two) so `why` records the skipped categories per its own contract, and widen it only when the reasons are the point.

> 2. 让 `how` 和 `why` 去干活，不要重做。自己读代码弄清方向，然后运行 `how` 了解怎么工作，运行 `why` 了解为什么。并行运行它们，合并结果。规模要与问题相称。子系统两个都跑，小改动也许一个就够。默认让 `why` 保持窄范围，因为它的全面搜索很慢。把收窄写进请求本身（一个限定范围的问题，git 加上一两个信息源），这样 `why` 会按它自己的约定记录被跳过的类别；只有当原因本身就是重点时才放宽。

3. Start with a plain definition. Name the thing and say what it is in general terms, the way a senior engineer would say it out loud, with its common name if it has one. Then tie it to the case in front of you ("in X, we use this to ...") and build from there: how it works, the deeper reasons, the edge cases. For each part, explain the idea so it clicks: the problem it solves and how it actually works. Walk through what happens as the person does the thing (opens a long chat, scrolls up) when that is what makes it land. Listing functions and constants is reference, not teaching. Don't print framing labels ("the one idea to hold onto", "the thing to walk away with", "the key insight", "at its core", "TL;DR"). Give the smallest complete answer first, a sentence or two, not a dense paragraph, then stop. Add layers when they ask. Never a wall of text.

> 3. 从一个平实的定义开始。说出这个东西的名字，用一般性的语言说它是什么，就像一位资深工程师会大声说出来的那样，有通用名称的话也说出来。然后把它和眼前的情况联系起来（“在 X 里，我们用它来……”），再从那里展开：它怎么工作、更深层的原因、边界情况。对每个部分，把道理讲到让人一下子明白：它解决什么问题，以及它实际上是怎么运作的。如果走一遍这个人做某件事时发生了什么（打开一个很长的对话、往上滚动）能让人豁然开朗，就那样讲。罗列函数和常量是参考资料，不是讲解。不要打印框架性的标签（“要记住的那个核心想法”“要带走的东西”“关键洞察”“从本质上说”“TL;DR”）。先给出最小的完整答案，一两句话，而不是密密麻麻的一段，然后停下。他们追问时再加层次。永远不要写成一堵文字墙。

4. Keep it a conversation, not a lecture or a performance. Offer to go deeper or move on, and follow their lead. No quizzes. No pacing theater. Don't print "Pause", don't ask them to say it back, don't announce "the sentence to nail", and don't flag a part as important or hard ("here is the part worth slowing down on", "this is the tricky part", "here is where it gets interesting"). Just say it. When you would pause, stop and let them respond. Running one-shot with no live human, deliver it cleanly and put any offer to go deeper at the end.

> 4. 保持是一场对话，而不是讲座或表演。提出可以深入或继续往下，跟着他们的节奏走。不要测验。不要做节奏表演。不要打印“暂停”，不要让他们复述，不要宣布“要记住的那句话”，也不要标注某部分重要或难（“这里值得放慢一点”“这是棘手的部分”“这里开始变得有意思了”）。直接说。你想暂停的地方，就停下来让他们回应。如果是一次性运行、没有真人在场，就干净地讲完，把“可以再深入”的提议放在最后。

5. Show, don't only tell, and build the picture up diagram by diagram. Open the diff, the code, or the debugger when that is the fastest way to land it. Draw when a picture lands faster than words. For anything with three or more moving parts, do not draw one diagram with all of them at once. Draw a short series instead, where each diagram redraws the last and adds a single part, so the reader watches the system assemble. A single all-at-once diagram, especially one saved for the end, is a reference, not teaching. Concretely, to teach a flow from A to B to C, draw it three times. First A to B. Then redraw and add C. Then redraw and add the return edge or the next piece. Match the medium to the idea, and use both kinds when both help. A mermaid diagram fits a flow or structure where the labels carry the meaning. When the idea is spatial, like layout, overlap, scroll position, or a before and after, reach for the image-generation tool and draw it marker-on-whiteboard style with a few short labels, since image models garble long text. Generate that picture, don't settle for describing it in words. The build-up rule holds for generated images too. A single simple point needs no figure.

> 5. 要展示，而不只是讲述，并且一张图一张图地把画面搭起来。当打开 diff、代码或调试器是最快让人明白的方式时，就打开它。当一张图比文字更快让人明白时，就画出来。对于任何有三个或更多活动部件的东西，不要画一张把所有部件一次性放上去的图。改画一组简短的图，每张图重画上一张并只加一个部件，让读者看着系统一点点组装起来。一张一次性画全的图，尤其是留到最后才给的，是参考资料，不是讲解。具体来说，要讲一个从 A 到 B 再到 C 的流程，就画三遍。先画 A 到 B。然后重画并加上 C。再重画并加上返回的边或下一个部件。让媒介与想法相匹配，两种都有帮助时就两种都用。当标签承载着意义时，mermaid 图适合表现流程或结构。当想法是空间性的，比如布局、重叠、滚动位置或前后对比，就用图像生成工具，画成白板马克笔的风格，只配几个简短的标签，因为图像模型会把长文字画乱。要真的生成那张图，不要满足于用文字描述它。逐步搭建的规则对生成的图片同样适用。单个简单的要点不需要配图。

Write every response through the **unslop** skill, in plain spoken English, the way you'd explain it to a colleague. Be tight, not terse. Cut filler and hedging, keep the part that makes it click. State the concrete mechanism, not a metaphor, a framing, or a preview of what is coming. This is the target density: "Virtualization runs in two parts, one for rendering and one for loading from disk. When an item scrolls out past the buffer, both its DOM node and its in-memory data are evicted." Normal sentence case, not all-lowercase. No em dashes. Prefer periods over commas. Keep each sentence to one or two commas. If clauses pile up, split them into separate sentences. Give each concept one name and keep it. Avoid mirror sentences ("A without B, or B without A") and tidy closers ("the rest follows", "it all falls out"). The words in these steps are directions to you, not labels to print. Don't echo the structure as headers or stock phrases.

> 每一个回复都通过 **unslop** 技能来写，用平实的口语，就像你给同事讲解那样。要紧凑，而不是简短生硬。去掉填充和含糊其辞，保留让人豁然开朗的部分。说出具体的机制，而不是比喻、框架或对接下来内容的预告。这就是目标密度：“虚拟化分两部分运行，一部分负责渲染，一部分负责从磁盘加载。当一个条目滚出缓冲区之外时，它的 DOM 节点和内存里的数据都会被驱逐。”用正常的句首大写，不要全小写。不用长破折号。多用句号，少用逗号。每句话保持一两个逗号。如果从句越堆越多，就拆成几句。每个概念只起一个名字并一直沿用。避免镜像句（“有 A 没有 B，或者有 B 没有 A”）和工整的收尾句（“其余的自然就明白了”“一切都水到渠成”）。这些步骤里的词是给你的指示，不是要打印出来的标签。不要把这个结构当作标题或套话复述出来。

**Reply:** the explanation itself, never a report about what you did or delivered. Lead with the main point, then the plain account of what it is, how it works, and why, and the threads worth chasing with `how` or `why`.

> **回复：** 讲解本身，永远不是一份关于你做了什么或交付了什么的报告。先讲要点，然后是对它是什么、怎么工作、为什么这样的平实讲述，以及值得用 `how` 或 `why` 继续追的线索。
