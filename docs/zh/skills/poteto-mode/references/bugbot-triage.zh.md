<!-- 中英对照译文。原文：skills/poteto-mode/references/bugbot-triage.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/references/bugbot-triage.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/references/bugbot-triage.md)

# Bugbot triage｜Bugbot 分流

Use this reference when the Babysit playbook (`../playbooks/babysit.md`) handles Bugbot or review-automation comments. The goal is not to ignore Bugbot by default. The goal is to stop treating every comment as a required code change.

> 当 Babysit 剧本（`../playbooks/babysit.md`）处理 Bugbot 或评审自动化工具的评论时，使用这份参考。目的不是默认忽略 Bugbot。目的是不再把每条评论都当作必须改代码。

## Decision rubric｜判定标准

Classify each Bugbot thread before acting:

> 在行动之前，先给每个 Bugbot 讨论分类：

- `fix`: The comment identifies a plausible correctness, security, privacy, data loss, auth, billing, migration, idempotency, race, or shipped-behavior issue. Fix it in the lowest owning PR, then reply with the commit SHA and resolve the thread.

> - `fix`（修复）：评论指出了一个可信的正确性、安全、隐私、数据丢失、认证、计费、迁移、幂等性、竞态或已发布行为的问题。在拥有该代码的最低那个 PR 里修复，然后回复提交 SHA 并关闭讨论。

- `dismiss`: The comment matches a documented low-risk noisy pattern, and the current code/context proves the concern does not need a code change. Reply with a short reason and resolve the thread.

> - `dismiss`（驳回）：评论符合一个有记录的低风险噪音模式，并且当前代码和上下文证明这个担心不需要改代码。回复一个简短的理由并关闭讨论。

- `ask`: The comment is novel, high-severity, security/privacy/data-related, or ambiguous. Ask the user instead of guessing.

> - `ask`（询问）：评论是新类型的、高严重度的、与安全/隐私/数据有关的，或者有歧义。去问用户，不要猜。

When in doubt, ask. Skipping a noisy code-quality comment is cheap; skipping a real data or security bug is not.

> 拿不准时，就问。跳过一条吵闹的代码质量评论代价很小；跳过一个真正的数据或安全 bug 代价不小。

## Learned pattern format｜已学到的模式的格式

Add future patterns in this shape:

> 以后新增的模式按这个形式写：

```markdown
### <short pattern name>

- Confidence: candidate | recurring | strong
- Skip when: <conditions that must be true>
- Do not skip when: <risk boundaries>
- Example signal: <phrases or code context that identify the pattern>
- Source: <PR/comment URL or short historical note>
```

Use `candidate` for one or two examples. Use `recurring` after multiple real dismissals. Use `strong` only when the pattern is narrow, repeatedly verified, and low-risk.

> 一两个例子时用 `candidate`（候选）。多次真实驳回之后用 `recurring`（反复出现）。只有当模式范围很窄、被反复验证并且风险很低时才用 `strong`（强）。

## Recurring skip candidates｜反复出现的可跳过候选

### Intentional UI or design-system visual changes｜有意的 UI 或设计系统视觉改动

- Confidence: candidate

> - 置信度：候选

- Skip when: The PR description, screenshots, design review, or nearby code makes the visual change explicit, and the Bugbot comment is only restating that a shared visual default changed.

> - 可跳过的情况：PR 描述、截图、设计评审或附近的代码明确说明了这个视觉改动，而 Bugbot 的评论只是在复述某个共享的视觉默认值变了。

- Do not skip when: The comment points to accessibility, focus visibility, keyboard navigation, color contrast, or a component API contract that the PR did not intentionally change.

> - 不可跳过的情况：评论指向无障碍、焦点可见性、键盘导航、颜色对比度，或者 PR 并没有有意改变的组件 API 契约。

- Example signal: Comments about focus outlines, button sizes, spacing, or shared component visual defaults where the owner replies "intentional" or "intended".

> - 识别信号：关于焦点轮廓、按钮大小、间距或共享组件视觉默认值的评论，而负责人回复“有意的”或“本来就是这样设计的”。

### Upstack or stack-local usage Bugbot cannot see｜Bugbot 看不到的上游或栈内用法

- Confidence: candidate

> - 置信度：候选

- Skip when: Bugbot flags an export, component, helper, or file as unused, and the active forge's PR list and diffs, upper-stack diffs, or PR context show it is used by a later PR in the stack.

