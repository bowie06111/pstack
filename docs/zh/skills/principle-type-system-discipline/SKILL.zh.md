<!-- 中英对照译文。原文：skills/principle-type-system-discipline/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-type-system-discipline/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-type-system-discipline/SKILL.md)

```yaml
name: principle-type-system-discipline
description: "Apply when designing types, reviewing a function signature, or writing code in any statically-typed language. Make illegal states unrepresentable, brand semantic primitives, parse external data at boundaries, refuse to lie to the compiler, exhaust variants, derive from authoritative schemas."
disable-model-invocation: true
```

> **说明（description）：** 在设计类型、审查函数签名，或用任何静态类型语言写代码时使用。让非法状态无法表示，给有语义的基本类型加品牌，在边界解析外部数据，拒绝对编译器撒谎，穷尽所有变体，从权威的 schema 推导类型。

# Type System Discipline｜类型系统纪律

Make the compiler reject impossible states, mismatched primitives, and unhandled variants. Prefer defining errors and special cases out of existence over proliferating handlers. For TypeScript syntax, see the `typescript-best-practices` skill.

> 让编译器拒绝不可能的状态、不匹配的基本类型和未处理的变体。宁可从定义上消除错误和特例，也不要让处理程序越来越多。TypeScript 的写法见 `typescript-best-practices` 技能。

- **Make illegal states unrepresentable.** Model variants as sum types (discriminated unions, enums with payloads, sealed classes), not as a bag of optional fields where contradictory combinations compile. `{ completed: boolean; completedAt?: Date }` admits `completed: true` with no date. Derive the boolean from `completedAt`, or model `{ kind: 'open' } | { kind: 'done'; at: Date }`. If you can write a comment explaining when a combination of fields is valid, split it into a sum type.

> - **让非法状态无法表示。** 用和类型（可辨识联合、带数据的枚举、密封类）来建模变体，而不是用一堆可选字段，让矛盾的组合也能编译通过。`{ completed: boolean; completedAt?: Date }` 允许出现 `completed: true` 却没有日期。要么从 `completedAt` 推导出这个布尔值，要么建模成 `{ kind: 'open' } | { kind: 'done'; at: Date }`。如果你能写一段注释解释字段的哪些组合有效，就把它拆成和类型。

- **Types are constructions, not restrictions.** Build the type up from the values you want instead of carving them out of a looser type with checks. A non-empty list is a head plus a rest. A valid time range is a start plus a duration. Choose the shape that cannot build the illegal value and expose the interface callers need on top.

> - **类型是构造，而不是限制。** 从你想要的值出发构建类型，而不是从一个更宽松的类型里用检查把它们挖出来。非空列表就是一个头加上其余部分。有效的时间范围就是一个起点加上一段时长。选择构造不出非法值的形状，再在上面暴露调用方需要的接口。

- **Brand semantic primitives.** `UserId` and `OrderId` are strings underneath but should not be interchangeable. Give each its own type, such as a branded intersection in TypeScript or a newtype elsewhere. Validate once at creation, trust the type downstream. If two arguments share a primitive type but mean different things, brand them.

> - **给有语义的基本类型加品牌。** `UserId` 和 `OrderId` 底层都是字符串，但不应该能互换。给每个一个自己的类型，比如 TypeScript 里的品牌交叉类型，或其他语言里的 newtype。在创建时校验一次，下游信任这个类型。如果两个参数共用一个基本类型但含义不同，就给它们加品牌。

- **External data is untyped until parsed.** RPC payloads, JSON, IPC messages, CLI args, config files, environment variables, database rows. Have a parse function at every boundary that turns unstructured input into the typed model. The **boundary-discipline** principle skill says where to put validation.

> - **外部数据在解析之前是没有类型的。** RPC 载荷、JSON、IPC 消息、命令行参数、配置文件、环境变量、数据库行。在每个边界都有一个解析函数，把无结构的输入变成有类型的模型。**boundary-discipline** 原则技能说明了校验放在哪里。

- **Don't lie to the type system.** Casts, unsafe coercions, and assertion functions that bypass the compiler are latent runtime crashes. If the compiler can't prove a fact, prove it (validate, narrow, refine the model) or accept that the cast is a hazard. Trace each `any`, `as`, or `assertNotNull` to the boundary and validate there.

> - **不要对类型系统撒谎。** 绕过编译器的强制转换、不安全的类型转换和断言函数，都是潜在的运行时崩溃。如果编译器证明不了某个事实，就去证明它（校验、收窄、细化模型），或者承认这个转换是个隐患。把每个 `any`、`as` 或 `assertNotNull` 追溯到边界，在那里校验。

- **Exhaustive matching is the compiler's job.** A match on a sum type must fail compilation when a new variant is added without handling, so the compiler tells the next agent where to add a case. Use your language's idiom: a `never`-typed binding in TypeScript, an unannotated `match` in Rust, `-Wincomplete-patterns` in Haskell, sealed-class match exhaustiveness in Kotlin.

> - **穷尽匹配是编译器的工作。** 当新增一个变体而没有处理时，对和类型的匹配必须编译失败，这样编译器会告诉下一个代理在哪里加分支。用你所用语言的惯用法：TypeScript 里的 `never` 类型绑定、Rust 里不加注解的 `match`、Haskell 的 `-Wincomplete-patterns`、Kotlin 密封类的穷尽匹配。

- **Derive types from authoritative schemas.** When a protocol buffer, OpenAPI spec, GraphQL schema, database migration, design-token file, or another file owns a shape, derive from it instead of hand-rolling a parallel type. See the **encode-lessons-in-structure** principle skill.

> - **从权威的 schema 推导类型。** 当 protocol buffer、OpenAPI 规范、GraphQL schema、数据库迁移、设计令牌文件或其他文件拥有某个形状时，从它推导，而不是手写一个平行的类型。见 **encode-lessons-in-structure** 原则技能。

- **Strengthen a type only where partiality appears.** A runtime assertion, null check, or "this should never happen" throw marks the place a type is too weak. Push that check up into the type. Then stop. The type system's job is to track the cases each use site must handle, not to describe the data as precisely as possible. Prefer total functions. `sum` of an empty list is 0, so it takes the plain list. `head` of an empty list has no answer, so it demands the non-empty one. If nothing would otherwise panic, keep the plain type.

> - **只在出现不完全性的地方加强类型。** 运行时断言、空值检查或“这不应该发生”的抛出，标出了类型太弱的地方。把那个检查推到类型里去。然后就停下。类型系统的工作是跟踪每个使用点必须处理的情况，而不是尽可能精确地描述数据。优先用全函数。空列表的 `sum` 是 0，所以它接收普通列表。空列表的 `head` 没有答案，所以它要求非空列表。如果没有什么会因此崩溃，就保留普通类型。
