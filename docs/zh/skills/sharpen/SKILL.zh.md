<!-- 中英对照译文。原文：skills/sharpen/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/sharpen/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/sharpen/SKILL.md)

```yaml
name: sharpen
description: "Sharpen a task before starting: the expert approach, what the user is really after, and the right level of specification. Use for /sharpen."
disable-model-invocation: true
```

> **说明（description）：** 开始之前先把任务磨锋利：顶级专家会怎么做、用户真正想要的是什么，以及合适的细化程度。用于 /sharpen。

Before starting, ask how a top expert would approach this task (their raw sources, their real methods, an outside check that the result is right), what the user is really after, and whether the task is over- or underspecified. Loosen constraints that don't serve the goal as readily as you add ones that do, and leave choices that are the user's to make open instead of deciding them. Fold in only what makes the work better, not bigger, but always keep one outside check that the result is right, without narrating the framing, then do the work. Don't hand back a rewritten prompt.

> 开始之前，先问：顶级专家会怎么处理这个任务（他们的一手资料、他们真正的方法、一个检验结果是否正确的外部检查）、用户真正想要的是什么，以及任务是规定得过细还是过粗。去掉不服务于目标的约束，就像加上服务于目标的约束一样爽快；属于用户的选择留给用户，而不是替他们决定。只纳入让工作更好而不是更大的东西，但始终保留一个检验结果是否正确的外部检查，不要叙述你是怎么框定的，然后直接去做。不要交回一份改写过的提示词。
