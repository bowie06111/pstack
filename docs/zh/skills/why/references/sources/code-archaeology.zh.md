<!-- 中英对照译文。原文：skills/why/references/sources/code-archaeology.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/sources/code-archaeology.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/sources/code-archaeology.md)

# Code Archaeology (git + in-repo)｜代码考古（git + 仓库内）

## What this source contains｜这个信息源包含什么

- Commit history (messages, dates, authors, diffs)

> - 提交历史（信息、日期、作者、diff）

- PR descriptions, review comments, and discussion threads (via `gh`)

> - PR 描述、评审评论和讨论串（通过 `gh`）

- Inline code comments, TODOs, FIXMEs, deprecation notes

> - 代码内注释、TODO、FIXME、弃用说明

- ADRs (architectural decision records) if the repo keeps them

> - ADR（架构决策记录），如果仓库有保留的话

- Tests, whose names and assertions often encode the edge cases that motivated a change

> - 测试，它们的名字和断言经常记录着促成某个改动的边界情况

- Related files modified in the same commits (co-change signal)

> - 在同一批提交里被修改的相关文件（共同变更信号）

- CHANGELOG entries and release notes in the repo

> - 仓库里的 CHANGELOG 条目和发布说明

- Issue/ticket IDs in commit messages and PR bodies

> - 提交信息和 PR 正文里的问题/工单编号

The most trustworthy source, tied directly to the code, and the most complete. Everything that went through the repo should be here.

> 最可信的信息源，直接和代码绑定，也最完整。所有经过仓库的东西都应该在这里。

## How to search it｜怎么搜索

Expand the seed commit list:

> 扩展种子提交列表：

```bash
# History through renames
git log --follow --oneline -- <file>

# Pickaxe: commits that added or removed this exact text
git log -S '<exact_string_from_code>' -- <file>

# Or for patterns:
git log -G '<regex>' -- <file>

# Who wrote each line and when
git blame -L <start>,<end> <file>

# One commit's full diff
git show <hash>

# Commits between two points affecting this file
git log <old>..<new> -p -- <file>
```

> 上面代码块的中文版：

```bash
# 跨越重命名的历史
git log --follow --oneline -- <file>

# Pickaxe：加入或删除了这段确切文字的提交
git log -S '<代码里的确切字符串>' -- <file>

# 或者按模式：
git log -G '<正则>' -- <file>

# 每一行是谁、在什么时候写的
git blame -L <start>,<end> <file>

# 一个提交的完整 diff
git show <hash>

# 两个点之间影响这个文件的提交
git log <old>..<new> -p -- <file>
```

Look for out-of-band docs:

> 找仓库之外的文档：

```bash
# ADRs often live in docs/adr/ or similar
rg -l -i 'architecture.decision' --glob '*.md'

# TODOs and FIXMEs near the target
rg -n -C2 '(TODO|FIXME|HACK|XXX|NOTE)' <target_file>

# Related tests
rg -l '<symbol>' --glob '*test*'
```

> 上面代码块的中文版：

```bash
# ADR 通常放在 docs/adr/ 或类似的地方
rg -l -i 'architecture.decision' --glob '*.md'

# 目标附近的 TODO 和 FIXME
rg -n -C2 '(TODO|FIXME|HACK|XXX|NOTE)' <target_file>

# 相关的测试
rg -l '<symbol>' --glob '*test*'
```
