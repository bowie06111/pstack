<!-- 中英对照译文。原文：skills/why/references/sources/datadog.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/datadog.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/datadog.md)

# Datadog Telemetry｜Datadog 遥测

## What this source contains｜这个信息源包含什么

Datadog holds the runtime record, what actually happened in production, as opposed to what was planned or discussed.

> Datadog 保存着运行时的记录，也就是生产环境里实际发生了什么，而不是计划或讨论了什么。

- **Metrics.** Counters, gauges, histograms instrumented by the team. A metric's *presence* is itself evidence that someone thought the number worth watching.

> - **指标。** 团队埋点的计数器、仪表、直方图。一个指标的*存在*本身就是证据，说明有人认为这个数字值得盯着。

- **Monitors & alerts.** Conditions the team decided warranted waking someone up. A monitor firing on `rate_limit_hit > 10/min` is direct evidence the team worried about that threshold.

> - **监控和告警。** 团队认为值得把人叫醒的条件。一个在 `rate_limit_hit > 10/min` 时触发的监控，直接证明团队担心那个阈值。

- **Dashboards.** Curated views of what the team considers important for a subsystem.

> - **仪表盘。** 团队认为对某个子系统重要的东西的精选视图。

- **APM traces & spans.** Request-level runtime data, for "why is this slow" / "why is there a timeout here" questions.

> - **APM trace 和 span。** 请求级别的运行时数据，用于“为什么这么慢”/“为什么这里有超时”之类的问题。

- **Logs.** High-volume event records, often holding the error conditions that motivated defensive code.

> - **日志。** 大量的事件记录，经常包含促成防御性代码的错误情况。

- **Incidents.** Formal incident records with timelines and linked postmortems.

> - **事故。** 带有时间线和链接复盘的正式事故记录。

- **Notebooks.** The team's exploratory investigations, often with hypotheses and analyses.

> - **笔记本。** 团队的探索性调查，经常带有假设和分析。

## How to search it｜怎么搜索

Use the Datadog MCP. Start broad, then narrow.

> 使用 Datadog 的 MCP。先宽后窄。

1. **Identify the owning service(s).**

> 1. **确定所属的服务。**

   ```
   search_datadog_services (filter by name or team)
   search_datadog_service_dependencies (see upstream/downstream)
   ```

> 上面代码块的中文版：

   ```
   search_datadog_services（按名称或团队过滤）
   search_datadog_service_dependencies（查看上游/下游）
   ```

2. **Dashboards and monitors first.**

> 2. **先看仪表盘和监控。**

   ```
   search_datadog_dashboards (query: feature name, service name, symbol)
   search_datadog_monitors   (same queries)
   ```

> 上面代码块的中文版：

   ```
   search_datadog_dashboards（查询：功能名、服务名、符号）
   search_datadog_monitors   （同样的查询）
   ```

   When a dashboard or monitor covers the target, note its queries and watched thresholds. The threshold is frequently the answer to "why is this clamped at N?"

>    当某个仪表盘或监控覆盖了目标时，记下它的查询和所盯的阈值。这个阈值经常就是“为什么这里被限制在 N？”的答案。

3. **Metrics around the target.**

> 3. **目标附近的指标。**

   ```
   search_datadog_metrics (by name pattern, e.g., the feature or symbol)
   get_datadog_metric_context (metadata: description, units, tags)
   get_datadog_metric (timeseries; "was there a spike around the PR date?")
   ```

> 上面代码块的中文版：

   ```
   search_datadog_metrics（按名称模式，例如功能或符号）
   get_datadog_metric_context（元数据：描述、单位、标签）
   get_datadog_metric（时间序列；“PR 日期前后有没有激增？”）
   ```

   Correlating a metric's trajectory with the target's add/change date is strong supporting evidence: "the `payment_timeout` metric spiked 2023-11-03, and the retry logic merged 2023-11-06."

>    把指标的走势和目标被加入/修改的日期关联起来，是很强的佐证：“`payment_timeout` 指标在 2023-11-03 激增，而重试逻辑在 2023-11-06 合并。”

4. **Logs. Narrow, don't dump.**

> 4. **日志。要缩小范围，不要整个倒出来。**

   ```
   search_datadog_logs (raw log patterns near the target, set use_log_patterns=true)
   analyze_datadog_logs (SQL-style aggregations, only when you need counts)
   ```

> 上面代码块的中文版：

   ```
   search_datadog_logs（目标附近的原始日志模式，设置 use_log_patterns=true）
   analyze_datadog_logs（SQL 风格的聚合，只在需要计数时用）
   ```

   Search with symbols, error strings, or feature names. **Strongly prefer time-bounded queries** (e.g., 30 days before/after the change).

>    用符号、错误字符串或功能名搜索。**强烈建议用限定时间的查询**（例如改动前后各 30 天）。

