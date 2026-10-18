<!-- 中英对照译文。原文：skills/poteto-help/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-help/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-help/SKILL.md)

```yaml
name: poteto-help
description: Guides users through pstack setup, /poteto-mode, and picking the skill, playbook, or principle for a task. Type /poteto-help with a question.
disable-model-invocation: true
```

> **说明（description）：** 引导用户完成 pstack 的设置、使用 /poteto-mode，以及为某个任务挑选技能、剧本或原则。输入 /poteto-help 加上你的问题。

# Poteto help｜Poteto 帮助

Answer the user's question about pstack, hand them a prompt they can send, and link the file the answer came from. For a help question, don't start the work. The user asked how, and a pstack run spends real tokens, so let them send the prompt.

> 回答用户关于 pstack 的问题，给他们一句可以直接发送的提示词，并链接答案出处的文件。对于求助类的问题，不要开始干活。用户问的是怎么做，而一次 pstack 运行会花掉真实的 token，所以让他们自己去发那句提示词。

A message that asks for work, such as "use pstack to fix this bug", is not a help question. Read [`poteto-mode`](../poteto-mode/SKILL.zh.md), do the work under it, and mention once that a Custom Mode keeps it on.

> 一条要求干活的消息，比如“用 pstack 修这个 bug”，不是求助问题。读 [`poteto-mode`](../poteto-mode/SKILL.zh.md)，在它的规则下干活，并提一次“自定义模式”可以让它一直生效。

This file maps questions to the skills and guide pages that hold the answers. Those files own the details. Read the file you route to before you quote it, and trust it when it disagrees with this map. The links here point into the installed plugin, which the user may not be able to open, so give the user the file's public copy: `https://github.com/cursor/plugins/blob/main/pstack/` followed by its path.

> 这个文件把问题映射到存放答案的技能和指南页面。细节由那些文件负责。引用之前先读你要分派到的文件，当它和这张映射表不一致时，以它为准。这里的链接指向已安装的插件内部，用户可能打不开，所以给用户文件的公开副本：`https://github.com/cursor/plugins/blob/main/pstack/` 加上它的路径。

## Find out what they need｜弄清他们需要什么

Infer the need from the message and the conversation. A named situation, such as "which skill reviews a PR?", goes straight to its section. If the need is still unclear, ask one multiple-choice question with these options, then answer only the section they pick:

> 从消息和对话里推断需求。点名了具体情况的，比如“哪个技能审查 PR？”，直接去对应的章节。如果需求仍然不清楚，就用下面这些选项问一个多选题，然后只回答他们选的那一节：

- Get set up

> - 完成设置

- Start a task with `/poteto-mode`

> - 用 `/poteto-mode` 开始一个任务

- Pick a skill for a situation

> - 为某个情况挑选技能

- Fix a run that went wrong

> - 修复一次跑偏的运行

- Make pstack my own

> - 把 pstack 变成我自己的

Check the state that changes the answer, and mention it only when it does:

> 检查会改变答案的状态，只在它确实改变答案时才提：

- No pstack settings file (`~/.cursor/rules/pstack-models.mdc` in Cursor, `~/.agents/pstack-models.md` in other harnesses) means `/setup-pstack` hasn't run for this user, so every role uses its default model.

> - 没有 pstack 设置文件（Cursor 里是 `~/.cursor/rules/pstack-models.mdc`，其他运行环境里是 `~/.agents/pstack-models.md`），说明这个用户还没运行过 `/setup-pstack`，所以每个角色都用默认模型。

- No `verify-*` skill or other app harness in the project means agents have no scripted way to drive the app. Mention `/create-verification-skill` when the question is about proving a change works.

> - 项目里没有 `verify-*` 技能或其他操作应用的测试工具，说明代理没有用脚本操作应用的方式。当问题是关于证明改动能用时，提一下 `/create-verification-skill`。

When the model rule is missing and it matters, ask whether the user wants to pick a model for each role and a reasoning budget now. It matters when the user is new, the question is about setup or cost, or the answer depends on which models run. Ask at most once per chat. If the need is also unclear, ask both questions together. Offer two choices:

> 当缺少模型规则、而这一点又重要时，问用户要不要现在为每个角色选模型并选一个推理预算。在用户是新手、问题关于设置或成本，或者答案取决于跑的是哪些模型时，这一点就重要。每个对话最多问一次。如果需求也不清楚，就把两个问题一起问。给出两个选择：

