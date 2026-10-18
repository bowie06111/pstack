<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/runtime-forensics.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/runtime-forensics.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/runtime-forensics.md)

### Runtime forensics｜运行时取证

**You own the diagnosis. Instrument the live process, don't theorize from source.** The deliverable is a cited diagnosis, not a fix.

> **诊断由你负责。对运行中的进程插桩，不要从源码推理。** 交付物是一个带引用的诊断，不是修复。

1. Capture the live signal on the matching surface via the control skill: a CPU profile for a spinning process, a heap snapshot for a leak, a CDP trace for a visual glitch. A real artifact, not a guess.

> 1. 通过控制技能在对应的界面上抓取实时信号：空转的进程抓 CPU 性能分析，泄漏抓堆快照，画面故障抓 CDP trace。要真实的产物，不要猜测。

2. Reduce the artifact to the smoking gun: the function on the hot path, the retainer chain from the leaked object to a GC root, the loop firing without input. Parse large artifacts in a subagent (the **guard-the-context-window** principle skill), keep the reduced finding in the main thread. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 2. 把产物精简到确凿证据：热路径上的函数、从泄漏对象到 GC 根的引用链、没有输入却在不停触发的循环。在子代理里解析大的产物（**guard-the-context-window** 原则技能），主线程里只保留精简后的发现。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

3. Prove the mechanism before believing it. Inject instrumentation via CDP eval on the running process, or hotfix the live code without reloading, to confirm the hypothesis cheaply.

> 3. 先证明机制，再相信它。通过 CDP eval 向运行中的进程注入插桩，或者不重新加载直接热修实时代码，低成本地确认假设。

4. Map the finding back to source: file, symbol, the line that allocates or schedules.

> 4. 把发现对应回源码：文件、符号、执行分配或调度的那一行。

5. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only forensics`.

> 5. 吞吐量检查点只写一行：`throughput checkpoint: n/a, read-only forensics`。

**Reply:** the signal captured, the reduced finding, how you proved the mechanism, the source location, artifact paths. No fix unless asked. Hand back to Bug fix or Perf once the cause is known.

> **回复：** 抓到的信号、精简后的发现、你如何证明了机制、源码位置、产物路径。除非被要求，不做修复。原因一旦明确，就交回给 Bug fix 或 Perf。
