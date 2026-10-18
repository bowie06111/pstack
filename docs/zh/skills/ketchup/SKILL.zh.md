<!-- 中英对照译文。原文：skills/ketchup/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/ketchup/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/ketchup/SKILL.md)

```yaml
name: ketchup
description: "Catch the user up on what changed in this chat since their last message, in a few plain sentences and small visuals. Give each action item just enough context to make the call. Use for /ketchup."
disable-model-invocation: true
```

> **说明（description）：** 用几句平实的话和小图，让用户跟上这个对话自他上条消息以来发生的变化。给每个待办事项刚好够做决定的上下文。用于 /ketchup。

# Ketchup｜Ketchup（跟上进度）

Catch the user up on what changed in this chat since the last message they wrote before `/ketchup`, and help them understand each change, not only where it stands. Notifications and subagent results are not their messages. Assume the user remembers their last message and nothing after it.

> 让用户跟上这个对话里自他在 `/ketchup` 之前写的最后一条消息以来发生的变化，并帮助他理解每个变化，而不只是知道它处于什么状态。通知和子代理的结果不算他的消息。假设用户记得自己的最后一条消息，而之后的事情一概不记得。

Use [`/bro`](../bro/SKILL.zh.md)'s voice, not its task: plain words, short sentences, no jargon, like one person catching up another. Say what changed for the user, not how you did it. Say "the tests pass", not "CI is green". Leave out hashes, file paths, commands, build numbers, and test counts unless the user needs one to act. Call each PR, branch, or agent by a name the user would know, not by a label you invented. If a fact needs context to make sense, give the context or leave the fact out.

> 用 [`/bro`](../bro/SKILL.zh.md) 的语气，但不是它的任务：平实的词语、简短的句子、不用术语，就像一个人在给另一个人补课。说对用户来说什么变了，而不是你是怎么做的。说“测试通过了”，而不是“CI 是绿的”。除非用户需要用它来行动，否则不要写哈希、文件路径、命令、构建编号和测试数量。用用户熟悉的名字来称呼每个 PR、分支或代理，而不是你自己起的标签。如果某个事实需要上下文才能看懂，就给出上下文，或者干脆不写这个事实。

## Gather what changed｜收集发生了什么变化

Keep this turn read-only. Make visuals only from what you already have: do not change code, start an app or a job, or post anything. Collect your own work since that message, reports from subagents, and messages from anyone else in this chat. Check the current state of each PR, CI run, or agent this chat touched. Check each number you will show against the facts it comes from, and say so when you correct one.

> 这一回合保持只读。只用你已有的东西做图：不改代码，不启动应用或任务，不发布任何东西。收集自那条消息以来你自己的工作、子代理的报告，以及这个对话里其他人发的消息。检查这个对话涉及的每个 PR、CI 运行或代理的当前状态。把你要展示的每个数字和它所依据的事实核对一遍，纠正了某个数字时要说明。

An action item is a decision or step only the user can take, such as a product choice, an approval, an irreversible action, or access you lack. Include any still waiting on the user, even one raised before their last message. Work you can still do yourself is not an action item. Never make up a call to fill the reply. A step the user did not ask for and the work does not need is not an action item, unless it is irreversible and waiting on the user: give it one action line, no visual, and skip what happens if the user does not answer.

> 待办事项是只有用户才能做的决定或步骤，比如产品选择、审批、不可逆的操作，或者你没有的访问权限。包括所有仍在等用户的事项，即使是在他最后一条消息之前提出的。你自己还能做的工作不算待办事项。永远不要为了填满回复而编造一个决定。用户没要求、工作也不需要的步骤不算待办事项，除非它是不可逆的、并且在等用户：给它一行待办，不配图，也不写用户不回应会怎样。

## Write the reply｜撰写回复

Keep the whole reply well under 150 words, not counting visuals. When something is easier to grasp as a picture than as sentences, draw it: how a design or system works, a PR stack or dependency chain with each PR's state, a before and after of a fix, or options side by side. Never draw a status list or boxes with nothing connecting them. Never give a fact twice, in text or across visuals. Leave out work that changed nothing for the user. When words run short, cut from what happened before you cut from the action items.

> 整个回复远少于 150 个词，不算图。当某样东西用图比用句子更容易理解时，就画出来：一个设计或系统是怎么工作的、一个 PR 栈或依赖链以及每个 PR 的状态、一个修复的前后对比，或者并排的选项。永远不要画状态列表，也不要画彼此不相连的方框。任何事实都不要说两遍，无论是在文字里还是在多张图之间。不写对用户没有带来任何变化的工作。字数不够时，先删“发生了什么”，再删待办事项。