> - 可跳过的情况：Bugbot 标出某个导出、组件、辅助函数或文件未被使用，而当前托管平台的 PR 列表和 diff、栈上方的 diff 或 PR 上下文显示，它被栈里后面的某个 PR 用到了。

- Do not skip when: The current PR is not part of a stack, the symbol is public API, or the supposed upstack use cannot be verified.

> - 不可跳过的情况：当前 PR 不属于某个栈，该符号是公开 API，或者所谓的上游用法无法核实。

- Example signal: "Exported component is never used" with a human reply like "used upstack".

> - 识别信号：“导出的组件从未被使用”，而有人回复“上游用到了”。

### Temporary duplication during parallel implementation｜并行实现期间的临时重复

- Confidence: candidate

> - 置信度：候选

- Skip when: The PR intentionally duplicates a small amount of code to keep a new path parallel to an old path that is being deleted, replaced, or proven out.

> - 可跳过的情况：PR 有意复制一小段代码，让一条新路径和一条正在被删除、替换或验证的旧路径并行。

- Do not skip when: The duplicated code changes security, billing, data access, API behavior, or a long-lived shared abstraction would clearly reduce risk.

> - 不可跳过的情况：重复的代码改变了安全、计费、数据访问、API 行为，或者一个长期存在的共享抽象显然能降低风险。

- Example signal: "Significant duplication" or "duplicated validation logic" where the owner explains the old path will be deleted or the duplicate logic is intentionally local.

> - 识别信号：“大量重复”或“重复的校验逻辑”，而负责人解释旧路径会被删除，或重复的逻辑是有意保持局部的。

### Existing framework or component invariant covers the warning｜已有的框架或组件不变量覆盖了这个警告

- Confidence: candidate

> - 置信度：候选

- Skip when: The concern is already guaranteed by a shared component, framework contract, type invariant, or single source of truth visible in the current diff or nearby code.

> - 可跳过的情况：这个担心已经由当前 diff 或附近代码里可见的共享组件、框架契约、类型不变量或单一事实来源保证了。

- Do not skip when: The invariant is assumed but not enforced, depends on timing, or crosses async/state boundaries where values can diverge.

> - 不可跳过的情况：不变量只是被假设而没有被强制，依赖时序，或者跨越了值可能出现分歧的异步/状态边界。

- Example signal: Comments about missing max-height on an inner popover when the shared popover enforces viewport bounds, or nullable values where the local checked value and passed value share the same source.

> - 识别信号：共享弹出层已经强制了视口边界，却有评论说内部弹出层缺少最大高度；或者本地检查的值和传出去的值来自同一个来源，却有评论说可能为空。

### Owner-declared follow-up or deferred cleanup｜负责人声明的后续工作或延后清理

- Confidence: candidate

> - 置信度：候选

- Skip when: The PR owner explicitly says the issue is a known follow-up, the behavior is not made worse by the current PR, and the comment is not about a high-risk area.

> - 可跳过的情况：PR 负责人明确说这个问题是已知的后续工作，当前 PR 没有让这个行为变得更糟，并且评论不涉及高风险领域。

- Do not skip when: The agent is acting without owner input, the issue is medium/high severity product behavior, or deferring would merge a new regression.

> - 不可跳过的情况：Agent 在没有负责人意见的情况下行动，问题是中高严重度的产品行为，或者延后处理会合并进一个新的回归。

- Example signal: "I'll worry about that later" or "we'll delete this eventually".

> - 识别信号：“这个我以后再操心”或“我们最终会删掉它”。

### Self-withdrawn or explicit false-positive rule comments｜自行撤回或明确承认误报的规则类评论

- Confidence: recurring

> - 置信度：反复出现

- Skip when: The comment body or a later Bugbot reply explicitly says the finding is withdrawn, compliant, or a false positive, and the agent can verify the relevant rule locally.

> - 可跳过的情况：评论正文或之后 Bugbot 的回复明确说这个发现已撤回、符合规范或是误报，并且 Agent 能在本地核实相关规则。

- Do not skip when: The only evidence is a human saying "false positive" on a high-risk issue without explanation.

> - 不可跳过的情况：唯一的证据是有人在一个高风险问题上不加解释地说“误报”。

- Example signal: A file-naming rule comment whose body says the file is already compliant.

> - 识别信号：一条关于文件命名规则的评论，正文里却说该文件已经符合规范。

## Ask by default｜默认询问

Do not auto-skip these categories, even if a previous PR dismissed something similar:

> 不要自动跳过这些类别，即使之前的 PR 驳回过类似的东西：

