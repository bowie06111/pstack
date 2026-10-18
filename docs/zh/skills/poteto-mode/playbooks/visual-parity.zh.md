<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/visual-parity.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/visual-parity.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/visual-parity.md)

### Visual parity｜视觉一致

**You own pixel-exact equivalence. The baseline is the spec. You do not touch it.** Equivalence is verified by image diff, not by eye.

> **像素级的一致由你负责。基线就是规格。你不能动它。** 一致性靠图像比对来验证，而不是靠肉眼。

1. Establish the baseline first, before any migration: a visual regression harness that screenshots the current component across its states, plus the target when matching two implementations. No baseline, no parity claim. A blocking prerequisite, not a follow-up.

> 1. 在任何迁移之前先建立基线：一个视觉回归测试工具，对当前组件的各种状态截图，在要对齐两套实现时再加上目标的截图。没有基线，就不能声称一致。这是阻塞性的前提，不是后续事项。

2. Anti-shortcut clauses, stated and held: no harness modifications, no baseline tampering, no component restructuring to make a diff pass. If the baseline looks wrong, stop and ask, don't edit it.

> 2. 明确并坚守防止走捷径的条款：不修改测试工具，不篡改基线，不为了让比对通过而重构组件。如果基线看起来不对，停下来问，不要去改它。

3. Migrate one component at a time. Parallelize across worktrees, one owner per component (the **separate-before-serializing-shared-state** principle skill). Shared primitives migrate first as a blocking phase.

> 3. 一次迁移一个组件。跨多个 worktree 并行，每个组件一个负责人（**separate-before-serializing-shared-state** 原则技能）。共享的基础组件作为阻塞阶段最先迁移。

4. Verify each component against its baseline via image diff on the matching surface via the control skill. A nonzero diff is a fail. Investigate the pixel delta. `/loop` per component until the diff is zero.

> 4. 通过控制技能在对应的界面上，用图像比对把每个组件和它的基线对照验证。比对差异不为零就是失败。调查像素差异。对每个组件 `/loop`，直到差异为零。

5. Run **Opening a PR** per component or per safe batch.

> 5. 按组件或按安全的批次运行 **Opening a PR**。

**Reply:** components migrated, the diff result for each, the baseline harness location, what's left.

> **回复：** 已迁移的组件、每个组件的比对结果、基线测试工具的位置、还剩下什么。
