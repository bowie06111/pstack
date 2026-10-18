<!-- 中英对照译文。原文：skills/create-verification-skill/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/create-verification-skill/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/SKILL.md)

```yaml
name: create-verification-skill
description: "Generate a project-local verification skill that drives your app the way a user does — any language, framework, or platform. Use for /create-verification-skill, \"make a control skill for this repo\", or when a project has no scripted way to prove UI/CLI/service behavior."
disable-model-invocation: true
```

> **说明（description）：** 生成一个项目级的验证技能，让它像用户那样操作你的应用，适用于任何语言、框架或平台。用于 /create-verification-skill、“为这个仓库做一个控制技能”，或者当项目没有用脚本证明 UI/命令行/服务行为的方式时。

# Create a verification skill｜创建验证技能

Every serious project needs a scripted way to drive the real app and prove behavior: launch it, exercise a feature the way a user would, and capture evidence. This skill generates that as a project-local skill (`<skills>/verify-<app>/`) tailored to the repo. `<skills>` is your harness's project skill folder: `.cursor/skills/` in Cursor, `.claude/skills/` in Claude Code, `.pi/skills/` in Pi, `.agents/skills/` in Codex, OpenCode, and others. If the repo already keeps skills in one of these, use that one. You write the generator's output for the next agent, not for a human: it will be read cold, mid-task, by an agent that has never seen the app.

> 每个正经的项目都需要一种用脚本操作真实应用、证明其行为的方式：启动它，像用户那样使用某个功能，并收集证据。这个技能把它生成为一个为该仓库量身定制的项目级技能（`<skills>/verify-<app>/`）。`<skills>` 是你运行环境的项目技能文件夹：Cursor 里是 `.cursor/skills/`，Claude Code 里是 `.claude/skills/`，Pi 里是 `.pi/skills/`，Codex、OpenCode 等是 `.agents/skills/`。如果仓库已经把技能放在其中某个文件夹里，就用那个。生成器的输出是写给下一个代理看的，而不是给人看的：它会在任务进行到一半时，被一个从没见过这个应用的代理毫无准备地读到。

## 1. Interview the repo, not the user｜1. 访谈仓库，而不是用户

Answer these from the codebase and only ask the user what you cannot observe:

> 从代码库回答下面这些问题，只有你观察不到的才去问用户：

- **Surface:** what does a user actually touch? A web UI, a CLI/TUI, a desktop app, an API, a mobile app, a library? A repo can have several; pick the primary one and note the rest.

> - **界面：** 用户实际碰的是什么？网页 UI、命令行/终端界面、桌面应用、API、移动应用，还是一个库？一个仓库可能有好几个；选主要的那个，记下其余的。

- **Run:** how does the app start locally? Prefer the repo's own documented dev command (package scripts, Makefile, README quickstart). Note ports, env vars, seed data, auth.

> - **运行：** 应用在本地怎么启动？优先用仓库自己文档里写的开发命令（package 脚本、Makefile、README 快速入门）。记下端口、环境变量、预置数据、认证。

- **Drive:** how can an agent interact with it programmatically? Existing harnesses first — Playwright/Cypress specs, expect scripts, PTY helpers, curl-able endpoints, a debug port. Only then pick a generic recipe: browser/CDP for web and Electron, a tmux/PTY harness for CLI/TUI, plain HTTP for services.

> - **操作：** 代理怎样用程序和它交互？先找已有的测试工具：Playwright/Cypress 测试、expect 脚本、PTY 辅助工具、能用 curl 访问的端点、调试端口。然后才选通用的方案：网页和 Electron 用浏览器/CDP，命令行/终端界面用 tmux/PTY 测试工具，服务用普通 HTTP。

- **Observe:** what evidence can be captured? Screenshots, terminal transcripts, response bodies, logs, exit codes, DB state.

> - **观察：** 能收集到什么证据？截图、终端记录、响应正文、日志、退出码、数据库状态。

- **Isolate:** can two instances run side by side (ports, data dirs, profiles)? If not, say so in the generated skill: refusing to double-drive a shared instance beats corrupting the user's session.

> - **隔离：** 能不能同时运行两个实例（端口、数据目录、配置文件）？如果不能，在生成的技能里写明：拒绝同时操作一个共享的实例，胜过弄坏用户的会话。

