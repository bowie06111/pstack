<!-- 中英对照译文。原文：skills/why/references/epistemics.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/why/references/epistemics.md](https://github.com/bowie06111/pstack/blob/main/skills/why/references/epistemics.md)

# Epistemics｜认识论

How to reason about confidence when evidence is historical, fragmentary, and sometimes contradictory, and how to tell the user without flattening it into false certainty. Code shows what it does, not *why it exists*. The why lives in commits, PRs, tickets, docs, and conversations, all incomplete, biased, and sometimes missing. Pretending otherwise produces confident-sounding guesses that mislead the user.

> 当证据是历史性的、零碎的、有时相互矛盾时，如何推理置信度，以及如何把它告诉用户而不把它压扁成虚假的确定性。代码展示的是它做什么，而不是*它为什么存在*。“为什么”存在于提交、PR、工单、文档和对话里，它们都不完整、有偏差，有时干脆缺失。假装不是这样，只会产出听起来很自信、却误导用户的猜测。

## Confidence Tiers｜置信度等级

Every claim in the output must sit in one tier. The tier sets its section and its phrasing.

> 输出里的每个论断都必须属于一个等级。等级决定它放在哪一节、用什么措辞。

### 1. Direct｜1. 直接

An explicit, textual citation answers the question: something an author actually *wrote* that says why, not "the code does X so the author must have wanted X." For example, a PR description "this fixes the bug where users with >1000 items couldn't paginate", a ticket "we're adding this because customer Acme requested it in their security review", a code comment "// clamp to 100 because the upstream API rejects larger values", a design doc "we chose option A over option B because we need persistence across restarts", or a chat message from the author "switching to this approach since the old one was flaky in tests".

> 一条明确的、文字形式的引用回答了这个问题：是作者真正*写下来的*、说明了原因的东西，而不是“代码做了 X，所以作者一定想要 X”。例如，一条 PR 描述“这修复了拥有超过 1000 个条目的用户无法分页的 bug”，一张工单“我们加这个是因为客户 Acme 在安全审查中提出了要求”，一条代码注释“// 限制在 100，因为上游 API 会拒绝更大的值”，一份设计文档“我们选择方案 A 而不是方案 B，因为我们需要在重启后保持数据”，或者作者的一条聊天消息“换成这种做法，因为旧的在测试里不稳定”。

Phrasing: confident, present tense, cited. "This exists because X."

> 措辞：自信、现在时、附出处。“这之所以存在，是因为 X。”

### 2. Supported｜2. 有支撑

Multiple pieces of indirect evidence converge on one conclusion. No single source states it explicitly, but the pattern across sources makes it likely. For example, the PR title says "improve performance", the ticket is labeled "perf", and the surrounding commits all touch the same hot path. Or several tests added with the change all exercise very large inputs. Or the author's other PRs from the same week all name the same incident.

> 多条间接证据汇聚到同一个结论上。没有任何一个来源明确说出来，但跨来源的模式让它很可能成立。例如，PR 标题写着“提升性能”，工单的标签是“perf”，周围的提交都碰了同一条热路径。或者随改动一起加入的几个测试都在测非常大的输入。或者作者同一周的其他 PR 都提到了同一次事故。

Phrasing: confident but clearly derived, citing each item. "The evidence points strongly to X: [the specific pieces]."

> 措辞：自信但明确是推导出来的，引用每一条证据。“证据强烈指向 X：[具体的证据]。”

### 3. Inferred｜3. 推断

A reasonable reading of the context that nothing explicitly supports. It is *your interpretation*, not a fact from the record. For example, the PR doesn't say why, but the error was happening in production (per the incident channel timing) and the fix was rushed (merged the same day), so it was likely a hotfix. Or the function name suggests retry logic, and its retry count of 3 matches the team's 3-retry convention elsewhere in the codebase.

> 对上下文的一种合理解读，但没有任何东西明确支持它。它是*你的解读*，不是记录里的事实。例如，PR 没说原因，但这个错误当时正在生产环境里发生（根据事故频道的时间），而且修复很仓促（当天就合并了），所以它很可能是一个热修复。或者函数名暗示了重试逻辑，它重试 3 次，与代码库其他地方团队“重试 3 次”的约定吻合。

Phrasing: hedged, with the chain explicit. "Given A and B, C seems likely because D."

> 措辞：带保留语气，推理链条要明确。“鉴于 A 和 B，C 看起来很可能，因为 D。”

### 4. Speculative｜4. 猜测

A plausible hypothesis on thin evidence, where other explanations fit equally well. Worth presenting, since the user may know which is right, but mark it as a guess. For example, "This might be a workaround for a browser bug that's since been fixed, but we found no contemporary evidence of that", or "It's possible this threshold was chosen to match an SLA commitment, but no SLA doc references it."

> 一个基于薄弱证据的、看似合理的假设，其他解释也同样说得通。值得提出来，因为用户可能知道哪个是对的，但要标明这是猜测。例如，“这可能是针对某个后来已修复的浏览器 bug 的变通办法，但我们没找到当时的证据”，或者“这个阈值可能是为了匹配某项 SLA 承诺而选的，但没有任何 SLA 文档提到它。”

Phrasing: "One possibility is X, but we have no direct evidence." Usually lives in Competing Hypotheses, alongside other possibilities.

> 措辞：“一种可能是 X，但我们没有直接证据。”通常放在“相互竞争的假设”里，和其他可能性放在一起。

### 5. Unknown｜5. 未知

You looked and couldn't find out. A valid, important outcome. Document it.

> 你找过了，但没查出来。这是一个有效的、重要的结果。把它记录下来。

Phrasing: "We searched X, Y, and Z and found no evidence of why." Be specific about *what* you searched. "We couldn't find out" is less useful than "We searched the ticket tracker with keywords A and B, scanned the 6 PRs that touched this file since 2023, and grep'd the repo for string literals matching the threshold. None surfaced a rationale."

> 措辞：“我们搜索了 X、Y 和 Z，没有找到任何说明原因的证据。”要具体说明你搜了*什么*。“我们没查出来”不如“我们用关键词 A 和 B 搜了工单系统，看了自 2023 年以来改过这个文件的 6 个 PR，并在仓库里 grep 了与这个阈值匹配的字符串字面量。都没有找到理由。”有用。

## Phrasing Guide｜措辞指南

**Confidence words** imply Direct or Supported evidence and need a citation immediately adjacent. Never use them for inferences: "because", "the reason is", "was designed to" (claims author intent), "fixes" / "addresses" / "solves" (claims the change achieved its goal), "the team decided" (claims a group decision happened).

> **表示确信的词**意味着直接或有支撑的证据，旁边必须紧跟出处。永远不要把它们用在推断上：“因为”“原因是”“被设计成”（断言了作者的意图）、“修复了”/“解决了”（断言了改动达到了目标）、“团队决定”（断言了发生过一次集体决定）。

**Hedges** signal interpretation, not report. Use them liberally in What We Can Reasonably Infer: "appears to", "seems to", "likely", "suggests", "is consistent with", "one reading is", "plausibly", "may have been", "the evidence points toward".

> **保留语气**表示这是解读，而不是报告。在“我们可以合理推断的”一节里尽管用：“看起来”“似乎”“很可能”“暗示”“与……一致”“一种解读是”“可能”“也许曾经”“证据指向”。

**Words to avoid:**

> **要避免的词：**

- "obviously". If it were obvious, the user wouldn't be asking

> - “显然”。如果真那么显然，用户就不会问了

- "clearly". Almost always precedes a claim that isn't clear

> - “明显地”。几乎总是出现在一个并不明显的论断前面

- "of course". Same

> - “当然”。同上

- "just" (as in "it's just X for performance"). Dismissive and usually hides uncertainty

> - “只是”（比如“这只是为了性能的 X”）。轻慢，而且通常掩盖了不确定性

- "I think" / "I believe". You're synthesizing evidence, not giving a personal opinion. Use "the evidence suggests" instead.

> - “我认为”/“我相信”。你是在综合证据，而不是发表个人意见。改用“证据表明”。

## Avoid Rationalization｜避免合理化

Code that "makes sense" today may have been written for reasons that no longer apply, or that were wrong at the time. Don't retrofit a clean rationale onto messy history. Don't assume the author did the "right" thing and work backward to justify it. Don't assume a consistent pattern was intentional when it might be copy-paste. Don't turn an absence of evidence into evidence of absence ("no one mentioned security concerns, so it must not have been a concern").

> 今天看起来“说得通”的代码，当初写它的理由可能已经不再适用，或者在当时就是错的。不要给混乱的历史硬套一个整洁的理由。不要假设作者做了“正确”的事，然后倒推来为它辩护。不要假设一个一致的模式是有意为之，它可能只是复制粘贴。不要把“没有证据”变成“证明没有”（“没人提到安全顾虑，所以那一定不是顾虑”）。

## The Sycophancy Trap｜迎合的陷阱

Users often embed a hypothesis: "Why do we do it this way, I assume it's for performance?" Don't simply confirm it. The guess is a prompt for investigation, not a conclusion to validate. Treat it as one candidate among others and check the evidence independently. If the evidence supports it, say so with citations. If not, say so, and present what the evidence *does* support.

> 用户经常在问题里夹带一个假设：“我们为什么这么做，我猜是为了性能？”不要简单地确认它。这个猜测是调查的提示，而不是要去认可的结论。把它当作若干候选之一，独立地检查证据。如果证据支持它，就附上出处说出来。如果不支持，也要说出来，并展示证据*确实*支持的是什么。

## When Evidence Contradicts｜证据相互矛盾时

Surface both sources in the output, with citations. Don't pick the one that fits a tidier narrative. For example, the ticket says "we need this for customer X's compliance requirement" and the PR says "cleaning up tech debt in this area". Both may be true (the ticket motivated the work, the PR gave the author's framing), or one may be wrong. Let the user make the call.

> 在输出里把两个来源都摆出来，附上出处。不要挑那个更符合整齐叙事的。例如，工单说“我们需要这个来满足客户 X 的合规要求”，而 PR 说“清理这一块的技术债”。两者可能都是真的（工单促成了这项工作，PR 给出的是作者的说法），也可能有一个是错的。让用户来判断。

## When Evidence Is Missing｜证据缺失时

An honest "we don't know" is one of the most valuable outputs. The user learns the answer isn't in the obvious places, that they'll need to ask a human (the original author, the product owner, the team lead), or that they can decide the question isn't worth pursuing further. Failing to mark a gap, and instead filling it with a confident guess, actively harms the user, because they'll act on the guess. Name each gap concretely: the question, the sources searched, what you searched for in each, and what you found (nothing, or only tangential material).

> 一句诚实的“我们不知道”是最有价值的输出之一。用户会知道答案不在那些显而易见的地方，知道他们需要去问人（原作者、产品负责人、团队负责人），或者可以决定这个问题不值得再追下去。不标出缺口、反而用一个自信的猜测去填补它，会实实在在地伤害用户，因为他们会依据这个猜测行动。具体说出每一个缺口：问题是什么、搜了哪些来源、在每个来源里搜了什么，以及找到了什么（什么都没有，或者只有不相干的材料）。

## Before Finalizing｜定稿之前

Before the synthesizer delivers the output, it checks every claim in "What We Found" and "What We Can Reasonably Infer". Each claim has a citation or moves to Inferred or Hypotheses. Its phrasing matches its tier. It doesn't treat code as evidence for its own intent (if it does, remove or reclassify it). The output has a "What We Don't Know" section. If no gaps are mentioned, that's suspicious. Either the evidence was unusually complete or something is being swept under the rug.

> 在交付输出之前，综合者要检查“我们发现了什么”和“我们可以合理推断的”里的每一个论断。每个论断都有出处，否则就移到“推断”或“假设”里。它的措辞与它的等级相符。它没有把代码当作其自身意图的证据（如果有，就删掉或重新归类）。输出里有“我们不知道什么”一节。如果一个缺口都没提到，那就很可疑。要么证据异常完整，要么有什么东西被藏起来了。