Open with one sentence that says how many things need the user, or that nothing does. If nothing happened and nothing waits on the user, say so and stop.

> 开头用一句话说明有几件事需要用户处理，或者没有需要处理的。如果什么都没发生、也没有等用户的事，就这么说，然后停下。

Then show what happened, pictures first. Embed inline the screenshots and videos the work produced, then any diagram that explains a change. Then sum up in one short bullet per thing the user asked for: what came of it, and whether it is done, still running, failed, or not checked. Write each line the way you would say it out loud: "Search was slow from a missing index", not "Search review: done" or "Slow search: missing index". Name any bug found and fixed on the way. Each line must stay true when read alone. Never mark something done that still waits on the user, and leave out anything the action items say. Under the summary, give at most three short lines that change what the user knows or must do. Then name in one line any work left that you can do yourself.

> 然后展示发生了什么，图片优先。内嵌工作产出的截图和视频，然后是解释某个变化的图表。接着，对用户要求的每件事用一个简短的要点总结：结果是什么，是已完成、仍在运行、失败了还是没检查。每一行都写成你会说出口的样子：“搜索慢是因为缺了一个索引”，而不是“搜索审查：完成”或“搜索慢：缺索引”。点出途中发现并修好的任何 bug。每一行单独读也必须是真的。永远不要把还在等用户的事标成已完成，也不要写待办事项里已经说了的内容。总结下面，最多写三行改变用户所知或必须做的事的短句。然后用一行说出你自己还能做的剩余工作。

Then list the action items as plain bullets, never as a diagram or box. Give each two lines, or one for an unasked irreversible step: the decision with your recommendation and why, then what happens if the user does not answer. Ask the call the user owns, such as merge, approve, or pick. A step you would take first goes in your recommendation. So does anything that must happen first, and anything that cannot be undone. Write "unknown" for a part you do not know. Link to the detail.

> 然后用普通的要点列出待办事项，永远不要用图或方框。每项两行，没被要求的不可逆步骤一行：第一行是要做的决定以及你的建议和理由，第二行是用户不回应会怎样。问的是用户拥有的那个决定，比如合并、批准或选择。你会先采取的步骤写进你的建议里。必须先发生的事、无法撤销的事也一样。不知道的部分写“未知”。链接到详情。

Show a choice after the list. If the screenshots above already show its options, write each option's gain and cost under its screenshot instead of drawing a new visual; otherwise, compare the options side by side, with the current state when there is one. Give every option the work weighed, each with what it gets the user and what it costs. Keep them out of the rest of the text.

> 在列表之后展示选择。如果上面的截图已经展示了它的选项，就在每张截图下写出该选项的得失，而不是再画一张新图；否则，把选项并排比较，有当前状态时也放上去。给出工作所权衡过的每个选项，各写它能给用户带来什么、代价是什么。不要把它们放到其余的文字里。

## Visuals｜图

A visual is a picture: a screenshot, a video, a Mermaid diagram, a chart or diagram from `/visualize` (Cursor) or your harness's charting tool, or a generated image. A Markdown table is not a visual, even with before and after columns.

> 图是一张图片：截图、视频、Mermaid 图、用 `/visualize`（Cursor）或你运行环境的图表工具画的图表或示意图，或者一张生成的图片。Markdown 表格不算图，即使它有前后对比的列。

Keep each visual small enough to read at a glance: at most six boxes, with at most eight words in each. Write labels as plain words a person would say, not tags like "Tests: pass". A fact that needs a sentence belongs in the text, or nowhere.

> 每张图都要小到一眼就能读完：最多六个方框，每个方框最多八个词。标签写成人会说的平实词语，而不是“Tests: pass”这样的标记。需要一句话才能说清的事实，要么放进文字里，要么就不写。

Embed screenshots and videos inline when the change is visible, not only as links. If a visible change has none, mark its summary line "No screenshot yet," and say there what changed on screen. Draw diagrams and charts from facts you have: a Mermaid diagram for how things connect and a chart for numbers (`/visualize` in Cursor, or your harness's charting tool). Never invent a number, and never draw or generate a picture of a screen you did not capture. Link only artifacts that exist.

> 当变化是可见的时，把截图和视频内嵌进来，而不只是给链接。如果一个可见的变化没有截图，就在它的总结行标上“暂无截图”，并在那里说明屏幕上变了什么。根据你掌握的事实画示意图和图表：用 Mermaid 图表示东西之间如何连接，用图表表示数字（Cursor 里用 `/visualize`，或者你运行环境的图表工具）。永远不要编造数字，也永远不要画或生成一张你没有截取过的屏幕的图片。只链接确实存在的产物。
