<!-- 中英对照译文。原文：skills/benchmark-checklist/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/benchmark-checklist/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/benchmark-checklist/SKILL.md)

```yaml
name: benchmark-checklist
description: "Vet a perf measurement (limiter, tuning, limits, errors, repeatability, relevance, and whether the work happened) before you report or act on it. Use when you run a benchmark or report a speedup or regression you measured."
disable-model-invocation: true
```

> **说明（description）：** 在报告或依据一个性能测量行动之前审查它（限制因素、调优、极限、错误、可重复性、相关性，以及工作是否真的发生了）。在你运行基准测试，或报告你测出来的提速或退化时使用。

# Benchmark checklist｜基准测试检查清单

Use this when you produce a performance number: a PR's before and after, a regression claim, a hillclimb harness, or a library or config choice. Answer each question with evidence from a run, not a guess about the code.

> 当你产出一个性能数字时使用：PR 的前后对比、一个退化的说法、一个爬坡测量工具，或者对库或配置的选择。每个问题都用一次运行得来的证据回答，而不是对代码的猜测。

For a quick ballpark the user asked for, one run is enough. Still check questions 4 and 7, and say that it is one run. Skip the rest unless that run looks wrong. A choice between options is never a ballpark.

> 如果用户要的只是一个大概的估计，跑一次就够了。但仍要检查第 4 和第 7 个问题，并说明这只是一次运行。除非那次运行看起来不对劲，否则其余的可以跳过。在多个选项之间做选择永远不算“大概估计”。

## Before you run anything｜运行任何东西之前

- Write down the claim you expect to make, in the words you would ship ("export is 30% faster at p50 on the 60k-row dataset"). The questions test that sentence.

> - 把你预期要得出的结论写下来，用你会发布的那种措辞（“在 6 万行数据集上，导出的 p50 快了 30%”）。这些问题检验的就是这句话。

- Read the measurement script. Note what it times, counts, and ignores.

> - 读测量脚本。记下它计时什么、计数什么、忽略什么。

- Check the load average with `uptime` and the core count with `nproc`. If the machine is busy, find out what is running. If you cannot stop it, interleave the sides so both see the same noise, and say so in the report.

> - 用 `uptime` 查负载，用 `nproc` 查核心数。如果机器很忙，查清楚在跑什么。如果停不掉它，就交替运行两边，让两边受到同样的噪音影响，并在报告里说明。

## The questions｜问题

1. **Why not double?** Name the limiter. Profile in a run you do not report, because profilers and tracers slow the work down. Use CPU per process (`top`, `pidstat`), a runtime profiler (`node --cpu-prof`, `py-spy`, `perf`), I/O wait, and syscall counts (`strace -c` on Linux), then map the hot spot to source. Watch the load generator too: if it saturates first, you measured the load generator. If a change did not move the number, the limiter explains why, so find it before you call the change useless.

> 1. **为什么不是两倍？** 说出限制因素。在一次不报告的运行里做性能分析，因为性能分析器和 trace 工具会拖慢工作。用每个进程的 CPU（`top`、`pidstat`）、运行时性能分析器（`node --cpu-prof`、`py-spy`、`perf`）、I/O 等待和系统调用计数（Linux 上的 `strace -c`），然后把热点对应到源码。也要盯着负载生成器：如果它先饱和了，你测的就是负载生成器。如果某个改动没有让数字变化，限制因素能解释原因，所以在宣布这个改动没用之前先找到它。

2. **Was it tuned?** Run every side the way production runs it: release builds, production flags and env, batching and transaction settings, connection pools, caches as warm or cold as production sees them, and the same versions and data. If one side runs on defaults, you compared configurations, not implementations. A limiter that is a setting (a commit per row, a debug build, a missing index) means that side is untuned. Tune it and measure again before you pick a winner. If you cannot tune it, do not pick a winner from that run. When the user is choosing what to adopt, narrowing the claim to the code as it ships today does not fix this, because they adopt the option, not today's settings.

> 2. **调优了吗？** 让每一边都按生产环境的方式运行：release 构建、生产环境的参数和环境变量、批处理和事务设置、连接池、和生产环境一样热或冷的缓存，以及相同的版本和数据。如果一边用的是默认设置，你比较的是配置，而不是实现。如果限制因素是某个设置（每行提交一次、debug 构建、缺少索引），说明那一边没调优。调优后再测一次，然后再选赢家。如果没法调优，就不要根据那次运行选赢家。当用户在选择要采用什么时，把结论缩小到“按今天发布的代码来说”也解决不了这个问题，因为他们采用的是这个选项，而不是今天的设置。

