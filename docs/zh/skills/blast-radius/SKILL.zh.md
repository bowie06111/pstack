<!-- 中英对照译文。原文：skills/blast-radius/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/blast-radius/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/blast-radius/SKILL.md)

```yaml
name: blast-radius
description: "Find what a change could break somewhere else before it ships, beyond the diff, and prove the one fact it's safe because of by running real code instead of writing it up. Use for 'blast radius of X', 'what could this break', or reviewing a small diff you don't trust."
disable-model-invocation: true
```

> **说明（description）：** 在发布之前找出一个改动可能在 diff 之外的别处弄坏什么，并通过运行真实代码而不是写文章，来证明它之所以安全所依赖的那一个事实。用于“X 的影响范围”“这个可能弄坏什么”，或审查一个你不放心的小 diff。

# Blast radius｜影响范围（Blast radius）

Find what a change breaks somewhere else, before it ships. Use for "blast radius of X", "what could this break", or reviewing a small diff you don't trust yet.

> 在发布之前，找出一个改动会在别处弄坏什么。用于“X 的影响范围”“这个可能弄坏什么”，或审查一个你还不放心的小 diff。

Companion to `how` and `why`. `how` tells you what the code does. `why` tells you why it's shaped that way. Blast radius tells you what it breaks somewhere else.

> 与 `how` 和 `why` 配套。`how` 告诉你代码做什么。`why` 告诉你它为什么是这个形状。影响范围告诉你它会在别处弄坏什么。

Listing the callers is not the job. The agent can grep those in a second. The job is the breakage grep won't show you.

> 列出调用方不是这项工作的重点。代理一秒钟就能 grep 出来。重点是 grep 显示不出来的破坏。

## Don't trust your own writeup｜不要相信你自己写的分析

A blast-radius writeup that sounds right is worthless. It reads as convincing whether or not it's true. So don't hand back the writeup. Find the one or two facts the whole thing depends on and prove them by running code.

> 一份听起来正确的影响范围分析毫无价值。不管它是不是真的，读起来都很有说服力。所以不要只交回这份分析。找出整件事所依赖的那一两个事实，通过运行代码来证明它们。

### How sure are you｜你有多确定

For each fact the change's safety depends on, get it as far down this list as is cheap, and say where it stopped.

> 对于改动的安全性所依赖的每一个事实，在成本不高的前提下，尽量把它推到下面这个列表的更靠后的位置，并说明它停在了哪一级。

1. You said so. Worthless on its own.

> 1. 你说是这样。单凭这个毫无价值。

2. You pointed at the line. A real `file:line`, or the library's own source.

> 2. 你指出了那一行。一个真实的 `文件:行`，或者库自己的源码。

3. You showed the bad case can't happen. You walked the failure step by step and it doesn't reach.

> 3. 你证明了坏情况不会发生。你一步步走了一遍失败路径，它到达不了。

4. You ran it. A script or test that calls the real code and fails loud if you're wrong.

> 4. 你运行了它。一个调用真实代码的脚本或测试，如果你错了它会大声失败。

5. You reproduced it in the running app.

> 5. 你在运行中的应用里复现了它。

Step 4 is usually one small script that imports the same library the app ships and calls the exact function you're worried about.

> 第 4 级通常就是一个小脚本：导入应用所用的同一个库，调用你所担心的那个确切的函数。

## Steps｜步骤

1. Read the change. The diff, the symbols it adds, changes, and deletes, and what it now does differently, including the part the diff doesn't spell out. Use `why` step 2 to pull the PR and commits.

> 1. 读这个改动。diff、它新增、修改和删除的符号，以及它现在的行为有什么不同，包括 diff 没有写明的部分。用 `why` 的第 2 步拉取 PR 和提交。

2. Find the one fact it's safe because of. Most changes that look risky are safe because of a single fact, like "this call only drops already-dead cache entries and does nothing else". Find that fact. If it holds, most risky cases are cleared at once. Spend your time here, not on a long list of maybes.

> 2. 找出它之所以安全所依赖的那一个事实。大多数看起来有风险的改动之所以安全，是因为某一个事实，比如“这个调用只会丢掉本来就已失效的缓存条目，别的什么都不做”。找到那个事实。如果它成立，大多数有风险的情况就一下子都排除了。把时间花在这里，而不是一长串“也许”上。

3. Look where grep stops. Read the source of the library you call, and check its pinned version and any local patch. Work out when things run: microtasks, unmount and teardown, Solid versus React. Follow what a symbol search misses: the JSON an API returns, a DB column, a wire format, another language reading the same bytes, a feature flag, code three hops downstream.

> 3. 去 grep 到不了的地方看。读你调用的库的源码，检查它锁定的版本和任何本地补丁。弄清楚东西什么时候运行：微任务、卸载和清理、Solid 和 React 的区别。追踪符号搜索会漏掉的东西：API 返回的 JSON、数据库的某一列、线路格式、用另一种语言读取同样字节的代码、功能开关、下游隔了三跳的代码。

4. Be honest about each risk. Give it a real chance of happening and a real cost if it does. Keep the risks you confirmed. List the ones you checked and cleared separately. Same rules as `why`. Cite a real `file:line`, a search that finds nothing is still an answer, and never make up a caller or an API.

> 4. 对每个风险诚实。给它一个真实的发生概率，以及发生时的真实代价。保留你确认过的风险。把你检查过并排除掉的单独列出来。规则和 `why` 一样。引用真实的 `文件:行`，什么都没搜到也是一个答案，永远不要编造调用方或 API。

5. Prove the one fact. Write a script or test that runs the real code, run it, and paste what happened.

> 5. 证明那一个事实。写一个运行真实代码的脚本或测试，运行它，把结果贴出来。

6. For a big or wide change, run it as an `arena`. Ask more than one model the same question and merge the answers. Different models catch different real bugs.

> 6. 对于大的或影响面广的改动，用 `arena` 来跑。用同一个问题问不止一个模型，然后合并答案。不同的模型会抓到不同的真 bug。

## What to hand back｜交回什么

- **What it does.** What changed, including the part that isn't obvious.

> - **它做了什么。** 改了什么，包括不明显的部分。

- **The one fact it's safe because of.** State it, say which step you got it to, and show the proof. If you couldn't prove it, write unproven.

> - **它之所以安全所依赖的那一个事实。** 说出这个事实，说明你把它推到了第几级，并展示证明。如果你没能证明它，就写“未证明”。

- **Risks.** Each names how it breaks, the `file:line`, how likely and how bad, and how to check. Paste the proof for the ones that matter.

> - **风险。** 每个风险都说明它怎么坏、`文件:行`、可能性和严重程度，以及怎么检查。重要的风险要贴出证明。

- **Cleared.** What you checked and why it's fine.

> - **已排除。** 你检查过什么，以及为什么没问题。

- **Before you merge.** The cheapest test or repro that catches the real bug, including the script you wrote.

> - **合并之前。** 能抓住真正 bug 的最便宜的测试或复现，包括你写的那个脚本。

Write it through `unslop`, cite real code, and strip anything private before it goes anywhere public.

> 用 `unslop` 来写，引用真实的代码，在发到任何公开的地方之前去掉所有私密内容。

**Reply:** the writeup above, with the one safety fact either proven or marked unproven.

> **回复：** 上面的分析，那一个安全事实要么已证明，要么标记为未证明。
