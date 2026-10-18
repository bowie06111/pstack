<!-- 中英对照译文。原文：skills/architect/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/architect/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/architect/SKILL.md)

```yaml
name: architect
description: "Sketch types, signatures, and module structure before code, then stay in the loop while implementation fills in. Use for /architect, 'architect this', 'design this', or non-trivial work where jumping to code would lock in the wrong shape."
disable-model-invocation: true
```

> **说明（description）：** 在写代码之前先勾画类型、签名和模块结构，然后在实现逐步填充时保持参与。用于 /architect、“architect this”、“design this”，或者直接写代码会锁定错误形状的非平凡工作。

# Architect｜Architect（架构师）

Sketch types, function signatures, class shapes, and module boundaries with `not implemented` bodies and pseudocode. Synthesize across multiple model perspectives, then fill in code against the chosen sketch.

> 用 `not implemented` 的函数体和伪代码，勾画类型、函数签名、类的形状和模块边界。综合多个模型的视角，然后按选定的草图填充代码。

## Start｜开始

Open a todolist with one entry per phase before starting.

> 开始之前，打开一个待办清单，每个阶段一项。

1. Ground

> 1. 打基础

2. Sketch

> 2. 画草图

3. Agree

> 3. 达成一致

4. Implement

> 4. 实现

5. Scrap

> 5. 推倒重来

## Phase A: Ground the problem｜阶段 A：为问题打基础

Build a real mental model of every system the new code touches. Run the **how** skill over the relevant subsystems. Naming a file isn't grounding. Produce the architectural explanation `how` prescribes. If the design redefines ownership or layering, also run the **why** skill on the existing shape so the rationale becomes a constraint, not a guess.

> 为新代码触及的每个系统建立真实的心智模型。对相关的子系统运行 **how** 技能。说出一个文件名不算打基础。产出 `how` 所规定的架构讲解。如果设计重新定义了归属或分层，还要对现有形状运行 **why** 技能，让理由成为约束，而不是猜测。

Skip Phase A only when the work is genuinely greenfield with no surrounding system to integrate.

> 只有当工作确实是全新的、没有周边系统需要集成时，才跳过阶段 A。

## Phase B: Sketch｜阶段 B：画草图

Run the **arena** skill with the design-sketch task and the Phase A grounding artifacts. Pass `references/runner-prompt.md` as each runner's prompt.

> 带着设计草图任务和阶段 A 的基础材料运行 **arena** 技能。把 `references/runner-prompt.md` 作为每个执行者的提示词。

Take the runners from the `architect runners` line in the pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses), in place of the `arena runners` line. If the file or that line is missing, use `claude-opus-5-5-xhigh` and `grok-4.7-xhigh-fast`. Alias and rejected entries follow the runner rules in the **arena** skill's Phase A.

