<!-- 中英对照译文。原文：skills/why/references/sources/slack.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/slack.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/slack.md)

# Slack Conversations｜Slack 对话

## How to search it｜怎么搜索

Slack MCP tools vary. Check which Slack MCP is available and inspect its tool schema first. It may require `mcp_auth`. If authentication fails, stop and report the gap.

> Slack 的 MCP 工具各不相同。先检查有哪个 Slack MCP 可用，并查看它的工具参数结构。它可能需要 `mcp_auth`。如果认证失败，停下并报告这个缺口。

1. **Author-bounded search.** Messages from the PR author around the PR merge date. Limits scope dramatically and often hits gold.

> 1. **按作者限定搜索。** 在 PR 合并日期前后，搜 PR 作者发的消息。能极大地缩小范围，而且经常挖到宝。

3. **PR URL search.** Slack often links PRs when they're reviewed or discussed. Search for the PR URL (or just `/pull/<number>`).

> 3. **搜索 PR 链接。** Slack 里在评审或讨论 PR 时经常贴出链接。搜索 PR 的 URL（或者只搜 `/pull/<编号>`）。

4. **Error string search.** If the code handles a specific error, search for the error string. Incident threads often surface.

> 4. **搜索错误字符串。** 如果代码处理某个特定错误，就搜索那个错误字符串。经常能找到事故讨论串。

## Common pitfalls｜常见陷阱

- **Channel archaeology limits.** Very old messages may be gone due to retention policies. If you can't find anything before a certain date, note the retention cliff.

> - **频道考古的局限。** 很老的消息可能因为保留策略已经没了。如果某个日期之前什么都找不到，记下这个保留期的断崖。

- **Unsearched DMs.** Many decisions happen in DMs that aren't searchable. You'll miss them. That's a known limitation.

> - **搜不到的私信。** 很多决定是在私信里做的，而私信搜不到。你会错过它们。这是已知的局限。

- **Speculative jokes as "decisions."** Slack is casual. "Lol just do the thing" isn't a decision, even if it preceded the commit. Look for considered discussion.

> - **把玩笑当成“决定”。** Slack 很随意。“哈哈，就那么干吧”不是一个决定，即使它出现在提交之前。去找经过考虑的讨论。

- **Context collapse in single messages.** Without the thread, a single message often reads differently than in context. Always fetch threads.

> - **单条消息里的语境丢失。** 脱离讨论串，单条消息读起来常常和在语境中不一样。始终要把整个讨论串拉下来。

- **Auth failures.** If the MCP isn't authenticated, stop. Don't make up findings. Report that Slack wasn't searchable.

> - **认证失败。** 如果 MCP 没有认证，就停下。不要编造发现。报告 Slack 无法搜索。
