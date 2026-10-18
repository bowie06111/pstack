<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/worktree-cleanup.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/worktree-cleanup.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/worktree-cleanup.md)

### Worktree and simulator cleanup｜Worktree 和模拟器清理

1. Snapshot and audit. Record `df -h /`, then run `scripts/worktree-audit.sh [-b <pr-base-ref>] [repo-path]` (principle-build-the-lever). Without `-b` it checks `MERGED` against `origin/HEAD`, so pass `-b origin/<branch>` when PRs target another branch. It reads paths from `git worktree list`, never hand-typed, since a hand-typed `myrepo-worktrees/x` misses one that lives at `.cursor/worktrees/myrepo/x` (principle-encode-lessons-in-structure). It classifies each worktree by size, age, merge state, uncommitted work, PR state, and the newest chat that touched it, then suggests a bucket. The transcript scan is slow, so background it.

> 1. 快照并审计。记下 `df -h /`，然后运行 `scripts/worktree-audit.sh [-b <pr-base-ref>] [repo-path]`（principle-build-the-lever）。不加 `-b` 时它对照 `origin/HEAD` 检查 `MERGED`，所以当 PR 的目标是另一个分支时，传 `-b origin/<branch>`。它从 `git worktree list` 读取路径，从不用手打的路径，因为手打的 `myrepo-worktrees/x` 会漏掉位于 `.cursor/worktrees/myrepo/x` 的那个（principle-encode-lessons-in-structure）。它按大小、时间、合并状态、未提交的工作、PR 状态以及最近碰过它的对话给每个 worktree 分类，然后建议一个分组。扫描对话记录很慢，所以放到后台运行。

2. The bucket is advice, not permission. The pinned and active chats are the real artifact (principle-prove-it-works). Get that set from the user or sidebar and cross-check every candidate. The lever has marked `safe` a worktree the user had pinned, so the pinned set wins.

> 2. 分组只是建议，不是许可。被置顶和正在进行的对话才是真正的依据（principle-prove-it-works）。从用户或侧边栏拿到这个集合，逐个核对每个候选项。这个工具曾经把用户置顶的 worktree 标成 `safe`，所以以置顶集合为准。

3. Verify usage before deleting. For every `verify-recent-chat` row, or anything you doubt, fan subagents out to read the transcripts and report whether the chat is pinned or ongoing and which worktrees it touches (principle-guard-the-context-window, transcripts are bulk). A pinned chat spawns arena and repro trees into sibling worktrees via background subagents, and those are in use even when their names never hit the sidebar. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section.

> 3. 删除前核实使用情况。对每一行 `verify-recent-chat`，或任何你拿不准的，扇出子代理去读对话记录，报告该对话是否被置顶或仍在进行，以及它碰过哪些 worktree（principle-guard-the-context-window，对话记录量很大）。一个置顶的对话会通过后台子代理在相邻的 worktree 里派生 arena 和复现用的树，即使它们的名字从没出现在侧边栏，它们也在使用中。按“子代理”一节，为每个子代理选择适合这项工作的推理等级以及快速或标准速度。

4. Pause on irreversible loss. `wip:N` is N tracked uncommitted edits. Show the diff and get a decision first, since removing a clean worktree is recoverable from its branch but uncommitted work is gone. `scratch:N` is untracked throwaway, safe to drop, but name the files. Per Autonomy, clean and merged and not-in-use proceeds. `wip` and in-use pause.

> 4. 遇到不可逆的损失就暂停。`wip:N` 表示 N 个已跟踪但未提交的修改。先展示 diff 并取得决定，因为删除一个干净的 worktree 可以从它的分支恢复，但未提交的工作就没了。`scratch:N` 是未跟踪的临时文件，可以安全丢弃，但要列出文件名。按“自主”一节，干净、已合并、未在使用的直接处理。`wip` 和使用中的要暂停。

5. Prune the confirmed set. Per path, `git worktree remove --force <path>`. If the dir survives on ignored build artifacts, `rm -rf` it, then `git worktree prune`. Branch refs survive, so no commits are lost. Confirm with `df -h /` and re-list.

> 5. 清理确认过的集合。逐个路径执行 `git worktree remove --force <path>`。如果目录因为被忽略的构建产物而残留，就 `rm -rf` 它，然后 `git worktree prune`。分支引用会保留，所以不会丢失提交。用 `df -h /` 确认，并重新列出。

6. Simulators and other reclaimers. Simulators are usually the next-biggest win. `xcrun simctl --set testing delete all` (XCTestDevices clones), `xcrun simctl delete unavailable`, and `xcrun simctl runtime list` then `runtime delete <id>` for old runtimes. More when needed: Xcode `DerivedData` and `iOS DeviceSupport`, `~/Library/Application Support/Cursor` (`state.vscdb.backup`, and `snapshots/roots/<root>` where a `<root>` named for a folder you opened as a workspace balloons), package caches (pnpm, uv, brew, yarn). Clear only caches the user has not said to keep.

> 6. 模拟器和其他可回收空间。模拟器通常是第二大的收获。`xcrun simctl --set testing delete all`（XCTestDevices 克隆）、`xcrun simctl delete unavailable`，以及先 `xcrun simctl runtime list` 再 `runtime delete <id>` 删除旧运行时。需要时还有更多：Xcode 的 `DerivedData` 和 `iOS DeviceSupport`、`~/Library/Application Support/Cursor`（`state.vscdb.backup`，以及 `snapshots/roots/<root>`，其中以你打开过的工作区文件夹命名的 `<root>` 会膨胀）、包缓存（pnpm、uv、brew、yarn）。只清理用户没说要保留的缓存。

This is the one playbook that deletes user state with no code review to catch a slip, so the gates above are the review.

> 这是唯一一个在没有代码审查兜底的情况下删除用户状态的剧本，所以上面这些关卡就是审查。

**Reply:** `df -h /` before and after with space reclaimed, the worktrees pruned, and a one-line reason for each held back (in-use by which chat, or uncommitted work).

> **回复：** 清理前后的 `df -h /` 以及回收的空间、已清理的 worktree，以及每个被保留的 worktree 的一行理由（被哪个对话使用中，或有未提交的工作）。
