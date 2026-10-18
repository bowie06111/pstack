<!-- 中英对照译文。原文：skills/correct/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/correct/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/correct/SKILL.md)

```yaml
name: correct
description: "Find the mistakes agents keep repeating in this repo and make each one impossible. Try architecture first, then types, then a lint whose error names the fix, then a test, and write docs last. Prove each check fails on a real past mistake. Repeat this each time the operator corrects you. Use for /correct."
disable-model-invocation: true
```

> **说明（description）：** 找出代理在这个仓库里反复犯的错误，让每一个都变得不可能。先试架构，然后是类型，然后是错误信息会说出修法的 lint，然后是测试，最后才写文档。证明每个检查都会在一个真实的历史错误上失败。每次操作者纠正你时都重复这个过程。用于 /correct。

# Correct｜纠正

The operator keeps correcting agents in this repo for the same mistakes. Change the repo so the next agent can't make them.

> 操作者一直在为同样的错误纠正这个仓库里的代理。改变仓库，让下一个代理没法再犯。

Assume every contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Design the repo so a change that looks right from one file is right for the whole repo.

> 假设每个贡献者都是一个代理：它只看到自己打开的文件，照抄最近的例子，走能编译通过的最短路径。把仓库设计成：一个从单个文件看起来正确的改动，对整个仓库也是正确的。

## Find the mistake classes｜找出错误类别

First, read recent commits, reverts, review comments, agent instruction files, and comments that explain workarounds. Group the mistakes into classes. A class counts once it has happened twice.

> 首先，读最近的提交、回滚、评审评论、代理指令文件，以及解释变通办法的注释。把错误分成类别。一个类别发生过两次才算数。

## Fix each class at the highest level that works｜在能奏效的最高层级修复每个类别

1. **Eliminate it with architecture.** Give each piece of state one owner and each task one supported way. Hide internals so the wrong import fails. Replace hand-synced lists with one source of truth. Delete old ways and dead code an agent would copy.

> 1. **用架构消除它。** 让每块状态只有一个所有者，每项任务只有一种受支持的做法。隐藏内部实现，让错误的导入直接失败。用单一事实来源代替手工同步的列表。删掉代理会照抄的旧做法和死代码。

2. **Enforce it with types so the bad state can't be written.** If bad code still compiles, add a lint or CI check whose error names the file, type, or function to use instead. If the pattern is already common, fail only when a change adds more.

> 2. **用类型强制它，让错误的状态写不出来。** 如果错误的代码仍然能编译，就加一个 lint 或 CI 检查，它的错误信息要说出应该改用哪个文件、类型或函数。如果这种写法已经很普遍，只在某次改动又增加了它时才失败。

3. **Test the behavior.** Fix or delete any test that still passes when the behavior it checks breaks, or that fails when only the internals change.

> 3. **测试行为。** 修好或删掉那些在所检查的行为坏掉时仍然通过、或者只改了内部就失败的测试。

4. **Write docs or agent rules last, only for judgment calls.** Nothing fails when an agent skips them.

> 4. **最后才写文档或代理规则，而且只用于需要判断的事情。** 代理跳过它们时，什么都不会失败。

## Fix and prove｜修复并证明

Then fix the most frequent classes now, one commit each. Prove each new check fails on a real past mistake. Run the same command locally and in CI. Exceptions go on the offending line with a reason, an expiry date, and a human's approval.

> 然后现在就修最常见的几个类别，每个一次提交。证明每个新检查都会在一个真实的历史错误上失败。在本地和 CI 里运行同一条命令。例外写在出问题的那一行上，附上理由、到期日期和一个人的批准。

## Keep the rule table｜维护规则表

Last, keep a table in the agent instruction file that pairs each rule with what enforces it. When the operator corrects you, fix the mistake and add the rule. If the rule was already there and nothing enforces it, that's a repeat, so fix it at the highest level in the same change. Drop a rule once its mistake can't happen.

> 最后，在代理指令文件里维护一张表，把每条规则和执行它的东西配对。当操作者纠正你时，修好错误并加上规则。如果规则早就在那里而没有东西执行它，那就是重犯，所以在同一个改动里在最高层级修复它。一旦某个错误不可能再发生，就删掉对应的规则。

**Reply:** each class with its evidence, the level you picked, and why a higher level didn't work.

> **回复：** 每个类别及其证据、你选的层级，以及为什么更高的层级行不通。
