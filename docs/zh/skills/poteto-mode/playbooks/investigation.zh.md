<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/investigation.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/investigation.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/investigation.md)

### Investigation｜调查

**You own the answer. Plan, route, write.**

> **答案由你负责。规划、分派、撰写。**

Investigation requests are read-only. They produce a cited explanation or a recommendation, not a code change.

> 调查请求是只读的。它产出一个带引用的解释或一个建议，而不是代码改动。

1. Route through the **how** skill. For motivation questions, also route through the **why** skill.

> 1. 通过 **how** 技能处理。关于动机的问题，还要通过 **why** 技能处理。

2. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only investigation`.

> 2. 吞吐量检查点只写一行：`throughput checkpoint: n/a, read-only investigation`。

3. Produce the `how`-shaped output (Overview / Key Concepts / How It Works / Where Things Live / Gotchas), or a recommendation with a tradeoffs table if the request is a decision between alternatives.

> 3. 产出 `how` 格式的输出（概览 / 关键概念 / 工作原理 / 代码位置 / 注意事项），或者在请求是多选一的决定时，给出一个带取舍表的建议。

4. Apply the **unslop** skill to the reply.

> 4. 对回复应用 **unslop** 技能。

No PR, no babysit, no `architect` unless the investigation precedes a code change. If it does, hand back to the user and re-route to Bug fix or Feature.

> 不开 PR，不 babysit，不用 `architect`，除非这次调查是代码改动的前奏。如果是，就交回给用户，并重新分派到 Bug fix 或 Feature。

**Reply:** the investigation output. For "are we sure?" answers, include your real judgment with reasons. Push back if the premise is wrong (see Autonomy).

> **回复：** 调查的输出。对“我们确定吗？”这类问题，附上你真实的判断和理由。如果前提错了就反驳（见“自主”一节）。
