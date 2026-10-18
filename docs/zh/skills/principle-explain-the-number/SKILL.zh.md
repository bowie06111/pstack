<!-- 中英对照译文。原文：skills/principle-explain-the-number/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-explain-the-number/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-explain-the-number/SKILL.md)

```yaml
name: principle-explain-the-number
description: "Apply before you trust, report, or act on a number you measured: a speedup, a regression, a throughput, a latency, or an eval result. Find what limits it, and rule out that it measured something other than the work you think."
disable-model-invocation: true
```

> **说明（description）：** 在信任、报告或依据一个你测出来的数字（提速、退化、吞吐量、延迟或评测结果）行动之前使用。找出是什么限制了它，并排除它测的其实不是你以为的那项工作。

# Explain the Number｜解释这个数字

- **Ask "why not double?"** Name the resource or code path that bounds the result, such as a core, a lock, the disk, the network, or the load generator itself. Get it from a profile or from system counters taken during a run, then map it to source. A guess from reading the code is not a limiter. If you cannot say why the number is not twice as good, you do not know what you measured.

> - **问“为什么不是两倍？”** 说出限制结果的资源或代码路径，比如某个 CPU 核、一把锁、磁盘、网络，或者负载生成器本身。从运行期间的性能分析或系统计数器里得到它，然后对应到源码。靠读代码猜出来的不算限制因素。如果你说不出为什么这个数字不是好一倍，你就不知道自己测的是什么。

- **List what else the number could be measuring, and rule out each one with evidence.** The usual suspects are errors, skipped or cached work, code that never ran, an untuned side (one left on default settings), noise, and a piece too small to matter end to end.

> - **列出这个数字还可能在测什么，并用证据逐一排除。** 常见的嫌疑有：错误、被跳过或被缓存的工作、从没运行过的代码、没调优的一方（用了默认设置的那一边）、噪音，以及一个小到在端到端层面无关紧要的部分。

- **Keep the evidence with the number.** Put the run count, the spread, and the limiter in the notes or a linked artifact.

> - **让证据和数字放在一起。** 把运行次数、离散程度和限制因素写在备注或链接的产物里。

For a performance number, run the full procedure with the [benchmark-checklist](../benchmark-checklist/SKILL.zh.md) skill. For an eval result, ask the same of the trials: did every run do the task, does the gap hold across trials and models, and does the scenario matter.

> 对于性能数字，用 [benchmark-checklist](../benchmark-checklist/SKILL.zh.md) 技能走完整流程。对于评测结果，对各次试验问同样的问题：每次运行都完成了任务吗？差距在多次试验和不同模型之间都成立吗？这个场景重要吗？

You skipped this when the evidence behind a number has no run count, no spread, or no named limiter, or when the time saved is larger than the time the changed piece took.

> 如果一个数字背后的证据没有运行次数、没有离散程度、或者没有点名的限制因素，或者节省的时间比被改动的那部分本身花的时间还多，那就说明你跳过了这一步。

Distinct from [Prove It Works](../principle-prove-it-works/SKILL.zh.md), which checks that an output is real.

> 不同于 [证明它能用](../principle-prove-it-works/SKILL.zh.md)，后者检查一个产出是否真实。
