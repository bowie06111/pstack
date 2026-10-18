# pstack 中文对照译文

这里是 pstack 全部技能、剧本、原则和子代理原文的中英对照译文。

- 每个文件保留英文原文，紧跟其后的引用块（`>`）是中文译文；标题写成“英文｜中文”。
- frontmatter 以 yaml 代码块展示，并附上 description 的中文说明。
- 文件名统一为 `*.zh.md`，不会被 skills CLI 当成重复技能安装。
- 技能名、命令、路径保留英文，便于直接对照使用。

共 112 个文件。

## 核心：poteto-mode 与剧本（25）

- [Poteto mode｜Poteto 模式](skills/poteto-mode/SKILL.zh.md) — `skills/poteto-mode/SKILL.md`
- [authoring-a-skill.zh.md](skills/poteto-mode/playbooks/authoring-a-skill.zh.md) — `skills/poteto-mode/playbooks/authoring-a-skill.md`
- [autonomous-run.zh.md](skills/poteto-mode/playbooks/autonomous-run.zh.md) — `skills/poteto-mode/playbooks/autonomous-run.md`
- [autopilot-full.zh.md](skills/poteto-mode/playbooks/autopilot-full.zh.md) — `skills/poteto-mode/playbooks/autopilot-full.md`
- [autopilot-stack.zh.md](skills/poteto-mode/playbooks/autopilot-stack.zh.md) — `skills/poteto-mode/playbooks/autopilot-stack.md`
- [babysit.zh.md](skills/poteto-mode/playbooks/babysit.zh.md) — `skills/poteto-mode/playbooks/babysit.md`
- [bug-fix.zh.md](skills/poteto-mode/playbooks/bug-fix.zh.md) — `skills/poteto-mode/playbooks/bug-fix.md`
- [eval.zh.md](skills/poteto-mode/playbooks/eval.zh.md) — `skills/poteto-mode/playbooks/eval.md`
- [feature.zh.md](skills/poteto-mode/playbooks/feature.zh.md) — `skills/poteto-mode/playbooks/feature.md`
- [hillclimb.zh.md](skills/poteto-mode/playbooks/hillclimb.zh.md) — `skills/poteto-mode/playbooks/hillclimb.md`
- [investigation.zh.md](skills/poteto-mode/playbooks/investigation.zh.md) — `skills/poteto-mode/playbooks/investigation.md`
- [<Program> plan](skills/poteto-mode/playbooks/multi-phase-plan.zh.md) — `skills/poteto-mode/playbooks/multi-phase-plan.md`
- [opening-a-pr.zh.md](skills/poteto-mode/playbooks/opening-a-pr.zh.md) — `skills/poteto-mode/playbooks/opening-a-pr.md`
- [orchestrate.zh.md](skills/poteto-mode/playbooks/orchestrate.zh.md) — `skills/poteto-mode/playbooks/orchestrate.md`
- [pause-safely.zh.md](skills/poteto-mode/playbooks/pause-safely.zh.md) — `skills/poteto-mode/playbooks/pause-safely.md`
- [perf-issue.zh.md](skills/poteto-mode/playbooks/perf-issue.zh.md) — `skills/poteto-mode/playbooks/perf-issue.md`
- [prototype.zh.md](skills/poteto-mode/playbooks/prototype.zh.md) — `skills/poteto-mode/playbooks/prototype.md`
- [refactoring.zh.md](skills/poteto-mode/playbooks/refactoring.zh.md) — `skills/poteto-mode/playbooks/refactoring.md`
- [runtime-forensics.zh.md](skills/poteto-mode/playbooks/runtime-forensics.zh.md) — `skills/poteto-mode/playbooks/runtime-forensics.md`
- [session-pickup.zh.md](skills/poteto-mode/playbooks/session-pickup.zh.md) — `skills/poteto-mode/playbooks/session-pickup.md`
- [shipping.zh.md](skills/poteto-mode/playbooks/shipping.zh.md) — `skills/poteto-mode/playbooks/shipping.md`
- [trace-forensics.zh.md](skills/poteto-mode/playbooks/trace-forensics.zh.md) — `skills/poteto-mode/playbooks/trace-forensics.md`
- [visual-parity.zh.md](skills/poteto-mode/playbooks/visual-parity.zh.md) — `skills/poteto-mode/playbooks/visual-parity.md`
- [worktree-cleanup.zh.md](skills/poteto-mode/playbooks/worktree-cleanup.zh.md) — `skills/poteto-mode/playbooks/worktree-cleanup.md`
- [Bugbot triage｜Bugbot 分流](skills/poteto-mode/references/bugbot-triage.zh.md) — `skills/poteto-mode/references/bugbot-triage.md`

