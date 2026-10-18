<!-- 中英对照译文。原文：skills/interrogate/references/code-quality-review.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/interrogate/references/code-quality-review.md](https://github.com/bowie06111/pstack/blob/main/skills/interrogate/references/code-quality-review.md)

# Code Quality Review｜代码质量审查

Each reviewer applies this code-quality lens in addition to the rubric. It is a strict standard focused on implementation quality, maintainability, abstraction quality, and codebase health.

> 每个审查者在评分标准之外，还要应用这个代码质量视角。这是一个严格的标准，聚焦于实现质量、可维护性、抽象质量和代码库健康。

Above all, be ambitious about code structure. Do not merely identify local cleanup. Actively search for "code judo" moves, restructurings that preserve behavior while making the implementation dramatically simpler, smaller, more direct, and more elegant.

> 最重要的是，对代码结构要有野心。不要只找局部的清理点。主动去寻找“代码柔道”式的招数，也就是在保持行为不变的前提下，让实现大幅变得更简单、更小、更直接、更优雅的重构。

## Core Prompt｜核心提示词

Start from this baseline:

> 从这个基线出发：

> Perform a deep code quality audit of the current branch's changes.
> Rethink how to structure / implement the changes to meaningfully improve code quality without impacting behavior.
> Work to improve abstractions, modularity, reduce Spaghetti code, improve succinctness and legibility.
> Be ambitious, if there is a clear path to improving the implementation that involves restructuring some of the codebase, go for it.
> Be extremely thorough and rigorous. Measure twice, cut once.

> > 对当前分支的改动做一次深入的代码质量审计。
> > 重新思考如何组织/实现这些改动，在不影响行为的前提下实质性地提升代码质量。
> > 努力改进抽象和模块化，减少面条代码，提高简洁性和可读性。
> > 要有野心，如果有一条明确的改进实现的路径需要重构部分代码库，就去做。
> > 要极其彻底和严谨。量两次，切一次。

## Dimensions｜维度

Each dimension is stated once. Apply the ones that are relevant.

> 每个维度只说一次。应用相关的那些。

0. **Be ambitious about structural simplification.** Do not stop at "this could be a bit cleaner." Look for reframings that make whole branches, helpers, modes, conditionals, or layers disappear. Assume a "code judo" move is often available. It uses the existing architecture more effectively and makes the change dramatically simpler. If you can delete complexity rather than rearrange it, push hard for that.

> 0. **对结构简化要有野心。** 不要停留在“这里可以稍微干净一点”。去找能让整个分支、辅助函数、模式、条件判断或层消失的重新框定。假设“代码柔道”式的招数经常是存在的。它能更有效地利用现有架构，让改动大幅变简单。如果你能删掉复杂度而不是挪动它，就大力推动这一点。

1. **Do not let a PR push a file from under 1k lines to over 1k lines without a very strong reason.** Treat this as a strong smell. Prefer extracting helpers, subcomponents, or modules. If the diff crosses that threshold, ask whether the code should be decomposed first. Waive only for a compelling structural reason where the resulting file stays clearly organized.

> 1. **不要让一个 PR 在没有非常充分理由的情况下，把一个文件从 1000 行以下推到 1000 行以上。** 把这当作一个强烈的异味。优先提取辅助函数、子组件或模块。如果 diff 越过了这个门槛，就问一问代码是否应该先被拆分。只有在有令人信服的结构性理由、并且结果文件仍然组织清楚时才豁免。

2. **Do not allow spaghetti growth in existing code.** Be suspicious of new ad-hoc conditionals, scattered special cases, or one-off branches inserted into unrelated flows. Treat "weird if statements in random places" as a design problem, not a style nit. Prefer pushing the logic into a dedicated helper, state machine, or module instead of tangling an existing path.

> 2. **不要允许现有代码长成面条。** 对新加入的临时条件判断、散落的特例，或插进无关流程里的一次性分支保持怀疑。把“到处冒出来的奇怪 if 语句”当作设计问题，而不是风格上的小毛病。优先把逻辑推到一个专门的辅助函数、状态机或模块里，而不是缠绕一条现有的路径。

3. **Bias toward cleaning the design, not just accepting working code.** If behavior can stay the same while the structure becomes meaningfully cleaner, push for the cleaner version. Prefer simplifications that remove moving pieces over refactors that spread the same complexity around.

> 3. **倾向于清理设计，而不只是接受能跑的代码。** 如果行为可以保持不变、而结构可以变得实质性地更干净，就推动更干净的版本。优先选择去掉活动部件的简化，而不是把同样的复杂度挪来挪去的重构。

4. **Prefer direct, boring, maintainable code over hacky or magical code.** Treat brittle, ad-hoc, or "magic" behavior as a problem. Be skeptical of generic mechanisms that hide simple data-shape assumptions. Flag thin abstractions, identity wrappers, or pass-through helpers that add indirection without buying clarity.

> 4. **优先选择直接、平淡、可维护的代码，而不是取巧或魔法般的代码。** 把脆弱的、临时拼凑的或“魔法”般的行为当作问题。对隐藏了简单数据形状假设的通用机制保持怀疑。标出只增加间接层、却没换来清晰度的薄抽象、恒等包装或透传辅助函数。

5. **Push on type and boundary cleanliness when it affects maintainability.** Question unnecessary optionality, `unknown`, `any`, or cast-heavy code when a clearer type boundary could exist. Prefer explicit typed models over loosely-shaped ad-hoc objects. If a branch leans on a silent fallback to paper over an unclear invariant, ask whether the boundary should be made explicit.

> 5. **当类型和边界的整洁影响可维护性时，要推动它。** 当本可以有更清晰的类型边界时，质疑不必要的可选性、`unknown`、`any` 或大量强转的代码。优先用明确的类型化模型，而不是形状松散的临时对象。如果某个分支靠一个悄无声息的回退来掩盖一个不清楚的不变量，就问一问边界是否应该被明确化。

6. **Keep logic in the canonical layer and reuse existing helpers.** Call out feature logic leaking into shared paths or implementation details leaking through APIs. Prefer existing canonical utilities over bespoke one-offs. Push code toward the right package, service, or module instead of normalizing drift.

> 6. **把逻辑放在规范的层次里，并复用已有的辅助函数。** 指出功能逻辑泄漏到共享路径里，或实现细节通过 API 泄漏出去。优先用已有的规范工具，而不是定制的一次性代码。把代码推向正确的包、服务或模块，而不是让偏移变成常态。

7. **Treat unnecessary sequential orchestration and non-atomic updates as design smells when the cleaner structure is obvious.** If independent work is serialized for no reason, ask whether it should run in parallel. If related updates can leave state half-applied, push for a more atomic structure. Do not over-index on micro-optimizations, but do flag avoidable orchestration complexity that makes the code more brittle.

> 7. **当更干净的结构显而易见时，把不必要的串行编排和非原子的更新当作设计异味。** 如果独立的工作毫无理由地被串行化，就问一问它是否应该并行运行。如果相关的更新可能让状态处于应用了一半的情况，就推动更原子化的结构。不要过分纠结微优化，但要标出那些让代码更脆弱的、可以避免的编排复杂度。

## Output Expectations｜输出要求

Prioritize structural code-quality regressions and missed simplifications first, then spaghetti and branching complexity, then boundary, type, and file-size concerns, then smaller modularity and legibility issues.

> 先关注结构性的代码质量退化和错过的简化，然后是面条代码和分支复杂度，然后是边界、类型和文件大小的问题，最后是较小的模块化和可读性问题。

## Approval Bar｜批准门槛

Do not approve merely because behavior seems correct. Treat these as presumptive blockers unless the author can justify them: the PR keeps a lot of incidental complexity when a code-judo move would delete it. Pushes a file from below 1000 lines to above 1000 lines. Adds ad-hoc branching that tangles an existing flow. Scatters feature checks across shared code. Adds an unnecessary abstraction, wrapper, or cast-heavy contract, or duplicates an existing helper or puts logic in the wrong layer when there is a clear canonical home. If those conditions are not met, leave explicit, actionable feedback and push for a cleaner decomposition.

> 不要仅仅因为行为看起来正确就批准。以下情况默认视为阻塞项，除非作者能给出理由：PR 保留了大量本可以用一招代码柔道删掉的附带复杂度；把一个文件从 1000 行以下推到 1000 行以上；加入了缠绕现有流程的临时分支；把功能检查散落到共享代码里；加入了不必要的抽象、包装或大量强转的约定，或者在有明确的规范归宿时重复了已有的辅助函数、或把逻辑放错了层。如果不满足这些条件，就留下明确、可执行的反馈，并推动更干净的拆分。

## Review Tone｜审查语气

Be direct, serious, and demanding about quality. Do not be rude, but do not soften major maintainability issues into mild suggestions. If the code is making the codebase messier, say so. If the implementation missed an obvious dramatic simplification, say that too. Do not be satisfied with "maybe rename this" when the real issue is structural.

> 对质量要直接、认真、严格。不要粗鲁，但也不要把重大的可维护性问题弱化成温和的建议。如果代码在让代码库变得更乱，就直说。如果实现错过了一个明显的大幅简化，也要直说。当真正的问题是结构性的时，不要满足于“也许改个名”。
