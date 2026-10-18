<!-- 中英对照译文。原文：skills/create-verification-skill/references/feature-map-example/create-note.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/create-verification-skill/references/feature-map-example/create-note.md](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/references/feature-map-example/create-note.md)

# Create a note｜创建笔记

Create note lets a user save a titled note from the browser or CLI, cancel an unfinished draft, and confirm the saved note from a second user-facing view.

> 创建笔记让用户可以从浏览器或命令行保存一条带标题的笔记，取消一份没写完的草稿，并从另一个面向用户的视图确认已保存的笔记。

## Sub-features｜子功能

- `create-open` opens a blank editor from each browser entry point.

> - `create-open` 从每个浏览器入口打开一个空白编辑器。

- `create-save` persists a title and body.

> - `create-save` 持久化标题和正文。

- `create-cancel` discards an unfinished browser draft.

> - `create-cancel` 丢弃一份没写完的浏览器草稿。

- `create-cli` creates the same note shape from the terminal.

> - `create-cli` 从终端创建同样形状的笔记。

## How to get to it (user POV)｜怎么到达这里（用户视角）

- Choose the `New note` button in the browser toolbar.

> - 在浏览器工具栏点 `New note` 按钮。

- Press `n` in the browser while focus is outside an editable field.

> - 焦点不在可编辑字段里时，在浏览器里按 `n`。

- Run `notes create --title <title> --body <body>` in a terminal.

> - 在终端运行 `notes create --title <title> --body <body>`。

## Driving it with control-notes｜用 control-notes 操作

Preconditions:

> 前提条件：

- Notes is healthy at `http://127.0.0.1:4173`.

> - Notes 在 `http://127.0.0.1:4173` 运行正常。

- No note is titled `Release checklist`.

> - 没有标题为 `Release checklist` 的笔记。

- `control-notes doctor` reports the expected URL and disposable data directory.

> - `control-notes doctor` 报告了预期的 URL 和用完即弃的数据目录。

- **Open editor.** Choose `New note`. Run `control-notes browser click --role button --name "New note"`. A form named `Note editor` appears with focus in the `Title` textbox.

> - **打开编辑器。** 点 `New note`。运行 `control-notes browser click --role button --name "New note"`。出现一个名为 `Note editor` 的表单，焦点在 `Title` 文本框里。

- **Enter content.** Type the title and body. Run `control-notes browser fill --role textbox --name "Title" --value "Release checklist"` and `control-notes browser fill --role textbox --name "Body" --value "Tag and publish"`. The `Save note` button becomes enabled.

> - **输入内容。** 输入标题和正文。运行 `control-notes browser fill --role textbox --name "Title" --value "Release checklist"` 和 `control-notes browser fill --role textbox --name "Body" --value "Tag and publish"`。`Save note` 按钮变为可用。

- **Save note.** Choose `Save note`. Run `control-notes browser click --role button --name "Save note"`. A status named `Note saved` appears and the heading reads `Release checklist`.

> - **保存笔记。** 点 `Save note`。运行 `control-notes browser click --role button --name "Save note"`。出现一个名为 `Note saved` 的状态，标题显示 `Release checklist`。

- **Confirm persistence.** Return to the note list and reopen the note. Run `control-notes browser click --role link --name "All notes"` and `control-notes browser click --role link --name "Release checklist"`. The editor shows both saved values.

> - **确认持久化。** 回到笔记列表，重新打开这条笔记。运行 `control-notes browser click --role link --name "All notes"` 和 `control-notes browser click --role link --name "Release checklist"`。编辑器显示两个已保存的值。

- **Cancel draft.** Open a new note, enter `Discard me`, and choose `Cancel`. Run `control-notes browser click --role button --name "New note"`, `control-notes browser fill --role textbox --name "Title" --value "Discard me"`, and `control-notes browser click --role button --name "Cancel"`. The note list returns and has no `Discard me` link.

> - **取消草稿。** 新建一条笔记，输入 `Discard me`，然后点 `Cancel`。运行 `control-notes browser click --role button --name "New note"`、`control-notes browser fill --role textbox --name "Title" --value "Discard me"` 和 `control-notes browser click --role button --name "Cancel"`。回到笔记列表，里面没有 `Discard me` 链接。

- **CLI entry.** Create a second note. Run `control-notes cli -- notes create --title "CLI note" --body "Created from terminal" --format json`. Exit code `0` and stdout contain the new note ID and title.

> - **命令行入口。** 创建第二条笔记。运行 `control-notes cli -- notes create --title "CLI note" --body "Created from terminal" --format json`。退出码为 `0`，stdout 里包含新笔记的 ID 和标题。

- **Proof.** Reopen both saved notes from `All notes`. Run `control-notes browser snapshot --aria --path artifacts/create-note/list.aria.txt` and `control-notes browser screenshot --path artifacts/create-note/list.png`. The artifacts show `Release checklist` and `CLI note`.

> - **证明。** 从 `All notes` 重新打开两条已保存的笔记。运行 `control-notes browser snapshot --aria --path artifacts/create-note/list.aria.txt` 和 `control-notes browser screenshot --path artifacts/create-note/list.png`。证明材料里显示 `Release checklist` 和 `CLI note`。

## Gotchas｜注意事项

- Pressing `n` while a textbox has focus types the character instead of opening a new editor.

> - 文本框有焦点时按 `n`，会输入这个字符，而不是打开新编辑器。

- Titles are trimmed on save. Assert the rendered title, not the draft input value.

> - 标题在保存时会去掉首尾空格。断言渲染出来的标题，而不是草稿里输入的值。

- A save status alone is insufficient proof. Reopen the note from the list.

> - 只有一个保存状态不足以作为证明。要从列表重新打开这条笔记。

- Remove `Release checklist` and `CLI note` during fixture cleanup, but retain their proof artifacts.

> - 清理测试数据时删除 `Release checklist` 和 `CLI note`，但保留它们的证明材料。
