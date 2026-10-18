<!-- 中英对照译文。原文：skills/tdd/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/tdd/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/tdd/SKILL.md)

```yaml
name: tdd
description: "Use only when the user explicitly asks for TDD, a failing test, or a regression test, OR when the bug has an obvious cheap local test target. Skip when the test path is unclear, expensive, integration-heavy, or not requested."
disable-model-invocation: true
```

> **说明（description）：** 只在用户明确要求 TDD、失败测试或回归测试时使用，或者当 bug 有一个明显便宜的本地测试目标时使用。测试路径不清楚、成本高、依赖大量集成或用户没要求时跳过。

# TDD Bug Fix｜TDD 修 bug

When fixing a bug with a clear, cheap test path, make the broken behavior executable before changing production code. The goal is a focused regression test that fails before the fix and passes after it.

> 修一个有清楚、便宜的测试路径的 bug 时，在改生产代码之前，先让出问题的行为变得可执行。目标是一个聚焦的回归测试：修复前失败，修复后通过。

Do not force a test when it would be impractical. If the available test would require broad harness setup, brittle mocks, slow end-to-end infrastructure, production-only state, vague reproduction steps, or large unrelated fixture churn, skip adding a new test and use the closest useful verification instead.

> 不现实的时候不要强行写测试。如果可用的测试需要大范围搭建测试工具、脆弱的模拟、缓慢的端到端基础设施、只在生产环境才有的状态、模糊的复现步骤，或者大量无关的测试数据改动，就跳过新增测试，改用最接近、最有用的验证方式。

## Workflow｜工作流

1. **Understand the bug.** Identify the intended behavior, current behavior, affected path, and smallest observable reproduction.

> 1. **理解 bug。** 弄清预期行为、当前行为、受影响的路径，以及最小的可观察复现。

2. **Choose the narrowest executable check.** Prefer the closest unit, component, integration, or regression test already used for that codepath. If no practical test path is obvious, do not create one from scratch just to satisfy the workflow.

> 2. **选择最窄的可执行检查。** 优先用这条代码路径已经在用的、最接近的单元、组件、集成或回归测试。如果看不出有现实可行的测试路径，不要只是为了满足工作流而从头搭一个。

3. **Write the failing test first.** Add the smallest focused test that would have caught the bug. The test should encode intended behavior, not mirror the current implementation.

> 3. **先写失败的测试。** 加一个最小的、聚焦的、本来能抓住这个 bug 的测试。测试应该编码预期行为，而不是照搬当前的实现。

4. **Run the new test before fixing.** Confirm it fails for the intended reason. If it passes or fails for an unrelated reason, correct the test or reproduction before editing the implementation.

> 4. **修复之前先跑新测试。** 确认它是因为预期的原因失败。如果它通过了，或者因为无关的原因失败，先修正测试或复现，再去改实现。

5. **Fix the bug.** Make the smallest production change that satisfies the intended behavior while preserving nearby contracts.

> 5. **修 bug。** 做满足预期行为的最小生产代码改动，同时保持附近的约定不变。

6. **Rerun the regression test.** Confirm the test now passes.

> 6. **重跑回归测试。** 确认测试现在通过了。

## If a Failing Test Is Impractical｜如果写失败测试不现实

Use the closest executable regression check instead: a targeted script, manual reproduction command, browser automation, snapshot comparison, log assertion, or focused integration check.

> 改用最接近的可执行回归检查：一个针对性的脚本、手工复现命令、浏览器自动化、快照比较、日志断言，或聚焦的集成检查。

Prefer no new test over a bad test. A bad test is one that mostly tests mocks, encodes current implementation details, depends on timing or unrelated global state, needs expensive infrastructure for a small fix, or would be deleted immediately after proving the fix.

> 宁可不加新测试，也不要加一个坏测试。坏测试是指：主要在测模拟、编码了当前的实现细节、依赖时序或无关的全局状态、为一个小修复需要昂贵的基础设施，或者证明完修复后立刻就会被删掉的测试。

## Guardrails｜护栏

- Do not change tests merely to match a wrong implementation.

> - 不要仅仅为了迎合错误的实现去改测试。

- Do not weaken existing assertions unless the expected behavior has genuinely changed and the reason is clear.

> - 不要削弱已有的断言，除非预期行为确实变了，而且原因清楚。

- Keep the regression test focused on the bug. Avoid broad fixture churn or unrelated coverage expansion.

> - 让回归测试聚焦在这个 bug 上。避免大范围改动测试数据，或扩展无关的覆盖。

- If the bug is flaky, make the test deterministic where possible and document the signal being locked down.

> - 如果 bug 时有时无，尽可能让测试变得确定，并记录下被锁定的信号。

- If the bug exposes a broader class of failures, first land the focused regression path, then consider additional sibling coverage.

> - 如果 bug 暴露了一类更广的失败，先落地聚焦的回归路径，再考虑额外的同类覆盖。

## Final Response｜最终回复

Report the evidence, not just the outcome:

> 报告证据，而不只是结果：

- Name the failing-before test or executable check and the failure it produced.

> - 说出修复前失败的测试或可执行检查，以及它产生的失败。

- Name the passing-after test run and any nearby validation performed.

> - 说出修复后通过的那次测试运行，以及做过的任何附近验证。

- If failing-before evidence could not be demonstrated, state why and describe the closest regression check used instead.

> - 如果无法展示修复前失败的证据，说明原因，并描述改用的最接近的回归检查。
