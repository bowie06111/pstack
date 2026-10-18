<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/multi-phase-plan.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/multi-phase-plan.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/multi-phase-plan.md)

### Multi-phase or multi-PR plan｜多阶段或多 PR 计划

**You own the plan, not the code. The plan is a checklist an owner runs box by box and the operator audits from the evidence.** The plan is the deliverable. Do not implement.

> **你负责计划，不负责代码。计划是一份清单，由负责人逐项执行，由操作者根据证据审计。** 计划就是交付物。不要实现。

1. When the change is one or two files with an obvious approach, skip the plan. Say so and stop.

> 1. 如果改动只涉及一两个文件、做法显而易见，就跳过计划。说明这一点，然后停下。

2. Settle open questions by prototype before you write. Run `playbooks/prototype.md` for each. Keep the branch, the SHA, and the screenshots for Appendix A. Ask the operator only about a product or preference call that no run can settle. Give options (the **never-block-on-the-human** principle skill).

> 2. 动笔之前，用原型解决开放问题。每个问题跑一次 `playbooks/prototype.md`。把分支、SHA 和截图留给附录 A。只就没有任何运行能解决的产品或偏好问题去问操作者。给出选项（**never-block-on-the-human** 原则技能）。

3. Explore in subagents with `subagent_type: "poteto-agent"` and an explicit model per the Subagents section (the **guard-the-context-window** principle skill). Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section. Each returns file pointers, conventions, test commands, and entry points. No inlined dumps.

> 3. 用 `subagent_type: "poteto-agent"` 的子代理去探索，并按“子代理”一节明确指定模型（**guard-the-context-window** 原则技能）。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。每个子代理返回文件指针、约定、测试命令和入口点。不要内联大段内容。

4. Copy the skeleton below into the plan file and fill every placeholder. Unless the operator names a path, write the file under the agent store's `docs/`. Keep every heading and every sub-block in the order shown. One section per PR. One PR is one change with its own evidence (the **sequence-verifiable-units** principle skill). Name the execution playbook in **How to read this**. Pick between `playbooks/autopilot-full.md` and `playbooks/autopilot-stack.md` per the rule at the end of `playbooks/autopilot-stack.md`. A standing program takes `playbooks/orchestrate.md`.

> 4. 把下面的骨架复制到计划文件里，填好每个占位符。除非操作者指定了路径，否则把文件写在代理存储的 `docs/` 下。保留每个标题和每个子块，并按所示顺序排列。每个 PR 一节。一个 PR 就是一个带有自己证据的改动（**sequence-verifiable-units** 原则技能）。在 **How to read this** 里写明执行剧本。按 `playbooks/autopilot-stack.md` 末尾的规则，在 `playbooks/autopilot-full.md` 和 `playbooks/autopilot-stack.md` 之间选择。持续的项目级计划用 `playbooks/orchestrate.md`。

5. Write under `/technical-writing` in full, then `/unslop`. The body is one Diátaxis mode, how-to. Appendices hold explanation and reference. Each heading states the task or the finding. No long dashes. No mid-sentence colons.

> 5. 完整按 `/technical-writing` 来写，然后用 `/unslop`。正文只用一种 Diátaxis 模式：操作指南。附录放解释和参考。每个标题说明任务或发现。不用长破折号。句子中间不用冒号。

6. Run `node pstack/skills/poteto-mode/scripts/check-plan.mjs <plan.md>` and fix every line it prints (the **encode-lessons-in-structure** principle skill).

> 6. 运行 `node pstack/skills/poteto-mode/scripts/check-plan.mjs <plan.md>`，修复它打印出的每一行（**encode-lessons-in-structure** 原则技能）。

7. Hand back. Post the plan path and the script's output, then stop. Execution starts on the operator's explicit go, under the execution playbook the plan names.

> 7. 交回。贴出计划路径和脚本的输出，然后停下。执行要等操作者明确同意，并按计划指定的执行剧本进行。

