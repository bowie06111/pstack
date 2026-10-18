<!-- 中英对照译文。原文：skills/typescript-best-practices/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/typescript-best-practices/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/typescript-best-practices/SKILL.md)

```yaml
name: typescript-best-practices
description: TypeScript best practices. Use when reading or editing any .ts or .tsx file.
paths: ["**/*.ts", "**/*.tsx"]
disable-model-invocation: true
```

> **说明（description）：** TypeScript 最佳实践。在阅读或编辑任何 .ts 或 .tsx 文件时使用。

# TypeScript best practices｜TypeScript 最佳实践

Apply the **type-system-discipline** principle skill first.

> 先应用 **type-system-discipline** 原则技能。

| Rule | Summary |
|------|---------|
| Discriminated unions | Model variants with a `kind` literal discriminant so impossible states can't be represented. No optional-field bags. |
| Branded types | Brand primitives with `& { readonly __brand: "X" }` so they can't be mixed up. Validate once at the boundary. |
| Constructive modeling | Build the shape so the illegal value can't be constructed. `[T, ...T[]]` for non-empty, `[T, T][]` for even length, `start` plus `duration` for a range. Not a runtime guard, not a wish for refinement types. |
| Simplest total type | Keep `T[]` while every operation on it stays total. Strengthen to `NonEmpty<T>` only where the loose type forces `!`, a cast, or a "should never happen" throw. |
| `unknown` over `any` | External data is `unknown`. |
| Schemas before guards | Before hand-writing a property-by-property type guard, use the repository's runtime schema library and infer the type from the schema, such as `z.infer`. |
| No `as` casts | Every `as` is a runtime crash waiting. Cast only after validation. |
| Narrowing hierarchy | Discriminant switch > `in` operator > `typeof`/`instanceof` > user-defined type guard > `as`. |
| Type guards | Must verify the claim. A lying guard is worse than `as` because the bug hides behind a name that says it's safe. Name them `isX` or `hasX`. |
| Exhaustiveness | Inline `const _exhaustive: never = x;` in default arms so the compiler errors when a new variant is added. |
| `satisfies` over `as` | Validates the value without widening literal types. |
| Boundary validation | Parse where data crosses in, into a named domain type. `Record<string, unknown>` (however spelled) stops at that parse. Trust types inside. See the **boundary-discipline** principle skill. |
| Schema-derived types | Reach for `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof` before declaring a new interface. |
| Object args | Pass objects, not positional, so argument order is self-documenting. Skip on hot paths (per-frame render, tokenizers, parsers). |
| Real tests | Don't mock what you can run. Prefer the framework's real test primitives with leak/disposable checks, and verify UI in a running build. Mock only what you can't run locally. |
| Structured telemetry | Prefer structured logger diagnostics with enough context to debug from an id. No `console.log` in shipped code. |

> | 规则 | 概要 |
> |------|---------|
> | 可辨识联合 | 用 `kind` 字面量作为判别字段来建模变体，让不可能的状态无法表示。不要用一堆可选字段。 |
> | 品牌类型 | 用 `& { readonly __brand: "X" }` 给基本类型加品牌，让它们不会被混用。在边界校验一次。 |
> | 构造式建模 | 构造出让非法值无法被创建的形状。非空用 `[T, ...T[]]`，偶数长度用 `[T, T][]`，范围用 `start` 加 `duration`。不是运行时防护，也不是寄希望于细化类型。 |
> | 最简单的全类型 | 只要对它的每个操作都保持完全，就保留 `T[]`。只有在宽松类型迫使你用 `!`、强制转换或“不应该发生”的抛出时，才加强为 `NonEmpty<T>`。 |
> | 用 `unknown` 而不是 `any` | 外部数据是 `unknown`。 |
> | 先用 schema 再写防护 | 在手写逐个属性检查的类型守卫之前，先用仓库里的运行时 schema 库，从 schema 推导类型，比如 `z.infer`。 |
> | 不用 `as` 强转 | 每个 `as` 都是一个等待发生的运行时崩溃。只在校验之后才强转。 |
> | 收窄的优先级 | 判别字段 switch > `in` 运算符 > `typeof`/`instanceof` > 自定义类型守卫 > `as`。 |
> | 类型守卫 | 必须真正验证它所声称的内容。说谎的守卫比 `as` 更糟，因为 bug 藏在一个声称安全的名字后面。命名为 `isX` 或 `hasX`。 |
> | 穷尽检查 | 在 default 分支里内联 `const _exhaustive: never = x;`，这样新增变体时编译器会报错。 |
> | 用 `satisfies` 而不是 `as` | 校验值的同时不会拓宽字面量类型。 |
> | 边界校验 | 在数据进入的地方解析成一个具名的领域类型。`Record<string, unknown>`（不管怎么写）止步于那次解析。内部信任类型。见 **boundary-discipline** 原则技能。 |
> | 从 schema 推导的类型 | 在声明新接口之前，先考虑 `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof`。 |
> | 对象参数 | 传对象而不是按位置传参，这样参数顺序自带说明。热路径上跳过（每帧渲染、分词器、解析器）。 |
> | 真实的测试 | 能运行的就不要模拟。优先用框架真实的测试原语，配合泄漏/可释放检查，并在运行中的构建里验证 UI。只模拟本地跑不了的东西。 |
> | 结构化遥测 | 优先用结构化日志诊断，带足够的上下文，能从一个 id 开始调试。发布的代码里不要有 `console.log`。 |

Examples: `references/patterns.md`.

> 示例：`references/patterns.md`。
