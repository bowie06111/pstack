<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/autopilot-full.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/autopilot-full.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/autopilot-full.md)

### Autopilot-full｜完全自动驾驶

**You own the verdicts, never the PRs. One owner carries each PR from build to merge, and nothing merges until your swarm, which never wrote the code, has proven the code that lands.**

> **你负责结论，绝不负责 PR。每个 PR 由一个负责人从构建一直带到合并，在你那个从没写过这些代码的 swarm 证明了将要落地的代码之前，什么都不合并。**

A verdict covers one patch and green CI covers one head. Step 4 runs a full swarm once, and again only for a major change. The patch-id rule in `playbooks/shipping.md` can carry a lane's result, and every new head needs its own green CI.

> 一个结论只覆盖一个补丁，一次 CI 全绿只覆盖一个最新提交。第 4 步只跑一次完整的 swarm，只有重大改动才再跑。`playbooks/shipping.md` 里的 patch-id 规则可以延续某个验证通道的结果，而每个新的最新提交都需要它自己的 CI 全绿。

1. **The operator's gates come first.** The grant covers only the queue's items, so items the operator names as theirs and PRs outside the queue stop at merge-ready. A request to state the plan is not a go.

> 1. **操作者设的关卡优先。** 授权只覆盖队列里的事项，所以操作者点名归自己的事项，以及队列之外的 PR，都停在可合并状态。要求说明计划不等于同意开工。

