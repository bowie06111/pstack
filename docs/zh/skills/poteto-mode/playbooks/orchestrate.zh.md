<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/orchestrate.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/orchestrate.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/orchestrate.md)

### Orchestrate｜编排

**You own the program, never the code. Author briefs, drain the queue, keep the frontier green, decide.** For a whole project handed to one standing coordinator chat: multi-day, many stacked PRs, dozens to hundreds of subagents, the human checking in twice a day instead of every five minutes. One task driven to a predicate is Autonomous run. One ambitious run needing a bespoke workflow is figure-it-out. Route here when the work outlives any single agent. Work one agent could finish inside the session's budget is not a program.

> **你负责计划，绝不负责代码。写任务说明，清空队列，保持前沿全绿，做决定。** 用于交给一个常驻协调者对话的整个项目：多日、许多堆叠的 PR、几十到几百个子代理，人一天来看两次而不是每五分钟看一次。把一个任务推进到一个判定条件属于 Autonomous run。一次需要定制工作流的雄心勃勃的运行属于 figure-it-out。当工作比任何单个代理都活得更久时，才分派到这里。一个代理在会话预算内就能做完的工作不算项目级计划。

Ceremony must scale with the program. On cheap near-identical units, collapse it as each section directs.

> 仪式感必须和计划的规模相称。对于便宜而且几乎相同的单元，按各节的指示把它压缩掉。

Three rules carry the rest.

> 三条规则撑起其余所有内容。

- Completions are queue events, not interrupts.

> - 完成通知是队列事件，不是中断。

- Every spawn and every resume carries the standing orders verbatim.

> - 每次派生和每次恢复都原样携带常驻命令。

- The brief is the product. A vague brief fails quietly, because a worker cannot ask you a question.

> - 任务说明就是产品。模糊的任务说明会悄无声息地失败，因为工作者没法问你问题。

#### Roles and placement｜角色和放置位置