**Verification.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked (the **prove-it-works** principle skill). That sentence is the verification rule. Every verification block opens with it. The live block is mandatory. Ten lanes at the PR head drive the real surface through its control skill, per the **swarm** skill, on the `swarm workers` model (default `grok-4.7-xhigh-fast`). Each lane is one box with a concrete scenario, the screenshot it saves, and its pass predicate. One lane is the **Regression lane against trunk.** It runs the same load-bearing scenario on trunk and head. If trunk does not have the feature, the lane records that fact and gates the behavior the diff adds plus the end state the user waits for instead of inventing a trunk result. The perf gate is dual-sided. Trunk and head must both produce the named metric. If trunk lacks the feature, also isolate the work the diff adds and set an absolute budget for that work plus the end-to-end state the user waits for. Do not claim a ratio between unlike scenarios. The perf block names the metric, the interleaved probe, the trunk baseline measured first, and the rule with the number that fails. A PR that changes an interaction is review-gated. The operator reviews it in chat with screenshots and a video before merge. A PR that changes no interaction writes `**Review gate.** None. <PR id> is not review-gated.` and no boxes under it.

> **验证。** 光有测试不足以算作验证。只有当一个 PR 的单元、实地和性能三项都打勾时，它才算验证通过（**prove-it-works** 原则技能）。这句话就是验证规则。每个验证块都以它开头。实地验证块是必须的。按 **swarm** 技能，在 PR 最新提交上用 `swarm workers` 模型（默认 `grok-4.7-xhigh-fast`）跑十条通道，通过控制技能操作真实界面。每条通道是一个勾选项，包含一个具体场景、它保存的截图和它的通过判定。其中一条是 **针对主干的回归通道。** 它在主干和最新提交上跑同一个关键场景。如果主干上没有这个功能，这条通道就记录这一事实，并改为把关 diff 新增的行为以及用户等待的最终状态，而不是编造一个主干结果。性能关卡是双边的。主干和最新提交都必须产出指定的指标。如果主干缺少这个功能，还要把 diff 新增的工作单独隔离出来，为这部分工作以及用户等待的端到端状态设定绝对预算。不要在不同的场景之间声称比例。性能块要写明指标、交替运行的探针、先测量的主干基线，以及会判定失败的数字和规则。改变交互的 PR 需要审查把关。操作者在合并前在聊天里看截图和视频进行审查。不改变交互的 PR 写 `**Review gate.** None. <PR id> is not review-gated.`，下面不放勾选项。

**Control skill.** Pick it by surface. Browser, Electron, and web UIs use `control-ui` from `cursor-team-kit`. CLIs and TUIs use `control-cli` from `cursor-team-kit`. Native mobile uses whatever simulator-driving skill the repo has. A PR that touches two surfaces gets lanes on both. A surface with no control skill is a risk in Appendix C, and its live block still names how each lane drives it.

> **控制技能。** 按界面来选。浏览器、Electron 和网页界面用 `cursor-team-kit` 里的 `control-ui`。命令行和终端界面用 `cursor-team-kit` 里的 `control-cli`。原生移动端用仓库里已有的模拟器操控技能。涉及两种界面的 PR 在两边都要有通道。没有控制技能的界面算作附录 C 里的风险，它的实地验证块仍然要写明每条通道如何操控它。

````markdown
# <Program> plan

<Under ten lines. What changes, for whom, the rule the program enforces, and the PR ids in order.>

## How to read this

One box is one unit of work. Every box names the evidence that checks it. A nested box is a sub-step of the box above it. Check a box only when its evidence exists, a file, a log line, a screenshot, a test run, or a SHA. The body is a how-to. The appendices explain and record.

The program runs `pstack/skills/poteto-mode/playbooks/<execution playbook>.md`. <Who merges, and which PR ids are the operator's items that stop at merge-ready.>

Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

## Program checklist

### Arm the program

- [ ] State the protocol and this plan to the operator, then stop. Start execution only on the operator's explicit go.
- [ ] Read these from trunk at program start. Re-read them at every tick.
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/<execution playbook>.md`
  - [ ] `git show origin/main:pstack/skills/swarm/SKILL.md`
  - [ ] `git show origin/main:<control skill path>`
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/opening-a-pr.md`
  - [ ] `git show origin/main:pstack/skills/<each other leaf skill the program uses>`