- Now: give them `/setup-pstack` to type, and answer their question too.

> - 现在：给他们 `/setup-pstack` 去输入，同时也回答他们的问题。

- Later: answer their question, and add one line saying every role keeps its default model until they run `/setup-pstack`.

> - 以后：回答他们的问题，再加一行说明，在他们运行 `/setup-pstack` 之前，每个角色都保持默认模型。

## Get set up｜完成设置

1. Install with `/add-plugin pstack` in chat, or from Customize in the sidebar. Outside Cursor, run `npx skills add backnotprop/pstack` and pick the skills.

> 1. 在聊天里用 `/add-plugin pstack` 安装，或者从侧边栏的 Customize 安装。在 Cursor 之外，运行 `npx skills add backnotprop/pstack` 并挑选技能。

2. Run [`/setup-pstack`](../setup-pstack/SKILL.zh.md). It asks for a reasoning budget, maps a model to each role, and writes a rule. The rule applies to new chats.

> 2. 运行 [`/setup-pstack`](../setup-pstack/SKILL.zh.md)。它会询问推理预算，为每个角色对应一个模型，并写入一条规则。这条规则对新对话生效。

3. Start a real task with `/poteto-mode`, a goal, and a check that can pass or fail.

> 3. 用 `/poteto-mode`、一个目标和一个能判定通过或失败的检查，开始一个真实的任务。

Installing changes nothing until the user invokes a skill. Only `/setup-pstack` loads from the user's words. The [README](https://github.com/bowie06111/pstack/blob/main/README.md) and [guide page 1](https://github.com/bowie06111/pstack/blob/main/docs/guide/01-setup.md) have the details. Offer to word their first prompt with them, per [`references/prompting.md`](references/prompting.zh.md).

