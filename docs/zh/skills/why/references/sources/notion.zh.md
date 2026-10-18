<!-- 中英对照译文。原文：skills/why/references/sources/notion.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/notion.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/notion.md)

# Notion Docs｜Notion 文档

## What this source contains｜这个信息源包含什么

- PRDs (product requirement documents)

> - PRD（产品需求文档）

- Technical specs and RFCs

> - 技术规格和 RFC

- Architectural decision records (ADRs)

> - 架构决策记录（ADR）

- Meeting notes from design reviews

> - 设计评审的会议记录

- Team pages with domain context

> - 带有领域背景的团队页面

- Postmortems from incidents

> - 事故复盘

- Runbooks that may explain defensive code

> - 可能解释防御性代码的运维手册

- Strategy documents that set priorities

> - 设定优先级的战略文档

Notion is where "why" often lives in long-form before it becomes code. A significant feature usually has a doc.

> 在“为什么”变成代码之前，它经常以长文的形式住在 Notion 里。一个重要的功能通常都有一份文档。

## How to search it｜怎么搜索

Use the Notion MCP.

> 使用 Notion 的 MCP。

1. **Keyword searches with `notion-search`.** Try:
   - The feature name
   - Key symbols / class names from the target code
   - Author handles (design docs are often authored before the code lands)
   - Error strings or user-visible terms
   - Time-bounded queries if you know when the code shipped

> 1. **用 `notion-search` 做关键词搜索。** 试试：
>    - 功能名
>    - 目标代码里的关键符号 / 类名
>    - 作者的账号名（设计文档通常在代码落地之前就写好了）
>    - 错误字符串或用户可见的术语
>    - 如果你知道代码是什么时候发布的，就用限定时间的查询

2. **Fetch candidate pages with `notion-fetch`.** Read the full content, not the preview. Rationale is often buried mid-document.

> 2. **用 `notion-fetch` 拉取候选页面。** 读完整内容，而不是预览。理由经常埋在文档中间。

3. **Follow backlinks and child pages.** Design docs often have sub-pages for alternatives considered, appendices, or implementation notes.

> 3. **顺着反向链接和子页面走。** 设计文档经常有子页面，放考虑过的备选方案、附录或实现笔记。

4. **Check related databases.** `notion-query-data-sources` and `notion-query-meeting-notes` can surface meeting notes that discussed the decision.

> 4. **检查相关的数据库。** `notion-query-data-sources` 和 `notion-query-meeting-notes` 能找出讨论过这个决定的会议记录。

5. **Search author-specific spaces.** If the PR author has a personal notebook (common at some companies), it may hold exploratory thinking that preceded the code.

> 5. **搜索作者的个人空间。** 如果 PR 作者有个人笔记本（在一些公司很常见），里面可能有代码之前的探索性思考。

## What good evidence looks like here｜好的证据在这里是什么样

- A PRD with a "Problem statement" or "Motivation" section that matches the target code's purpose

> - 一份 PRD，其中“问题陈述”或“动机”一节与目标代码的用途吻合

- An "Alternatives considered" or "Rejected approaches" section

> - 一节“考虑过的备选方案”或“被否决的做法”

- A postmortem that names the target code as the fix for a specific incident

> - 一份复盘，点名目标代码是某次具体事故的修复

- Meeting notes that record "we decided X because Y" and tie to the same author/date range as the PR

> - 记录了“我们决定 X，因为 Y”的会议记录，并且与 PR 的作者和日期范围对得上

- An ADR template filled out non-trivially (status, context, decision, consequences)

> - 一份认真填写过的 ADR 模板（状态、背景、决定、后果）

## Common pitfalls｜常见陷阱

- **Outdated docs.** Specs are often written before implementation and not updated. The doc may describe a plan that changed. Cross-check against the actual PR.

> - **过时的文档。** 规格常常在实现之前写好就不再更新。文档描述的可能是一个已经变了的计划。和实际的 PR 交叉核对。

- **Doc vs. reality drift.** A spec may say "we'll do X" but the code actually does Y. Flag the divergence. The synthesizer will surface the contradiction.

> - **文档与现实偏离。** 规格可能说“我们会做 X”，但代码实际做的是 Y。标出这个分歧。综合者会把这个矛盾摆出来。

- **Boilerplate templates.** Some orgs require a "Why" section that gets filled with fluff. Look for specificity.

> - **套话模板。** 有些组织要求写一节“为什么”，结果填满了空话。要找具体的内容。

- **Unlinked docs.** The most relevant doc may not be linked from anywhere. Broad keyword searches help.

> - **没被链接的文档。** 最相关的文档可能哪里都没有链接到。宽泛的关键词搜索有帮助。

- **Multiple drafts.** If a topic has multiple docs, find the one that was finalized or most recently updated. Check dates.

> - **多份草稿。** 如果一个主题有多份文档，找到最终定稿或最近更新的那份。检查日期。

- **Access-restricted pages.** If you can't access a page, note it as a gap.

> - **有访问限制的页面。** 如果你访问不了某个页面，把它记为一个缺口。

## What to return｜返回什么

For each relevant doc:

> 对每份相关的文档：

- Title and URL

> - 标题和 URL

- Authors and last-updated date

> - 作者和最后更新日期

- The motivation text (verbatim quote), with page/section location

> - 动机的原文（逐字引用），附上页面/小节位置

- Relevant linked pages (so the synthesizer can cite them)

> - 相关的链接页面（以便综合者引用）

- Whether the doc was finalized or draft

> - 文档是定稿还是草稿
