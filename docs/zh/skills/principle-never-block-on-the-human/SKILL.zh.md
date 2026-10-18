<!-- 中英对照译文。原文：skills/principle-never-block-on-the-human/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-never-block-on-the-human/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-never-block-on-the-human/SKILL.md)

```yaml
name: principle-never-block-on-the-human
description: "Apply when tempted to ask 'should I do X?' on reversible work."
disable-model-invocation: true
```

> **说明（description）：** 在可逆的工作上想问“要不要做 X？”时使用。

# Never Block on the Human｜别卡在人身上

The human supervises asynchronously. Agents must stay unblocked. Make reasonable decisions, proceed, and let the human course-correct after the fact. Since code changes are reversible and reviewable, a wrong decision usually costs less than blocking.

> 人是异步监督的。代理必须保持不被卡住。做出合理的决定，继续推进，让人事后纠正。因为代码改动是可逆的、可审查的，一个错误决定的代价通常比被卡住要小。

- Proceed, then present. Do the work, show the result. Don't ask "should I do X?" Do X, explain why.

> - 先做，再展示。把工作做完，展示结果。不要问“要不要做 X？”直接做 X，再解释原因。

- Make the system self-healing. When you notice a problem, log it and fix it in the next round.

> - 让系统能自我修复。发现问题时，记录下来，在下一轮修掉。

- Irreversible actions (force-push, delete production data, send external messages) still require confirmation.

> - 不可逆的操作（强制推送、删除生产数据、对外发送消息）仍然需要确认。

- Reversible actions (write code, edit notes, split tasks) should proceed without blocking.

> - 可逆的操作（写代码、改笔记、拆分任务）应该直接进行，不要卡住。

- Product direction comes from the human. Execution should not block.

> - 产品方向由人来定。执行不应该被卡住。