If the checkout doesn't build or start as-is, fix that first (or report it precisely) before generating; a skill written against a broken base teaches wrong steps. When an irrelevant missing asset blocks startup (a static dir the API never serves, a sample config), the generated skill may create it, clearly marked as verification scaffolding, and remove it in cleanup.

> 如果检出的代码原样无法构建或启动，先修好它（或者准确地报告问题），再生成技能；在坏掉的基础上写出来的技能会教错误的步骤。当一个无关的缺失资源阻碍了启动（API 从不提供的静态目录、一个示例配置）时，生成的技能可以创建它，清楚地标明是验证用的脚手架，并在清理时删掉。

## 2. Generate the skill｜2. 生成技能

Write `<skills>/verify-<app>/SKILL.md` with YAML frontmatter (`name: verify-<app>` and a `description` that names the app, the surface, and when to reach for it — without frontmatter the skill never registers) and these sections, each grounded in what the interview actually found (no placeholders left):

> 写入 `<skills>/verify-<app>/SKILL.md`，带 YAML frontmatter（`name: verify-<app>`，以及一个写明应用、界面和何时使用它的 `description`；没有 frontmatter，技能永远不会被注册），以及下面这些章节，每一节都要以访谈实际发现的内容为依据（不留占位符）：

- **Launch:** the exact command that starts the app for verification, and how to tell it's ready (a log line, a prompt). Launch must reach a usable state from an empty data dir, including any migrations and seed data. Ready means usable, not just that a process answers. A database health check can pass while its init scripts are still running. Include teardown. For a short-lived CLI or TUI there is no server to keep alive: launch means build the binary (or install deps) once, then start each drive in its own isolated PTY or tmux session.

> - **Launch（启动）：** 为验证启动应用的确切命令，以及如何判断它已就绪（一行日志、一个提示符）。启动必须能从一个空的数据目录达到可用状态，包括任何迁移和预置数据。就绪的意思是可用，而不只是有个进程在应答。数据库的健康检查可能已经通过，而它的初始化脚本还在运行。包括清理。对于短命的命令行或终端界面工具，没有需要保持运行的服务器：启动的意思是构建一次二进制（或安装依赖），然后每次操作都在自己隔离的 PTY 或 tmux 会话里开始。

- **Doctor:** one read-only check that answers "is this instance worth driving?" — process up, right version/build, port owned by us, auth valid. An agent runs this first whenever anything looks off.

> - **Doctor（自检）：** 一个只读的检查，回答“这个实例值得操作吗？”：进程在运行、版本/构建正确、端口归我们所有、认证有效。只要看起来有什么不对劲，代理就先运行它。

- **Drive:** the harness recipe with real selectors/commands from this repo, not examples. Prefer stable handles (ARIA labels, data attributes, prompt strings, route paths) over coordinates and tab order.

> - **Drive（操作）：** 测试工具的操作步骤，用这个仓库里真实的选择器/命令，而不是示例。优先用稳定的标识（ARIA 标签、data 属性、提示符字符串、路由路径），而不是坐标和 Tab 顺序。

- **Evidence:** what to capture for a proof and where it goes. State the proof standards: exercise the real user path, not internal setters or test-only endpoints; capture the action and the resulting state, not just the final screen; verify side effects (files written, rows inserted, messages sent) alongside what's visible; mocks only where a production boundary already isolates the external system. When the safe path is a dry-run or test mode, verify what it actually skips by observing (files, network, git refs) rather than trusting its name: some dry-runs still touch the network or open a browser.

> - **Evidence（取证）：** 为一次证明要收集什么、放在哪里。说明证明的标准：走真实的用户路径，而不是内部的 setter 或仅供测试的端点；记录动作和由此产生的状态，而不只是最终画面；在验证可见内容的同时验证副作用（写入的文件、插入的行、发出的消息）；只在生产环境的边界本来就隔离了外部系统的地方才用模拟。当安全的路径是试运行或测试模式时，通过观察（文件、网络、git 引用）来验证它实际跳过了什么，而不是相信它的名字：有些试运行仍然会访问网络或打开浏览器。

