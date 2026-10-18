<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/pause-safely.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/pause-safely.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/pause-safely.md)

### Pause safely｜安全暂停

**You own a clean stop. Leave a checkpoint a cold-start agent can resume from.** This is explicit only. On "keep going", "going to bed, keep going", or "don't stop", do not pause.

> **干净地停下由你负责。留下一个冷启动的代理能接着做的检查点。** 只有被明确要求时才执行。听到“keep going”“我去睡了，继续”或“别停”时，不要暂停。

1. Stop at a safe boundary. Finish the current atomic step or back out of it. Start nothing new, and cancel any nested subagents.

> 1. 在一个安全的边界停下。完成当前的原子步骤，或者撤出它。不开始任何新的事情，取消所有嵌套的子代理。

2. Take no irreversible action to pause. No PR and no push unless you already had one out.

> 2. 不为暂停做任何不可逆的操作。不开 PR，不推送，除非你本来就已经推出去了。

3. Make the work durable. Commit uncommitted edits as one clear `wip:` commit on the current branch so nothing is lost. If the tree is broken, say so in the commit body in one line.

> 3. 让工作持久化。把未提交的修改作为一个清楚的 `wip:` 提交放在当前分支上，确保什么都不丢。如果代码树是坏的，在提交正文里用一行说明。

4. Write the resume note off-context. Capture intent, what you were doing, progress and what's verified, current state, next steps, key files, and gotchas. For the compaction trigger write it to a file like `/tmp/<slug>-resume.md`. If a show-me-your-work trail exists, point at it instead of duplicating it.

> 4. 把恢复说明写到上下文之外。记下意图、你正在做什么、进度和已验证的内容、当前状态、下一步、关键文件和注意事项。如果是因为上下文压缩触发的，就写到 `/tmp/<slug>-resume.md` 这样的文件里。如果已有 show-me-your-work 的记录，就指向它，不要重复写一份。

**Reply:** where you are in the loop, what's on disk versus still in your head (paths, no diff dumps), the commits you made and whether the tree is clean, and the first action on resume. This is a pause, not a final report.

> **回复：** 你在循环中的位置、哪些已经落盘、哪些还只在你脑子里（给路径，不要贴 diff）、你做了哪些提交以及代码树是否干净，以及恢复时的第一个动作。这是一次暂停，不是最终报告。
