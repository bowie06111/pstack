<!-- 中英对照译文。原文：skills/create-verification-skill/references/feature-map-example/search.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/create-verification-skill/references/feature-map-example/search.md](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/references/feature-map-example/search.md)

# Search notes｜搜索笔记

Search lets a user find notes by title or body text, inspect a matching note, and distinguish no matches from an unavailable search.

> 搜索让用户可以按标题或正文找到笔记、查看一条匹配的笔记，并区分“没有匹配”和“搜索不可用”。

## Sub-features｜子功能

- `search-open` opens search from each supported browser entry point.

> - `search-open` 从每个支持的浏览器入口打开搜索。

- `search-match` returns title and body matches without changing note data.

> - `search-match` 返回标题和正文的匹配，不改变笔记数据。

- `search-open-result` opens a result in the note editor.

> - `search-open-result` 在笔记编辑器里打开一个结果。

- `search-empty` shows a complete empty state for a query with no matches.

> - `search-empty` 对没有匹配的查询显示完整的空状态。

- `search-clear` removes the query and restores the recent-notes view.

> - `search-clear` 清除查询，恢复最近笔记视图。

- `search-cli` returns the same matching notes from the terminal.

> - `search-cli` 从终端返回同样的匹配笔记。

## How to get to it (user POV)｜怎么到达这里（用户视角）

- Choose the `Search` button in the browser toolbar.

> - 在浏览器工具栏点 `Search` 按钮。

- Press `/` in the browser while focus is outside an editable field.

> - 焦点不在可编辑字段里时，在浏览器里按 `/`。

- Run `notes search <query>` in a terminal.

> - 在终端运行 `notes search <query>`。

## Driving it with control-notes｜用 control-notes 操作

Preconditions:

> 前提条件：

- Notes is healthy at `http://127.0.0.1:4173`.

> - Notes 在 `http://127.0.0.1:4173` 运行正常。

- The disposable data directory contains `Quarterly plan` with body text `Draft budget`.

> - 用完即弃的数据目录里有 `Quarterly plan`，正文为 `Draft budget`。

- `control-notes doctor` reports the expected URL and data directory.

> - `control-notes doctor` 报告了预期的 URL 和数据目录。

- **Toolbar entry.** Choose the `Search` button. Run `control-notes browser click --role button --name "Search"`. A dialog named `Search notes` appears with focus in its searchbox.

> - **工具栏入口。** 点 `Search` 按钮。运行 `control-notes browser click --role button --name "Search"`。出现一个名为 `Search notes` 的对话框，焦点在它的搜索框里。

- **Keyboard entry.** Close the dialog, focus the page, and press `/`. Run `control-notes browser press --key "/"`. The same dialog appears and the page does not insert a slash.

> - **键盘入口。** 关闭对话框，让页面获得焦点，按 `/`。运行 `control-notes browser press --key "/"`。出现同一个对话框，页面没有插入斜杠。

- **Title match.** Type `quarterly`. Run `control-notes browser fill --role searchbox --name "Search notes" --value "quarterly"`. The `Search results` list contains `Quarterly plan` and does not contain `Grocery list`.

> - **标题匹配。** 输入 `quarterly`。运行 `control-notes browser fill --role searchbox --name "Search notes" --value "quarterly"`。`Search results` 列表里有 `Quarterly plan`，没有 `Grocery list`。

- **Body match.** Replace the query with `budget`. Run `control-notes browser fill --role searchbox --name "Search notes" --value "budget"`. The result `Quarterly plan` remains visible with a body-match excerpt.

> - **正文匹配。** 把查询换成 `budget`。运行 `control-notes browser fill --role searchbox --name "Search notes" --value "budget"`。结果 `Quarterly plan` 仍然可见，并带有正文匹配的摘录。

- **Open result.** Choose `Quarterly plan`. Run `control-notes browser click --role link --name "Quarterly plan"`. The dialog closes and the editor heading reads `Quarterly plan`.

> - **打开结果。** 点 `Quarterly plan`。运行 `control-notes browser click --role link --name "Quarterly plan"`。对话框关闭，编辑器标题显示 `Quarterly plan`。

- **Empty state.** Reopen search and enter `volcano`. Run `control-notes browser fill --role searchbox --name "Search notes" --value "volcano"`. A status named `No matching notes` appears after search completes.

> - **空状态。** 重新打开搜索，输入 `volcano`。运行 `control-notes browser fill --role searchbox --name "Search notes" --value "volcano"`。搜索完成后出现一个名为 `No matching notes` 的状态。

- **Clear query.** Choose `Clear search`. Run `control-notes browser click --role button --name "Clear search"`. The searchbox is empty and the `Recent notes` region replaces the result list.

> - **清除查询。** 点 `Clear search`。运行 `control-notes browser click --role button --name "Clear search"`。搜索框变空，`Recent notes` 区域取代了结果列表。

- **CLI match.** Search from the terminal. Run `control-notes cli -- notes search "quarterly" --format json`. Exit code `0` and stdout contain one object whose title is `Quarterly plan`.

> - **命令行匹配。** 从终端搜索。运行 `control-notes cli -- notes search "quarterly" --format json`。退出码为 `0`，stdout 里有一个标题为 `Quarterly plan` 的对象。

- **CLI miss.** Search for an absent value. Run `control-notes cli -- notes search "volcano" --format json`. Exit code `0` and stdout are `[]`.

> - **命令行无匹配。** 搜索一个不存在的值。运行 `control-notes cli -- notes search "volcano" --format json`。退出码为 `0`，stdout 为 `[]`。

- **Proof.** Capture the populated result state. Run `control-notes browser snapshot --aria --path artifacts/search/results.aria.txt` and `control-notes browser screenshot --path artifacts/search/results.png`. Both artifacts identify Notes, the query, and `Quarterly plan`.

> - **证明。** 记录有结果时的状态。运行 `control-notes browser snapshot --aria --path artifacts/search/results.aria.txt` 和 `control-notes browser screenshot --path artifacts/search/results.png`。两个证明材料都能辨认出 Notes、查询内容和 `Quarterly plan`。

## Gotchas｜注意事项

- Pressing `/` while the editor or searchbox has focus inserts text instead of opening search.

> - 编辑器或搜索框有焦点时按 `/`，会插入文字而不是打开搜索。

- Results update after a short debounce. Wait for the results list or empty status, not a fixed sleep.

> - 结果在短暂的防抖延迟后才更新。等结果列表或空状态出现，而不是固定睡眠一段时间。

- Archived notes are excluded unless the user enables `Include archived`.

> - 已归档的笔记会被排除，除非用户打开 `Include archived`。

- The CLI defaults to human-readable output. Use `--format json` for stable assertions.

> - 命令行默认输出给人看的格式。用 `--format json` 做稳定的断言。

- Opening a result changes browser state. Reopen search before proving another query.

> - 打开一个结果会改变浏览器状态。证明下一个查询之前，先重新打开搜索。