> 在用户调用某个技能之前，安装不会改变任何东西。只有 `/setup-pstack` 会根据用户的话自动加载。细节见 [README](https://github.com/bowie06111/pstack/blob/main/README.md) 和 [指南第 1 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/01-setup.md)。按 [`references/prompting.md`](references/prompting.zh.md)，提议和他们一起斟酌第一句提示词。

If cost is the worry, say where the tokens go and how to spend fewer. pstack spends extra tokens on subagents and review panels. Rerun `/setup-pstack` and pick a smaller budget or cheaper models. A role set to `auto` or `inherit-parent` runs on the chat's model, which saves tokens when the chat runs on Auto or a cheaper model. A shorter panel list runs fewer subagents, one for each entry. Save `/poteto-mode` for work that needs rigor.

> 如果担心的是成本，就说明 token 花在了哪里，以及怎样少花。pstack 会在子代理和评审团上额外花 token。重新运行 `/setup-pstack`，选一个更小的预算或更便宜的模型。设为 `auto` 或 `inherit-parent` 的角色会在对话的模型上运行，当对话用的是 Auto 或更便宜的模型时，这能省 token。更短的评审团名单会运行更少的子代理，每个条目一个。只在需要严谨的工作上用 `/poteto-mode`。

pstack was built for Cursor. This mirror's skills also work in other harnesses (Claude Code, Codex, Pi, OpenCode, and others): the Harness section of [`poteto-mode`](../poteto-mode/SKILL.zh.md) maps Cursor's subagent, question, transcript, and settings paths to the harness's own. Custom Modes are a Cursor feature.

> pstack 是为 Cursor 构建的。这个镜像的技能在其他运行环境（Claude Code、Codex、Pi、OpenCode 等）里也能用：[`poteto-mode`](../poteto-mode/SKILL.zh.md) 的“运行环境”一节，把 Cursor 的子代理、提问、会话记录和设置路径，对应到各个运行环境自己的路径。自定义模式是 Cursor 的功能。

## Start a task with `/poteto-mode`｜用 `/poteto-mode` 开始一个任务

`/poteto-mode` matches the task to a playbook, copies the playbook's steps into the todo list, and runs the other skills as the steps need them. A step it skips stays in the list as `skip: <reason>`. A good prompt states the goal and how to tell it's done. It doesn't list skills, because a hand-written sequence tends to drop or reorder steps the playbook would keep. Read [`references/prompting.md`](references/prompting.zh.md) before you help word one. [Guide page 2](https://github.com/bowie06111/pstack/blob/main/docs/guide/02-poteto-mode.md) has examples.

> `/poteto-mode` 把任务匹配到一个剧本，把剧本的步骤抄进待办清单，并在步骤需要时运行其他技能。它跳过的步骤会以 `skip: <原因>` 的形式留在清单里。好的提示词说明目标以及怎么判断完成。它不罗列技能，因为手写的顺序往往会漏掉或打乱剧本本来会保留的步骤。帮忙斟酌提示词之前，先读 [`references/prompting.md`](references/prompting.zh.md)。[指南第 2 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/02-poteto-mode.md) 有示例。

Whether `/poteto-mode` stays on depends on how the user starts it:

> `/poteto-mode` 是否持续生效，取决于用户怎么启动它：

- Enter on `/poteto-mode` attaches the skill to one message. It fades as the chat moves on.

> - 在 `/poteto-mode` 上按回车，会把技能附在一条消息上。随着对话推进，它会逐渐淡出。

- Option+Enter on Mac or Alt+Enter on Windows, or Use as Mode from the skill entry, makes it a Custom Mode. It stays in context every turn until the user exits the mode, and it stays out of casual turns.

> - 在 Mac 上按 Option+回车、在 Windows 上按 Alt+回车，或者在技能条目上选 Use as Mode，会把它变成一个自定义模式。在用户退出模式之前，它每一回合都留在上下文里，而在随意的闲聊回合里它不会出来碍事。

- Cursor's docs list Custom Modes in the Agents Window and the CLI. Elsewhere, start each new task with `/poteto-mode`.

> - Cursor 的文档说自定义模式可以在 Agents Window 和 CLI 里使用。在其他地方，每个新任务都以 `/poteto-mode` 开头。

Link [Cursor's skills docs](https://cursor.com/docs/skills) when this comes up. Mid-chat, "new task" makes the mode match a fresh playbook. `/poteto-mode` already uses `poteto-agent` for the subagents its playbook steps spawn. To get the same style from a subagent of your own, spawn it with `subagent_type: "poteto-agent"`.

> 说到这个时，附上 [Cursor 的技能文档](https://cursor.com/docs/skills) 链接。对话中途说“new task”，会让模式重新匹配一个剧本。`/poteto-mode` 已经在用 `poteto-agent` 作为它的剧本步骤所派生的子代理。如果想让你自己的子代理也有同样的风格，就用 `subagent_type: "poteto-agent"` 来派生它。

## Pick a skill｜挑选技能

The default answer is `/poteto-mode`, which runs most of the others when its steps need them. Name a skill directly when the user wants more or less of something than the playbook gives. Read the skill before you recommend it, and give one example prompt.

> 默认答案是 `/poteto-mode`，它会在步骤需要时运行其他大部分技能。当用户想要比剧本给的更多或更少的某样东西时，再直接点名某个技能。推荐之前先读这个技能，并给一句示例提示词。

| The user wants to | Skill |
|---|---|
| Do any non-trivial task with rigor | [`/poteto-mode`](../poteto-mode/SKILL.zh.md) |
| Check that the agent understood their goal and the problem before it starts | [`/in-your-own-words`](../in-your-own-words/SKILL.zh.md) |
| Aim a task higher before starting: more expert, more outcome-focused, not overspecified | [`/sharpen`](../sharpen/SKILL.zh.md) |
| Know how code works now, or where new code should live | [`/how`](../how/SKILL.zh.md) |
| Know why code is shaped this way, or where a number came from | [`/why`](../why/SKILL.zh.md) |
| Understand a change or subsystem, explained plainly | [`/teach`](../teach/SKILL.zh.md) |
| Catch up on their own recent work on a topic | [`/recall`](../recall/SKILL.zh.md) |
| Catch up on what changed in this chat since their last message, in a few plain sentences and small visuals, with enough context to decide each action item | [`/ketchup`](../ketchup/SKILL.zh.md) |
| Know what a small diff could break outside itself | [`/blast-radius`](../blast-radius/SKILL.zh.md) |
| Settle types and module shape before code that crosses a function boundary | [`/architect`](../architect/SKILL.zh.md) |
| Get several attempts at one brief, merged into the best one | [`/arena`](../arena/SKILL.zh.md) |
| Run parallel checks over slices, or race workers, as cloud agents | [`/swarm`](../swarm/SKILL.zh.md) |
| Have different models review a diff and try to break it | [`/interrogate`](../interrogate/SKILL.zh.md) |
| Fix a bug test-first when a cheap local test exists | [`/tdd`](../tdd/SKILL.zh.md) |
| Apply TypeScript rules to `.ts` or `.tsx` work | [`/typescript-best-practices`](../typescript-best-practices/SKILL.zh.md) |
| Strip comments before review, using a reviewer that didn't write them | [`/no-comments`](../no-comments/SKILL.zh.md) |
| Clean AI tells out of prose | [`/unslop`](../unslop/SKILL.zh.md) |
| Write docs, an RFC, a README, a PR description, or a commit message to a standard | [`/technical-writing`](../technical-writing/SKILL.zh.md) |
| Hear the last reply again in plain words | [`/bro`](../bro/SKILL.zh.md) |
| Give agents a scripted way to drive the app and prove behavior | [`/create-verification-skill`](../create-verification-skill/SKILL.zh.md) |
| Bring a verification skill and its feature map back in line with the app | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.zh.md) |
| Vet a performance number before reporting or acting on it | [`/benchmark-checklist`](../benchmark-checklist/SKILL.zh.md) |
| Run a large or cross-cutting change, or one to review after stepping away | [`/figure-it-out`](../figure-it-out/SKILL.zh.md) |
| Keep a decision log during a run, and review it afterward | [`/show-me-your-work`](../show-me-your-work/SKILL.zh.md) |
| Pick a model for each role and a reasoning budget | [`/setup-pstack`](../setup-pstack/SKILL.zh.md) |
| Turn their own working habits into a personal mode skill | [`/automate-me`](../automate-me/SKILL.zh.md) |
| Turn what a finished task taught into skill edits | [`/reflect`](../reflect/SKILL.zh.md) |
| Stop agents from repeating the same mistakes in this repo | [`/correct`](../correct/SKILL.zh.md) |
| Build a page whose buttons wake a Grok Bot over a webhook | [`/make-bot-ui`](../make-bot-ui/SKILL.zh.md) |
| Find their way around pstack | `/poteto-help` |

> | 用户想要 | 技能 |
> |---|---|
> | 严谨地完成任何非平凡的任务 | [`/poteto-mode`](../poteto-mode/SKILL.zh.md) |
> | 在开始之前确认代理理解了他们的目标和问题 | [`/in-your-own-words`](../in-your-own-words/SKILL.zh.md) |
> | 开始之前把任务的目标定得更高：更专业、更关注结果、不过度规定 | [`/sharpen`](../sharpen/SKILL.zh.md) |
> | 知道代码现在怎么工作，或者新代码应该放在哪里 | [`/how`](../how/SKILL.zh.md) |
> | 知道代码为什么是这个形状，或者某个数字是从哪来的 | [`/why`](../why/SKILL.zh.md) |
> | 理解一个改动或子系统，用平实的话讲清楚 | [`/teach`](../teach/SKILL.zh.md) |
> | 跟上自己最近在某个话题上的工作 | [`/recall`](../recall/SKILL.zh.md) |
> | 用几句平实的话和小图，跟上这个对话自他上条消息以来的变化，并有足够的上下文来决定每个待办事项 | [`/ketchup`](../ketchup/SKILL.zh.md) |
> | 知道一个小 diff 可能在它之外弄坏什么 | [`/blast-radius`](../blast-radius/SKILL.zh.md) |
> | 在写跨越函数边界的代码之前，先定好类型和模块形状 | [`/architect`](../architect/SKILL.zh.md) |
> | 对同一份任务说明做几次尝试，合并成最好的一个 | [`/arena`](../arena/SKILL.zh.md) |
> | 用云端代理对多个切片做并行检查，或者让工作者竞速 | [`/swarm`](../swarm/SKILL.zh.md) |
> | 让不同的模型审查一个 diff，试着把它弄坏 | [`/interrogate`](../interrogate/SKILL.zh.md) |
> | 有便宜的本地测试时，先写测试再修 bug | [`/tdd`](../tdd/SKILL.zh.md) |
> | 对 `.ts` 或 `.tsx` 的工作应用 TypeScript 规则 | [`/typescript-best-practices`](../typescript-best-practices/SKILL.zh.md) |
> | 在审查之前，让一个没写这些代码的审查者删掉注释 | [`/no-comments`](../no-comments/SKILL.zh.md) |
> | 清除文字里的 AI 痕迹 | [`/unslop`](../unslop/SKILL.zh.md) |
> | 按标准写文档、RFC、README、PR 描述或提交信息 | [`/technical-writing`](../technical-writing/SKILL.zh.md) |
> | 用平实的话把上一条回复再听一遍 | [`/bro`](../bro/SKILL.zh.md) |
> | 给代理一种用脚本操作应用、证明行为的方式 | [`/create-verification-skill`](../create-verification-skill/SKILL.zh.md) |
> | 让验证技能及其功能地图重新与应用保持一致 | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.zh.md) |
> | 在报告或依据一个性能数字行动之前审查它 | [`/benchmark-checklist`](../benchmark-checklist/SKILL.zh.md) |
> | 执行一个大型或跨领域的改动，或者一个离开后再回来审查的改动 | [`/figure-it-out`](../figure-it-out/SKILL.zh.md) |
> | 在运行期间记决策日志，之后再审阅 | [`/show-me-your-work`](../show-me-your-work/SKILL.zh.md) |
> | 为每个角色选模型和推理预算 | [`/setup-pstack`](../setup-pstack/SKILL.zh.md) |
> | 把自己的工作习惯变成一个个人模式技能 | [`/automate-me`](../automate-me/SKILL.zh.md) |
> | 把一个已完成任务的教训变成技能修改 | [`/reflect`](../reflect/SKILL.zh.md) |
> | 阻止代理在这个仓库里重复同样的错误 | [`/correct`](../correct/SKILL.zh.md) |
> | 做一个页面，让它的按钮通过 webhook 唤醒 Grok Bot | [`/make-bot-ui`](../make-bot-ui/SKILL.zh.md) |
> | 在 pstack 里找到方向 | `/poteto-help` |

If a skill directory next to this one is missing from the table, read its frontmatter and route by its description. The `principle-*` directories are covered under principles below.

> 如果本目录旁边的某个技能目录不在表里，读它的 frontmatter，按它的描述来分派。`principle-*` 目录在下面的“原则”里介绍。

Close calls:

> 容易混淆的情况：

- `/how` explains what the code does. `/why` explains the reasons. `/teach` runs one or both and explains the result plainly.

> - `/how` 解释代码做什么。`/why` 解释原因。`/teach` 运行其中一个或两个，并用平实的话讲清结果。

- `/arena` gives every worker the same brief and merges the best parts. `/swarm` splits work into slices or a race and returns one report.

> - `/arena` 给每个工作者同一份任务说明，并合并最好的部分。`/swarm` 把工作分成切片或一场竞速，返回一份报告。

- `/architect` implements right after it settles the design. Add "with checkpoint" to review the design before it writes code.

> - `/architect` 定好设计后会紧接着实现。加上“with checkpoint”就能在它写代码之前审查设计。

- `/interrogate` reviews the diff. `/blast-radius` looks for breakage outside the diff and proves the one fact that makes the change safe.

> - `/interrogate` 审查 diff。`/blast-radius` 寻找 diff 之外的破坏，并证明让改动安全的那一个事实。

- `/recall` rebuilds context across recent chats. `/ketchup` covers only this chat since the user's last message. Resuming one specific chat or branch is the Session pickup playbook.

> - `/recall` 跨越最近多个对话重建上下文。`/ketchup` 只覆盖这个对话自用户上条消息以来的内容。恢复某一个具体的对话或分支，属于 Session pickup 剧本。

- `/figure-it-out` designs one rigorous run. The Orchestrate playbook runs a program that spans days and many PRs. The Autonomous run playbook drives one task to a finish condition.

> - `/figure-it-out` 设计一次严谨的运行。Orchestrate 剧本运行一个跨越多天、多个 PR 的计划。Autonomous run 剧本把一个任务推进到一个完成条件。

Not in pstack:

> 不在 pstack 里的：

- `/deslop`, `control-cli`, and `control-ui` ship in the `cursor-team-kit` plugin.

> - `/deslop`、`control-cli` 和 `control-ui` 在 `cursor-team-kit` 插件里。

- `/loop` and `/create-skill` are Cursor built-ins.

> - `/loop` 和 `/create-skill` 是 Cursor 内置的。

- pstack has no `/orchestrate` skill. Orchestrate is a `/poteto-mode` playbook. If the slash menu shows `/orchestrate`, another plugin provides it.

> - pstack 没有 `/orchestrate` 技能。Orchestrate 是 `/poteto-mode` 的一个剧本。如果斜杠菜单里出现了 `/orchestrate`，那是别的插件提供的。

## Playbooks and principles｜剧本和原则

Playbooks are step lists inside `/poteto-mode`, not skills, so they have no slash command. Inside `/poteto-mode`, describing the task picks one, and these phrases name one directly:

> 剧本是 `/poteto-mode` 内部的步骤清单，不是技能，所以没有斜杠命令。在 `/poteto-mode` 里，描述任务就会选中一个剧本，而下面这些说法会直接点名某个剧本：

- "babysit this pr" or "check on pr 123" runs Babysit. It drives the PR to merge-ready and stops there. It doesn't merge unless the user asks to merge, land, or ship.

> - “babysit this pr”或“check on pr 123”运行 Babysit。它把 PR 推进到可合并，然后停下。除非用户要求合并、落地或发布，否则它不合并。

- "land the stack" runs Shipping.

> - “land the stack”运行 Shipping。

- "take over this branch" runs Session pickup.

> - “take over this branch”运行 Session pickup。

- "pause safely" runs Pause safely.

> - “pause safely”运行 Pause safely。

- "full autopilot on this queue" runs Autopilot-full. "stack them, don't ship" runs Autopilot-stack.

> - “full autopilot on this queue”运行 Autopilot-full。“stack them, don't ship”运行 Autopilot-stack。

- "run the eval playbook" runs Eval.

> - “run the eval playbook”运行 Eval。

Without `/poteto-mode`, a phrase such as "babysit this pr" can start Cursor's own skill for the same job instead. The Playbooks section of [`poteto-mode`](../poteto-mode/SKILL.zh.md) lists every playbook and when it applies. [Guide page 6](https://github.com/bowie06111/pstack/blob/main/docs/guide/06-verify-and-ship.md) covers opening, babysitting, and landing a PR.

> 没有 `/poteto-mode` 时，“babysit this pr”这样的说法可能会改为启动 Cursor 自己做同样事情的技能。[`poteto-mode`](../poteto-mode/SKILL.zh.md) 的“剧本”一节列出了每个剧本以及它何时适用。[指南第 6 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/06-verify-and-ship.md) 讲了开 PR、看护和合并 PR。

pstack has no planning skill. Cursor's Plan Mode works alongside it. For work that spans phases or stacked PRs, asking `/poteto-mode` for a plan runs the [Multi-phase plan playbook](../poteto-mode/playbooks/multi-phase-plan.zh.md), which writes the plan and doesn't implement it. For a design question, the Prototype playbook or `/architect` settles it in code first.

> pstack 没有规划技能。Cursor 的 Plan Mode 可以和它配合使用。对于跨越多个阶段或堆叠 PR 的工作，让 `/poteto-mode` 出一份计划会运行 [多阶段计划剧本](../poteto-mode/playbooks/multi-phase-plan.zh.md)，它会写出计划但不实现。对于设计问题，Prototype 剧本或 `/architect` 会先用代码来解决它。

Principles are one-rule skills that `/poteto-mode` reads and cites in its replies. The user rarely invokes one. They steer with the names instead, as in "apply prove it works. show me the real output." Typing `/principle-<name>` still loads one on demand. [Guide page 8](https://github.com/bowie06111/pstack/blob/main/docs/guide/08-principles.md) lists them.

> 原则是只有一条规则的技能，`/poteto-mode` 会读它们并在回复里引用。用户很少直接调用它们。用户是用它们的名字来纠偏，比如“apply prove it works。给我看真实的输出。”输入 `/principle-<名字>` 仍然可以按需加载某一条。[指南第 8 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/08-principles.md) 列出了它们。

## Fix a run that went wrong｜修复一次跑偏的运行

| Symptom | Fix |
|---|---|
| The mode stopped applying after a few turns | It was started with Enter. Start it as a Custom Mode, or start each task with `/poteto-mode`. |
| A question got treated as the next step of the last task | Say "new task", or say the turn doesn't need the mode. |
| A new model choice had no effect | The rule from `/setup-pstack` applies to new chats. Start one. |
| Runs cost more than expected | See the cost paragraph under Get set up. |
| A skill didn't load on its own | Only `/setup-pstack` loads from the user's words. The others load when the user types them or when `/poteto-mode` runs them, and it doesn't run every skill. |
| Parallel agents overwrote each other | Give each agent its own worktree, or run them as cloud agents, which each get their own machine. |
| An overnight run moved but finished nothing | `/loop` needs a check that can pass or fail, not a duration. See [guide page 7](https://github.com/bowie06111/pstack/blob/main/docs/guide/07-overnight.md). |
| The reply claims success from a green build | Ask for the real command, flow, stored value, or profile. That's the prove-it-works principle. |

> | 症状 | 解决办法 |
> |---|---|
> | 几个回合之后模式不再生效了 | 它是用回车启动的。用自定义模式启动它，或者每个任务都以 `/poteto-mode` 开头。 |
> | 一个问题被当成了上一个任务的下一步 | 说“new task”，或者说这一回合不需要模式。 |
> | 新的模型选择没有生效 | `/setup-pstack` 写的规则对新对话生效。开一个新对话。 |
> | 运行花费比预期多 | 见“完成设置”下关于成本的那一段。 |
> | 某个技能没有自己加载 | 只有 `/setup-pstack` 会根据用户的话自动加载。其他技能在用户输入它们、或者 `/poteto-mode` 运行它们时才加载，而 `/poteto-mode` 不会运行所有技能。 |
> | 并行的代理互相覆盖了 | 给每个代理自己的 worktree，或者把它们作为云端代理运行，每个云端代理都有自己的机器。 |
> | 通宵运行一直在动却什么都没完成 | `/loop` 需要一个能判定通过或失败的检查，而不是一段时长。见 [指南第 7 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/07-overnight.md)。 |
> | 回复以构建通过来宣称成功 | 要求看真实的命令、流程、存储的值或性能分析。这就是“证明它能用”原则。 |

For a run that drifts, [`references/prompting.md`](references/prompting.zh.md) has one-line steers. [Guide page 10](https://github.com/bowie06111/pstack/blob/main/docs/guide/10-recipes-and-pitfalls.md) has more pitfalls and the recipes worth copying.

> 对于跑偏的运行，[`references/prompting.md`](references/prompting.zh.md) 里有一句话就能纠偏的说法。[指南第 10 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/10-recipes-and-pitfalls.md) 有更多陷阱和值得照抄的提示词。

## Make pstack my own｜把 pstack 变成我自己的

- [`/automate-me`](../automate-me/SKILL.zh.md) drafts a personal mode skill from the user's own history, to use alongside `/poteto-mode`.

> - [`/automate-me`](../automate-me/SKILL.zh.md) 根据用户自己的历史起草一个个人模式技能，和 `/poteto-mode` 并排使用。

- [`/reflect`](../reflect/SKILL.zh.md) after a session turns its lessons into skill edits the user approves.

> - 一次会话结束后用 [`/reflect`](../reflect/SKILL.zh.md)，把它的教训变成经用户批准的技能修改。

- `/poteto-mode write a skill for <workflow>` runs the authoring playbook. The eval playbook tests a skill change blind.

> - `/poteto-mode write a skill for <工作流>` 会运行编写技能的剧本。Eval 剧本会盲测一次技能改动。

- Fix a misbehaving skill in its own PR, not inside the feature work where it went wrong.

> - 在单独的 PR 里修复表现异常的技能，而不是在它出问题的那个功能工作里修。

[Guide page 9](https://github.com/bowie06111/pstack/blob/main/docs/guide/09-make-it-yours.md) covers each of these.

> [指南第 9 页](https://github.com/bowie06111/pstack/blob/main/docs/guide/09-make-it-yours.md) 逐一介绍了这些。

## Reply｜回复

Lead with the answer. Give at most one example prompt in a code block, adapted from [`references/recipes.md`](references/recipes.zh.md) when one fits, then the link to that file. Keep it short unless the user asked for the whole map.

> 先给答案。最多在代码块里给一句示例提示词，合适时从 [`references/recipes.md`](references/recipes.zh.md) 改编，然后附上那个文件的链接。除非用户要的是整张映射表，否则保持简短。
