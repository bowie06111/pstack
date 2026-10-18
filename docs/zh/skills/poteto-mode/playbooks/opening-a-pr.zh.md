<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/opening-a-pr.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/opening-a-pr.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/opening-a-pr.md)

### Opening a PR｜开 PR

Invoked at the end of every other playbook.

> 在其他每个剧本结束时调用。

**Worktree.** Work from a git worktree off main. Subagents inherit it. Multiple `Task` calls on the same branch each get their own worktree, or `git fetch && git reset --hard origin/<branch>` between them. Dirty branch with unrelated work: patch out, fresh worktree, apply. Snarled worktree: reset from main, redo minimally.

> **Worktree。** 在从 main 拉出的 git worktree 里工作。子代理会继承它。同一分支上的多个 `Task` 调用各自用自己的 worktree，或者在两次调用之间执行 `git fetch && git reset --hard origin/<branch>`。分支上混着无关的工作时：把补丁导出来，开一个新 worktree，再应用。worktree 乱成一团时：从 main 重置，以最小的方式重做。

**Commits.** Commit liberally. Rebase into small, ordered commits before opening PRs. Each commit is a future PR: landable, ordered to tell the story. Amend when the fix belongs in a just-made commit. New commit when separable. Commit bodies don't restate the subject.

> **提交。** 勤提交。开 PR 之前变基成小而有序的提交。每个提交都是未来的一个 PR：可以单独落地，按讲故事的顺序排列。修复属于刚做的那个提交时就 amend。能分开的就开新提交。提交正文不要重复标题。

**PRs.** Run `/deslop` from `cursor-team-kit` over the diff before commit and `/no-comments` before review. Write every PR title, description, and commit body with `/technical-writing` (every layer except Diátaxis, and apply the STE vocabulary rules: one word per action, keep articles, a plain verb over `-ing` where one works), then apply `/unslop`.

> **PR。** 提交前对 diff 运行 `cursor-team-kit` 里的 `/deslop`，审查前运行 `/no-comments`。每个 PR 标题、描述和提交正文都用 `/technical-writing` 来写（用除 Diátaxis 之外的每一层，并应用 STE 词汇规则：一个动作一个词，保留冠词，能用简单动词就不用 `-ing` 形式），然后应用 `/unslop`。

**Titles.** Conventional Commits `type(scope): subject`, no trailing period. Type such as `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, or `perf`. Scope is the area, such as `pstack` or `poteto-mode`. Subject is short, imperative, and names a real symbol when one carries the change, for example `fix(pstack): retarget opening-a-pr babysit trigger`.

> **标题。** 用 Conventional Commits 格式 `type(scope): subject`，结尾不加句号。type 比如 `feat`、`fix`、`docs`、`refactor`、`test`、`chore` 或 `perf`。scope 是涉及的区域，比如 `pstack` 或 `poteto-mode`。subject 简短、用祈使语气，改动落在某个真实符号上时就写出它的名字，例如 `fix(pstack): retarget opening-a-pr babysit trigger`。

**Descriptions.** The PR body is a one-minute briefing, not a lab notebook, for a reviewer who has the diff. It says why the change exists, what it leaves out, what it could break, and how you proved it works. Write short, simple sentences with few identifiers, no walls of text. The body becomes the squash commit body, so keep that commit under about 40 lines. Use `##` headings, not bold lead-ins, in this order, and drop a section with nothing to say:

> **描述。** PR 正文是一份一分钟的简报，给手里有 diff 的审查者看，而不是实验记录本。它说明这个改动为什么存在、它没做什么、它可能弄坏什么，以及你如何证明它能用。用简短的句子，少用标识符，不要大段文字。正文会成为 squash 提交的正文，所以这个提交保持在 40 行左右以内。用 `##` 标题而不是加粗的开头，按下面的顺序，没话可说的小节就去掉：

