<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/feature.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/feature.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/feature.md)

### Feature｜功能

**You own the design. Plan, review, verify.** Delegate implementation. Stay in the lead.

> **设计由你负责。规划、审查、验证。** 把实现委派出去。自己保持主导。

1. `how` over the affected subsystem.

> 1. 用 `how` 梳理受影响的子系统。

2. `architect` for parallel design exploration.

> 2. 用 `architect` 并行探索设计方案。

3. Write the throughput checkpoint as four todo items. A dimension that genuinely does not apply (single file, no fan-out) keeps its item with `n/a: <reason>` rather than being dropped:
   - **Blocking first steps.** Gates run before fan-out.
   - **Independent workstreams.** Disjoint files, services, or layers parallelize. Shared writes serialize.
   - **Shared mutable state.** Default to splitting the target (the **separate-before-serializing-shared-state** principle skill). Serialize only for real invariants.
   - **Smallest safe decomposition.** If one worker is best, name why.

> 3. 把吞吐量检查点写成四个待办项。确实不适用的维度（只有一个文件、不需要扇出）保留它的待办项，写 `n/a: <原因>`，而不是删掉：
>    - **阻塞性的前置步骤。** 扇出之前要先跑的关卡。
>    - **独立的工作流。** 互不重叠的文件、服务或层可以并行。共享的写操作要串行。
>    - **共享的可变状态。** 默认拆分目标（**separate-before-serializing-shared-state** 原则技能）。只有真正的不变量才串行化。
>    - **最小的安全拆分。** 如果一个工作者最好，说明原因。

4. Delegate code-writing to a subagent using your configured feature model (default `grok-4.7-xhigh-fast`) with a specific scope (file paths, named data shape and its organizing structure per **principle-model-the-domain**, a state machine over scattered booleans, a table/registry over branching, a typed model over repeated shape assumptions, chosen before the delegate writes logic, and success criteria). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section. When the implementation admits multiple valid shapes (error handling, abstraction layer, test structure), delegate via the **arena** skill instead so the runners surface the alternatives and the cross-judge guards the pick. Mandatory: no skip-with-reason escape, and Laziness Protocol does not override it (the gain is review separation, not lines saved). A subagent forbidden to spawn satisfies this by owning the diff directly with the same review separation. No "standing by" reply that waits on a nested agent. Comments per **Comments**. Surgical edits, re-ground against the source for upstream-derived files. Port shared-primitive improvements to all consumers and verify each. Commit liberally.

> 4. 用你配置的 feature 模型（默认 `grok-4.7-xhigh-fast`）把写代码委派给子代理，并给出明确的范围（文件路径；按 **principle-model-the-domain** 命名的数据形状及其组织结构，比如用状态机代替散落的布尔值、用表或注册表代替分支、用类型化模型代替重复的形状假设，这些要在委派者写逻辑之前选好；以及成功标准）。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。当实现可以有多种合理的形状时（错误处理、抽象层、测试结构），改为通过 **arena** 技能委派，让执行者把备选方案都摆出来，由交叉评委把关选择。这是强制的：没有“说明理由后跳过”的退路，懒惰协议也不能覆盖它（收益在于审查分离，而不是省几行代码）。被禁止派生子代理的子代理，通过直接负责 diff 并保持同样的审查分离来满足这一点。不要用“待命”的回复去等一个嵌套代理。注释按 **注释** 一节。做外科手术式的修改，对来自上游的文件要重新对照源头。把对共享基础组件的改进移植到所有使用方，并逐一验证。勤提交。

5. Verify on the matching surface. "Inconclusive" or wrong-surface is not a pass. Flag it.

> 5. 在对应的界面上验证。“无法判断”或测错了界面都不算通过。要标出来。

6. Rebase into small, ordered commits. Stack follow-ups.
   Use the **sequence-verifiable-units** principle skill, building, verifying, and committing each small unit before the next.

> 6. 变基成小而有序的提交。后续工作堆叠上去。
>    使用 **sequence-verifiable-units** 原则技能，每个小单元构建、验证、提交之后再做下一个。

7. If the design is contested, `interrogate` before shipping.

> 7. 如果设计有争议，发布前用 `interrogate`。

8. Run **Opening a PR**.

> 8. 运行 **Opening a PR**。

Code-coupled work (one feature, one migration) goes to a single owner with the checkpoint inline. That owner fans out internally after the blocking phase. Parent-level fan-out is for slices that produce independent artifacts (audits, cross-subsystem investigations, competing experiments). Rewrite the checkpoint at phase boundaries. Spawn a fresh owner rather than chaining interrupts.

> 代码耦合的工作（一个功能、一次迁移）交给单个负责人，检查点写在其中。这个负责人在阻塞阶段之后在内部扇出。父级扇出用于产出独立产物的分片（审计、跨子系统的调查、相互竞争的实验）。在阶段边界重写检查点。宁可派生一个新的负责人，也不要串联中断。

**Reply:** what you built, what you chose and why, the throughput checkpoint, open decisions. Tables for design alternatives.

> **回复：** 你构建了什么、你选了什么以及为什么、吞吐量检查点、待决事项。设计备选方案用表格。