5. **APM spans and traces.**

> 5. **APM span 和 trace。**

   ```
   aggregate_spans    (stats: "how often does this endpoint fail?")
   search_datadog_spans (inspect individual spans)
   get_datadog_trace  (a specific trace ID)
   ```

> 上面代码块的中文版：

   ```
   aggregate_spans    （统计：“这个端点多久失败一次？”）
   search_datadog_spans（查看单个 span）
   get_datadog_trace  （某个具体的 trace ID）
   ```

   Useful for timeouts, retries, slow paths, and cross-service behavior.

>    适用于超时、重试、慢路径和跨服务行为。

6. **Incidents.**

> 6. **事故。**

   ```
   search_datadog_incidents (by title, team, date range)
   get_datadog_incident     (full detail for a specific incident)
   ```

> 上面代码块的中文版：

   ```
   search_datadog_incidents（按标题、团队、日期范围）
   get_datadog_incident     （某个具体事故的完整详情）
   ```

   If the target looks defensive, search for incidents around the time it was added. An incident whose timeline includes "added defensive check for X" is near-direct evidence.

>    如果目标看起来是防御性的，就搜索它被加入时前后的事故。时间线里写着“为 X 加了防御性检查”的事故，几乎就是直接证据。

## What good evidence looks like here｜好的证据在这里是什么样

- A monitor whose query and threshold match the constraint the code enforces (code clamps to 100, monitor alerts when requests exceed 100/min)

> - 一个监控，它的查询和阈值与代码所强制的约束吻合（代码限制在 100，监控在请求超过 100/分钟时告警）

- A dashboard created by the target's author, with widgets that correspond to what the code measures or guards against

> - 一个由目标作者创建的仪表盘，里面的组件对应着代码所测量或防范的东西

- A metric showing a production spike immediately before the code was merged, and stable values after

> - 一个指标，显示代码合并前生产环境刚出现过激增，之后数值稳定

- An incident record referencing the target code, the same symbols, or the same error strings

> - 一条引用了目标代码、同样的符号或同样的错误字符串的事故记录

- Logs showing a specific error pattern the defensive code would prevent, timestamped in the window before the change

> - 日志显示出防御性代码会阻止的某种特定错误模式，时间戳落在改动之前的那段时间

## Common pitfalls｜常见陷阱

- **Correlation is not causation.** A spike before a PR and stabilization after is suggestive, not definitive, since other changes may have landed in the same window. Check neighboring PRs.

> - **相关不等于因果。** PR 之前激增、之后稳定只是一个暗示，不是定论，因为同一时间段里可能还落地了别的改动。检查邻近的 PR。

- **Overfitting to the chart you found.** A chart reflects its maker's framing. A chart named "retry success rate" is evidence the team cared about retry success, not that it's why a specific line of code exists.

> - **对你找到的图表过度解读。** 图表反映的是制作者的视角。一个叫“重试成功率”的图表，证明的是团队关心重试成功率，而不是某一行代码存在的原因。

- **Vanished telemetry.** Metrics can be renamed, deleted, or have short retention. If you can't find data from the relevant window, that's a gap, not a null result.

> - **消失的遥测。** 指标可能被改名、删除，或者保留期很短。如果你找不到相关时间段的数据，那是一个缺口，而不是一个“没有”的结果。

- **Noise at scale.** Narrow log searches by service, tag, and time window aggressively. Use `analyze_datadog_logs` to aggregate rather than dumping raw logs.

> - **规模带来的噪音。** 按服务、标签和时间窗口大力缩小日志搜索范围。用 `analyze_datadog_logs` 做聚合，而不是倒出原始日志。

- **Instrumented != caused.** A metric shows someone cared enough to measure something, not that the code was added *because* of it. Cross-reference with commit/PR dates.

> - **有埋点不等于是原因。** 一个指标说明有人在意到愿意去测量某样东西，而不是说明代码是*因为*它才加的。与提交/PR 日期交叉对照。

## What to return｜返回什么

For each relevant item:

> 对每个相关的条目：

- Type (dashboard / monitor / metric / log pattern / trace / incident / notebook)

> - 类型（仪表盘 / 监控 / 指标 / 日志模式 / trace / 事故 / 笔记本）

- Title or name

> - 标题或名称

- Link or identifier (dashboard ID, monitor ID, metric name, incident ID)

> - 链接或标识（仪表盘 ID、监控 ID、指标名、事故 ID）

- Owner/author and created/modified date

> - 所有者/作者和创建/修改日期

- The specific condition, query, or quote that bears on the question (verbatim where possible)

> - 与问题相关的具体条件、查询或引用（尽可能逐字）

- Relevance: what this suggests about the target code, and how strong the connection is

> - 相关性：它对目标代码说明了什么，以及这种联系有多强
