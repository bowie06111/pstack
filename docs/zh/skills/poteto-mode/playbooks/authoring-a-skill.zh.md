<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/authoring-a-skill.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/authoring-a-skill.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/authoring-a-skill.md)

### Authoring or modifying a skill｜编写或修改技能

**You own the skill's voice.**

> **技能的语气由你负责。**

1. Use your skill-authoring skill: **create-skill** in Cursor (built in), or Anthropic's `skill-creator` elsewhere. With neither, follow the Agent Skills format at agentskills.io.

> 1. 使用你的技能编写技能：Cursor 里是 **create-skill**（内置），其他环境里是 Anthropic 的 `skill-creator`。两者都没有的话，遵循 agentskills.io 上的 Agent Skills 格式。

2. Validate the skill: frontmatter has `name` and `description`, referenced files exist, cross-skill links resolve.

> 2. 校验技能：frontmatter 里有 `name` 和 `description`，引用的文件存在，跨技能链接能解析。

3. Test cases if structural. Skip if subjective.

> 3. 如果是结构性的改动，写测试用例。如果是主观的，跳过。

4. Run **Opening a PR**.

> 4. 运行 **Opening a PR**。

When in doubt, delete. Keep only prose that changes a decision. Tell it to do the thing and skip the reason. Explain only when the rule is confusing without one. Match tone to scope. Point at structural sources (types, READMEs, config) per the **encode-lessons-in-structure** principle skill. Delegate to other skills by path. Don't restate. A workflow you keep hitting but isn't captured → propose a new skill.

> 拿不准时，就删掉。只保留会改变决策的文字。告诉它去做这件事，省略理由。只有当规则不解释就会让人困惑时才解释。语气要和范围相称。按 **encode-lessons-in-structure** 原则技能，指向结构性的来源（类型、README、配置）。通过路径委派给其他技能。不要重复说。你反复遇到但还没被记录下来的工作流 → 提议一个新技能。

**Reply:** summary of the skill, key design decisions, validation notes.

> **回复：** 技能的概要、关键设计决定、校验说明。