- **Cleanup:** how to tear down instances the run created. Never kill by process name; kill what you started. Cleanup removes instances and scratch state, never the evidence: proof artifacts survive the teardown, in a location the skill names.

> - **Cleanup（清理）：** 如何清理本次运行创建的实例。永远不要按进程名杀进程；杀你自己启动的。清理删除的是实例和临时状态，永远不删证据：证明材料在清理之后仍然保留，放在技能指定的位置。

- **Helpers:** any script the skill ships is executable and its invocation is shown in the skill body. A helper the reader has to reverse-engineer is not a helper.

> - **Helpers（辅助脚本）：** 技能附带的任何脚本都是可执行的，它的调用方式写在技能正文里。读者需要逆向分析才能用的辅助脚本，不算辅助脚本。

## 3. Seed the feature map｜3. 初始化功能地图

Create `<skills>/verify-<app>/features/README.md` plus one file per user-facing feature you can identify (aim for the top 3-5 to start, from routes, commands, menus, or docs). Follow the shape in [`references/feature-map-example/`](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/references/feature-map-example), with a README index and one file per feature. Each file answers, from the user's point of view: what the feature is, how to reach it, how to drive it with the harness, and what observable end state proves it works. The four H2s are `Sub-features`, `How to get to it (user POV)`, `Driving it with <harness>`, and `Gotchas`. If the seed data lacks a precondition that a journey needs, the journey reports a failure and the reason goes in that feature's `Gotchas`. Never weaken the journey to make it pass. The map is the repo's maintained verification source; a proof that drives one convenient entry point is incomplete when the map lists others.

> 创建 `<skills>/verify-<app>/features/README.md`，再为你能识别出的每个面向用户的功能各建一个文件（开始时以最重要的 3 到 5 个为目标，从路由、命令、菜单或文档里找）。按 [`references/feature-map-example/`](https://github.com/bowie06111/pstack/blob/main/skills/create-verification-skill/references/feature-map-example) 的形式来写：一个 README 索引，加上每个功能一个文件。每个文件都从用户的角度回答：这个功能是什么、怎么到达它、怎么用测试工具操作它，以及什么可观察的最终状态能证明它能用。四个 H2 是 `Sub-features`、`How to get to it (user POV)`、`Driving it with <harness>` 和 `Gotchas`。如果预置数据缺少某个流程需要的前提条件，这个流程就报告失败，原因写进该功能的 `Gotchas`。永远不要为了让它通过而削弱这个流程。地图是仓库维护的验证来源；当地图列出了其他入口时，只操作一个方便的入口所做的证明是不完整的。

## 4. Prove the generated skill before handing it over｜4. 交付之前先证明生成的技能

Run its own instructions end to end once: launch, doctor, drive ONE mapped feature (one is enough; the map exists so later runs can cover the rest), capture evidence, clean up. After cleanup, confirm the evidence still exists at the named location — a cleanup that eats the proof fails this step. Fix what fails, and run the generated cleanup after every failed iteration too, so broken attempts don't strand processes and ports. A generated skill that was never executed is a draft, not a deliverable. If you proved it in a stand-in for the real target (a container in place of a fresh VM, a local stack in place of a remote one), list every setup step the stand-in did that the target will not. Those steps stay unverified until a run on the real target covers them.

> 按它自己的指示从头到尾跑一遍：启动、自检、操作一个地图里的功能（一个就够了；地图的存在就是为了让之后的运行覆盖其余的）、收集证据、清理。清理之后，确认证据仍然在指定的位置。吃掉了证明的清理会让这一步失败。修好失败的地方，并且每次迭代失败后也要运行生成的清理，这样失败的尝试就不会遗留进程和端口。一个从没被执行过的生成技能是草稿，不是交付物。如果你是在真实目标的替代环境里证明它的（用容器代替全新的虚拟机，用本地栈代替远程栈），就列出替代环境做了、而真实目标不会做的每一个设置步骤。在真实目标上运行一次覆盖它们之前，这些步骤都还没有经过验证。

## 5. Offer the maintenance loop｜5. 提供维护循环

Point the user at `/maintain-verification-skill` for keeping the map honest as the app changes. Suggest a cadence only if they ask.

> 告诉用户用 `/maintain-verification-skill` 在应用变化时让地图保持真实。只有在用户问起时才建议频率。