3. **Did it break limits?** Do the arithmetic. Compare bytes per second with disk and network bandwidth, operations per second times cost per operation with your cores, and the time saved with the time the changed piece took. Removing a piece that takes 10% of the run can make the run at most about 11% faster. A result past a limit measured something other than the work, such as a cache, a no-op, or a bug.

> 3. **突破极限了吗？** 算一算。把每秒字节数和磁盘、网络带宽比较，把每秒操作数乘以每次操作的开销和你的核心数比较，把节省的时间和被改动部分本身所花的时间比较。去掉一个占运行时间 10% 的部分，最多能让运行快大约 11%。超过极限的结果，测到的是工作以外的东西，比如缓存、空操作或 bug。

4. **Did it error?** Count failures and non-success responses, and check that outputs are correct, not just present. Rejections are often fast, and timeouts and retries are slow. If the script does not count errors, add the count.

> 4. **出错了吗？** 统计失败和非成功的响应数，并检查输出是正确的，而不只是存在。拒绝往往很快，而超时和重试很慢。如果脚本不统计错误，就把计数加上。

5. **Does it reproduce?** Run each side at least 5 times, alternating (A, B, A, B, and so on) so warmup, lazy initialization, caches, and drift favor neither. Report the median and range. A gap smaller than the run-to-run variation is no measurable difference. When the call is close, use a rank-sum test or the harness's own statistics.

> 5. **能复现吗？** 每一边至少跑 5 次，交替进行（A、B、A、B，依此类推），让预热、延迟初始化、缓存和漂移不偏向任何一边。报告中位数和范围。比每次运行之间的波动还小的差距，就是没有可测量的差异。结果很接近时，用秩和检验或测量工具自带的统计。

6. **Does it matter?** Next to any micro result, measure the end-to-end path a user waits on, with realistic data sizes and concurrency, and report the micro result as a share of the whole. A helper that takes 1% of a request can make the request at most 1% faster, however fast the helper gets.

> 6. **重要吗？** 在任何微观结果旁边，测量用户实际等待的端到端路径，使用真实的数据量和并发，并把微观结果作为整体的一部分来报告。一个只占请求 1% 时间的辅助函数，不管它变得多快，最多也只能让请求快 1%。

7. **Did it even happen?** Confirm the work ran inside the timed region: the request reached the server, the rows were written, the bytes were read, and the code used the result. Lazy code (generators nobody iterates, promises nobody awaits, results the JIT can discard) and timeouts produce numbers for work that never happened.

> 7. **工作真的发生了吗？** 确认工作在计时区间里运行了：请求到达了服务器、行被写入了、字节被读取了，代码也用到了结果。惰性代码（没人迭代的生成器、没人 await 的 promise、JIT 可以丢弃的结果）和超时，会为从没发生过的工作产出数字。

## Report｜报告

- Lead with the verdict: faster, slower, no measurable difference, or inconclusive.

> - 先说结论：更快、更慢、没有可测量的差异，或无法判断。

- Give the number with its unit, the run count, the range, and the limiter. For example, "p50 41 ms → 33 ms, median of 7 runs per side, range 32 to 35 ms after, bound by JSON parsing on one core."

> - 给出带单位的数字、运行次数、范围和限制因素。例如，“p50 41 ms → 33 ms，每边 7 次运行的中位数，改动后范围 32 到 35 ms，受限于单核上的 JSON 解析。”

- Call the verdict inconclusive when you claim a difference but cannot name the limiter, when a side ran untuned, or when you could not check questions 4 and 7. Name the gap.

> - 当你声称有差异却说不出限制因素、某一边没调优，或者没法检查第 4 和第 7 个问题时，结论就是无法判断。说出缺口在哪里。

- Keep a PR body to one primary number, per the **Opening a PR** playbook. Put the runs, range, and limiter evidence in a linked artifact or notes file.

> - 按 **Opening a PR** 剧本，PR 正文里只放一个主要数字。把各次运行、范围和限制因素的证据放到链接的产物或笔记文件里。

## Other perf material｜其他性能相关内容

- Vet the **Perf issue** playbook's baseline before the playbook plans from it, and every number after that.

> - 在 **Perf issue** 剧本根据基线做规划之前审查这个基线，之后的每个数字也都要审查。

- Vet the **Hillclimb** playbook's harness before it is frozen. The frozen harness then prints error and work counts, so each keep-or-revert checks questions 4 and 7 for free.

> - 在 **Hillclimb** 剧本的测量工具被冻结之前审查它。冻结后的工具会打印错误数和工作量，所以每次保留或回滚时，第 4 和第 7 个问题都能免费检查到。
