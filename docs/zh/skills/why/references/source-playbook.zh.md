<!-- 中英对照译文。原文：skills/why/references/source-playbook.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/source-playbook.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/source-playbook.md)

# Source playbooks｜各信息源的操作手册

The why skill spawns one investigator per available evidence category, each reading a single source-specific playbook below. The playbooks are concrete examples for common MCPs. Adapt them for a different MCP in the same category.

> why 技能为每一类可用的证据派生一个调查者，每个调查者读下面一份针对特定信息源的操作手册。这些手册是常见 MCP 的具体示例。遇到同一类别里的其他 MCP 时，照着改编。

| Category | Playbook | Example MCP it documents |
|---|---|---|
| Source control history | [`code-archaeology.md`](./sources/code-archaeology.zh.md) | git, `gh` |
| Issue / ticket tracker | [`linear.md`](./sources/linear.zh.md) | Linear (adapt for Jira, GitHub Issues, Plane, Shortcut) |
| Long-form documents | [`notion.md`](./sources/notion.zh.md) | Notion (adapt for Confluence, Google Docs, Coda) |
| Real-time team chat | [`slack.md`](./sources/slack.zh.md) | Slack (adapt for Discord, Microsoft Teams, Mattermost) |
| Infrastructure observability | [`datadog.md`](./sources/datadog.zh.md) | Datadog (adapt for New Relic, Honeycomb, Grafana, Splunk) |
| Error / exception tracking | [`sentry.md`](./sources/sentry.zh.md) | Sentry (adapt for Rollbar, Bugsnag, Airbrake) |
| Product analytics warehouse | [`databricks.md`](./sources/databricks.zh.md) | Databricks SQL (adapt for Snowflake, BigQuery, ClickHouse, dbt) |

> | 类别 | 操作手册 | 它记录的示例 MCP |
> |---|---|---|
> | 源代码版本历史 | [`code-archaeology.md`](./sources/code-archaeology.zh.md) | git、`gh` |
> | 问题 / 工单跟踪 | [`linear.md`](./sources/linear.zh.md) | Linear（可改编用于 Jira、GitHub Issues、Plane、Shortcut） |
> | 长篇文档 | [`notion.md`](./sources/notion.zh.md) | Notion（可改编用于 Confluence、Google Docs、Coda） |
> | 实时团队聊天 | [`slack.md`](./sources/slack.zh.md) | Slack（可改编用于 Discord、Microsoft Teams、Mattermost） |
> | 基础设施可观测性 | [`datadog.md`](./sources/datadog.zh.md) | Datadog（可改编用于 New Relic、Honeycomb、Grafana、Splunk） |
> | 错误 / 异常跟踪 | [`sentry.md`](./sources/sentry.zh.md) | Sentry（可改编用于 Rollbar、Bugsnag、Airbrake） |
> | 产品分析数据仓库 | [`databricks.md`](./sources/databricks.zh.md) | Databricks SQL（可改编用于 Snowflake、BigQuery、ClickHouse、dbt） |

Cross-cutting:

> 跨类别的：

- [`incident-postmortem.md`](./sources/incident-postmortem.zh.md). Add this if the target code looks defensive (null checks, retry, timeout, rate limit, feature flag, egress guard, OOM handler).

> - [`incident-postmortem.md`](./sources/incident-postmortem.zh.md)。如果目标代码看起来是防御性的（空值检查、重试、超时、限流、功能开关、出口防护、内存溢出处理），就加上这一份。