- **Coordinator (this chat).** Local. Frames, authors briefs, drains the inbox, owns the human report, makes judgment calls. It never authors or edits code. Conflicted merges, chain rebases, and code changes are always tasks. Mechanically landing a verified unit (fast-forward or clean cherry-pick of a worker's commit, then push) is bookkeeping the coordinator may do itself on repos where local git is cheap. Queueing finished work behind an idle stacker is how a deadline harvests nothing. The loop is agentic end to end. Agents are spawned, resumed, and drained only through the Task tool. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section. State reads and writes go through `scripts/orch/orch.ts` at drain points, one command in and one line out. The CLI never spawns, waits, or wakes anything.

> - **协调者（本对话）。** 本地运行。负责框定、写任务说明、清空收件箱、负责给人的报告、做判断。它从不编写或修改代码。有冲突的合并、链式变基和代码改动永远是任务。在本地 git 操作很便宜的仓库里，机械地落地一个已验证的单元（快进或干净地 cherry-pick 某个工作者的提交，然后推送）属于记账工作，协调者可以自己做。把完成的工作排在一个空闲的堆栈者后面，正是截止时一无所获的原因。整个循环从头到尾都是智能代理式的。代理只通过 Task 工具来派生、恢复和清空。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。状态的读写在清空点通过 `scripts/orch/orch.ts` 进行，一条命令进，一行输出。这个 CLI 从不派生、等待或唤醒任何东西。

- **Sub-coordinator.** Always local, durable, one per track, and only when the program exceeds what one coordinator's drains can manage. A track the coordinator can drain itself needs no middle layer. Each nested layer re-pays a full orientation preamble, and a blocking sub-coordinator hides its children while the parent idles. Owns its track's units and boards, authors its workers' briefs, spawns its own workers and verifiers (nesting works to depth 3, and a nested spawn has the full Task schema including `environment`). Rolls up aggregates at wave boundaries. Never forwards raw child reports. Cap in-flight children at what one drain can process, roughly ten, as a rolling window. Never as blocking batches, which cost the slowest child of every batch.

> - **子协调者。** 永远在本地、持久运行，每条轨道一个，并且只在计划超出一个协调者的清空能力时才设。协调者自己能清空的轨道不需要中间层。每多嵌套一层，就要重新付一次完整的熟悉情况的开销，而一个阻塞的子协调者会把它的子代理藏起来，让父代理闲着。它负责自己轨道的单元和看板，为自己的工作者写任务说明，派生自己的工作者和验证者（嵌套可以到 3 层，嵌套派生拥有完整的 Task 参数，包括 `environment`）。在波次边界汇总。从不转发子代理的原始报告。同时在途的子代理数量上限是一次清空能处理的量，大约十个，以滚动窗口的方式进行。永远不要用阻塞的批次，那样每一批都要等最慢的那个子代理。

- **Worker / verifier.** Always `environment: "cloud"` unless the task needs this machine: `control-ui` or `control-cli` runtime verification (from `cursor-team-kit`). Reading local transcripts under `agent-transcripts/`. Simulators and local IDE state. Auth that exists only here. Cloud agents cannot read the local store, so their briefs inline what they need or point at repo paths. Prefer fewer, broader workers. One writer per worktree or branch (principle-separate-before-serializing-shared-state). Run a unit's verifier on a different model family from its worker.

> - **工作者 / 验证者。** 永远用 `environment: "cloud"`，除非任务需要这台机器：`control-ui` 或 `control-cli` 的运行时验证（来自 `cursor-team-kit`）、读取 `agent-transcripts/` 下的本地对话记录、模拟器和本地 IDE 状态、只存在于本机的认证。云端代理读不了本地存储，所以它们的任务说明要内联所需内容，或者指向仓库路径。宁可要更少、范围更广的工作者。每个 worktree 或分支只有一个写入者（principle-separate-before-serializing-shared-state）。单元的验证者要用和它的工作者不同的模型家族。

Depth stays at coordinator, track, worker. Author the track decomposition per project (build, landing, and verification are common cuts, not a required shape). Hard-coded swarm trees were tried and parked as too rigid.

> 层级保持在协调者、轨道、工作者三层。按项目拆分轨道（构建、落地和验证是常见的切法，不是必须的形状）。写死的 swarm 树试过，因为太僵硬被搁置了。

#### Store layout｜存储布局

Create `orchestrate/<project-slug>/` in the current agent's store (path in the system prompt). Every file has exactly one writer. Owners publish facts, readers aggregate at read time. Use `bun scripts/orch/orch.ts` for bookkeeping, written below as `orch`, while its canonical plain TSV and JSON stay readable without the CLI.

> 在当前代理的存储（路径见系统提示）里创建 `orchestrate/<project-slug>/`。每个文件只有一个写入者。负责人发布事实，读者在读取时汇总。用 `bun scripts/orch/orch.ts` 记账，下面写作 `orch`，而它的规范格式是普通的 TSV 和 JSON，不用 CLI 也能读。

- `preferences.md` is the standing-orders register: numbered lines, one constraint each (model policy, stack shape and count, verification bar, forbidden paths, escalation policy). Paste it verbatim into every spawn and every resume. Directives decay across resumes, and each dropped one costs a human turn. When you catch yourself restating an instruction, append the line before you act (principle-encode-lessons-in-structure).

> - `preferences.md` 是常驻命令登记表：带编号的行，每行一条约束（模型策略、栈的形状和数量、验证标准、禁止的路径、升级策略）。把它原样贴进每次派生和每次恢复。指示会在多次恢复之间衰减，每丢一条就要花人的一个回合。当你发现自己在重复一条指示时，先把这一行追加进去再行动（principle-encode-lessons-in-structure）。

- `overview.md` is the durable PR and issue DB. Append. Never rewrite wholesale per event.

> - `overview.md` 是持久的 PR 和问题数据库。只追加。永远不要每个事件就整体重写一遍。

- `units.tsv` has one row per unit: id, track, state, branch, PR, head SHA, brief path. Update rows in place.

> - `units.tsv` 每个单元一行：编号、轨道、状态、分支、PR、最新提交 SHA、任务说明路径。原地更新行。

- `frontier.json` is the computed merge frontier, per Stack safety.

> - `frontier.json` 是计算出来的合并前沿，见“栈的安全”。

- `ledger.tsv` is the verification ledger, per Verification.

> - `ledger.tsv` 是验证账本，见“验证”。

- `inbox/` holds completion pointers. `gates.md` parks human gates (question, options, default on no answer).

> - `inbox/` 存放完成通知的指针。`gates.md` 停放需要人决定的关卡（问题、选项、无人回答时的默认值）。

- `decisions.tsv` is the trail via the show-me-your-work skill.

> - `decisions.tsv` 是通过 show-me-your-work 技能留下的决策记录。

- `status.md` is derived from `units.tsv` and `ledger.tsv` at each drain, never hand-maintained. Regenerate it from the tables instead of narrating events into it.

> - `status.md` 在每次清空时由 `units.tsv` 和 `ledger.tsv` 推导生成，从不手工维护。从表格重新生成它，而不是往里面叙述事件。

#### The brief｜任务说明

Your prompts to agents are your only product, and a sloppy brief compounds into slop across the whole tree. Every spawn carries all of it. A field you cannot fill is a unit you have not scoped yet.

> 你给代理的提示词是你唯一的产品，一份草率的任务说明会在整棵树上累积成垃圾。每次派生都携带下面全部内容。填不出来的字段，说明这个单元你还没界定清楚。

```
GOAL         one sentence, the outcome, executable by a stranger with no chat access
SCOPE        paths this unit may write; paths it may not; its exclusive worktree or branch
CONTEXT      pointers to files and PRs; upstream reports pasted in full when this unit
             depends on them, because workers cannot see siblings
ACCEPTANCE   checkable criteria, one per line
VERIFY       exact commands or the control-skill path, plus known gotchas
TIMEBOX      rough cap on runtime; on expiry, return partial findings and stop rather than run on
FORBIDDEN    no PR retargets or merges, no rebase, no force-push, no fixes outside scope,
             plus unit-specific bans
REPORT       status, branch, head SHA, PRs, verdict, what you actually ran, deviations,
             suggested follow-ups
STANDING     <preferences.md pasted verbatim>
```

> 上面代码块的中文版：

```
GOAL（目标）        一句话，说明结果，让一个看不到聊天记录的陌生人也能执行
SCOPE（范围）       这个单元可以写的路径；不可以写的路径；它独占的 worktree 或分支
CONTEXT（上下文）   指向文件和 PR 的指针；当这个单元依赖上游报告时，把它们完整贴进来，
                    因为工作者看不到兄弟单元
ACCEPTANCE（验收）  可检查的标准，每行一条
VERIFY（验证）      确切的命令或控制技能路径，以及已知的坑
TIMEBOX（时限）     运行时间的大致上限；到时就返回部分发现并停下，而不是继续跑
FORBIDDEN（禁止）   不改 PR 目标分支、不合并、不变基、不强制推送、不做范围外的修复，
                    以及这个单元特有的禁令
REPORT（报告）      状态、分支、最新提交 SHA、PR、结论、你实际运行了什么、偏差、
                    建议的后续工作
STANDING（常驻）    <原样贴入 preferences.md>
```

Size the brief to the unit. A one-command unit gets the template collapsed to a paragraph that still names goal, scope, the verify command, and the report shape. A 4KB scaffold around a two-line edit costs more to write and obey than the edit. Local spawns may reference the standing-orders file by store path. Verbatim paste is for cloud spawns and every resume.

> 任务说明的篇幅要和单元相称。只有一条命令的单元，把模板压缩成一段话，但仍要写明目标、范围、验证命令和报告格式。为一个两行的修改搭一个 4KB 的架子，写起来和遵守起来都比修改本身还费事。本地派生可以通过存储路径引用常驻命令文件。原样粘贴用于云端派生和每次恢复。

A sub-coordinator brief adds its track boundary and unit list, its spawn budget with the cloud default and the local exception list, the drain protocol, and the rollup format (per child: name, status, PR, head SHA, verdict, one line, plus track status and frontier delta).

> 子协调者的任务说明还要加上：它的轨道边界和单元列表；它的派生预算，包括默认用云端以及本地例外清单；清空协议；以及汇总格式（每个子代理：名字、状态、PR、最新提交 SHA、结论、一行说明，再加上轨道状态和前沿的变化）。

A dependency is a context relay, not just ordering. Undeclared upstream context makes the worker guess. Missing fields are a refuse-to-spawn condition. Audit one sampled worker brief per sub-coordinator per wave, concurrently with the wave it samples, never as a gate in front of it. A failing brief stops that track and fixes the sub-coordinator's instructions, not just the worker, because brief quality decays late in a run. Never resume-chain a brief. Respawn fresh with consolidated scope.

> 依赖是上下文的传递，而不只是顺序。没有声明上游上下文，工作者就只能猜。缺少字段就拒绝派生。每一波，对每个子协调者抽查一份工作者的任务说明，和被抽查的那一波同时进行，永远不要把它当作挡在前面的关卡。不合格的任务说明会让那条轨道停下，并修正子协调者的指示，而不只是修正那个工作者，因为任务说明的质量在运行后期会衰减。永远不要用恢复来串联任务说明。用合并后的范围重新派生。

#### Steps｜步骤

1. **Frame.** State the done predicate as something countable ("all 126 units merged, each ledger-verified `unit-test-verified` or better"). Quantify scope: units, rough effort, expected stacks, and the wall-clock budget. If one agent could finish inside that budget, stop here and run Autonomous run instead. Collapsing must not depend on another document being present. It means do the work directly in this session, plain workers where they help, verification inline, landing as you go, and none of the store, register, or pilot machinery below. Schedule landing against the budget. By roughly 70% of it, stop spawning and land what is verified. Name the tracks per project. A contested decomposition or one-way door goes through the arena skill before the pilot. Present the framing once. Reversible prep proceeds without waiting.

> 1. **框定。** 把完成判定写成可以计数的东西（“全部 126 个单元已合并，每个在账本上都验证为 `unit-test-verified` 或更好”）。量化范围：单元数、大致工作量、预期的栈数，以及实际时间预算。如果一个代理在这个预算内就能做完，就到此为止，改用 Autonomous run。压缩不能依赖其他文档存在。它的意思是：在本会话里直接干活，需要时用普通的工作者，验证就地完成，边做边落地，下面的存储、登记表和试点机制统统不要。按预算安排落地。用到大约 70% 时，停止派生，落地已验证的内容。按项目命名轨道。有争议的拆分或单向门决策，在试点之前先过 arena 技能。框定只展示一次。可逆的准备工作不用等待直接进行。

2. **Install the runtime.** Run `orch init`. Open the trail via the show-me-your-work skill, write the standing orders before any spawn, and seed `frontier.json` from existing PRs with `orch frontier set --repo <repo-dir>`, adding `--forge origin` when the forge is Origin.

> 2. **安装运行时。** 运行 `orch init`。通过 show-me-your-work 技能打开决策记录，在任何派生之前写好常驻命令，并用 `orch frontier set --repo <repo-dir>` 从已有的 PR 初始化 `frontier.json`，托管平台是 Origin 时加上 `--forge origin`。

3. **Pilot.** Push one unit through the whole path: brief, worker, verification, stack entry, ledger row, merge. The pilot exists to falsify the brief template, the verify recipe, and the unit size while that costs one agent instead of fifty. Fix the contract from pilot evidence before any fan-out. Scale the pilot to the unit. On programs of near-identical cheap units, the first unit is the pilot, run as a normal unit with its verify command inline, and fan-out starts the moment it lands. The dedicated pilot pipeline (separate verifier agent, audit gate) is for expensive or novel unit shapes, not for clone-units where a serialized pilot has nothing to falsify.

> 3. **试点。** 让一个单元走完整条路径：任务说明、工作者、验证、入栈、账本记录、合并。试点的存在是为了在只花一个代理而不是五十个代理的代价时，证伪任务说明模板、验证步骤和单元大小。在任何扇出之前，根据试点证据修正约定。试点的规模要和单元相称。对于由几乎相同的便宜单元组成的计划，第一个单元就是试点，作为普通单元运行，验证命令就地执行，它一落地就开始扇出。专门的试点流程（单独的验证者代理、审计关卡）是为昂贵或新颖的单元形状准备的，而不是为克隆式的单元，后者串行试点没有什么可证伪的。

4. **Scale.** Spawn a rolling window of workers up to the in-flight cap, refilling as children finish. Blocking batches pay the slowest child of every batch. Spawn track sub-coordinators only past the one-drain threshold in Roles. Recompute ready work after each drain. Relay upstream reports into downstream briefs. Keep sibling communication upward only. The sampled brief audit runs alongside the wave it samples and stops the next refill on failure, not the current one.

> 4. **扩大规模。** 以滚动窗口派生工作者，直到在途上限，子代理完成一个就补一个。阻塞的批次每一批都要等最慢的那个子代理。只有超过“角色”一节里一次清空的阈值时，才派生轨道子协调者。每次清空后重新计算就绪的工作。把上游报告传递到下游的任务说明里。兄弟之间的沟通只能向上走。抽查任务说明和被抽查的那一波同时进行，不合格时停止下一次补充，而不是当前这一波。

5. **Drain.** Run the queue discipline below at every drain point.

> 5. **清空。** 在每个清空点执行下面的队列纪律。

6. **Land.** Landing is continuous, never a terminal phase. Integration starts with the first verified unit and runs alongside the remaining waves. On heavy repos the stacker is a standing role from wave one, integrating as units verify. On repos where local git is cheap, the coordinator lands verified units itself per Roles. Keep the frontier green before upper-stack work. Stack safety governs. Advance `frontier.json` only on merge or reported new head SHAs.

> 6. **落地。** 落地是持续进行的，永远不是最后一个阶段。集成从第一个已验证的单元开始，和剩余的波次同步进行。在笨重的仓库里，堆栈者从第一波起就是常驻角色，单元验证一个集成一个。在本地 git 操作很便宜的仓库里，协调者按“角色”一节自己落地已验证的单元。在做栈上方的工作之前保持前沿全绿。以“栈的安全”一节为准。只有在合并或报告了新的最新提交 SHA 时，才推进 `frontier.json`。

7. **Close.** Drain the final inbox, reconcile every spawned agent to a terminal row (done, abandoned, zombie-reconciled), confirm the predicate on the real artifact, confirm every landed PR has a verdict for its current head SHA, audit the trail per show-me-your-work including its cross-model review, encode recurring corrections into `preferences.md` or the brief template. Leave the store intact. It is the postmortem.

> 7. **收尾。** 清空最后的收件箱，把每个派生过的代理对应到一个终态行（完成、放弃、僵尸已对账），在真实产物上确认判定，确认每个落地的 PR 在其当前最新提交 SHA 上都有结论，按 show-me-your-work 审计决策记录（包括它的跨模型审查），把反复出现的纠正编码进 `preferences.md` 或任务说明模板。保留存储原样。它就是复盘材料。

#### Queue and drain｜队列和清空

- On a completion notification, run `orch inbox push <agent> <unit> <status> [--report PATH]` and return to what you were doing. Never deep-review inline. A completion that needs review becomes a verifier unit. Never review a diff inside a drain.

> - 收到完成通知时，运行 `orch inbox push <agent> <unit> <status> [--report PATH]`，然后回到你正在做的事。永远不要就地深入审查。需要审查的完成项变成一个验证者单元。永远不要在清空过程中审查 diff。

- Drain in batches at four points: the end of a critical section, a track rollup, a frontier watcher wake (arm it via the loop skill, with a long heartbeat fallback), and before a human report. Begin each batch with `orch inbox drain`. Arrivals during a drain wait for the next one.

> - 在四个时间点分批清空：一个关键段结束时、一次轨道汇总时、前沿监视器唤醒时（通过 loop 技能开启，配一个间隔较长的心跳作为后备），以及给人报告之前。每一批都以 `orch inbox drain` 开始。清空期间到达的通知等下一批。

- Critical sections you finish first: authoring a brief, a stack operation, a conflict decision, writing a gate, updating ledger or frontier.

> - 必须先完成的关键段：写任务说明、栈操作、冲突决定、写关卡、更新账本或前沿。

- Each drain classifies every pointer (landed, needs-verify, failed, zombie, noise), writes the resulting rows through `orch unit add`, `orch unit set`, and `orch ledger record`, runs `orch status`, then spawns the next wave in one message.

> - 每次清空给每个指针分类（已落地、需验证、失败、僵尸、噪音），通过 `orch unit add`、`orch unit set` 和 `orch ledger record` 写入相应的行，运行 `orch status`，然后在一条消息里派生下一波。

- Account for every spawned child at its track's rollup: arrived, respawned, or its scope explicitly absorbed. Silently redoing a missing child's work hides both the wasted spend and the coverage gap its result existed to close.

> - 在每条轨道汇总时，清点每个派生过的子代理：已到达、已重新派生，或者它的范围被明确吸收了。悄悄重做一个缺失子代理的工作，会把浪费的开销和它的结果本应弥补的覆盖缺口都藏起来。

- A drain turn ends with the three lines from `orch status`: counts against the states, what changed, gates open. Detail lives in `status.md`. The full reply contract applies at checkpoints and close.

> - 一个清空回合以 `orch status` 输出的三行结束：各状态的计数、发生了什么变化、开着的关卡。细节放在 `status.md` 里。完整的回复约定适用于检查点和收尾。

#### Stack safety｜栈的安全

- The frontier is a computed object, never narrative. Recompute `frontier.json` after every merge and stack mutation: ordered PR list, branch names, head SHAs, a generation number, the lowest unmerged PR. The command walks the PR base chain through the checkout's current branch, so chain order and PR states come from the forge. It uses `gh` by default. Pass `--forge origin` when the program resolved the forge to Origin. Head SHAs come from local branch refs, so resolve it from the stacker's clone. A branch with no PR errors rather than guessing.

> - 前沿是一个计算出来的对象，永远不是叙述。每次合并和每次栈变动之后，重新计算 `frontier.json`：有序的 PR 列表、分支名、最新提交 SHA、一个代号，以及最低的未合并 PR。这条命令通过检出的当前分支沿着 PR 的基础分支链走，所以链的顺序和 PR 状态来自托管平台。它默认用 `gh`。当计划确定的托管平台是 Origin 时，传 `--forge origin`。最新提交 SHA 来自本地分支引用，所以要在堆栈者的克隆里运行它。没有 PR 的分支会报错，而不是去猜。

- Exactly one stacker per stack may mutate topology (chain rebases, force-pushes, base retargets, merges), serialized within its stack. Record the holder in the standing orders. Chain rebases run in cloud. A local rebase at this scale takes the laptop down.

> - 每个栈恰好只有一个堆栈者可以改变拓扑（链式变基、强制推送、改基础分支、合并），在它的栈内串行进行。把持有者记在常驻命令里。链式变基在云端运行。这种规模的本地变基会把笔记本拖垮。

- Workers never rebase and never retarget or merge PRs. Babysitters follow `playbooks/babysit.md`, one per stack, scoped to one immutable frontier generation. They report conflicts to the stacker rather than rebasing.

> - 工作者从不变基，也从不改 PR 目标分支或合并 PR。看护者遵循 `playbooks/babysit.md`，每个栈一个，范围限定在一个不可变的前沿代号上。他们把冲突报告给堆栈者，而不是自己变基。

- PR closes and retargets go through the stacker only. Closing a base PR orphans every chain above it. Merges and stack surgery are units with briefs like any other.

> - 关闭 PR 和改目标分支只能通过堆栈者。关闭一个基础 PR 会让它上面的每条链都成为孤儿。合并和栈手术也是单元，和其他单元一样有任务说明。

- One retro watcher follows merged PRs for reverts, post-merge CI breaks, and orphaned follow-ups.

> - 一个回顾监视器跟踪已合并的 PR，留意回滚、合并后 CI 出错和被遗忘的后续工作。

#### Verification｜验证

Scale verification to the unit. When VERIFY is a single cheap command, the worker runs it and reports the output, and the coordinator spot-checks receipts. A dedicated verifier agent (on a different model family than the worker) is for units whose verification is expensive, judgment-laden, or high-blast-radius. A verifier agent whose entire product would be rerunning one command is ceremony, not verification.

> 验证的规模要和单元相称。当 VERIFY 只是一条便宜的命令时，工作者运行它并报告输出，协调者抽查凭据。专门的验证者代理（用和工作者不同的模型家族）是为验证昂贵、需要判断或影响范围大的单元准备的。一个全部产出就是重跑一条命令的验证者代理，只是仪式，不是验证。

Write ledger rows with `orch ledger record`. Check the current PR and head SHA with `orch ledger check`. `ledger.tsv`, one row per verdict, keyed by PR number plus head SHA: `live-ui-verified | unit-test-verified | type-check-only | verifier-blocked | verifier-failed`. CI green is an input to a verdict, not a verdict. Behavioral work needs better than `type-check-only`. `verifier-blocked` is not a pass. Respawn when the environment heals. `verifier-failed` gets a fix unit, not a re-verify. A worker may self-report. A verifier overrides it on the same key. A new head SHA voids the row, so re-verify after restack. The ledger answers "was this verified", not memory and not the transcript.

> 用 `orch ledger record` 写账本行。用 `orch ledger check` 检查当前 PR 和最新提交 SHA。`ledger.tsv` 每个结论一行，以 PR 编号加最新提交 SHA 为键：`live-ui-verified | unit-test-verified | type-check-only | verifier-blocked | verifier-failed`。CI 全绿是结论的输入，不是结论。行为方面的工作需要比 `type-check-only` 更好的结论。`verifier-blocked` 不算通过。环境恢复后重新派生。`verifier-failed` 要开一个修复单元，而不是重新验证。工作者可以自我报告。验证者在同一个键上覆盖它。新的最新提交 SHA 会让该行作废，所以重新堆栈后要重新验证。“这个验证过没有”由账本回答，而不是记忆，也不是对话记录。

A unit is not done until its output is externalized the moment it lands, never batched to the end of the run. A worker pushes its branch, a verifier writes its ledger row, receipts land in the store. Work that exists only on one VM when that VM dies was never done.

> 一个单元的产出在落地那一刻就外部化之前，它都不算完成，永远不要攒到运行结束再一起做。工作者推送它的分支，验证者写下它的账本行，凭据落到存储里。只存在于某台虚拟机上的工作，在那台虚拟机挂掉时，就等于从没做过。

#### Liveness and failure｜存活状态和故障

- Never resume an agent to check on it. A resume restarts an idle agent. Probe read-only: the ledger, `units.tsv`, `gh`, pushed branches, the cloud agent's status in the Cursor dashboard. Transcript mtime is not liveness.

> - 永远不要为了查看状况而恢复一个代理。恢复会重新启动一个空闲的代理。用只读的方式探查：账本、`units.tsv`、`gh`、已推送的分支、Cursor 控制台里云端代理的状态。对话记录的修改时间不代表还活着。

- A silent death gets a synthetic postmortem row in the inbox (unit, failure mode, last evidence, options). Replan on evidence as it arrives. Never wait for full quiescence.

> - 悄无声息的死亡在收件箱里写一行合成的复盘记录（单元、故障模式、最后的证据、选项）。证据到达时就据此重新规划。永远不要等到全部静止。

- Retry by mode: cap-hit or oom, respawn with smaller scope. Network-drop, retry as-is. Tool-error, retry on a different model. Unknown, retry once. Two retries, then abandon the unit and replan around it.

> - 按故障模式重试：达到上限或内存不足，缩小范围重新派生。网络断开，原样重试。工具出错，换个模型重试。未知原因，重试一次。重试两次后，放弃这个单元，围绕它重新规划。

- A zombie that returns hours late reconciles against the current frontier and ledger before anything is accepted. Salvage unique findings through a fresh unit, never a blind merge.

> - 几小时后才回来的僵尸代理，在接受任何东西之前，先对照当前的前沿和账本对账。通过一个新单元挽救独有的发现，永远不要盲目合并。

- When continued spawning would produce garbage tree-wide (bad upstream output, broken acceptance, dead infra), write a stop line at the top of the standing orders, let in-flight work finish, fix the cause, clear it.

> - 当继续派生会在整棵树上产出垃圾时（上游输出有问题、验收标准坏了、基础设施挂了），在常驻命令顶部写一行停止令，让在途的工作完成，修复原因，再清除它。

- Bound your own infra retries the same way you bound a child's. After a few consecutive tool aborts, stop retrying. Write a terminal handoff to durable state (what is done, where it lives, the exact command to resume) and end the run.

> - 像限制子代理的重试一样限制你自己的基础设施重试。连续几次工具中止之后，停止重试。把一份终态交接写到持久状态里（做完了什么、在哪里、恢复用的确切命令），然后结束运行。

- After a Cursor restart: local agents are dead, cloud work is not. Re-read the standing orders and `units.tsv`, recompute the frontier, reattach cloud work by PR and branch rather than agent id, respawn one sub-coordinator per track from its stored brief plus current state, drain, resume. The dead session's store lock clears itself on the next write. `orch` replaces a lock whose holder pid is gone.

> - Cursor 重启之后：本地代理死了，云端工作没有。重读常驻命令和 `units.tsv`，重新计算前沿，按 PR 和分支（而不是代理 id）重新接上云端工作，按每条轨道存储的任务说明加上当前状态重新派生一个子协调者，清空，恢复。死掉的会话留下的存储锁会在下一次写入时自动清除。`orch` 会替换持有者进程已不存在的锁。

#### Escalation｜升级

Reaches the human, batched into the status page rather than per item: irreversible actions (force-push to shared branches, deploys, deletions, closing someone else's PR), genuine product or preference calls no experiment settles, a standing order that contradicts observed reality, a program-level dead end that survived a replan. Park each as a `gates.md` entry before asking, and route work around it.

> 需要交给人的事项，攒到状态页里一起报告，而不是一条一条地报：不可逆的操作（强制推送到共享分支、部署、删除、关闭别人的 PR）、没有实验能解决的真正的产品或偏好决定、与观察到的现实相矛盾的常驻命令、重新规划之后仍然存在的计划级死路。在提问之前，把每一项作为一条 `gates.md` 记录停放好，并让工作绕开它进行。

Never reaches the human: frontier nudges, rebase mechanics, retries, CI flake triage, review-thread triage, format fixes, scope the brief already forbids (refuse and continue), and "should I keep going". When in doubt, act and log.

> 永远不要交给人的事项：前沿的推进、变基的具体操作、重试、CI 不稳定的分流、评审讨论的分流、格式修复、任务说明已经禁止的范围（拒绝并继续），以及“我要不要继续”。拿不准时，就做，并记录下来。

Mid-run discoveries fix only what blocks the frontier. Everything else parks in follow-ups. At this fan-out a small scope leak multiplies into PRs nobody asked for.

> 运行中途的发现只修阻塞前沿的部分。其他的都停放到后续工作里。在这种扇出规模下，一点点范围外溢都会成倍变成没人要的 PR。

**Reply:** at checkpoints and close: the predicate and the count against it from `units.tsv` and `ledger.tsv`, tracks and what each landed, the frontier (PR list plus SHAs), verdicts summary, what was abandoned and why, gates awaiting the human (the only asks), the store path, and the trail path. Numbers from the tables, not narrative. Include PR links.

> **回复：** 在检查点和收尾时：判定，以及来自 `units.tsv` 和 `ledger.tsv` 的对照计数、各条轨道以及每条落地了什么、前沿（PR 列表加 SHA）、结论汇总、放弃了什么以及原因、等待人决定的关卡（唯一的提问）、存储路径和决策记录路径。数字来自表格，而不是叙述。附上 PR 链接。
