<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/refactoring.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/refactoring.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/refactoring.md)

### Refactoring｜重构

**You own the contract. The structure changes. The behavior does not.** Distinct from Feature, which adds behavior, and Bug fix, which corrects it.

> **契约由你负责。结构改变。行为不变。** 与 Feature 不同，后者增加行为；与 Bug fix 也不同，后者纠正行为。

If the cleanup reveals a missing feature or a real bug, split it out and ship the structural change first against the pinned contract. A redesign is allowed, but name it and route to Feature. Large or cross-cutting structural work belongs to the **figure-it-out** skill. This playbook is the focused-to-medium change.

> 如果清理过程中发现缺少某个功能或有真正的 bug，就把它拆出去，先对照固定下来的契约发布结构性改动。可以重新设计，但要说明，并转到 Feature。大型或跨领域的结构性工作属于 **figure-it-out** 技能。本剧本处理小到中等规模的改动。

1. Pin the behavior contract first. Run the **how** skill over the affected subsystem to learn the contract, then write a characterization test, snapshot, or equivalence harness that captures current behavior before any structure moves. If the area has no coverage, write the pin before touching structure. Type check and lint are not a pin.

> 1. 先固定行为契约。用 **how** 技能梳理受影响的子系统，弄清契约，然后在动任何结构之前，写一个特征测试、快照或等价性测试工具，记录当前行为。如果这块区域没有测试覆盖，先写固定装置再动结构。类型检查和 lint 不算固定装置。

2. Name the structure the code is missing per **principle-model-the-domain**. Boring code stays when the shape is already clear and local. The reshape must delete branches or invalid states, not add indirection.

> 2. 按 **principle-model-the-domain** 说出代码缺少的结构。形状已经清楚且局部时，平淡的代码就保留。重塑必须删掉分支或非法状态，而不是增加间接层。

3. Name the target shape. State what the module layout, types, and call graph should be if built today (**principle-foundational-thinking**, **principle-redesign-from-first-principles**). If the target crosses a function boundary, run the **architect** skill for parallel design exploration of the shape before the move.

> 3. 说出目标形状。说明如果今天从头写，模块布局、类型和调用图应该是什么样（**principle-foundational-thinking**、**principle-redesign-from-first-principles**）。如果目标跨越函数边界，在动手之前用 **architect** 技能并行探索形状的设计方案。

4. Subtract before you add. Delete dead code, collapse one-caller wrappers, drop redundant validators, and remove orphan references before introducing the new shape (**principle-subtract-before-you-add**). The smallest change that reaches the target shape ships (**principle-laziness-protocol**). A speculative cleanup that "might help" gets reverted.

> 4. 先减后加。在引入新形状之前，删掉死代码，合并只有一个调用方的包装层，去掉多余的校验，移除孤立的引用（**principle-subtract-before-you-add**）。只发布能达到目标形状的最小改动（**principle-laziness-protocol**）。“也许有用”的投机性清理要回滚。

5. Move in small behavior-preserving steps, each keeping the pin green. For API reshapes, migrate every caller and delete the old API in the same wave (**principle-migrate-callers-then-delete-legacy-apis**). No compatibility shims, no parallel old-and-new paths. Spot-check every rename against the actual files. Renames silently miss usages in strings, prose, and back-references. Delegate the mechanical edits to a subagent using your configured refactoring model (default `grok-4.7-xhigh-fast`) with a specific scope (file paths, the names being moved, the behavior to hold). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 5. 用小的、保持行为不变的步骤推进，每一步都让固定装置保持绿色。对于接口重塑，在同一波里迁移所有调用方并删除旧接口（**principle-migrate-callers-then-delete-legacy-apis**）。不留兼容垫片，不保留新旧并行的路径。对照实际文件抽查每一次重命名。重命名会悄悄漏掉字符串、文字说明和反向引用里的用法。用你配置的 refactoring 模型（默认 `grok-4.7-xhigh-fast`）把机械性修改委派给子代理，给出明确的范围（文件路径、要移动的名字、要保持的行为）。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

6. Prove behavior is unchanged on the real artifact, not "it compiles" (**principle-prove-it-works**). For larger reshapes, run an equivalence check: a script that diffs old-vs-new outputs, a recorded baseline replayed against the new code, or a smoke run on the matching surface via the relevant control skill.

> 6. 在真实产物上证明行为没变，而不是“能编译”（**principle-prove-it-works**）。对于较大的重塑，跑一次等价性检查：一个比较新旧输出的脚本、一份对新代码重放的录制基线，或者通过相关控制技能在对应界面上的冒烟测试。

7. Confirm the change is worth keeping. The success measure is reduced reader load (**principle-minimize-reader-load**). If the diff does not lower reader load somewhere, revert it.

> 7. 确认这个改动值得保留。成功的衡量标准是阅读负担降低了（**principle-minimize-reader-load**）。如果 diff 没有在任何地方降低阅读负担，就回滚它。

8. Rebase into small ordered commits. A subtraction commit, then the reshape, then any follow-on cleanup. Shape them with the **sequence-verifiable-units** principle skill, so each behavior-preserving slice stays green before the next. Run **Opening a PR**.

> 8. 变基成小而有序的提交。先是删减的提交，然后是重塑，再是后续清理。用 **sequence-verifiable-units** 原则技能组织它们，让每个保持行为不变的切片在下一个之前都保持绿色。运行 **Opening a PR**。

**Reply:** the structure that changed, the pin you held it against, the equivalence proof, the reader-load delta, what shipped and what got reverted. No new behavior.

> **回复：** 改变了什么结构、你用什么固定装置守住它、等价性证明、阅读负担的变化、发布了什么以及回滚了什么。没有新行为。