2. **Spawn one owner per PR, and keep its work where a replacement can find it.** Resolve the forge once for the program. GitHub CLI (`gh`) is the default. If `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Otherwise stay on `gh` and record the fallback. Never require Graphite (`gt`). Each owner is a Cursor cloud agent that builds, opens its PR per `playbooks/opening-a-pr.md`, proves the change on the real artifact, babysits to green per `playbooks/babysit.md`, and merges once steps 4 and 5 clear it.

> 2. **每个 PR 派生一个负责人，并把它的工作放在替代者能找到的地方。** 为整个计划确定一次托管平台。默认用 GitHub CLI（`gh`）。如果 `command -v origin` 成功并且 Origin 能识别这个仓库，所有 PR 操作都用 `origin pr`。否则继续用 `gh`，并记录这次回退。永远不要依赖 Graphite（`gt`）。每个负责人都是一个 Cursor 云端代理，负责构建、按 `playbooks/opening-a-pr.md` 开 PR、在真实产物上证明改动、按 `playbooks/babysit.md` 看护到全绿，并在第 4 和第 5 步放行后合并。

   Open the PR early so CI and bots run alongside the work. The code is ready only after the slop-strip (the `deslop` skill from the `cursor-team-kit` plugin (`/deslop`)) and the `/no-comments` pass, so the full swarm runs on finished code. Then rebase onto current trunk and report the code-ready head before self-proof, so verification runs on the base the patch lands on. Report every later head, and merge-ready once self-proof, CI, and babysit are done. Fix rounds stay on that base unless trunk conflicts or breaks CI, because each rebase restarts CI. Run the repo's checks for the touched paths on the committed head before the code-ready push and before a later push that needs a verifier.

>    尽早开 PR，让 CI 和机器人和工作同步运行。只有做完去垃圾处理（`cursor-team-kit` 插件里的 `deslop` 技能，`/deslop`）和 `/no-comments` 之后，代码才算就绪，这样完整的 swarm 跑在完成的代码上。然后变基到当前主干，并在自我证明之前报告代码就绪时的最新提交，这样验证跑在补丁将要落地的基础上。之后的每个最新提交都要报告，自我证明、CI 和看护都完成后报告可合并。修复轮次留在那个基础上，除非主干有冲突或弄坏了 CI，因为每次变基都会让 CI 重跑。在代码就绪的推送之前，以及之后需要验证者的推送之前，在已提交的最新提交上跑一遍仓库针对改动路径的检查。

   Assume the VM dies. Soon after starting, push and start a decision trail in the root agent's store (path in the system prompt) per the **show-me-your-work** skill, then push after every unit worth keeping. Whoever spawns a subagent records its id, state, and a stuck-after runtime from past runs of its kind in that store, so the root can probe it if the parent dies. Force-push only your own branch, with `--force-with-lease` pinned to the SHA you last pushed or took over, and on any push you did not make, stop and report to the root.

>    假设虚拟机会挂掉。开始后不久就推送，并按 **show-me-your-work** 技能在根代理的存储（路径见系统提示）里开始写决策记录，之后每完成一个值得保留的单元就推送。谁派生子代理，谁就在那个存储里记下它的 id、状态，以及根据同类任务以往运行得出的“超过多久算卡住”的时长，这样父代理挂掉时根代理可以去探查它。只强制推送你自己的分支，用 `--force-with-lease` 并固定到你上次推送或接手时的 SHA；遇到任何不是你做的推送，停下并报告根代理。

3. **Run owners in parallel off trunk, never stacked,** so each PR lands on its own.

> 3. **负责人从主干并行工作，永远不堆叠，** 这样每个 PR 都能单独落地。

4. **Swarm-verify once.** A full swarm runs once, at the code-ready head. Another full swarm runs only for new behavior, a redesign, or a large new diff. A rebase, a merge-conflict resolution, or a minor fix gets a single verifier at the new head. Fan out independent verifiers at the code-ready SHA per the **swarm** skill. They re-run the gates, prove the load-bearing behavior live on the real surface through its control skill (such as `control-ui` or `control-cli` from `cursor-team-kit`) or a named driver, and audit the diff. The verdict is not clean without the live lane, or before you audit the merge-ready report's receipts, because no lane trusts a claim it has not checked.

> 4. **只做一次 swarm 验证。** 完整的 swarm 只跑一次，在代码就绪的最新提交上。只有新行为、重新设计或很大的新 diff 才再跑一次完整的 swarm。变基、解决合并冲突或小修复，在新的最新提交上只派一个验证者。按 **swarm** 技能，在代码就绪的 SHA 上扇出独立的验证者。他们重跑各道关卡，通过对应的控制技能（比如 `cursor-team-kit` 里的 `control-ui` 或 `control-cli`）或一个指定的驱动程序，在真实界面上实地证明关键行为，并审计 diff。没有实地验证通道，或者在你审计可合并报告里的凭据之前，结论都不算干净，因为任何通道都不信任它没检查过的论断。

   Send every proven finding, whatever a lane labeled it, to the owner in one fix-forward, with a red test across every site with the same defect. A defect trunk shows without the PR is recorded against trunk and does not gate the PR, but one the PR copies or newly reaches is the PR's own. Never push a fix yourself, because each branch has one writer. Replace a stuck owner instead.

>    把每个经过证明的发现，不管通道给它贴了什么标签，一次性交给负责人做前向修复，并附上一个覆盖所有同类缺陷位置的失败测试。如果某个缺陷在没有这个 PR 的主干上也存在，就记在主干名下，不阻塞这个 PR；但如果是 PR 复制过来的或新触及到的，就归这个 PR。永远不要自己推送修复，因为每个分支只有一个写入者。负责人卡住了就换掉它。

5. **The owner merges only what you verified, tested on the trunk it lands on.** Green CI alone never authorizes a merge. Once the swarm's lanes have started, merge prep rebases onto current trunk, reports the new head, and waits for green CI.

> 5. **负责人只合并你验证过、并在它将要落地的主干上测试过的东西。** 单靠 CI 全绿永远不能授权合并。swarm 的各通道开始后，合并准备工作会变基到当前主干、报告新的最新提交，然后等 CI 全绿。

   Right before merging, fetch trunk and ask whether its changes can affect the patch, because rebasing for every trunk move never ends. Rebase and wait for CI when the code that would land was never tested, as with a conflict, a file both sides changed, or a change to which checks run for the PR. A lockfile both sides changed needs no rebase when a frozen install passes on the merge result and the packages the PR changes resolve there as on the head, because that install tests the lockfile that would land, and a failing install means the rebase must repair it. When trunk changed a caller of the patch or something the patch depends on, such as code it calls or a dependency version, run the patch's checks on the merge result and merge on a pass. Otherwise merge. If trunk would force a third rebase past merge prep, hold and report to the root, which sequences the colliding work.

>    就在合并之前，拉取主干，判断它的变化会不会影响这个补丁，因为主干每动一次就变基一次是没有尽头的。如果将要落地的代码从没被测试过，比如有冲突、双方都改了同一个文件，或者改动影响了这个 PR 要跑哪些检查，就变基并等 CI。如果双方都改了 lockfile，只要在合并结果上冻结安装能通过、并且 PR 改动的包在那里解析得和在最新提交上一样，就不需要变基，因为这次安装测试的正是将要落地的 lockfile；安装失败则说明必须通过变基来修复它。如果主干改了这个补丁的调用方，或补丁依赖的东西（比如它调用的代码或某个依赖的版本），就在合并结果上跑这个补丁的检查，通过就合并。否则直接合并。如果主干会迫使合并准备之后再做第三次变基，就暂停并报告根代理，由它来安排相互冲突的工作的先后。

6. **Run the root layer.** Anything that loosens a gate, such as a raised budget or an allowlist entry, needs a verifier's proof and then your fresh countersign, because the owner is the one who wants green. Under a grant that covers approvals, that countersign is the approval, and the owner's record of it points at your countersign for a lane to check. A disabled or soft-failed check always comes out, because it stops guarding every case. Never give or bypass an approval the forge enforces, because no countersign covers it.

> 6. **运行根代理这一层。** 任何放松关卡的事情，比如提高预算或加一条白名单，都需要验证者的证明，再加上你新的副署，因为想要全绿的正是负责人。在覆盖审批的授权下，那个副署就是审批，负责人对它的记录要指向你的副署，供验证通道核查。被禁用或被设为软失败的检查一律要恢复，因为它不再守护任何情况。永远不要给出或绕过托管平台强制要求的审批，因为没有任何副署能覆盖它。

   On the operator's go, arm `/loop 1h` to audit until no delegated work is left, because long runs drift and agents stall silently. Each tick re-reads this playbook from trunk and fixes drift, such as an unreported head or an unrecorded agent, by having its owner record it, because drift is a gap in the record, not a stall. It probes every agent, down to the leaf lanes, and judges each by its side effects, because status text and summaries can hide a stuck child. An agent is stuck when it errors, cannot start turns, or shows nothing new past its stuck-after runtime, and so is an owner that has neither pushed nor written a trail row by the first tick. One waiting on you or the operator is not stuck, so answer it. Replace a stuck agent at once, without waiting for its stop to land. A tick that changed something for the operator ends by catching them up per the **ketchup** skill, and one that changed nothing stays quiet.

>    操作者同意后，开启 `/loop 1h` 进行审计，直到没有委派出去的工作为止，因为长时间运行会偏移，代理也会悄无声息地卡住。每一轮都从主干重读本剧本并修正偏移，比如未报告的最新提交或未记录的代理，做法是让它的负责人补记，因为偏移是记录上的缺口，不是卡住。它探查每一个代理，一直到最底层的验证通道，并根据副作用来判断每一个，因为状态文字和总结可能掩盖一个卡住的子代理。一个代理出错、无法开始回合，或者超过它“算卡住”的时长后没有任何新进展，就是卡住了；到第一轮时既没推送也没写过一行记录的负责人也算卡住。在等你或操作者的代理不算卡住，所以要回应它。卡住的代理立刻替换，不用等它的停止生效。某一轮为操作者带来了变化的，最后按 **ketchup** 技能让操作者跟上进度；什么都没变的一轮保持安静。

7. **Stand down at once on the operator's stop.** A hold reaches every owner as a zero-writes order. Each reports where it stands and keeps its brief until the operator releases it.

> 7. **操作者喊停就立刻停下。** 暂停令以“零写入”命令的形式传达给每个负责人。每个负责人报告自己所处的位置，并保留自己的任务说明，直到操作者解除暂停。

**Reply:** catch the operator up per the **ketchup** skill, covering each PR's state and verdict, what merged, countersigns and why, and open operator gates, with links to each PR and the trails.

> **回复：** 按 **ketchup** 技能让操作者跟上进度，包括每个 PR 的状态和结论、合并了什么、副署了什么以及原因，以及仍待操作者决定的关卡，附上每个 PR 和各决策记录的链接。
