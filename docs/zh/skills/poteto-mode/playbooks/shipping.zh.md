<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/shipping.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/shipping.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/shipping.md)

### Shipping｜发布

**You own what lands. Verify each PR independently, land only the verified run from the root, then keep your hands off the stack.**

> **落地的内容由你负责。独立验证每个 PR，只合并从根部开始连续验证通过的那一段，然后别再碰这个栈。**

1. **Resolve the forge, then verify each PR independently before it lands.** GitHub CLI (`gh`) is the default. If `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Otherwise stay on `gh` and record the fallback. Never require Graphite (`gt`). One verifier per PR, each a Cursor cloud agent, runs the change the way a user would with the matching control skill (such as `control-ui` or `control-cli` from `cursor-team-kit`), parent against head, and posts `PASS`, `PASS+NOTES`, or `FAIL` on that PR. Only a verdict from an agent that did not write the code counts, because an author checks what it meant to build. Green CI and an approving bot review are not verdicts, because neither runs the change as a user would.

> 1. **确定代码托管平台，然后在每个 PR 落地前独立验证它。** 默认用 GitHub CLI（`gh`）。如果 `command -v origin` 成功并且 Origin 能识别这个仓库，所有 PR 操作都用 `origin pr`。否则继续用 `gh`，并记录这次回退。永远不要依赖 Graphite（`gt`）。每个 PR 一个验证者，每个都是 Cursor 云端代理，用对应的控制技能（比如 `cursor-team-kit` 里的 `control-ui` 或 `control-cli`）像用户那样运行这个改动，对比父分支和最新提交，然后在该 PR 上发布 `PASS`、`PASS+NOTES` 或 `FAIL`。只有没写这段代码的代理给出的结论才算数，因为作者检查的是自己想做出来的东西。CI 全绿和机器人的审查通过都不算结论，因为两者都没有像用户那样运行这个改动。

2. **Land only the contiguous verified run rooted at the bottom.** Walk up from the lowest unmerged PR and stop at the first one without `PASS` or `PASS+NOTES`. The PRs below it are the run, and the top one is the ceiling. A verified PR above an unverified one waits, because it cannot land without the code below it. One whose branch neither holds nor needs that code can move onto trunk and land on its own. It needs a fresh verdict first, because its old one ran with that code beneath it and trunk does not have it.

> 2. **只合并从底部开始连续验证通过的那一段。** 从最低的未合并 PR 往上走，在第一个没有 `PASS` 或 `PASS+NOTES` 的 PR 处停下。它下面的 PR 就是这一段，最上面那个就是上限。一个已验证的 PR 如果在未验证的 PR 上面，就要等待，因为没有下面的代码它无法落地。如果某个 PR 的分支既不包含也不需要那些代码，它可以移到主干上单独落地。它需要先拿一个新的结论，因为它原来的结论是在那些代码垫在下面的情况下跑的，而主干上没有那些代码。

3. **Re-check that the verdicts still describe the code.** A verdict covers a patch, and a chain rebase moves every PR above it to a new SHA without failing any check. Compare `git patch-id` at the verdict SHA with the current head before trusting an older verdict. When the patches differ only in tests, docs, or lint config, a lane's result stands if what it ran builds the same at both SHAs apart from noise. Noise is a difference that also shows up between two builds of the verdict SHA, or an embedded commit SHA. Report each kind of noise with its files, so a reader can check the call. The change itself still gets fresh checks and a review, because the old verdict never saw it. A lane with no build output, such as a dev server, cannot show this, so rerun it, and re-verify anything else that drifted.

> 3. **重新确认结论仍然对应当前的代码。** 结论覆盖的是一个补丁，而一次链式变基会把它上面的每个 PR 都移到新的 SHA，却不会让任何检查失败。在信任一个较早的结论之前，比较结论所在 SHA 和当前最新提交的 `git patch-id`。如果补丁的差异只在测试、文档或 lint 配置上，那么只要该验证通道所运行的东西在两个 SHA 上除了噪音之外构建结果相同，它的结果就仍然有效。噪音指的是在结论 SHA 的两次构建之间也会出现的差异，或者内嵌的提交 SHA。把每种噪音连同涉及的文件一起报告，让读者能核查这个判断。改动本身仍然要做新的检查和审查，因为旧结论从没看到过它。没有构建产出的验证通道（比如开发服务器）无法证明这一点，所以要重跑它，其他有偏移的东西也要重新验证。

4. **Land bottom-up, one PR at a time.** Always merge the run's lowest unmerged PR, with `origin pr merge <pr>` or `gh pr merge <pr> --squash`. On Origin the plain command also merges every PR below the one you name, and on GitHub a PR whose base is still its parent's branch merges into that branch, not trunk. Read the PR back for the result (`origin pr view <pr>` or `gh pr view <pr> --json state,mergedAt,baseRefName`), because a merge command can report success when a merge queue only queued the PR, and a requested auto-merge is not a merge. A landed PR reads merged with trunk as its base, not its parent's branch. Arm `--auto` (Origin merge-when-ready or GitHub auto-merge) only on a PR whose verdict covers its current head, because it lands whatever head is ready. After a merge lands, retarget the PR whose base was the merged branch to trunk (`origin pr edit <pr> --base <trunk>` or `gh pr edit <pr> --base <trunk>`), move its patch onto the merge result, and push with `--force-with-lease`. The verdict covers the patch, not its commits, so step 3's patch-id rule decides whether the verdict survives. Merge it once its checks pass and its verdict survives.

> 4. **从底向上，一次合并一个 PR。** 永远合并这一段里最低的未合并 PR，用 `origin pr merge <pr>` 或 `gh pr merge <pr> --squash`。在 Origin 上，普通命令还会把你指定的 PR 下面的所有 PR 一起合并；在 GitHub 上，基础分支仍是其父分支的 PR 会合并进那个分支，而不是主干。读回 PR 来确认结果（`origin pr view <pr>` 或 `gh pr view <pr> --json state,mergedAt,baseRefName`），因为当合并队列只是把 PR 排进队列时，合并命令也可能报告成功，而请求了自动合并也不等于已合并。已落地的 PR 会显示为已合并，基础分支是主干而不是其父分支。只在结论覆盖当前最新提交的 PR 上开启 `--auto`（Origin 的就绪即合并或 GitHub 的自动合并），因为它会合并任何就绪时的最新提交。一次合并落地后，把基础分支是被合并分支的那个 PR 的目标改为主干（`origin pr edit <pr> --base <trunk>` 或 `gh pr edit <pr> --base <trunk>`），把它的补丁移到合并结果上，用 `--force-with-lease` 推送。结论覆盖的是补丁而不是提交，所以由第 3 步的 patch-id 规则决定结论是否仍然有效。等它的检查通过且结论仍然有效时再合并它。

5. **Leave the stack alone while it lands.** Step 4's move of the next PR is the only change to a branch or base, because a merge lands whatever its branch and base hold at that moment, verdict or not. On GitHub, watch the run with the queued watcher from `playbooks/babysit.md` (`scripts/watch-pr/watch-pr --queued-stack --stack-prs <bottom-to-top>`) under `/loop` until it reports `COMPLETE` at the ceiling. On Origin, re-read the current PR with `origin pr view <pr> --checks` each time `origin pr checks <pr> --watch` returns, until the ceiling reads merged. A base that moves to trunk when its parent merges is expected, not damage to repair. If a merge stalls, find out why before you change anything.

> 5. **合并期间别碰这个栈。** 第 4 步对下一个 PR 的移动是唯一允许的对分支或基础分支的改动，因为合并会落地那一刻分支和基础分支上的任何内容，不管有没有结论。在 GitHub 上，用 `playbooks/babysit.md` 里的排队监视器（`scripts/watch-pr/watch-pr --queued-stack --stack-prs <从底到顶>`）在 `/loop` 下盯着这一段，直到它在上限处报告 `COMPLETE`。在 Origin 上，每次 `origin pr checks <pr> --watch` 返回时，用 `origin pr view <pr> --checks` 重新读当前 PR，直到上限显示已合并。父分支合并后基础分支变成主干是正常的，不是需要修复的损坏。如果合并卡住了，先查清楚原因，再改任何东西。

6. **Stop at the ceiling.** Extending the run takes new verdicts, so it is a new pass through step 1.

> 6. **在上限处停下。** 要延伸这一段需要新的结论，所以那是从第 1 步开始的新一轮。

**Reply:** the verified run and its ceiling, each PR's verdict and who produced it, what landed and how you confirmed it, and what the next gap needs.

> **回复：** 已验证的那一段及其上限、每个 PR 的结论以及由谁给出、落地了什么以及你如何确认的，以及下一个缺口需要什么。
