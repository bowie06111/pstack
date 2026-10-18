<!-- 中英对照译文。原文：skills/principle-build-the-lever/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-build-the-lever/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-build-the-lever/SKILL.md)

```yaml
name: principle-build-the-lever
description: "Apply to any non-trivial work, not just bulk work: edits, migrations, analyses, checks. Build the tool that does it or proves it (codemod, script, generator, or a skill your subagents follow) instead of working by hand. The tool is the artifact a reviewer can rerun."
disable-model-invocation: true
```

> **说明（description）：** 适用于任何非平凡的工作，而不只是批量工作：修改、迁移、分析、检查。打造完成它或证明它的工具（codemod、脚本、生成器，或供子代理遵循的技能），而不是手工去做。工具就是审查者可以重跑的产物。

# Build the Lever｜打造杠杆

When the work isn't trivial, build the tool that does it instead of doing it by hand.

> 当工作不是微不足道时，打造一个完成它的工具，而不是手工去做。

A codemod, generator, or script does the work the same way every time and reruns for free. The tool is one artifact a reviewer can read and rerun to check the work. Hand-done changes can only be re-verified by redoing them. A deterministic script turns "trust me" into "run this".

> codemod、生成器或脚本每次都以同样的方式完成工作，重跑不花钱。工具是审查者可以阅读并重跑来核对工作的一个产物。手工完成的改动只能通过重做来重新验证。一个确定性的脚本把“相信我”变成“跑一下这个”。

Default to building the lever. Skip it only when the task is trivial, a couple of obvious edits you can see at a glance.

> 默认打造杠杆。只有当任务微不足道，也就是一眼就能看清的几处明显修改时，才跳过它。

- Do the first unit by hand to learn the recipe, then build the tool. Prove it by rerunning it on that unit and diffing against your hand-done version. Make the lever safe to rerun.

> - 先手工做第一个单元来摸清做法，然后打造工具。在那个单元上重跑它，并与你手工做的版本做 diff，以此证明它可靠。让杠杆可以安全地重跑。

- Codemod or script for edits, generator for repetitive files, a dump-to-sqlite query for analysis, a rerunnable check for verification.

> - 修改用 codemod 或脚本，重复性的文件用生成器，分析用导入 sqlite 后查询，验证用可重跑的检查。

- A deterministic lever beats fan-out. If the tool can process every unit in one pass, run it yourself. Don't fan out delegates to hand-apply what a script can do.

> - 确定性的杠杆胜过扇出。如果工具一次就能处理所有单元，就自己运行它。不要扇出委派者去手工完成脚本能做的事。

- When you fan work out to subagents, write the lever as a skill they all read: the recipe, the verification contract, and the do-not-touch fences in one artifact. Keep it outside the delegates' write scope so they can't quietly edit the contract.

> - 当你把工作扇出给子代理时，把杠杆写成一个他们都要读的技能：做法、验证约定和禁止触碰的边界放在同一个产物里。把它放在委派者的写入范围之外，这样他们没法悄悄改动约定。

- Applying this principle produces a file. If you cited it and there is no codemod, script, generator, or delegate skill in the diff, you didn't apply it.

> - 应用这条原则会产出一个文件。如果你引用了它，而 diff 里却没有 codemod、脚本、生成器或委派技能，那你就没有应用它。

- Commit the lever when the work outlives the session.

> - 当工作比会话活得更久时，提交这个杠杆。

The bar is triviality, not repetition. A one-off still earns a lever when the lever is what makes the work checkable. Per the [Laziness Protocol](../principle-laziness-protocol/SKILL.zh.md), build the smallest script that does or proves the job, never a framework.

> 标准是“是否微不足道”，而不是“是否重复”。一次性的工作，如果杠杆正是让工作可以被核对的东西，也值得打造杠杆。按 [懒惰协议](../principle-laziness-protocol/SKILL.zh.md)，打造能完成或证明这项工作的最小脚本，永远不要做成框架。

Distinct from [Encode Lessons in Structure](../principle-encode-lessons-in-structure/SKILL.zh.md), which makes a recurring instruction a durable guardrail. This is throughput and reviewability on the work in front of you. For scripting the verification itself, see [Prove It Works](../principle-prove-it-works/SKILL.zh.md).

> 不同于 [把教训编码进结构](../principle-encode-lessons-in-structure/SKILL.zh.md)，后者把反复出现的指示变成持久的护栏。这条原则关注的是眼前这项工作的吞吐量和可审查性。关于把验证本身写成脚本，见 [证明它能用](../principle-prove-it-works/SKILL.zh.md)。
