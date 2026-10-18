<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/autopilot-stack.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/autopilot-stack.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/autopilot-stack.md)

### Autopilot-stack｜栈式自动驾驶

**You own the stack, never the landing. Hand the operator one linear stack of base-chained PRs to review and land.** **Autopilot-full** holds except for how verified work leaves. Each tick re-reads both playbooks from trunk, and this one wins where they differ.

> **你负责栈，绝不负责合并。交给操作者一条由基础分支串起来的线性 PR 栈，供他审查和合并。** 除了已验证的工作如何离开这一点，**Autopilot-full** 的规则全部适用。每一轮都从主干重新读这两个剧本，两者不同时以本剧本为准。

1. **Run Autopilot-full's owners, minus the landing.** Owners report STACK-READY in place of merge-ready. They never rebase onto another branch, trunk included, and never retarget, merge, close a PR, or arm auto-merge, because the root owns the chain's topology and the operator owns the landing. When an item depends on another, its owner branches from that item's branch, not from trunk.

> 1. **运行 Autopilot-full 的负责人，但去掉合并。** 负责人报告 STACK-READY，而不是可合并。他们永远不变基到其他分支上（包括主干），永远不改目标分支、不合并、不关闭 PR，也不开启自动合并，因为链的拓扑归根代理管，合并归操作者管。当某一项依赖另一项时，它的负责人从那一项的分支开分支，而不是从主干。

2. **Give the topology one writer.** Owners push only their own branches and report each tip and its intended parent. Only you rebase or retarget a link, because two writers on one chain undo each other. Append each clean PR in dependency order, with its base set to its parent's branch (`origin pr edit <pr> --base <parent-branch>` or `gh pr edit <pr> --base <parent-branch>`). Never require `gt`. An owner takes your push as its new base and lease, and on a conflict resolves its own slice for you to push.

> 2. **拓扑只有一个写入者。** 负责人只推送自己的分支，并报告每个分支的最新提交和它预期的父分支。只有你能变基或改目标分支，因为同一条链上有两个写入者会互相抵消。按依赖顺序追加每个干净的 PR，把它的基础分支设为父分支（`origin pr edit <pr> --base <parent-branch>` 或 `gh pr edit <pr> --base <parent-branch>`）。永远不要依赖 `gt`。负责人把你的推送当作新的基础和租约，遇到冲突时解决自己那一段，交给你推送。

3. **Absorb drift at the root.** Until landing starts, apply Autopilot-full step 5's test to each link and rebase the chain bottom-up where it calls for one. Then apply the patch-id rule link by link, since a link's base is its parent.

> 3. **在根部吸收偏移。** 在开始合并之前，对每一环应用 Autopilot-full 第 5 步的测试，需要时从下往上变基整条链。然后逐环应用 patch-id 规则，因为每一环的基础就是它的父分支。

4. **Deliver the chain, then keep your hands off it.** Each link carries its verdict in its PR. Once landing starts, nobody writes to the chain except the post-merge retarget that `playbooks/shipping.md` sanctions, because a second writer breaks a landing in flight. A mid-landing problem goes through you to the operator, and any fix waits for the operator's go.

> 4. **交付这条链，然后别再碰它。** 每一环的 PR 里都带着它的结论。合并一旦开始，除了 `playbooks/shipping.md` 认可的合并后改目标分支之外，谁都不能写这条链，因为第二个写入者会破坏进行中的合并。合并中途出现的问题经由你上报给操作者，任何修复都要等操作者同意。

**Choosing between the autopilots.** Autopilot-full when the PRs are independent and landing authority is granted. Autopilot-stack when the operator wants review before landing, the work is sequenced or coupled, or merge authority is withheld.

> **两种自动驾驶怎么选。** PR 相互独立、并且被授予了合并权限时，用 Autopilot-full。操作者想在合并前审查、工作有先后或相互耦合、或者合并权限被保留时，用 Autopilot-stack。

**Reply:** catch the operator up per the **ketchup** skill, with links to the stack root and tip, a one-line verdict per link, and anything parked or excluded with the reason.

> **回复：** 按 **ketchup** 技能让操作者跟上进度，附上栈底和栈顶的链接、每一环一行结论，以及被搁置或排除的事项和原因。