- [ ] On the operator's go, arm the audit tick as `/loop 1h` with the tick prompt below. Never leave the cadence to memory.
- [ ] Use this tick prompt, verbatim. "Re-read the execution playbook from trunk. Audit the operation against it and fix drift in this tick. Probe every active lane and judge progress by side effects only. Stand down a stuck lane and dispatch its replacement now. Then post a short status message to the operator in chat only when the audit found a tracked change that no earlier status message reported, such as a PR opened, a code-ready head, a round launched or closed, a verdict, a merge, a stuck agent and the action taken, a blocker added or cleared, or a decision only the operator can make. Name every such change and nothing else. Do not repeat a table, the merged list, or an unchanged blocker. If the audit found none, end the turn with no reply text. Either way, log this tick's row in your decision trail. The row names the items reported, or none."
- [ ] On the operator's hold or stand-down, send every owner a zero-writes order at once.

### Spawn owners

- [ ] Spawn one owner per PR with the full lifecycle the execution playbook names.
- [ ] Follow this dependency graph. Start dependent work only after its parent merges, or base it on the parent branch when the execution playbook stacks.
  - [ ] <PR id> and <PR id> are independent and first. Both branch from `main`.
  - [ ] <PR id> after <PR id>.
- [ ] Hold the file boundaries. <PR id or class> touches only `<glob>`.
- [ ] Hold the review gate. <PR ids> change an interaction. They wait for the operator's review in chat with screenshots and a video before merge.

### PR mechanics, for every PR