- `## Why`: the problem and approach, 1–3 short sentences. No SHAs, rebase genealogy, or "based on main" preamble.

> - `## Why`：问题和做法，1 到 3 个短句。不要写 SHA、变基的来龙去脉或“基于 main”之类的开场白。

- `## What changed`: 1–3 short bullets. Name a real symbol or path only when it carries the change. Name both sides of a rename or retarget.

> - `## What changed`：1 到 3 个简短的要点。只有当某个真实符号或路径承载了改动时才写出它。重命名或改目标时写出两边。

- `## Scope`: always. 1–3 short items on what it covers and what it deliberately leaves out (a follow-up, a known gap). No symbols, paths, or file-by-file essay.

> - `## Scope`：必须有。1 到 3 条简短内容，说明覆盖了什么、刻意没做什么（后续工作、已知缺口）。不写符号、路径，也不逐个文件长篇大论。

- `## Tradeoffs`: only rejected alternatives a reviewer would otherwise ask about. Skip when there was no real choice.

> - `## Tradeoffs`：只写审查者否则会问到的、被否决的备选方案。没有真正的选择时就跳过。

- `## Blast Radius`: 1–2 sentences on who or what it touches and why that is safe or risky. If main is red, state the cost of leaving it red.

> - `## Blast Radius`：1 到 2 句话，说明它影响到谁或什么，以及为什么这是安全的或有风险的。如果 main 是红的，说明让它继续红着的代价。

- `## Verification`: 1–3 bullets, each a real run path and its outcome. Perf: one primary number with its unit, `before → after`, and a link to the arena or swarm directory for the rest. No sample-size methodology, swarm recitals, or metric tables.

> - `## Verification`：1 到 3 个要点，每个都是一条真实的运行路径及其结果。性能：一个主要数字及其单位，`之前 → 之后`，其余的链接到 arena 或 swarm 目录。不写样本量方法论、swarm 的流水账或指标表。

After the sections, attach videos or screenshots when they prove a claim. Full SHAs, swarm or arena lane recitals, lever-correction essays, file-by-file checklists, and "CLEAN" verdicts go in a linked artifact, not the body.

> 在这些小节之后，视频或截图能证明某个论断时就附上。完整的 SHA、swarm 或 arena 各通道的流水账、关于工具修正的长文、逐个文件的清单和“CLEAN”结论，都放到链接的产物里，不放在正文。

**Forge.** Resolve the forge before the first PR operation and keep that choice for create, edit, view, watch, and merge. GitHub CLI (`gh`) is the default. If `command -v origin` succeeds and Origin can resolve the repository, prefer `origin pr ...`. If Origin is absent or cannot resolve the repository, stay on `gh` and record the fallback. Do not require Graphite (`gt`).

> **托管平台。** 在第一次 PR 操作之前确定托管平台，并在创建、编辑、查看、监视和合并时都沿用这个选择。默认用 GitHub CLI（`gh`）。如果 `command -v origin` 成功并且 Origin 能识别这个仓库，优先用 `origin pr ...`。如果没有 Origin 或它识别不了这个仓库，继续用 `gh` 并记录这次回退。不要依赖 Graphite（`gt`）。

**Built-in PR tool.** When the run provides a built-in PR tool, create, edit, retarget, and mark ready through it, never through a forge CLI. A CLI-made PR misses what the tool tracks. Use the resolved forge for everything the tool does not cover, and for every PR operation when the run has no such tool.

> **内置的 PR 工具。** 运行环境提供了内置 PR 工具时，创建、编辑、改目标分支和标记就绪都通过它来做，永远不要通过托管平台的 CLI。用 CLI 建的 PR 会缺少该工具跟踪的信息。工具覆盖不到的事情，以及运行环境没有这种工具时的所有 PR 操作，都用确定好的托管平台。

