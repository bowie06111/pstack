<!-- 中英对照译文。原文：skills/why/references/sources/incident-postmortem.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/incident-postmortem.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/incident-postmortem.md)

# Incident & Postmortem Context｜事故与复盘背景

- **Linear**: look for tickets labeled `incident`, `sev-*`, `postmortem-action-item`, `reliability`

> - **Linear**：找带有 `incident`、`sev-*`、`postmortem-action-item`、`reliability` 标签的工单

- **Slack**: search `#sev-*` and `#incident-*` channels around the dates the target code was added

> - **Slack**：在目标代码被加入的日期前后，搜索 `#sev-*` 和 `#incident-*` 频道

- **Git**: commits with messages like "fix for incident", "add defensive check", "revert" followed by "re-apply with..." are strong signals

> - **Git**：提交信息像“fix for incident”“add defensive check”，或者“revert”后面跟着“re-apply with...”的，都是强信号
