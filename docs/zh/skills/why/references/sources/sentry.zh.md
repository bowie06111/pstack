<!-- 中英对照译文。原文：skills/why/references/sources/sentry.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/sentry.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/sentry.md)

# Sentry Error History｜Sentry 错误历史

## How to search it｜怎么搜索

Use the Sentry MCP.

> 使用 Sentry 的 MCP。

1. **Orient.** If you don't know the project slug and organization:

> 1. **定位。** 如果你不知道项目 slug 和组织：

   ```
   find_organizations
   find_projects
   ```

2. **Search for issues related to the target.**

> 2. **搜索与目标相关的问题。**

   ```
   search_issues (natural language, e.g., "errors in PaymentService timeout", "unhandled exceptions in uploadFile")
   ```

> 上面代码块的中文版：

   ```
   search_issues（自然语言，例如 "errors in PaymentService timeout"、"unhandled exceptions in uploadFile"）
   ```

   Build queries from the exception class names the target handles, its function or class name, the error message strings it checks for, and its file path.

>    用目标处理的异常类名、它的函数名或类名、它检查的错误信息字符串，以及它的文件路径来构造查询。

3. **Narrow by release and time window.**

> 3. **按版本和时间窗口缩小范围。**

   ```
   search_issue_events (filter by release, time, environment, trace ID, tags)
   get_issue_tag_values (for an issue, see distribution across versions, users, environments)
   ```

> 上面代码块的中文版：

   ```
   search_issue_events（按版本、时间、环境、trace ID、标签过滤）
   get_issue_tag_values（针对某个问题，查看它在各版本、各用户、各环境上的分布）
   ```

   For a suspected issue, check **first seen** and **last seen** (does the stop line up with the target's ship date?), the **affected releases** (which saw it, and which was the fix?), and the **frequency trajectory** (did it spike, then get resolved?).

>    对一个可疑的问题，检查 **首次出现** 和 **最后出现**（它停止出现的时间和目标的发布日期吻合吗？）、**受影响的版本**（哪些版本出现过，哪个是修复版本？），以及 **频率走势**（是不是先激增、然后被解决了？）。

4. **Pull the full event for context.**

> 4. **拉取完整事件以获得上下文。**

   ```
   get_sentry_resource (pass a Sentry URL or type+ID)
   ```

   Does the stack trace pass through the target code? Do the tags and breadcrumbs match the conditions the target defends against?

>    堆栈跟踪是否经过目标代码？标签和面包屑是否符合目标所防御的条件？

5. **Check releases that landed near the target.**

> 5. **检查在目标附近落地的版本。**

   ```
   find_releases (around the commit date of the target)
   ```

> 上面代码块的中文版：

   ```
   find_releases（在目标的提交日期前后）
   ```

   Cross-reference release version with the PR's merge date.

>    把版本号和 PR 的合并日期对照起来。

6. **Use Seer sparingly.**

> 6. **谨慎使用 Seer。**

   ```
   analyze_issue_with_seer
   ```

   Seer's AI root-cause analyses are a hypothesis generator. Treat them as inference, not authoritative. The actual events and stack traces are the primary evidence.

>    Seer 的 AI 根因分析是假设生成器。把它们当作推断，而不是权威。真实的事件和堆栈跟踪才是主要证据。

## What to return｜返回什么

For each relevant issue:

> 对每个相关的问题：

- Issue ID and title

> - 问题编号和标题

- Project and organization

> - 项目和组织

- First seen / last seen timestamps

> - 首次出现 / 最后出现的时间戳

- Event count (and sampling rate if known)

> - 事件数量（如果知道的话，还有采样率）

- Affected releases

> - 受影响的版本

- A representative stack trace snippet showing relevance to the target (verbatim excerpt, not summary)

> - 一段能显示与目标相关的代表性堆栈跟踪片段（原文摘录，而不是总结）

- First/last-seen correlation with the target's ship date

> - 首次/最后出现时间与目标发布日期的关联

- Link to the issue

> - 问题链接

- Any author comments or resolution notes

> - 任何作者评论或解决说明