> 执行者取自 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`）里的 `architect runners` 这一行，而不是 `arena runners` 那一行。如果文件或这一行不存在，用 `claude-opus-5-5-xhigh` 和 `grok-4.7-xhigh-fast`。别名条目和被拒绝的条目遵循 **arena** 技能阶段 A 里的执行者规则。

Design it twice. Require at least two structurally distinct candidates before synthesis, even when the first looks sufficient. This is the **exhaust-the-design-space** principle skill made concrete. Whole-shape alternatives, not point fixes inside one shape.

> 设计两遍。在综合之前，至少要有两个结构上截然不同的候选方案，即使第一个看起来已经够用。这就是 **exhaust-the-design-space** 原则技能的具体化。要的是整体形状不同的备选方案，而不是同一个形状里的局部修补。

Screen every candidate against [`references/design-red-flags.md`](references/design-red-flags.zh.md) before synthesis. Assume the next contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Prefer the design where a change that looks right from one file is right for the whole repo.

> 在综合之前，用 [`references/design-red-flags.md`](references/design-red-flags.zh.md) 筛查每一个候选方案。假设下一个贡献者是一个代理：它只看到自己打开的文件，照抄最近的例子，走能编译通过的最短路径。优先选择这样的设计：一个从单个文件看起来正确的改动，对整个仓库也是正确的。

Compare viable candidates on interface depth. Prefer the design that hides more complexity behind a smaller, simpler public surface. A rich interface can keep call chains short by concentrating capability instead of scattering it across layers.

> 在接口深度上比较可行的候选方案。优先选择在更小、更简单的公开接口背后隐藏更多复杂度的设计。一个丰富的接口可以通过集中能力而不是把能力分散到各层，来保持调用链很短。

Arena returns one synthesized design package. Its synthesis decision fills the rationale's "Synthesis decision" section.

> Arena 返回一个综合后的设计包。它的综合决定填进设计理由里的“综合决定”一节。

## Phase C: Agree (opt-in)｜阶段 C：达成一致（可选）

Default: proceed directly to implementation with the synthesized design. No human checkpoint.

> 默认：直接用综合后的设计进入实现。没有人工检查点。

Opt in to a checkpoint when the invoker explicitly asks: "/architect with checkpoint," "stop and show me before implementing," or similar. Then surface the synthesized design and pause for sign-off.

> 当调用者明确要求时才开启检查点：“/architect with checkpoint”“实现之前停下来给我看”，或类似的说法。这时展示综合后的设计，暂停等待签字。

The synthesis can ship as its own commit either way, as the "scaffold first" mode of the **foundational-thinking** principle skill. Planned and scoped breakage during fill-in is fine, per the **outcome-oriented-execution** principle skill. For adversarial pressure on the design before implementing, run the **interrogate** skill on the synthesized sketch.

> 无论哪种情况，综合结果都可以作为单独的提交发布，这就是 **foundational-thinking** 原则技能的“脚手架优先”模式。按 **outcome-oriented-execution** 原则技能，填充过程中有计划、有范围的破坏是可以接受的。如果要在实现之前对设计施加对抗性的压力，就对综合后的草图运行 **interrogate** 技能。

If the human pushes back on the shape (in a checkpoint or after the fact), treat that as Phase A evidence. Re-ground and re-run Phase B before writing more code.

> 如果人对形状提出了反对（在检查点上或事后），把它当作阶段 A 的证据。重新打基础，重跑阶段 B，然后再写更多代码。

## Phase D: Implement against the sketch｜阶段 D：按草图实现

Replace `not implemented` bodies with code, pseudocode with logic. The synthesized sketch is the contract. Deviations from it are signal to surface, not friction to absorb silently. If a function needs a parameter the sketch didn't anticipate, ask whether the sketch was wrong, the requirement was missed, or the implementation is overreaching.

> 把 `not implemented` 的函数体替换成代码，把伪代码替换成逻辑。综合后的草图就是约定。偏离草图是需要提出来的信号，而不是需要悄悄吸收的摩擦。如果某个函数需要一个草图没有预料到的参数，就问一问是草图错了、需求被漏掉了，还是实现越界了。

## Phase E: Scrap when the architecture is wrong｜阶段 E：架构错了就推倒重来

If implementation keeps producing friction the sketch can't absorb, throw the sketch out. Don't bolt fixes onto a wrong design, per the **redesign-from-first-principles** and **fix-root-causes** principle skills.

> 如果实现不断产生草图消化不了的摩擦，就扔掉草图。按 **redesign-from-first-principles** 和 **fix-root-causes** 原则技能，不要在错误的设计上硬接修补。

The signal is a *pattern*, not single instances. Tells:

> 信号是一种*模式*，而不是单个实例。迹象：

- The same shape of workaround appearing repeatedly across unrelated code.

> - 同一种形状的变通办法在无关的代码里反复出现。

- Multiple unrelated edge cases that all need special-case branches.

> - 多个无关的边界情况都需要特例分支。

- Types that need escape hatches (`any`, casts, optional fields always set in practice) to compile.

> - 类型需要逃生口（`any`、强转、实际上总是被设置的可选字段）才能编译。

- The "we need a lock" reflex when the sketch said the state wasn't shared.

> - 草图说状态不共享，却出现了“我们需要一把锁”的条件反射。

- Callers having to know the abstraction's internal rules to use it.

> - 调用方必须知道抽象的内部规则才能使用它。

- Two or more independent Phase D deviations of the same shape across the implementation.

> - 实现中出现两个或更多独立的、形状相同的阶段 D 偏离。

Use judgment. A few edge cases don't condemn an architecture. Some problems are legitimately complex. Complexity in the data is not complexity in the design.

> 自己判断。几个边界情况并不能宣判一个架构死刑。有些问题本来就复杂。数据的复杂不等于设计的复杂。

When you scrap:

> 推倒重来时：

1. Re-run the **how** skill over what's been built.

> 1. 对已经建好的部分重新运行 **how** 技能。

2. Redesign as if the new constraints had been day-one assumptions, per redesign-from-first-principles.

> 2. 按 redesign-from-first-principles，就当新的约束从第一天起就是前提来重新设计。

3. Subtract before adding, per the **subtract-before-you-add** principle skill. The new sketch should be smaller than the old one before it grows.

> 3. 按 **subtract-before-you-add** 原则技能，先减后加。新草图在增长之前应该比旧的更小。

4. Return to Phase B and re-run arena.

> 4. 回到阶段 B，重跑 arena。

## Outputs｜输出

The caller's usage is written first and the type sketch derived from it. One file with new types and signatures for small changes. Module map plus type definitions for larger work. The rationale ships alongside, shaped per `references/rationale-template.md`, including the usage sketch and the synthesis decision.

> 先写调用方的用法，再从中推导出类型草图。小改动是一个包含新类型和签名的文件。较大的工作是模块地图加上类型定义。设计理由随之一起交付，按 `references/rationale-template.md` 的格式写，包括用法草图和综合决定。
