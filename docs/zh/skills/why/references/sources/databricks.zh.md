<!-- 中英对照译文。原文：skills/why/references/sources/databricks.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/databricks.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/databricks.md)

# Databricks Analytics & System Tables｜Databricks 分析与系统表

## What this source contains｜这个信息源包含什么

Databricks is the product-analytics, data-pipeline, and warehouse-telemetry layer. It complements Datadog. Datadog is the *infra/runtime* view, Databricks is the *product/data* view (what users did, which experiments ran, how feature usage evolved, where a threshold constant came from).

> Databricks 是产品分析、数据管道和数据仓库遥测这一层。它与 Datadog 互补。Datadog 是*基础设施/运行时*视角，Databricks 是*产品/数据*视角（用户做了什么、跑了哪些实验、功能使用情况如何演变、某个阈值常量从哪里来）。

- **Product analytics events.** `your_warehouse.events.analytics_track_event` (raw) and typed, deduplicated per-event dbt models in `<your_analytics_db>.<schema>.<table>`. User behavior: feature invocations, clicks, accepts/rejects, submissions, client-reported errors.

> - **产品分析事件。** `your_warehouse.events.analytics_track_event`（原始的），以及 `<your_analytics_db>.<schema>.<table>` 里有类型、按事件去重的 dbt 模型。用户行为：功能调用、点击、接受/拒绝、提交、客户端报告的错误。

- **Usage & billing events.** `your_warehouse.events.usage_event` / `<your_analytics_db>.<schema>.stg_usage_events`, `your_warehouse.events.raw_model_event` / `<your_analytics_db>.<schema>.stg_raw_model_events`. For cost- or volume-driven decisions.

> - **使用量和计费事件。** `your_warehouse.events.usage_event` / `<your_analytics_db>.<schema>.stg_usage_events`、`your_warehouse.events.raw_model_event` / `<your_analytics_db>.<schema>.stg_raw_model_events`。用于由成本或用量驱动的决定。

- **Experiment / feature-flag data.** Exposure and outcome tables. **Schema is company-specific.** Probe with `SHOW TABLES` before assuming names.

> - **实验 / 功能开关数据。** 曝光和结果表。**表结构因公司而异。** 在假定表名之前，先用 `SHOW TABLES` 探查。

- **System tables.** `system.query.history`, `system.compute.warehouses`, `system.billing.*`, `system.access.audit`. Answer "was this query expensive?", "how often did anyone run this?", "when did warehouse load spike?"

> - **系统表。** `system.query.history`、`system.compute.warehouses`、`system.billing.*`、`system.access.audit`。回答“这个查询贵不贵？”“有多少人跑过这个？”“数据仓库负载什么时候激增？”

- **dbt lineage.** Models in `<your_analytics_db>.<schema>` reveal what pipelines depend on a table/field. Upstream changes frequently motivate consumer-code changes.

> - **dbt 血缘。** `<your_analytics_db>.<schema>` 里的模型揭示了哪些管道依赖某张表/某个字段。上游的变化经常促成使用方代码的变化。

- **Databricks notebooks.** Exploratory analyses engineers wrote before code changes. **Not queryable via the SQL MCP.** If you suspect the rationale lives in a notebook, name it as a gap.

> - **Databricks 笔记本。** 工程师在改代码之前写的探索性分析。**无法通过 SQL MCP 查询。** 如果你怀疑理由在某个笔记本里，就把它记为一个缺口。

## How to search it｜怎么搜索

Use the Databricks SQL MCP. Primary tool: `execute_sql_read_only`. If it returns a `statement_id`, poll with `poll_sql_result` rather than re-running.

> 使用 Databricks SQL 的 MCP。主要工具：`execute_sql_read_only`。如果它返回一个 `statement_id`，就用 `poll_sql_result` 轮询，而不是重新运行。

**Orient before querying.** Schemas are company-specific. Probe before trusting a table name:

> **查询之前先定位。** 表结构因公司而异。在相信一个表名之前先探查：

```sql
SHOW TABLES IN <your_analytics_db>.<schema> LIKE '*<keyword>*';
DESCRIBE TABLE <your_analytics_db>.<schema>.stg_<event>;
```

**Time-bound every query.** These tables are huge and unconstrained scans time out. Filter on `_timestamp` (events) or `start_time` (`system.query.history`) with a window bracketing the ship date, typically ~30 days before and after, wider only for strong reason.

> **每个查询都要限定时间。** 这些表非常大，不加限制的扫描会超时。用一个包住发布日期的时间窗口过滤 `_timestamp`（事件）或 `start_time`（`system.query.history`），通常是前后各约 30 天，只有在有充分理由时才放宽。