## 原则（principle-*）（24）

- [Attack the Premise｜攻击前提](skills/principle-attack-the-premise/SKILL.zh.md) — `skills/principle-attack-the-premise/SKILL.md`
- [Boundary Discipline｜边界纪律](skills/principle-boundary-discipline/SKILL.zh.md) — `skills/principle-boundary-discipline/SKILL.md`
- [Build the Lever｜打造杠杆](skills/principle-build-the-lever/SKILL.zh.md) — `skills/principle-build-the-lever/SKILL.md`
- [Encode Lessons in Structure｜把教训编码进结构](skills/principle-encode-lessons-in-structure/SKILL.zh.md) — `skills/principle-encode-lessons-in-structure/SKILL.md`
- [Exhaust the Design Space｜穷尽设计空间](skills/principle-exhaust-the-design-space/SKILL.zh.md) — `skills/principle-exhaust-the-design-space/SKILL.md`
- [Experience First｜体验优先](skills/principle-experience-first/SKILL.zh.md) — `skills/principle-experience-first/SKILL.md`
- [Explain the Number｜解释这个数字](skills/principle-explain-the-number/SKILL.zh.md) — `skills/principle-explain-the-number/SKILL.md`
- [Fix Root Causes｜修根因](skills/principle-fix-root-causes/SKILL.zh.md) — `skills/principle-fix-root-causes/SKILL.md`
- [Foundational Thinking｜基础思维](skills/principle-foundational-thinking/SKILL.zh.md) — `skills/principle-foundational-thinking/SKILL.md`
- [Guard the Context Window｜守护上下文窗口](skills/principle-guard-the-context-window/SKILL.zh.md) — `skills/principle-guard-the-context-window/SKILL.md`
- [Laziness Protocol｜懒惰协议](skills/principle-laziness-protocol/SKILL.zh.md) — `skills/principle-laziness-protocol/SKILL.md`
- [Make Operations Idempotent｜操作幂等](skills/principle-make-operations-idempotent/SKILL.zh.md) — `skills/principle-make-operations-idempotent/SKILL.md`
- [Migrate Callers Then Delete Legacy APIs｜先迁移调用方再删除旧接口](skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.zh.md) — `skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md`
- [Minimize Reader Load｜降低阅读负担](skills/principle-minimize-reader-load/SKILL.zh.md) — `skills/principle-minimize-reader-load/SKILL.md`
- [Model the Domain｜建模领域](skills/principle-model-the-domain/SKILL.zh.md) — `skills/principle-model-the-domain/SKILL.md`
- [Never Block on the Human｜别卡在人身上](skills/principle-never-block-on-the-human/SKILL.zh.md) — `skills/principle-never-block-on-the-human/SKILL.md`
- [Outcome-Oriented Execution｜以结果为导向执行](skills/principle-outcome-oriented-execution/SKILL.zh.md) — `skills/principle-outcome-oriented-execution/SKILL.md`
- [Prove It Works｜证明它能用](skills/principle-prove-it-works/SKILL.zh.md) — `skills/principle-prove-it-works/SKILL.md`
- [Redesign From First Principles｜从第一性原理重新设计](skills/principle-redesign-from-first-principles/SKILL.zh.md) — `skills/principle-redesign-from-first-principles/SKILL.md`
- [Separate Before Serializing Shared State｜先分离再串行化共享状态](skills/principle-separate-before-serializing-shared-state/SKILL.zh.md) — `skills/principle-separate-before-serializing-shared-state/SKILL.md`
- [Sequence work into verifiable units｜把工作拆成可验证的单元](skills/principle-sequence-verifiable-units/SKILL.zh.md) — `skills/principle-sequence-verifiable-units/SKILL.md`
- [Subtract Before You Add｜先减后加](skills/principle-subtract-before-you-add/SKILL.zh.md) — `skills/principle-subtract-before-you-add/SKILL.md`
- [Test Behavior, Not Implementation｜测试行为而非实现](skills/principle-test-behavior-not-implementation/SKILL.zh.md) — `skills/principle-test-behavior-not-implementation/SKILL.md`
- [Type System Discipline｜类型系统纪律](skills/principle-type-system-discipline/SKILL.zh.md) — `skills/principle-type-system-discipline/SKILL.md`

## 其他技能（61）

