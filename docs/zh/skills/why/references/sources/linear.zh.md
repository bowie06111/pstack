<!-- 中英对照译文。原文：skills/why/references/sources/linear.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/linear.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/linear.md)

# Linear Tickets｜Linear 工单

## What this source contains｜这个信息源包含什么

- Issues describing features, bugs, and their motivation

> - 描述功能、bug 及其动机的问题

- Project docs attached to issues (often PRDs or specs)

> - 附在问题上的项目文档（通常是 PRD 或规格）

- Parent/sub-issue relationships (broader initiative → specific tickets)

> - 父问题/子问题的关系（更大的计划 → 具体的工单）

- Comments on issues (clarifications, scope changes, "why we're doing this" rationale)

> - 问题下的评论（澄清、范围变化、“我们为什么做这个”的理由）

- Labels (e.g., `compliance`, `customer-request`, `perf`) that signal the type of motivation

> - 表明动机类型的标签（例如 `compliance`、`customer-request`、`perf`）

- Status updates that explain scope changes

> - 解释范围变化的状态更新

- Attachments and linked GitHub PRs

> - 附件和关联的 GitHub PR

Linear is where the product/business context often lives: the "we're doing this because customer X asked" or "this is for the Q3 compliance initiative" layer.

> Linear 通常是产品/业务背景所在的地方：也就是“我们做这个是因为客户 X 提了要求”或“这是第三季度合规计划的一部分”这一层。

## How to search it｜怎么搜索

Use the Linear MCP.

> 使用 Linear 的 MCP。

1. **Start with linked tickets.** If the seed commits or PRs reference ticket IDs (e.g., `ENG-1234`, `[BUG-567]`), fetch those first with `get_issue`. Read the full issue including comments.

> 1. **从关联的工单开始。** 如果种子提交或 PR 引用了工单编号（例如 `ENG-1234`、`[BUG-567]`），先用 `get_issue` 拉取它们。读完整个问题，包括评论。

2. **List related issues by keyword.** Use `list_issues` with text search for the feature name, key symbol, or business term. Try multiple phrasings.

> 2. **按关键词列出相关问题。** 用 `list_issues` 对功能名、关键符号或业务术语做文本搜索。多换几种说法试试。

3. **Walk the issue tree.** If you land on a sub-issue, fetch its parent. Sub-issues are tactical. Parents often carry the "why."

> 3. **沿问题树走。** 如果你落在一个子问题上，就拉取它的父问题。子问题是战术性的。父问题经常承载着“为什么”。

4. **Read project docs.** If the issue belongs to a project, use `get_project` and check attached docs. Project-level documents are where specs and rationale are most often captured.

> 4. **读项目文档。** 如果问题属于某个项目，用 `get_project` 并检查附带的文档。规格和理由最常被记录在项目级的文档里。

5. **Check labels and milestones.** Labels hint at the category of motivation (customer-request, incident-followup, compliance). Milestones tie work to deadlines, which often reveal motivation.

> 5. **检查标签和里程碑。** 标签暗示动机的类别（客户需求、事故后续、合规）。里程碑把工作和截止日期绑在一起，这常常能揭示动机。

## What good evidence looks like here｜好的证据在这里是什么样

- An issue description stating the business problem: "Customer Acme needs X because of their SOC2 audit"

> - 一个说明业务问题的问题描述：“客户 Acme 因为 SOC2 审计需要 X”

- A comment recording a decision: "We decided to go with approach B because approach A would require touching the billing service"

> - 一条记录了决定的评论：“我们决定用方案 B，因为方案 A 需要改动计费服务”

- A parent issue titled like an initiative: "Q3 Enterprise Readiness" or "Reduce Payment Failures"

> - 一个标题像计划名的父问题：“第三季度企业就绪”或“减少支付失败”

- An attached PRD or spec

> - 一份附带的 PRD 或规格

- Labels like `customer:acme`, `incident-followup`, `compliance`, `perf-regression`

> - 像 `customer:acme`、`incident-followup`、`compliance`、`perf-regression` 这样的标签

## Common pitfalls｜常见陷阱

- **Scope drift.** The ticket the PR references may have been closed and reopened with a different scope. Read the whole history.

> - **范围漂移。** PR 引用的工单可能被关闭后又以不同的范围重新打开。读完整的历史。

- **Mechanical templates.** Some teams require "Why" sections but fill them with boilerplate. Generic text ("improve user experience") is probably not a real answer.

> - **机械的模板。** 有些团队要求写“为什么”一节，却用套话填满。泛泛的文字（“改善用户体验”）大概不是真正的答案。

- **Stale tickets.** Old tickets often reflect a version of the plan that changed. Check dates and cross-reference with the code's ship date.

> - **过时的工单。** 旧工单经常反映的是一个已经变了的计划版本。检查日期，并与代码的发布日期交叉对照。

- **Closed-as-duplicate chains.** Follow the duplicate-of relationships back to the canonical ticket.

> - **作为重复关闭的链条。** 顺着“重复于”的关系回溯到规范的那张工单。

- **Private workspace content.** If you can't access an issue, note that as a gap rather than guessing.

> - **私有工作区的内容。** 如果你访问不了某个问题，把它记为一个缺口，而不是去猜。

## What to return｜返回什么

For each relevant ticket:

> 对每张相关的工单：

- Ticket ID and title

> - 工单编号和标题

- The problem/motivation quoted from the description or comments (not paraphrased. The synthesizer needs the exact text to cite)

> - 从描述或评论里引用的问题/动机（不要转述。综合者需要确切的原文来引用）

- Labels, parent issue, project

> - 标签、父问题、项目

- Author, created date, closed date

> - 作者、创建日期、关闭日期

- Link to the ticket if available

> - 如果有的话，工单链接
