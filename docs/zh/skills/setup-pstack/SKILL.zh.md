<!-- 中英对照译文。原文：skills/setup-pstack/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/setup-pstack/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/setup-pstack/SKILL.md)

```yaml
name: setup-pstack
description: Configure which models pstack uses per role and at what reasoning budget. Detects your available models and writes an always-applied rule that overrides the skill defaults. Use for /setup-pstack, "configure pstack models", "pstack budget", or changing pstack's model choices.
```

> **说明（description）：** 配置 pstack 每个角色使用哪个模型、以什么推理预算运行。检测你可用的模型，并写入一条始终生效、覆盖技能默认值的规则。用于 /setup-pstack、“配置 pstack 模型”“pstack 预算”，或修改 pstack 的模型选择。

# Setup pstack｜设置 pstack

Write pstack's model settings, one model per role:

> 写入 pstack 的模型设置，每个角色一个模型：

- **Cursor:** `~/.cursor/rules/pstack-models.mdc`, an always-applied rule.

> - **Cursor：** `~/.cursor/rules/pstack-models.mdc`，一条始终生效的规则。

- **Every other harness** (Claude Code, Codex, Pi, OpenCode, and others): `~/.agents/pstack-models.md`. These harnesses don't load Cursor rules, so pstack skills read this file when they pick a model.

> - **其他所有运行环境**（Claude Code、Codex、Pi、OpenCode 等）：`~/.agents/pstack-models.md`。这些运行环境不加载 Cursor 规则，所以 pstack 技能在选择模型时会读这个文件。

"The settings file" below means the file for your harness. When reading, check both paths and use the one that exists.

> 下面说的“设置文件”指的是你所用运行环境对应的文件。读取时，两个路径都检查，用存在的那个。

## Steps｜步骤

### 1. Detect available models｜1. 检测可用的模型

Enumerate the model slugs you can pass to a subagent in this session (Cursor `Task`, Claude Code `Agent`, OpenCode `task`, Codex `spawn_agent`). That is the dependable source. If your harness also has a command that lists the user's models, prefer it for completeness (for example `opencode models` or `pi --list-models`). If you cannot detect any, ask the user to paste the slugs they have access to. Never write a real slug you have not confirmed is available. The aliases `inherit-parent` and `auto` are always valid even though they are not detected slugs.

> 列出在这个会话里你可以传给子代理的模型名（Cursor 的 `Task`、Claude Code 的 `Agent`、OpenCode 的 `task`、Codex 的 `spawn_agent`）。这是可靠的来源。如果你的运行环境还有列出用户模型的命令，为了完整优先用它（比如 `opencode models` 或 `pi --list-models`）。如果一个都检测不到，就请用户贴出他们有权限使用的模型名。永远不要写入一个你没有确认可用的真实模型名。别名 `inherit-parent` 和 `auto` 虽然不是检测到的模型名，但始终有效。

### 2. Load current state｜2. 加载当前状态

The default role-to-model mapping is the rule shape shown in step 5 below. If the settings file already exists, read it and treat its `# budget` line and its role values as the current choices. Otherwise start from those defaults. A line whose role is not in step 5, such as `how critics`, is from a retired role. Drop it.

> 默认的角色到模型的对应关系，就是下面第 5 步展示的规则形式。如果设置文件已经存在，就读取它，把它的 `# budget` 行和各角色的值当作当前选择。否则从那些默认值开始。如果某一行的角色不在第 5 步里，比如 `how critics`，那它来自一个已退役的角色。把它去掉。

### 3. Budget, map, and confirm｜3. 预算、对应和确认

**(a) Ask for a budget.** Prefer your structured-question tool (`AskQuestion` in Cursor, `AskUserQuestion` in Claude Code, `question` in OpenCode) over free text. Offer these four options with these exact labels, and name the current budget when the rule records one. With no rule, say that `large` matches the skill defaults.

> **(a) 询问预算。** 优先用你的结构化提问工具（Cursor 里是 `AskQuestion`，Claude Code 里是 `AskUserQuestion`，OpenCode 里是 `question`），而不是自由文本。提供下面四个选项，标签一字不差，并且当规则里记录了当前预算时说出来。没有规则时，说明 `large` 与技能默认值一致。

- `unlimited — max reasoning`

> - `unlimited — max reasoning`（无限，最高推理）

- `large — xhigh reasoning`

> - `large — xhigh reasoning`（大，超高推理）

- `medium — high reasoning`

> - `medium — high reasoning`（中，高推理）

- `small — medium reasoning`

> - `small — medium reasoning`（小，中等推理）