- [Architect｜Architect（架构师）](skills/architect/SKILL.zh.md) — `skills/architect/SKILL.md`
- [Design red flags｜设计危险信号](skills/architect/references/design-red-flags.zh.md) — `skills/architect/references/design-red-flags.md`
- [Rationale template｜设计理由模板](skills/architect/references/rationale-template.zh.md) — `skills/architect/references/rationale-template.md`
- [Architect runner prompt｜Architect 执行者提示词](skills/architect/references/runner-prompt.zh.md) — `skills/architect/references/runner-prompt.md`
- [Arena｜Arena（竞技场）](skills/arena/SKILL.zh.md) — `skills/arena/SKILL.md`
- [Automate me｜Automate me（把我自动化）](skills/automate-me/SKILL.zh.md) — `skills/automate-me/SKILL.md`
- [Benchmark checklist｜基准测试检查清单](skills/benchmark-checklist/SKILL.zh.md) — `skills/benchmark-checklist/SKILL.md`
- [Blast radius｜影响范围（Blast radius）](skills/blast-radius/SKILL.zh.md) — `skills/blast-radius/SKILL.md`
- [SKILL.zh.md](skills/bro/SKILL.zh.md) — `skills/bro/SKILL.md`
- [Correct｜纠正](skills/correct/SKILL.zh.md) — `skills/correct/SKILL.md`
- [Create a verification skill｜创建验证技能](skills/create-verification-skill/SKILL.zh.md) — `skills/create-verification-skill/SKILL.md`
- [Notes verification map｜Notes 验证地图](skills/create-verification-skill/references/feature-map-example/README.zh.md) — `skills/create-verification-skill/references/feature-map-example/README.md`
- [Create a note｜创建笔记](skills/create-verification-skill/references/feature-map-example/create-note.zh.md) — `skills/create-verification-skill/references/feature-map-example/create-note.md`
- [Search notes｜搜索笔记](skills/create-verification-skill/references/feature-map-example/search.zh.md) — `skills/create-verification-skill/references/feature-map-example/search.md`
- [Figure it out｜想办法搞定（Figure it out）](skills/figure-it-out/SKILL.zh.md) — `skills/figure-it-out/SKILL.md`
- [How｜How（怎么工作）](skills/how/SKILL.zh.md) — `skills/how/SKILL.md`
- [Explainer Prompt Template｜讲解者提示词模板](skills/how/references/explainer-prompt.zh.md) — `skills/how/references/explainer-prompt.md`
- [Explorer Prompt Template｜探索者提示词模板](skills/how/references/explorer-prompt.zh.md) — `skills/how/references/explorer-prompt.md`
- [SKILL.zh.md](skills/in-your-own-words/SKILL.zh.md) — `skills/in-your-own-words/SKILL.md`
- [Interrogate｜Interrogate（审问）](skills/interrogate/SKILL.zh.md) — `skills/interrogate/SKILL.md`
- [Code Quality Review｜代码质量审查](skills/interrogate/references/code-quality-review.zh.md) — `skills/interrogate/references/code-quality-review.md`
- [Lead Judgment Framework｜主审判断框架](skills/interrogate/references/lead-judgment.zh.md) — `skills/interrogate/references/lead-judgment.md`
- [Reviewer Prompt Template｜审查者提示词模板](skills/interrogate/references/reviewer-prompt.zh.md) — `skills/interrogate/references/reviewer-prompt.md`
- [Review Rubric｜审查评分标准](skills/interrogate/references/rubric.zh.md) — `skills/interrogate/references/rubric.md`
- [Ketchup｜Ketchup（跟上进度）](skills/ketchup/SKILL.zh.md) — `skills/ketchup/SKILL.md`
- [Maintain a verification skill｜维护验证技能](skills/maintain-verification-skill/SKILL.zh.md) — `skills/maintain-verification-skill/SKILL.md`
- [How to make a bot UI｜怎么做一个机器人界面](skills/make-bot-ui/SKILL.zh.md) — `skills/make-bot-ui/SKILL.md`
- [No comments｜不要注释](skills/no-comments/SKILL.zh.md) — `skills/no-comments/SKILL.md`
- [Comment Sicko｜注释狂魔（Comment Sicko）](skills/no-comments/references/comment-sicko.zh.md) — `skills/no-comments/references/comment-sicko.md`
- [Poteto help｜Poteto 帮助](skills/poteto-help/SKILL.zh.md) — `skills/poteto-help/SKILL.md`
- [Word the prompt｜怎么写提示词](skills/poteto-help/references/prompting.zh.md) — `skills/poteto-help/references/prompting.md`
- [Prompts worth copying｜值得照抄的提示词](skills/poteto-help/references/recipes.zh.md) — `skills/poteto-help/references/recipes.md`
- [Recall｜Recall（回忆）](skills/recall/SKILL.zh.md) — `skills/recall/SKILL.md`
- [Reflect｜Reflect（复盘）](skills/reflect/SKILL.zh.md) — `skills/reflect/SKILL.md`
- [divergent-reviewer.zh.md](skills/reflect/references/divergent-reviewer.zh.md) — `skills/reflect/references/divergent-reviewer.md`
- [judgment-reviewer.zh.md](skills/reflect/references/judgment-reviewer.zh.md) — `skills/reflect/references/judgment-reviewer.md`
- [synthesizer.zh.md](skills/reflect/references/synthesizer.zh.md) — `skills/reflect/references/synthesizer.md`
- [tooling-reviewer.zh.md](skills/reflect/references/tooling-reviewer.zh.md) — `skills/reflect/references/tooling-reviewer.md`
- [Setup pstack｜设置 pstack](skills/setup-pstack/SKILL.zh.md) — `skills/setup-pstack/SKILL.md`
- [SKILL.zh.md](skills/sharpen/SKILL.zh.md) — `skills/sharpen/SKILL.md`
- [Show me your work｜Show me your work（展示你的工作）](skills/show-me-your-work/SKILL.zh.md) — `skills/show-me-your-work/SKILL.md`
- [Swarm｜Swarm（蜂群）](skills/swarm/SKILL.zh.md) — `skills/swarm/SKILL.md`
- [TDD Bug Fix｜TDD 修 bug](skills/tdd/SKILL.zh.md) — `skills/tdd/SKILL.md`
- [Teach｜Teach（讲解）](skills/teach/SKILL.zh.md) — `skills/teach/SKILL.md`
- [Technical writing｜技术写作](skills/technical-writing/SKILL.zh.md) — `skills/technical-writing/SKILL.md`
- [TypeScript best practices｜TypeScript 最佳实践](skills/typescript-best-practices/SKILL.zh.md) — `skills/typescript-best-practices/SKILL.md`
- [TypeScript patterns｜TypeScript 写法示例](skills/typescript-best-practices/references/patterns.zh.md) — `skills/typescript-best-practices/references/patterns.md`
- [Unslop｜Unslop（去 AI 腔）](skills/unslop/SKILL.zh.md) — `skills/unslop/SKILL.md`
- [Why｜Why（为什么）](skills/why/SKILL.zh.md) — `skills/why/SKILL.md`
- [Epistemics｜认识论](skills/why/references/epistemics.zh.md) — `skills/why/references/epistemics.md`
- [Investigator Prompt Template｜调查者提示词模板](skills/why/references/investigator-prompt.zh.md) — `skills/why/references/investigator-prompt.md`
- [Source playbooks｜各信息源的操作手册](skills/why/references/source-playbook.zh.md) — `skills/why/references/source-playbook.md`
- [Code Archaeology (git + in-repo)｜代码考古（git + 仓库内）](skills/why/references/sources/code-archaeology.zh.md) — `skills/why/references/sources/code-archaeology.md`
- [Databricks Analytics & System Tables｜Databricks 分析与系统表](skills/why/references/sources/databricks.zh.md) — `skills/why/references/sources/databricks.md`
- [Datadog Telemetry｜Datadog 遥测](skills/why/references/sources/datadog.zh.md) — `skills/why/references/sources/datadog.md`
- [Incident & Postmortem Context｜事故与复盘背景](skills/why/references/sources/incident-postmortem.zh.md) — `skills/why/references/sources/incident-postmortem.md`
- [Linear Tickets｜Linear 工单](skills/why/references/sources/linear.zh.md) — `skills/why/references/sources/linear.md`
- [Notion Docs｜Notion 文档](skills/why/references/sources/notion.zh.md) — `skills/why/references/sources/notion.md`
- [Sentry Error History｜Sentry 错误历史](skills/why/references/sources/sentry.zh.md) — `skills/why/references/sources/sentry.md`
- [Slack Conversations｜Slack 对话](skills/why/references/sources/slack.zh.md) — `skills/why/references/sources/slack.md`
- [Synthesizer Prompt Template｜综合者提示词模板](skills/why/references/synthesizer-prompt.zh.md) — `skills/why/references/synthesizer-prompt.md`

## 子代理（agents）（2）

- [Comment Sicko｜注释狂魔（Comment Sicko）](agents/comment-sicko.zh.md) — `agents/comment-sicko.md`
- [Poteto subagent｜Poteto 子代理](agents/poteto-agent.zh.md) — `agents/poteto-agent.md`