**Size and stacks.** Prefer five narrow PRs to one large PR. A stack is a base-branch chain. The root PR targets trunk. Each child branch rebases onto its parent's exact tip and its PR targets the parent branch. As a parent merges, move its child onto trunk per `playbooks/shipping.md` step 4. Without a built-in PR tool, per the resolved forge, create a child with `origin pr create --status open --base <parent-branch>` or `gh pr create --base <parent-branch>`, and retarget one with `origin pr edit <pr> --base <parent-branch>` or `gh pr edit <pr> --base <parent-branch>`. Branch from trunk only for independent work. Rebase on trunk before substantial stack work.

> **大小和栈。** 宁可开五个窄的 PR，也不要开一个大 PR。栈是一条基础分支链。根 PR 以主干为目标。每个子分支变基到父分支的确切最新提交上，它的 PR 以父分支为目标。父分支合并后，按 `playbooks/shipping.md` 第 4 步把它的子分支移到主干上。没有内置 PR 工具时，按确定好的托管平台，用 `origin pr create --status open --base <parent-branch>` 或 `gh pr create --base <parent-branch>` 创建子 PR，用 `origin pr edit <pr> --base <parent-branch>` 或 `gh pr edit <pr> --base <parent-branch>` 改目标分支。只有独立的工作才从主干开分支。在栈上做大量工作之前先变基到主干。

**Readiness.** Open every PR ready, never as a draft. A built-in PR tool can default to draft, so set `draft: false` on every creation call through it. With Origin, pass `--status open`. With `gh`, omit `--draft`. If a PR still opens as a draft, mark it ready through the PR tool, or run `origin pr ready <number>` or `gh pr ready <number>` according to the resolved forge. Run `origin pr view <number>` or `gh pr view <number>` before you refer to PR status.

> **就绪状态。** 每个 PR 都以就绪状态打开，永远不开草稿。内置 PR 工具可能默认是草稿，所以通过它的每次创建调用都要设 `draft: false`。用 Origin 时，传 `--status open`。用 `gh` 时，不加 `--draft`。如果 PR 还是以草稿打开，就通过 PR 工具把它标记为就绪，或者按确定好的托管平台运行 `origin pr ready <number>` 或 `gh pr ready <number>`。在提到 PR 状态之前，先运行 `origin pr view <number>` 或 `gh pr view <number>`。

**Babysit.** Opening a PR does not start a babysit. Post the URL and keep building. Finish the phase or stack first. Run a separate babysit pass only when the user asks for one after the whole stack exists. A babysit for each new PR stalls the build and spends checks on commits later waves restart. Push back when feedback drifts from intent.

> **看护（Babysit）。** 开 PR 不会启动看护。贴出链接，继续构建。先完成这个阶段或这个栈。只有在整个栈都建好、用户要求时，才单独跑一轮看护。给每个新 PR 都看护会拖住构建，还会把检查浪费在之后的批次会重启的提交上。反馈偏离意图时要反驳。

A subagent that opens a PR runs `interrogate`, `/deslop`, and `/no-comments`, and posts the URL. It returns to the parent without babysitting, unless it is an Autopilot-full or Autopilot-stack owner. That owner's brief assigns the babysit loop and is the ask `playbooks/babysit.md` waits for. The owner starts the loop after its code-ready report, reports merge-ready or STACK-READY per its playbook, and is exempt from the rules here and in `playbooks/babysit.md` that hold babysitting until a whole stack is built.

> 开了 PR 的子代理要运行 `interrogate`、`/deslop` 和 `/no-comments`，然后贴出链接。它不做看护直接返回父代理，除非它是 Autopilot-full 或 Autopilot-stack 的负责人。那种负责人的任务说明会指派看护循环，这正是 `playbooks/babysit.md` 所等待的请求。负责人在报告代码就绪后启动这个循环，按它的剧本报告可合并或 STACK-READY，并且不受这里和 `playbooks/babysit.md` 中“要等整个栈建好才看护”的规则约束。