**Prefer typed dbt models over the raw table.** `<your_analytics_db>.<schema>.<table>` is deduplicated, typed, and liquid-clustered. `your_warehouse.events.analytics_track_event` has duplicates and untyped `properties_json`. Model-name pattern: `stg_<source>_<event_name_with_underscores>`, where `<source>` is `app`, `backend`, `website`, or `cli`. Confirm the exact model name with `SHOW TABLES` when the pattern alone doesn't resolve it. Drop to the raw table only when there's no dbt model yet, or you need events from inside the dbt refresh lag.

> **优先用有类型的 dbt 模型，而不是原始表。** `<your_analytics_db>.<schema>.<table>` 是去重过、有类型、做了 liquid clustering 的。`your_warehouse.events.analytics_track_event` 有重复，而且 `properties_json` 没有类型。模型名的模式：`stg_<source>_<用下划线连接的事件名>`，其中 `<source>` 是 `app`、`backend`、`website` 或 `cli`。光靠模式确定不了时，用 `SHOW TABLES` 确认确切的模型名。只有在还没有 dbt 模型，或者你需要 dbt 刷新延迟期内的事件时，才退回到原始表。

**Column conventions on the typed dbt models** (knowing these avoids a `DESCRIBE` round-trip):

> **有类型的 dbt 模型上的列约定**（知道这些可以省掉一次 `DESCRIBE`）：

- `_timestamp`, `_id`, `_auth_id`, `_request_id`, `event_name`. Standard on every model

> - `_timestamp`、`_id`、`_auth_id`、`_request_id`、`event_name`。每个模型都有

- `properties_<name>`. Typed, underscore-cased event properties (`properties_entrypoint`, `properties_size_bytes`, …)

> - `properties_<name>`。有类型、用下划线命名的事件属性（`properties_entrypoint`、`properties_size_bytes`……）

- `context_team_id`, `context_client_version`, `context_country`, `context_client_os`. Pre-extracted client context

> - `context_team_id`、`context_client_version`、`context_country`、`context_client_os`。预先提取好的客户端上下文

### Investigation patterns that tend to pay off｜往往有收获的调查模式

Pick the table + column combination that matches the target:

> 选择与目标匹配的表和列的组合：

1. **Event usage trajectory.** Daily counts on the relevant `stg_*` model across a ±30d window around the PR merge. A step function from zero to steady volume within a day or two of the merge is strong circumstantial evidence the PR launched the feature. A decay to zero suggests a deprecation or deletion.

> 1. **事件使用量走势。** 在相关的 `stg_*` 模型上，看 PR 合并前后 ±30 天窗口里的每日计数。在合并后一两天之内从零跳到稳定的量，是很强的旁证，说明这个 PR 上线了这个功能。衰减到零则暗示弃用或删除。

2. **Guard-rail / defensive-check origin.** Distribution (median / p99 / max) of the relevant `properties_<name>` column in the 14 days *before* the PR. A p99 that matches the target's threshold constant suggests the number was chosen from data.

> 2. **护栏 / 防御性检查的起源。** 看 PR 之前 14 天里相关 `properties_<name>` 列的分布（中位数 / p99 / 最大值）。如果 p99 与目标里的阈值常量吻合，暗示这个数字是根据数据选出来的。

3. **Experiment / feature-flag lookup.** `SHOW TABLES ... LIKE '*experiment*'` to find the exposure table, then pull exposure counts by variant for the relevant flag key near the PR date.

> 3. **实验 / 功能开关查询。** 用 `SHOW TABLES ... LIKE '*experiment*'` 找到曝光表，然后拉出 PR 日期附近相关开关键在各个变体上的曝光数。

4. **Query-history evidence for migrations, backfills, or perf rewrites.** `system.query.history` filtered by `statement_text ILIKE '%<table_or_symbol>%'` with a tight `start_time` window surfaces the expensive queries that likely motivated the change (sort by `total_duration_ms` or aggregate `SUM(read_bytes)`, `COUNT(*)`).

> 4. **为迁移、回填或性能重写找查询历史证据。** 用 `statement_text ILIKE '%<表名或符号>%'` 加上一个窄的 `start_time` 窗口过滤 `system.query.history`，能找出很可能促成这次改动的昂贵查询（按 `total_duration_ms` 排序，或者聚合 `SUM(read_bytes)`、`COUNT(*)`）。

5. **dbt lineage.** If the target reads from or writes into a `<your_analytics_db>.<schema>` model, the model's own git history (in this repo) often carries the rationale. Hand that lead back to the git investigator rather than chasing it yourself.

