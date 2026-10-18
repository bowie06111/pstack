<!-- 中英对照译文。原文：skills/principle-outcome-oriented-execution/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-outcome-oriented-execution/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-outcome-oriented-execution/SKILL.md)

```yaml
name: principle-outcome-oriented-execution
description: "Apply during planned rewrites and migrations with explicit phase boundaries. Converge on the target architecture; don't preserve smooth intermediate states with throwaway compatibility code."
disable-model-invocation: true
```

> **说明（description）：** 在有明确阶段边界的计划内重写和迁移中使用。直奔目标架构；不要为了保持平稳的中间状态而写一次性的兼容代码。

# Outcome-Oriented Execution｜以结果为导向执行

Optimize for the intended, verifiable end state rather than preserving smooth intermediate states.

> 为预期的、可验证的最终状态做优化，而不是为了保持平稳的中间状态。

**Why:** Keeping every intermediate step fully stable often creates temporary compatibility code that becomes long-lived debt. Converge on the target architecture and prove correctness at explicit verification boundaries.

> **原因：** 让每一个中间步骤都完全稳定，往往会产生临时的兼容代码，而这些代码会变成长期的技术债。直奔目标架构，并在明确的验证边界上证明正确性。

**Core rule:**

> **核心规则：**

- Prioritize end-state integrity over transitional stability

> - 最终状态的完整性优先于过渡期的稳定性

- Intermediate breakage is acceptable when it is planned, scoped, and reversible

> - 有计划、有范围、可逆的中间破坏是可以接受的

**Guardrails:**

> **护栏：**

- Use this for planned rewrites and migrations with explicit phase boundaries

> - 用于有明确阶段边界的计划内重写和迁移

- Declare where temporary breakage is acceptable

> - 声明在哪里可以接受临时破坏

- Keep high-signal checks for actively touched areas while migrating

> - 迁移期间，对正在改动的区域保留高信号的检查

- Require full static and runtime verification at plan completion

> - 计划完成时要求完整的静态和运行时验证