**(b) Apply it.** Build the working table from the skill defaults, and on a re-run keep any role you changed by family, list, or alias (`inherit-parent`, `auto`). `unlimited`, `large`, `medium`, and `small` set the effort token of every real slug, panel entries included, to `max`, `xhigh`, `high`, or `medium`. The effort token is the last token, or the one before a trailing `fast`, on the ladder `max` > `xhigh` > `high` > `medium` > `low`. If the result is not a detected slug, use the same family's detected slug with the highest effort at or below the target, else mark the role as needing a choice. `inherit-parent` and `auto` do not change. So `unlimited` turns `claude-opus-5-5-xhigh` into `claude-opus-5-5-max`. Grok slugs top out at `xhigh`, so under `unlimited` the fallback puts Grok at `xhigh` and keeps `grok-4.7-xhigh-fast` as it is. `large` keeps both defaults. `small` turns them into `claude-opus-5-5-medium` and `grok-4.7-medium-fast`.

> **(b) 应用预算。** 从技能默认值构建工作表；重新运行时，保留你按家族、列表或别名（`inherit-parent`、`auto`）改过的任何角色。`unlimited`、`large`、`medium` 和 `small` 会把每个真实模型名（包括评审团里的条目）的推理强度标记分别设为 `max`、`xhigh`、`high` 或 `medium`。推理强度标记是最后一个标记，或者结尾是 `fast` 时它前面那个，等级顺序为 `max` > `xhigh` > `high` > `medium` > `low`。如果结果不是一个检测到的模型名，就用同一家族中、推理强度在目标及以下的最高的那个检测到的模型名，否则把这个角色标记为需要选择。`inherit-parent` 和 `auto` 不变。所以 `unlimited` 会把 `claude-opus-5-5-xhigh` 变成 `claude-opus-5-5-max`。Grok 的模型名最高到 `xhigh`，所以在 `unlimited` 下，回退规则会让 Grok 停在 `xhigh`，`grok-4.7-xhigh-fast` 保持不变。`large` 保留两个默认值。`small` 会把它们变成 `claude-opus-5-5-medium` 和 `grok-4.7-medium-fast`。

**(c) Show the roles and confirm.** Show every role with its model, marking any real slug not in the detected set as needing a choice. Also list each line step 2 dropped. Ask whether to accept as-is or change specific roles, offering the detected models plus `inherit-parent` and `auto` (both mean: this role runs on the parent chat model, which is how Auto users stay on Auto) as the options. Prefer your structured-question tool over free text. For panel roles (arena runners, architect runners, interrogate reviewers) the value is a list, and one subagent runs per entry, alias entries included, so the list length sets the count. `arena cross-judge pool` is also a list, but Arena selects one value from it whose model family differs from the parent's when possible. `swarm workers` is the default model for every worker unless a race or comparison assigns another model per arm.

> **(c) 展示各角色并确认。** 展示每个角色及其模型，把不在检测集合里的真实模型名标记为需要选择。同时列出第 2 步去掉的每一行。询问是原样接受还是修改某些角色，选项是检测到的模型加上 `inherit-parent` 和 `auto`（两者都表示：这个角色在父对话的模型上运行，这也是 Auto 用户保持用 Auto 的方式）。优先用你的结构化提问工具，而不是自由文本。对于评审团类的角色（arena runners、architect runners、interrogate reviewers），值是一个列表，每个条目（包括别名条目）运行一个子代理，所以列表的长度决定了数量。`arena cross-judge pool` 也是一个列表，但 Arena 会尽可能从中选一个与父代理模型家族不同的值。`swarm workers` 是每个工作者的默认模型，除非某次竞速或比较为每一路指定了别的模型。

### 4. Validate｜4. 校验

Every real slug written must be in the detected set. `inherit-parent` and `auto` always pass. If a chosen real slug is not available, stop and ask again.

> 写入的每个真实模型名都必须在检测集合里。`inherit-parent` 和 `auto` 始终通过。如果选中的某个真实模型名不可用，停下来重新询问。

### 5. Write the rule｜5. 写入规则

Write the settings file with a `# budget` line with the chosen label and its target effort, and one line per role, using the same labels poteto-mode uses. In Cursor, include the frontmatter below with `alwaysApply: true`. In other harnesses, write `~/.agents/pstack-models.md` without the frontmatter (start at the first `#` line). Overwrite the whole file so re-runs stay idempotent. Shape:

> 写入设置文件：一行 `# budget`，写选定的标签及其目标推理强度；每个角色一行，使用 poteto-mode 所用的同样标签。在 Cursor 里，加上下面带 `alwaysApply: true` 的 frontmatter。在其他运行环境里，写入 `~/.agents/pstack-models.md`，不带 frontmatter（从第一行 `#` 开始）。覆盖整个文件，让重新运行保持幂等。形式如下：

```
---
description: pstack per-role model choices (overrides skill defaults)
alwaysApply: true
---
# pstack model configuration. One line per role. Delete a line to fall back to the skill default.
# `inherit-parent` or `auto` as a value: the role runs on the parent chat model (omit the subagent `model`). Alias entries in a panel list still count toward its fan-out.
# budget: large (xhigh)
feature, refactoring: grok-4.7-xhigh-fast
bug-fix: grok-4.7-xhigh-fast
perf-issue: grok-4.7-xhigh-fast
hillclimb: grok-4.7-xhigh-fast
judgment and prose: claude-opus-5-5-xhigh
hardest tasks: claude-opus-5-5-xhigh
how explorer: grok-4.7-xhigh-fast
how explainer: claude-opus-5-5-xhigh
why investigators: grok-4.7-xhigh-fast
why synthesizer: claude-opus-5-5-xhigh
reflect tooling: grok-4.7-xhigh-fast
reflect judgment, divergent, synthesizer: claude-opus-5-5-xhigh
arena runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
arena cross-judge pool: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
swarm workers: grok-4.7-xhigh-fast
architect runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
interrogate reviewers: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
```

> 上面代码块的中文版：

```
---
description: pstack per-role model choices (overrides skill defaults)
alwaysApply: true
---
# pstack 模型配置。每个角色一行。删掉某一行就回退到技能的默认值。
# 值为 `inherit-parent` 或 `auto`：该角色在父对话的模型上运行（省略子代理的 `model`）。评审团列表里的别名条目仍然计入扇出数量。
# budget: large (xhigh)
feature, refactoring: grok-4.7-xhigh-fast            # 功能、重构剧本
bug-fix: grok-4.7-xhigh-fast                          # 修 bug 剧本
perf-issue: grok-4.7-xhigh-fast                       # 性能问题剧本
hillclimb: grok-4.7-xhigh-fast                        # 爬坡剧本
judgment and prose: claude-opus-5-5-xhigh             # 判断和文字
hardest tasks: claude-opus-5-5-xhigh                  # 最难的任务
how explorer: grok-4.7-xhigh-fast                     # how 的探索者
how explainer: claude-opus-5-5-xhigh                  # how 的讲解者
why investigators: grok-4.7-xhigh-fast                # why 的调查者
why synthesizer: claude-opus-5-5-xhigh                # why 的综合者
reflect tooling: grok-4.7-xhigh-fast                  # reflect 的工具视角
reflect judgment, divergent, synthesizer: claude-opus-5-5-xhigh   # reflect 的判断、发散视角和综合者
arena runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast         # arena 的执行者
arena cross-judge pool: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast  # arena 的交叉评委候选池
swarm workers: grok-4.7-xhigh-fast                    # swarm 的工作者
architect runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast     # architect 的执行者
interrogate reviewers: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast # interrogate 的审查者
```
（译注：实际文件里不要加行尾的中文注释，这里只是为了说明每一行的含义。）

### 6. Confirm｜6. 确认

Tell the user which file was written. In Cursor, the rule applies to new sessions. In other harnesses, pstack skills read `~/.agents/pstack-models.md` when they pick a model. Re-running this skill updates it.

> 告诉用户写入了哪个文件。在 Cursor 里，这条规则对新会话生效。在其他运行环境里，pstack 技能在选择模型时会读 `~/.agents/pstack-models.md`。重新运行本技能会更新它。

### 7. Offer a verification skill (optional)｜7. 提供验证技能（可选）

Check whether the project has a way to drive the real app for proof (a `verify-*` skill, or an existing harness). If not, offer once: "want a project-local verification skill, so agents can drive the app the way a user does and prove changes work? I can generate one with /create-verification-skill." On yes, invoke `/create-verification-skill` (resolves wherever pstack is installed: workspace, user, or plugin). On no, move on without pushing.

> 检查项目里有没有能操作真实应用来取证的方式（一个 `verify-*` 技能，或已有的测试工具）。如果没有，提议一次：“要不要一个项目级的验证技能，让代理能像用户那样操作应用、证明改动能用？我可以用 /create-verification-skill 生成一个。”用户同意就调用 `/create-verification-skill`（无论 pstack 装在工作区、用户级还是插件里都能找到）。用户拒绝就继续，不要强推。