- [ ] Resolve the forge once. Default to `gh`; if `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Record any fallback to `gh`. Never require `gt`.
- [ ] Open the PR ready, never draft, per **Opening a PR**. Use the run's built-in PR tool when it has one, else `origin pr create --status open --base <base-branch>` or `gh pr create --base <base-branch>` according to the resolved forge. A stack child targets its parent branch.
- [ ] Run the repo's lint and typecheck once before the PR-facing push. Push with hooks on.
- [ ] Run `/deslop` before each commit and `/no-comments` before review.
- [ ] Triage every Bugbot and security-reviewer comment per `../references/bugbot-triage.md`.
- [ ] Rebase onto current trunk before the code-ready report and babysit. Keep that merge base in fix rounds. Rebase again only at merge prep, on a `git merge-tree` conflict with trunk, or on a CI failure that comes from a change on trunk.

### Verdict and merge, for every PR

- [ ] A full swarm runs once, at the code-ready head. Another full swarm runs only for new behavior, a redesign, or a large new diff. A rebase, a merge-conflict resolution, or a minor fix gets a single verifier at the new head. Run each full swarm per `pstack/skills/swarm/SKILL.md`. One gates lane. The ten live lanes from the PR's **Verify, live** block. The perf lane from its **Verify, perf** block. Two or more audit lanes, each with its own focus, that read the diff and the receipts and distrust the PR body. The root audits the receipts in the merge-ready report before the verdict.
- [ ] Clean only when every lane is `PASS`. Findings go back to the owner, including a defect that a lane filed as a note. A new head gets a fresh verdict, except for results that stay valid under the patch-id rule in `playbooks/shipping.md`.
- [ ] <The merge or append rule from the execution playbook, with the patch-id rule from `playbooks/shipping.md`.>

### Boot recipe, for every live lane

Each live lane runs on its own cloud VM at the PR head. Drive through `control-ui` or `control-cli` from `cursor-team-kit`.

- [ ] `git fetch origin <head-branch> && git checkout <head SHA>`.
- [ ] <Start the backend and the surface. Wait for ready.>
- [ ] <Deliver input only through the control skill's commands. Name the read-only diagnostics.>
- [ ] Save every screenshot to `/tmp/swarm-<pr-id>/worker-<n>/<slug>.png` and return the paths with the report.

## <Task as a verb phrase> (<PR id>)

**Depends on.** <PR id, or None.>

**Files.**

- [ ] Edit `<path>`.
- [ ] Create `<path>`.
- [ ] Delete `<path>`.

**Build.**

- [ ] <One change. Name the symbol and the file.>

**You see.**

- [ ] <One observable result, with the exact log line or screen state.>

**Verify, unit.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

- [ ] <Test file and the case it gains.> Run `<command>`.

**Verify, live.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked. Ten lanes on `<swarm workers model>` at the PR head, per the boot recipe.

- [ ] Lane 1. Regression lane against trunk. Run <the same load-bearing scenario> at trunk and head. If trunk lacks the feature, record that and gate <the behavior the diff adds plus the end state the user waits for>. Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 2. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 3. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 4. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 5. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 6. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 7. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 8. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 9. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 10. <Scenario.> Save `<slug>.png`. Pass when <predicate>.

**Verify, perf.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

- [ ] Metric. <What is measured at both trunk and head. If trunk lacks the feature, also name the diff-added work and the end-to-end state the user waits for.>
- [ ] Probe. <The command or procedure, run at trunk and at the head, interleaved. Both sides must produce the metric.>
- [ ] Baseline. Record the trunk <value> first.
- [ ] Rule. <Head against trunk, with the number that fails. If the scenarios differ, add absolute budgets for the diff-added work and the user-visible end state instead of an invalid ratio.>

**Review gate.** The operator reviews before merge.

- [ ] Copy lane <n> screenshots into `<media path>/<pr-id>-review-<slug>.png`.
- [ ] Record a 30 to 60 second video of the change on a lane VM. Save it as `<media path>/<pr-id>-review.mp4`.
- [ ] Post the screenshots and the video in chat. Stop at merge-ready. Wait for the operator's click.

**Merge.**

- [ ] Root's clean verdict at the exact head SHA.
- [ ] Bugbot triage done.
- [ ] Rebased onto current trunk after the verdict, patch-id unchanged.
- [ ] <The owner squash-merges its own PR, or the root appends it to the base-branch stack and the operator lands it bottom-up.>

## Close the program

- [ ] Every box above is checked with its evidence.
- [ ] Reply to the operator with the report the execution playbook names.

## Appendix A. Prototype evidence

<Each open question a prototype answered, with the branch, the SHA, and the artifact links. Each question that stays unproven.>

## Appendix B. Alternatives rejected

<Each approach weighed and why it lost.>

## Appendix C. Risks

<Each risk with the PR it lands in and what the owner watches.>

## Appendix D. Links and reading list

<Docs to read before editing. Which PRs get `pstack/skills/how/SKILL.md` and `pstack/skills/interrogate/SKILL.md`. The trail per `pstack/skills/show-me-your-work/SKILL.md`.>
````

> 上面代码块的中文版：

````markdown
# <计划名称> 计划

<不超过十行。改变什么、为谁改变、这个计划强制执行的规则，以及按顺序排列的 PR 编号。>

## 如何阅读本计划

一个勾选项是一个工作单元。每个勾选项都写明了用来核对它的证据。嵌套的勾选项是上一项的子步骤。只有当证据存在时才打勾：一个文件、一行日志、一张截图、一次测试运行或一个 SHA。正文是操作指南。附录用于解释和记录。

本计划按 `pstack/skills/poteto-mode/playbooks/<执行剧本>.md` 运行。<由谁合并，以及哪些 PR 编号是操作者自己的事项、停在可合并状态。>

光有测试不足以算作验证。只有当一个 PR 的单元、实地和性能三项都打勾时，它才算验证通过。

## 计划总清单

### 启动计划

- [ ] 向操作者说明流程和本计划，然后停下。只有在操作者明确同意后才开始执行。
- [ ] 在计划开始时从主干读取以下内容。每一轮都重新读。
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/<执行剧本>.md`
  - [ ] `git show origin/main:pstack/skills/swarm/SKILL.md`
  - [ ] `git show origin/main:<控制技能路径>`
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/opening-a-pr.md`
  - [ ] `git show origin/main:pstack/skills/<计划用到的其他每个原文技能>`
- [ ] 操作者同意后，用下面的轮次提示词开启 `/loop 1h` 审计。永远不要靠记忆维持节奏。
- [ ] 原样使用这段轮次提示词。“从主干重读执行剧本。对照它审计当前运作，在这一轮里修正偏移。探查每条活跃的通道，只根据副作用判断进展。让卡住的通道停下，立刻派出替代者。然后，只有当审计发现了一个之前任何状态消息都没报告过的被跟踪的变化时，才在聊天里给操作者发一条简短的状态消息，比如开了一个 PR、出现了代码就绪的最新提交、一轮开始或结束、一个结论、一次合并、一个卡住的代理及采取的行动、新增或解除了一个阻塞项，或者一个只有操作者才能做的决定。列出每一个这样的变化，别的什么都不写。不要重复表格、已合并列表或没有变化的阻塞项。如果审计什么都没发现，就不写回复文字结束这一轮。无论哪种情况，都在你的决策记录里记下这一轮的一行。这一行写明报告了哪些事项，或者写“无”。”
- [ ] 操作者要求暂停或撤下时，立刻向每个负责人发出零写入命令。

### 派生负责人

- [ ] 每个 PR 派生一个负责人，负责执行剧本所规定的完整生命周期。
- [ ] 遵循这张依赖图。依赖别人的工作只在其父 PR 合并后才开始；如果执行剧本是堆栈式的，就以父分支为基础。
  - [ ] <PR 编号> 和 <PR 编号> 相互独立，最先开始。都从 `main` 开分支。
  - [ ] <PR 编号> 在 <PR 编号> 之后。
- [ ] 守住文件边界。<PR 编号或类别> 只碰 `<glob>`。
- [ ] 守住审查关卡。<PR 编号> 改变了交互。它们在合并前要等操作者在聊天里看截图和视频进行审查。

### PR 操作，每个 PR 都适用

- [ ] 确定一次托管平台。默认用 `gh`；如果 `command -v origin` 成功并且 Origin 能识别这个仓库，所有 PR 操作都用 `origin pr`。记录任何回退到 `gh` 的情况。永远不要依赖 `gt`。
- [ ] 按 **Opening a PR** 以就绪状态打开 PR，永远不开草稿。运行环境有内置 PR 工具就用它，否则按确定好的托管平台用 `origin pr create --status open --base <基础分支>` 或 `gh pr create --base <基础分支>`。栈里的子 PR 以其父分支为目标。
- [ ] 在面向 PR 的推送之前，跑一次仓库的 lint 和类型检查。推送时保持 hook 开启。
- [ ] 每次提交前运行 `/deslop`，审查前运行 `/no-comments`。
- [ ] 按 `../references/bugbot-triage.md` 分流每一条 Bugbot 和安全审查的评论。
- [ ] 在报告代码就绪和看护之前，变基到当前主干。修复轮次里保持那个合并基础。只有在合并准备时、与主干出现 `git merge-tree` 冲突时，或者 CI 失败是由主干上的变化引起时，才再次变基。

### 结论与合并，每个 PR 都适用

- [ ] 完整的 swarm 只在代码就绪的最新提交上跑一次。只有新行为、重新设计或很大的新 diff 才再跑一次完整的 swarm。变基、解决合并冲突或小修复，在新的最新提交上只派一个验证者。每次完整的 swarm 都按 `pstack/skills/swarm/SKILL.md` 运行。一条关卡通道。PR 的 **Verify, live** 块里的十条实地通道。它的 **Verify, perf** 块里的性能通道。两条或更多审计通道，各有侧重，阅读 diff 和凭据，不信任 PR 正文。根代理在给出结论之前审计可合并报告里的凭据。
- [ ] 只有每条通道都是 `PASS` 时才算干净。发现交回给负责人，包括被某条通道记成备注的缺陷。新的最新提交要拿新的结论，除非结果按 `playbooks/shipping.md` 里的 patch-id 规则仍然有效。
- [ ] <执行剧本里的合并或追加规则，加上 `playbooks/shipping.md` 里的 patch-id 规则。>

### 启动步骤，每条实地通道都适用

每条实地通道都在 PR 最新提交上、在它自己的云端虚拟机里运行。通过 `cursor-team-kit` 里的 `control-ui` 或 `control-cli` 操控。

- [ ] `git fetch origin <最新提交所在分支> && git checkout <最新提交 SHA>`。
- [ ] <启动后端和界面。等待就绪。>
- [ ] <只通过控制技能的命令输入。写明只读的诊断手段。>
- [ ] 把每张截图保存到 `/tmp/swarm-<pr-id>/worker-<n>/<slug>.png`，随报告返回路径。

## <用动词短语描述的任务>（<PR 编号>）

**依赖。** <PR 编号，或“无”。>

**文件。**

- [ ] 编辑 `<路径>`。
- [ ] 创建 `<路径>`。
- [ ] 删除 `<路径>`。

**构建。**

- [ ] <一个改动。写明符号和文件。>

**你会看到。**

- [ ] <一个可观察的结果，附上确切的日志行或屏幕状态。>

**验证，单元。** 光有测试不足以算作验证。只有当一个 PR 的单元、实地和性能三项都打勾时，它才算验证通过。

- [ ] <测试文件以及它新增的用例。> 运行 `<命令>`。

**验证，实地。** 光有测试不足以算作验证。只有当一个 PR 的单元、实地和性能三项都打勾时，它才算验证通过。按启动步骤，在 PR 最新提交上用 `<swarm workers 模型>` 跑十条通道。

- [ ] 通道 1。针对主干的回归通道。在主干和最新提交上跑 <同一个关键场景>。如果主干缺少这个功能，记录下来，并改为把关 <diff 新增的行为以及用户等待的最终状态>。保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 2。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 3。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 4。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 5。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 6。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 7。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 8。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 9。<场景。> 保存 `<slug>.png`。<判定> 时通过。
- [ ] 通道 10。<场景。> 保存 `<slug>.png`。<判定> 时通过。

**验证，性能。** 光有测试不足以算作验证。只有当一个 PR 的单元、实地和性能三项都打勾时，它才算验证通过。

- [ ] 指标。<在主干和最新提交上都测量什么。如果主干缺少这个功能，还要写明 diff 新增的工作以及用户等待的端到端状态。>
- [ ] 探针。<命令或步骤，在主干和最新提交上交替运行。两边都必须产出该指标。>
- [ ] 基线。先记录主干的 <值>。
- [ ] 规则。<最新提交对比主干，以及判定失败的数字。如果场景不同，就为 diff 新增的工作和用户可见的最终状态设定绝对预算，而不是用一个无效的比例。>

**审查关卡。** 操作者在合并前审查。

- [ ] 把通道 <n> 的截图复制到 `<媒体路径>/<pr-id>-review-<slug>.png`。
- [ ] 在某条通道的虚拟机上录一段 30 到 60 秒的改动视频。保存为 `<媒体路径>/<pr-id>-review.mp4`。
- [ ] 在聊天里贴出截图和视频。停在可合并状态。等操作者点击。

**合并。**

- [ ] 根代理在确切的最新提交 SHA 上给出干净的结论。
- [ ] Bugbot 分流完成。
- [ ] 结论之后已变基到当前主干，patch-id 没有变化。
- [ ] <负责人 squash 合并自己的 PR，或者根代理把它追加到基础分支栈上，由操作者从底向上合并。>

## 结束计划

- [ ] 上面的每个勾选项都已打勾并有证据。
- [ ] 用执行剧本规定的报告回复操作者。

## 附录 A. 原型证据

<每个由原型回答了的开放问题，附上分支、SHA 和产物链接。每个仍未证实的问题。>

## 附录 B. 被否决的备选方案

<每个权衡过的做法，以及它为什么落选。>

## 附录 C. 风险

<每个风险，以及它落在哪个 PR 里、负责人要盯着什么。>

## 附录 D. 链接和阅读清单

<编辑前要读的文档。哪些 PR 要用 `pstack/skills/how/SKILL.md` 和 `pstack/skills/interrogate/SKILL.md`。按 `pstack/skills/show-me-your-work/SKILL.md` 留下的决策记录。>
````

**Reply:** the plan path, the PR ids with their dependencies and the review-gated set, what the prototypes proved and what stays unproven, and the check script's output.

> **回复：** 计划路径、PR 编号及其依赖关系和需要审查把关的集合、原型证明了什么以及什么仍未证实，以及检查脚本的输出。
