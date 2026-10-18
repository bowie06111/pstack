<!-- 中英对照译文。原文：skills/principle-sequence-verifiable-units/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-sequence-verifiable-units/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-sequence-verifiable-units/SKILL.md)

```yaml
name: principle-sequence-verifiable-units
description: "Apply to multi-step work (sweeps, migrations, runs of similar edits) and to how you stack commits and PRs. Break work into small units that each end in a verifiable state, check each before the next, and order delivery so the sequence proves itself to a reviewer."
disable-model-invocation: true
```

> **说明（description）：** 适用于多步骤工作（批量清理、迁移、一连串相似的修改）以及如何堆叠提交和 PR。把工作拆成小单元，每个都以一个可验证的状态结束，每个检查后再做下一个，并安排交付顺序，让这个序列向审查者自己证明自己。

# Sequence work into verifiable units｜把工作拆成可验证的单元

Order work as a sequence of small units, each ending in a state you can check, and don't advance until the current one is green.

> 把工作安排成一系列小单元，每个都以一个你能检查的状态结束，当前这个没变绿之前不往前走。

**Why:** A break caught at the unit that caused it is cheap to localize. A break caught after a batch is buried, and you have already built further on a broken base. Sequencing those same units into a delivery a reviewer can replay turns "trust me" into "watch it go red, then green."

> **原因：** 在引起问题的那个单元就抓到的破坏，很容易定位。在一整批之后才抓到的破坏被埋住了，而你已经在一个坏掉的基础上又往上建了不少。把同样这些单元排成一个审查者可以重放的交付，就把“相信我”变成了“看它先变红再变绿”。

**Execution.** In a sweep, migration, or any run of similar edits, verify each change before starting the next. Each unit is a before/after bracket: known-good state, one change, run the check, then proceed. Rebase onto clean trunk first so every check measures against the real baseline. When a lever does the edits, the per-unit check is nearly free. Run it anyway.

> **执行。** 在批量清理、迁移或任何一连串相似的修改中，每个改动验证后再开始下一个。每个单元都是一个前后对照：已知良好的状态、一个改动、运行检查，然后继续。先变基到干净的主干上，这样每次检查都是对照真实的基线。当修改由杠杆完成时，每个单元的检查几乎不花钱。照样要跑。

**Delivery.** Stack commits and PRs in the order that proves the work. The canonical shape is the failing test first, then the fix on top. Other story orders are a subtraction before the reshape, a baseline capture before the treatment, the scaffold before the feature. Each commit lands on its own and the sequence reads as an argument.

> **交付。** 按能证明工作的顺序堆叠提交和 PR。标准的形式是先有失败的测试，修复叠在上面。其他讲故事的顺序有：重塑之前先删减、处理之前先记录基线、功能之前先搭脚手架。每个提交都能单独落地，整个序列读起来就是一段论证。

The sequencing complement to the **prove-it-works** principle skill, which keeps each check real, and the **build-the-lever** principle skill, which makes the per-unit check cheap.

> 这是 **prove-it-works** 原则技能（让每次检查都真实）和 **build-the-lever** 原则技能（让每个单元的检查很便宜）在顺序安排上的补充。
