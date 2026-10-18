<!-- 中英对照译文。原文：skills/principle-foundational-thinking/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-foundational-thinking/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-foundational-thinking/SKILL.md)

```yaml
name: principle-foundational-thinking
description: "Apply before writing logic: choosing core types and data structures, sequencing scaffold-vs-feature work, asking what concurrent actors share. Get the data structures right so downstream code becomes obvious."
disable-model-invocation: true
```

> **说明（description）：** 在写逻辑之前使用：选择核心类型和数据结构、安排先搭脚手架还是先做功能的顺序、问清并发参与者共享什么。把数据结构选对，下游的代码就会水到渠成。

# Foundational Thinking｜基础思维

**Structural decisions** protect option value. **Code-level decisions** protect simplicity.

> **结构性决策**保护选择余地。**代码层面的决策**保护简洁。

**Data structures first.** Get the data shape right before writing logic. Define core types early, trace every access pattern, and choose structures that match the dominant paths.

> **数据结构优先。** 写逻辑之前先把数据的形状定对。尽早定义核心类型，追踪每一种访问模式，选择与主要路径相匹配的结构。

At code level, DRY the structure, not every line. Types and data models should converge. Three similar statements still beat a premature abstraction. Prefer explicit over clever. Test behavior and edge cases, not line counts.

> 在代码层面，对结构去重，而不是对每一行去重。类型和数据模型应该收敛。三条相似的语句仍然胜过一个过早的抽象。宁可明确，不要取巧。测试行为和边界情况，而不是行数。

**Concurrency corollary.** Before sharing state between actors, ask "what happens if another actor modifies this concurrently?" If not "nothing", isolate.

> **并发推论。** 在参与者之间共享状态之前，先问“如果另一个参与者同时修改它会怎样？”如果答案不是“什么都不会发生”，就隔离。

**Scaffold first.** If something helps every later phase, do it first. Ask "does every subsequent phase benefit from this existing?" CI, linting, test infrastructure, and shared types are scaffold. Sequence for option value: setup before features, tests before fixes. Keep commits small and single-purpose.

> **脚手架优先。** 如果某样东西对之后的每个阶段都有帮助，就先做它。问“之后的每个阶段都会因为它的存在而受益吗？”CI、lint、测试基础设施和共享类型都是脚手架。按保留选择余地的原则安排顺序：先搭环境再做功能，先写测试再修复。保持提交小而单一。

Each increment should land a coherent abstraction or deepen one that exists. Do not spread a new capability across callers as special-case coordination.

> 每一次增量都应该落地一个连贯的抽象，或者加深一个已有的抽象。不要把一项新能力以特例协调的方式散布到各个调用方。

Subtraction comes before scaffolding. Remove dead code first, then lay foundations.

> 删减先于搭脚手架。先删掉死代码，再打基础。