- Security, privacy, auth, billing, data retention, training-data, and permission-boundary findings.

> - 安全、隐私、认证、计费、数据保留、训练数据和权限边界方面的发现。

- High-severity findings.

> - 高严重度的发现。

- Migration, schema, idempotency, concurrency, and cross-system behavior findings.

> - 迁移、数据库结构、幂等性、并发和跨系统行为方面的发现。

- Comments where the suggested fix is small and clearly reduces risk without changing product intent.

> - 建议的修复很小、并且在不改变产品意图的前提下明显降低风险的评论。

Historical data showed humans sometimes dismiss security/data-flow comments. Treat those as owner judgment calls, not team-wide skip rules.

> 历史数据显示，人有时会驳回安全/数据流方面的评论。把那些当作负责人的个人判断，而不是全团队的跳过规则。

## Candidate learnings from recent babysits｜最近看护中得到的候选经验

Append new candidate learnings here during or after babysitting when they look team-useful but not yet mature. Prefer promoting recurring candidates into the section above once several PRs confirm the pattern.

> 在看护期间或之后，当某条经验看起来对团队有用但还不成熟时，追加到这里。等几个 PR 证实了这个模式之后，优先把反复出现的候选提升到上面那一节。

### Manual reimplementations of native browser behavior｜手工重新实现浏览器原生行为

- Confidence: candidate

> - 置信度：候选

- Skip when: Practically never. When a diff replaces native browser behavior with a manual equivalent (native sticky → JS-positioned clones, native scroll targeting → forwarded wheel/touch events, paint-order occlusion → masks/clip-path), Bugbot's logic-bug findings against that code have been consistently legitimate.

> - 可跳过的情况：几乎永远不能。当 diff 用手工的等价物替换浏览器原生行为时（原生 sticky → 用 JS 定位的克隆，原生滚动目标 → 转发的滚轮/触摸事件，绘制顺序遮挡 → 遮罩/clip-path），Bugbot 针对这类代码提出的逻辑 bug 一直都是真的。

- Do not skip when: The finding concerns event-forwarding gaps (wheel deltaMode, touch pans, scroll-chaining at edges, tap slop), mask/clip hit-testing divergence, or observer-vs-React state timing races in such code. Default to fix.

> - 不可跳过的情况：发现涉及这类代码里的事件转发缺口（滚轮 deltaMode、触摸平移、边缘处的滚动链、点击容差）、遮罩/裁剪与点击检测不一致，或者观察者回调与 React 状态之间的时序竞争。默认修复。

- Example signal: "masks do not affect hit-testing", "overlay blocks wheel scroll", "ignores deltaMode", "runs in the IntersectionObserver callback before React applies state".

> - 识别信号：“遮罩不影响点击检测”“覆盖层挡住了滚轮滚动”“忽略了 deltaMode”“在 React 应用状态之前就在 IntersectionObserver 回调里运行”。

- Source: one sticky-occlusion PR: six Bugbot passes, roughly eighteen findings, every one fixed rather than dismissed.

> - 来源：一个关于 sticky 遮挡的 PR：Bugbot 跑了六轮，大约十八条发现，每一条都被修复而不是驳回。

### Contract-test drift claims are cheaply verifiable — run the test first｜契约测试偏移的说法很容易核实，先跑测试

- Confidence: candidate

> - 置信度：候选

- Skip when: Never skip the verification itself; it costs one command. When a PR
  ships a contract test that pins protocol or documentation prose (regexes over
  a SKILL.md, snapshot of doc wording), and Bugbot claims "the test no longer
  matches the doc" (or vice versa), run that test on the PR tip before
  classifying. A red run confirms the claim empirically; a green run is a
  concrete disproof for the dismissal reply.

> - 可跳过的情况：核实这一步本身永远不要跳过；它只需要一条命令。当一个 PR 带有固定协议或文档文字的契约测试（对 SKILL.md 用正则匹配、对文档措辞做快照），而 Bugbot 声称“测试和文档对不上了”（或者反过来）时，在分类之前先在 PR 最新提交上跑这个测试。跑红了就从实证上确认了这个说法；跑绿了就是驳回回复里的具体反证。

- Do not skip when: n/a — this is a verification shortcut, not a dismissal
  pattern. Note that repeat-pass lean-dismiss heuristics would misfire here:
  prose-pinning tests drift precisely BECAUSE earlier fix rounds edit the prose.

