<!-- 中英对照译文。原文：skills/poteto-help/references/recipes.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-help/references/recipes.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-help/references/recipes.md)

# Prompts worth copying｜值得照抄的提示词

Swap in the real paths, skills, and done checks. Informal wording works.

> 换上真实的路径、技能和完成检查。随意的措辞也管用。

## Understand｜理解

- `/poteto-mode read <thread>. restate the underlying issue in your own words, in plain english.`

> - `/poteto-mode 读一下 <讨论串>。用你自己的话、用平实的语言复述背后的问题。`

- `/in-your-own-words <long or messy ask>`

> - `/in-your-own-words <又长又乱的需求>`

- `/sharpen <your task>`

> - `/sharpen <你的任务>`

- `/poteto-mode investigate why <symptom>. give me what we know, what data you used, and your best hypotheses. don't change any code yet.`

> - `/poteto-mode 调查为什么 <症状>。告诉我已知什么、你用了哪些数据，以及你最好的假设。先别改代码。`

- `use /how to understand <subsystem>. then use /why to find out why it broke recently.`

> - `先用 /how 弄懂 <子系统>。再用 /why 查它最近为什么坏了。`

- `/recall my work on <topic> from last week, then read <issue>.`

> - `/recall 我上周在 <话题> 上的工作，然后读 <问题>。`

- `/teach me why you implemented it this way and not <other way>. what did you trade off?`

> - `/teach 我你为什么这样实现，而不是 <另一种做法>。你做了什么取舍？`

- `/poteto-mode take over this branch. read the decision log, find what's done, and continue. don't redo finished work.`

> - `/poteto-mode 接手这个分支。读决策日志，找出哪些已经完成，然后继续。不要重做已完成的工作。`

## Build｜构建

- Bug: `/poteto-mode <symptom>. repro first, then fix and verify.`

> - Bug：`/poteto-mode <症状>。先复现，再修复并验证。`

- Bug in an app: `/poteto-mode repro this with /verify-<app>. if it repros on main, fix it and show me a video as proof.`

> - 应用里的 bug：`/poteto-mode 用 /verify-<app> 复现这个问题。如果在 main 上能复现，就修好它，并给我看一段视频作为证明。`

- Bug with a cheap test: `/poteto-mode repro <bug> first. if there's a cheap test path, /tdd it. then fix and rerun.`

> - 有便宜测试的 bug：`/poteto-mode 先复现 <bug>。如果有便宜的测试路径，就用 /tdd。然后修复并重跑。`

- Feature: `/poteto-mode add <behavior>. <current output> stays byte-identical. verify both.`

> - 功能：`/poteto-mode 加上 <行为>。<当前输出> 保持逐字节不变。两种都要验证。`

- Refactor: `/poteto-mode move <code> into one module, zero behavior change. record the current output first and prove it's unchanged after.`

> - 重构：`/poteto-mode 把 <代码> 挪进一个模块，行为零变化。先记录当前输出，改完证明没变。`

- Perf: `/poteto-mode <operation> takes <time> on <fixture>. trace it, fix the measured cause, show me before and after.`

> - 性能：`/poteto-mode <操作> 在 <测试数据> 上要 <时间>。跑 trace，修掉测出来的原因，给我看前后对比。`

## Design and plan｜设计和计划

- `/poteto-mode prototype a few options for <feature>. take screenshots or videos for me to compare.`

> - `/poteto-mode 给 <功能> 做几个选项的原型。截图或录屏给我比较。`

- `/poteto-mode we need <feature>. /architect it first, and answer open questions with prototypes. let me review before proceeding.`

> - `/poteto-mode 我们需要 <功能>。先用 /architect 设计，用原型回答开放问题。让我审查后再继续。`

- `/poteto-mode write a tutorial for how i would use <new package> first. then /teach me why it beats the current one.`

> - `/poteto-mode 先写一份我会如何使用 <新包> 的教程。然后用 /teach 告诉我它为什么比现在的好。`

- `ask /arena for a second opinion on this thread and our approach.`

> - `让 /arena 对这个讨论和我们的做法给个第二意见。`

- `/poteto-mode turn this design into a plan. small verifiable PRs, each with its own verification steps.`

> - `/poteto-mode 把这个设计变成一份计划。小而可验证的 PR，每个都有自己的验证步骤。`

- `/poteto-mode plan the migration of <library> to <target>. small verifiable PRs. the result must match the original exactly, bugs included.`

> - `/poteto-mode 规划把 <库> 迁移到 <目标>。小而可验证的 PR。结果必须和原来完全一致，包括 bug。`

## Review and ship｜审查和发布

- `/interrogate the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's a real bug or regression.` Read the dismissals too.

> - `/interrogate 整个分支，但要持怀疑态度。先什么都别改。除非是真正的 bug 或回归，否则不要挑小毛病。` 被驳回的内容也要读。

- `/swarm check every package under <dir> against its check script. one worker per package. one report.`

> - `/swarm 用各自的检查脚本检查 <目录> 下的每个包。每个包一个工人。一份报告。`

- `/poteto-mode open the pr. small ordered commits, evidence in the description.`

> - `/poteto-mode 开 PR。小而有序的提交，证据写在描述里。`

- `/poteto-mode babysit this pr. get it green.` For status only: `/poteto-mode check on pr <number>. anything outstanding?`

> - `/poteto-mode 看护这个 PR。把它弄到全绿。` 只想看状态：`/poteto-mode 看一下 PR <编号>。还有什么没处理的吗？`

- `/poteto-mode land the stack.`

> - `/poteto-mode 把这个栈合并掉。`

## Away and back｜离开和回来

- `/poteto-mode im going to bed. <goal> in a fresh worktree off <base>. done means <checks>. keep a decision log. don't ask me before committing. /loop until done. if you're truly stuck after a few hours, stop and write up why.`

> - `/poteto-mode 我去睡了。在基于 <基础分支> 的新 worktree 里完成 <目标>。完成意味着 <检查>。记决策日志。提交前不用问我。/loop 直到完成。如果几个小时后真的卡住了，停下并写明原因。`

- `/show-me-your-work catch me up on what you did last night.` Read its Attention section first.

> - `/show-me-your-work 告诉我你昨晚做了什么。` 先读它的 Attention（需要注意）一节。

- `/poteto-mode full autopilot on this queue. each item is independent.`

> - `/poteto-mode 对这个队列开完全自动驾驶。每一项都是独立的。`

- `/poteto-mode autopilot these changes but stack them, don't ship. i'll land the stack.`

> - `/poteto-mode 自动驾驶这些改动，但堆成栈，别发布。我来合并这个栈。`

- `/reflect capture what we learned so the next run doesn't repeat it.` Approve only edits that change a future decision.

> - `/reflect 把我们学到的记下来，让下次运行不再重复。` 只批准会改变将来某个决定的修改。

- `/bro` restates the last reply in plain words.

> - `/bro` 用大白话重述上一条回复。
