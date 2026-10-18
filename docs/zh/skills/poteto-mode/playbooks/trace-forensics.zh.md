<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/trace-forensics.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/trace-forensics.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/trace-forensics.md)

### Trace forensics｜性能文件取证

**You own the diagnosis from the artifact.**

> **从产物得出诊断由你负责。**

Distinct from **Runtime forensics**, which instruments the live process. Here the capture already exists. The artifact is a fixed dataset, read it, don't re-run it. Keep tooling generic: a DevTools or trace parser for cpuprofile and `.json.gz`, a text editor for a spindump, your heap tooling for a heapsnapshot.

> 与 **Runtime forensics** 不同，后者对运行中的进程插桩。这里的抓取已经存在。产物是一份固定的数据集，读它，不要重跑。工具保持通用：cpuprofile 和 `.json.gz` 用 DevTools 或 trace 解析器，spindump 用文本编辑器，heapsnapshot 用你的堆分析工具。

1. Identify the format and load it with the right tool. Parse large artifacts in a subagent (the **principle-guard-the-context-window** skill) and keep the reduced finding in the main thread. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 1. 识别格式，用合适的工具加载。在子代理里解析大的产物（**principle-guard-the-context-window** 技能），主线程只保留精简后的发现。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

2. Transform the raw artifact into a form you can query. Dump the trace or heap snapshot into sqlite, one row per sample, frame, or node. Reach the queryable shape before you read.

> 2. 把原始产物转换成可以查询的形式。把 trace 或堆快照导入 sqlite，每个采样、栈帧或节点一行。先得到可查询的形式，再去读。

3. Narrow to the cause. Query for the frames that hold the most time and walk the call tree to the hot path. For a leak, follow the retainer chain from the leaked object to a GC root. For a spindump, find the thread stuck on-CPU or blocked and its wait reason.

> 3. 缩小到原因。查询占用时间最多的栈帧，沿着调用树走到热路径。泄漏的话，从泄漏对象沿引用链走到 GC 根。spindump 的话，找到卡在 CPU 上或被阻塞的线程以及它的等待原因。

4. Attribute to source. Map the hot frame to file, symbol, and line via the artifact's own symbols. A frame with no source mapping is not yet a diagnosis. Resolve the symbols, or say plainly the artifact does not carry them.

> 4. 归因到源码。通过产物自带的符号，把热点栈帧对应到文件、符号和行。没有源码映射的栈帧还不算诊断。解析出符号，或者直接说明产物里没有符号。

5. If you have a paired capture, diff the before and after artifacts to confirm. Otherwise, mark the finding as the strongest hypothesis the artifact supports, not a confirmed cause.

> 5. 如果你有一对抓取，比较前后两份产物来确认。否则，把发现标记为产物所能支持的最强假设，而不是已确认的原因。

6. Hand back a cited diagnosis, no fix unless asked. Route to Bug fix or Perf issue once the cause is known. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only forensics`.

> 6. 交回一个带引用的诊断，除非被要求，不做修复。原因一旦明确，就分派到 Bug fix 或 Perf issue。吞吐量检查点只写一行：`throughput checkpoint: n/a, read-only forensics`。

**Reply:** the artifact and format, the reduced finding, the source location, the artifact paths, and whether a paired capture confirmed it.

> **回复：** 产物及其格式、精简后的发现、源码位置、产物路径，以及是否有成对的抓取确认了它。