> - 不可跳过的情况：不适用，这是一个核实的捷径，不是驳回模式。注意，“多轮之后倾向驳回”的经验法则在这里会失灵：固定文字的测试之所以会偏移，恰恰是因为之前的修复轮次改了那些文字。

- Example signal: "Contract test omits the pre-fix wait" on a PR whose earlier
  fix commits reworded the pinned passage; the test run on the tip failed on
  exactly the cited assertion.

> - 识别信号：某个 PR 之前的修复提交改写了被固定的段落，Bugbot 说“契约测试漏掉了修复前的等待”；在最新提交上跑测试，正好在被指出的那条断言上失败。

- Source: one prose-pinning PR with eight Bugbot passes; the claim was real on
  pass 7 despite every earlier pass being fixed-and-resolved.

> - 来源：一个固定文字的 PR，Bugbot 跑了八轮；尽管之前每一轮都被修复并关闭了，第 7 轮的说法仍然是真的。

### Stale security-review finding already fixed later in the same PR｜过时的安全审查发现，同一个 PR 后面已经修复了

- Confidence: candidate

> - 置信度：候选

- Skip when: An agentic security review (or similar) claims a missing authz/validation call, and the current PR tip clearly includes that exact gate (with tests), typically added in a later hardening commit after the review ran.

> - 可跳过的情况：智能安全审查（或类似工具）声称缺少某个授权/校验调用，而当前 PR 最新提交明显包含了那个关卡（并有测试），通常是审查跑完之后在一个加固提交里加上的。

- Do not skip when: The cited helper is a no-op for the principal under discussion, the check runs after the side effect it guards, or coverage for the claimed principal is missing.

> - 不可跳过的情况：被引用的辅助函数对所讨论的主体不起作用，检查在它所守护的副作用之后才运行，或者缺少对所声称主体的测试覆盖。

- Example signal: A HIGH "missing authorization check" finding while the exact guard is already called before the side effect on the tip.

> - 识别信号：一条“缺少授权检查”的高严重度发现，而最新提交上那个关卡已经在副作用之前被调用了。

- Source: one webhook-endpoint PR whose hardening commit postdated the review run.

> - 来源：一个 webhook 端点的 PR，它的加固提交晚于审查运行的时间。

### Widening a deliberately narrow error condition would mask the real error｜放宽一个刻意收窄的错误条件会掩盖真正的错误

- Confidence: candidate

> - 置信度：候选

- Skip when: The finding asks to broaden a narrow error condition (a specific
  `errno`, error code, or status class) into a catch-all, and that narrowness
  encodes a real distinction. The canonical shape is a dependency fallback
  gated on `ENOENT`: "binary is not installed" is a different situation from
  "the command ran and failed". Retrying on any non-zero exit would re-run a
  legitimate failure (not found, expired auth, network) against the fallback
  and then report the fallback's error, hiding the true one.

> - 可跳过的情况：发现要求把一个狭窄的错误条件（某个特定的 `errno`、错误码或状态类别）放宽成兜底捕获，而这种狭窄恰恰编码了一个真实的区别。典型的形式是以 `ENOENT` 为条件的依赖回退：“二进制没装”和“命令运行了但失败了”是两种不同的情况。对任何非零退出码都重试，会让一次正当的失败（找不到、认证过期、网络问题）在回退方案上再跑一遍，然后报告回退方案的错误，把真正的错误藏起来。

- Do not skip when: The narrow condition misses a case in the SAME category
  (another "binary unusable" errno such as `EACCES`, another transport-level
  failure), the unhandled path loses data or leaves partial state, or the retry
  is idempotent AND the original error is still surfaced.

> - 不可跳过的情况：狭窄的条件漏掉了同一类别里的情况（另一个表示“二进制不可用”的 errno，比如 `EACCES`，或另一种传输层失败），未处理的路径会丢失数据或留下不完整的状态，或者重试是幂等的并且原始错误仍然会被报告出来。

- Example signal: "only retries when X fails with ENOENT … never tries the
  fallback even when a working Y exists", pointing at code whose fallback
  exists for a missing dependency rather than a failed operation.

> - 识别信号：“只在 X 因 ENOENT 失败时才重试……即使存在可用的 Y 也从不尝试回退”，而所指的代码之所以有回退，是为了应对缺少依赖，而不是操作失败。

- Source: one CLI-rename PR whose fallback existed for a missing binary rather
  than a failed command.

> - 来源：一个 CLI 重命名的 PR，它的回退是为缺少二进制准备的，而不是为命令失败准备的。
