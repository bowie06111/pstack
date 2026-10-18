<!-- 中英对照译文。原文：skills/create-verification-skill/references/feature-map-example/README.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/create-verification-skill/references/feature-map-example/README.md](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/references/feature-map-example/README.md)

# Notes verification map｜Notes 验证地图

This directory is the maintained source for verifying the user-facing behavior of Notes. Read the index before driving the app, then use the matching feature file as the recipe.

> 这个目录是验证 Notes 面向用户的行为时所维护的来源。操作应用之前先读这份索引，然后把对应的功能文件当作操作步骤。

## Baseline preconditions｜基线前提条件

- Launch Notes at `http://127.0.0.1:4173` with a disposable data directory.

> - 在 `http://127.0.0.1:4173` 启动 Notes，使用一个用完即弃的数据目录。

- Set `NOTES_DATA_DIR=/tmp/notes-verify-$RUN_ID` so concurrent runs do not share state.

> - 设置 `NOTES_DATA_DIR=/tmp/notes-verify-$RUN_ID`，让并发的运行不共享状态。

- Seed notes titled `Quarterly plan` and `Grocery list`.

> - 预置标题为 `Quarterly plan` 和 `Grocery list` 的笔记。

- Put `control-notes` and the `notes` CLI on `PATH`.

> - 把 `control-notes` 和 `notes` 命令行工具放进 `PATH`。

- Run `control-notes doctor` and require the expected URL, data directory, and build revision.

> - 运行 `control-notes doctor`，要求输出预期的 URL、数据目录和构建版本。

- Never drive an instance that was not started by this verification run.

> - 永远不要操作不是本次验证运行启动的实例。

## Driving conventions｜操作约定

- Start every recipe from the baseline state unless its preconditions say otherwise.

> - 每个操作步骤都从基线状态开始，除非它的前提条件另有说明。

- Prefer ARIA roles and accessible names over CSS selectors or DOM position.

> - 优先用 ARIA 角色和无障碍名称，而不是 CSS 选择器或 DOM 位置。

- Treat every command as literal. Keep quoted names and flags unchanged.

> - 把每条命令都当作字面量。保持带引号的名字和参数不变。

- Run browser actions through `control-notes browser`.

> - 浏览器操作通过 `control-notes browser` 运行。

- Run terminal actions through `control-notes cli -- <command>`.

> - 终端操作通过 `control-notes cli -- <命令>` 运行。

- Restore seeded data after a mutation. Do not remove proof artifacts during cleanup.

> - 修改之后恢复预置数据。清理时不要删除证明材料。

## Proof and skip reporting｜证明与跳过的报告方式

- Capture the user action and the resulting state, not only the final screen.

> - 记录用户的操作和由此产生的状态，而不只是最终画面。

- UI proof includes an ARIA snapshot and a screenshot with the app identity visible.

> - UI 的证明包括一份 ARIA 快照和一张能看到应用身份的截图。

- CLI proof includes the command, stdout, stderr, and exit code.

> - 命令行的证明包括命令、stdout、stderr 和退出码。

- Mutation proof includes a read-only second view of the stored value.

> - 修改的证明包括对存储值的一次只读的二次查看。

- Record the feature ID and entry point used with every artifact.

> - 每个证明材料都记录所用的功能 ID 和入口。

- Report an unreachable path with the attempted command and the unmet precondition.

> - 报告无法到达的路径时，附上尝试的命令和未满足的前提条件。

- Do not report a skipped entry point as verified through a different path.

> - 不要把一个被跳过的入口，报告成通过另一条路径验证过了。

## Feature entry contract｜功能条目的约定

Each feature file starts with an H1 title and one paragraph describing the user-visible behavior. It then uses exactly four H2 sections in this order.

> 每个功能文件以一个 H1 标题和一段描述用户可见行为的文字开头。然后按这个顺序使用恰好四个 H2 小节。

1. `Sub-features` lists short IDs with one line for each behavior.

> 1. `Sub-features` 列出简短的 ID，每种行为一行。

2. `How to get to it (user POV)` lists every user entry point.

> 2. `How to get to it (user POV)` 列出用户的每一个入口。

3. `Driving it with <harness>` starts with `Preconditions:` and uses labeled bullets that pair each user action with an exact command and observable result.

> 3. `Driving it with <harness>` 以 `Preconditions:` 开头，用带标签的要点把每个用户操作和一条确切的命令以及可观察的结果配对。

4. `Gotchas` lists traps that can waste or invalidate a verification run.

> 4. `Gotchas` 列出会浪费验证运行或让它失效的陷阱。

Keep implementation details out of the map. Name only user paths, stable handles, required state, commands, and observable proof.

> 地图里不要写实现细节。只写用户路径、稳定的标识、需要的状态、命令和可观察的证明。

## Features｜功能

- [Create a note](./create-note.zh.md) covers browser and CLI creation, cancellation, persistence, and cleanup.

> - [创建笔记](./create-note.zh.md) 覆盖浏览器和命令行创建、取消、持久化和清理。

- [Search notes](./search.zh.md) covers toolbar, keyboard, and CLI search with matching, empty, and clear states.

> - [搜索笔记](./search.zh.md) 覆盖工具栏、键盘和命令行搜索，包括有匹配、无结果和清除的状态。