> 5. **dbt 血缘。** 如果目标从某个 `<your_analytics_db>.<schema>` 模型读取或写入，那个模型自己的 git 历史（在本仓库里）经常带着理由。把这条线索交回给 git 调查者，而不是自己去追。

## What good evidence looks like here｜好的证据在这里是什么样

Beyond the pattern shapes above:

> 除了上面那些模式之外：

- An error-classifying event's count drops to near zero in the days after a defensive-code PR. Suggests the PR resolved that error class

> - 一个对错误分类的事件，在一个防御性代码 PR 之后的几天里计数降到接近零。暗示这个 PR 解决了那一类错误

- An exposure table row names the target's feature-flag key with a "shipped" / "concluded" decision around the PR ship date

> - 一行曝光表记录点名了目标的功能开关键，并在 PR 发布日期前后给出了“已发布”/“已结束”的决定

## Common pitfalls｜常见陷阱

- **Instrumented ≠ caused.** An event's existence means someone cared enough to log it, not that the target code exists *because* of it. Pair with a PR/commit citation from the git investigator before claiming causation.

> - **有埋点 ≠ 是原因。** 一个事件存在，意味着有人在意到愿意记录它，而不是说目标代码是*因为*它才存在的。在断言因果之前，配上 git 调查者给出的 PR/提交出处。

- **Silent instrumentation changes.** A step function in event volume may mean a new event started being logged, not that user behavior changed. Check for instrumentation PRs in the same window before reading the ramp as a feature-launch signal.

> - **悄无声息的埋点变化。** 事件量出现阶跃，可能意味着开始记录一个新事件，而不是用户行为变了。在把这个爬升解读为功能上线信号之前，检查同一时间段里有没有埋点相关的 PR。

- **Schema drift.** Event properties evolve. A column on the typed dbt model today may not have existed when the target was written. Older data may carry the property only inside raw `properties_json`.

> - **表结构漂移。** 事件属性会演变。今天有类型的 dbt 模型上的某一列，在目标代码被写的时候可能还不存在。较早的数据可能只在原始的 `properties_json` 里带着这个属性。

- **dbt refresh lag.** `<your_analytics_db>.<schema>.*` is rebuilt on a schedule (often hourly/daily). For events from the last few hours, fall back to `your_warehouse.events.*` and deduplicate by `_id`.

> - **dbt 刷新延迟。** `<your_analytics_db>.<schema>.*` 是按计划重建的（通常每小时/每天）。对于最近几小时的事件，退回到 `your_warehouse.events.*`，并按 `_id` 去重。

- **Company-specific tables.** Experiment, feature-flag, billing, and usage tables vary. Reporting a result from a table whose existence you never confirmed is a classic failure mode. Probe with `SHOW TABLES` / `DESCRIBE TABLE` first.

> - **因公司而异的表。** 实验、功能开关、计费和使用量表各不相同。从一张你从没确认过存在的表报告结果，是一种典型的失败模式。先用 `SHOW TABLES` / `DESCRIBE TABLE` 探查。

- **Retention cliff.** If the relevant window predates the table's retention or the dbt model's creation date, that's a *gap*, not a null result. Name it explicitly so the synthesizer doesn't read "no results" as "no activity."

> - **保留期断崖。** 如果相关的时间窗口早于表的保留期或 dbt 模型的创建日期，那是一个*缺口*，而不是一个“没有”的结果。明确说出来，以免综合者把“没有结果”读成“没有活动”。

- **Notebooks aren't queryable.** The SQL MCP can't see Databricks notebooks. If you suspect the rationale lives in one, return a gap.

> - **笔记本无法查询。** SQL MCP 看不到 Databricks 笔记本。如果你怀疑理由在某个笔记本里，就返回一个缺口。

## What to return｜返回什么

For each relevant finding:

> 对每个相关的发现：

- Type (product event / experiment exposure / usage or billing event / system-table row / dbt model)

> - 类型（产品事件 / 实验曝光 / 使用量或计费事件 / 系统表记录 / dbt 模型）

- Fully-qualified table name and the exact query you ran

> - 完整的表名，以及你运行的确切查询

- Time window queried

> - 查询的时间窗口

- Compact numeric summary (counts, percentiles, first/last-seen timestamps). **Don't dump raw rows.**

> - 紧凑的数字摘要（计数、百分位数、首次/最后出现的时间戳）。**不要倒出原始行。**

- Temporal correlation with the target's ship date (e.g., "first row 2024-08-15, PR #49074 merged 2024-08-14")

> - 与目标发布日期的时间关联（例如，“第一行出现在 2024-08-15，PR #49074 于 2024-08-14 合并”）

- Relevance + strength: direct / circumstantial / weak

> - 相关性和强度：直接 / 旁证 / 弱
