<!-- 中英对照译文。原文：skills/principle-attack-the-premise/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-attack-the-premise/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-attack-the-premise/SKILL.md)

```yaml
name: principle-attack-the-premise
description: "Apply when two or more fixes that share one premise have failed the same gate. Write the premise down and test it directly before you write another fix that assumes it."
disable-model-invocation: true
```

> **说明（description）：** 在两次或更多基于同一前提的修复在同一个关卡失败时使用。先把前提写下来并直接验证，再写下一个假设该前提成立的修复。

# Attack the Premise｜攻击前提

When two or more fixes that share one premise have failed the same gate, suspect the premise, not the fixes.

> 当两次或更多基于同一前提的修复在同一个关卡失败时，怀疑前提，而不是怀疑修复。

**Why:** Each failed fix is evidence against the belief it assumed. A third fix on the same premise spends effort where the evidence already says the cause is not.

> **原因：** 每一次失败的修复都是反对它所假设的那个信念的证据。在同一个前提上做第三次修复，是把力气花在证据已经表明原因不在的地方。

**Pattern:**

> **做法：**

- **Write the premise down.** The premise is the one sentence that every failed fix assumed.

> - **把前提写下来。** 前提就是每一次失败的修复都假设成立的那一句话。

- **Test the premise before the next fix.** Find an observation that would come out differently if the premise were false, and make it. Look at how the failures are distributed, not only at how large they are. Make the check rerunnable per [Build the Lever](../principle-build-the-lever/SKILL.zh.md).

> - **在下一次修复之前验证前提。** 找一个如果前提为假就会得出不同结果的观察，然后去做它。看失败是怎么分布的，而不只是看失败有多大。按 [打造杠杆](../principle-build-the-lever/SKILL.zh.md) 让这个检查可以重跑。

- **Follow the evidence.** If it contradicts the premise, the cause is elsewhere. Look for what the failures have in common that the premise did not predict. If it confirms the premise, ask the next "why" per [Fix Root Causes](../principle-fix-root-causes/SKILL.zh.md). The earlier fixes may have treated a consequence instead of the cause.

> - **跟着证据走。** 如果证据与前提矛盾，原因就在别处。去找那些失败有哪些共同点是前提没有预料到的。如果证据证实了前提，就按 [修根因](../principle-fix-root-causes/SKILL.zh.md) 再问下一个“为什么”。之前的修复可能处理的是一个后果，而不是原因。

- **Remove the condition instead of compensating for it**, per the [Laziness Protocol](../principle-laziness-protocol/SKILL.zh.md). If the evidence shows that one part always lands on the losing side, change what puts it there. A mechanism that cleans up after it leaves the cause in place and adds work on every run.

> - **消除条件，而不是去补偿它**，按 [懒惰协议](../principle-laziness-protocol/SKILL.zh.md)。如果证据显示某个部分总是落在吃亏的一边，就改变把它放到那里的东西。事后收拾残局的机制会把原因留在原地，而且每次运行都增加工作量。

**Stop:** Do not start the next fix before the premise is written down and tested.

> **停：** 在前提被写下并验证之前，不要开始下一次修复。

This principle is distinct from [Redesign from First Principles](../principle-redesign-from-first-principles/SKILL.zh.md), which rebuilds a design around a new requirement. It questions a fact the current design assumes.

> 这条原则不同于 [从第一性原理重新设计](../principle-redesign-from-first-principles/SKILL.zh.md)，后者围绕一个新需求重建设计。这条原则质疑的是当前设计所假设的一个事实。
