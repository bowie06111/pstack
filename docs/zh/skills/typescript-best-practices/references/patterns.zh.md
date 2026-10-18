<!-- 中英对照译文。原文：skills/typescript-best-practices/references/patterns.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/typescript-best-practices/references/patterns.md](https://github.com/bowie06111/pstack/blob/main/skills/typescript-best-practices/references/patterns.md)

# TypeScript patterns｜TypeScript 写法示例

Code examples for each rule in `SKILL.md`. The underlying principles are language-agnostic. See the **type-system-discipline** and **boundary-discipline** principle skills.

> `SKILL.md` 里每条规则的代码示例。背后的原则与语言无关。见 **type-system-discipline** 和 **boundary-discipline** 原则技能。

## Branded types｜品牌类型

Brand primitives so they can't be mixed up. Validate once at the boundary. Downstream code trusts the type.

> 给基本类型加品牌，让它们不会被混用。在边界校验一次。下游代码信任这个类型。

```ts
type AgentId = string & { readonly __brand: "AgentId" };

function parseAgentId(input: string): AgentId {
  if (!isUUID(input)) throw new Error(`Invalid agent id: ${input}`);
  return input as AgentId;
}

function focusAgent(id: AgentId): void {
  /* input is trusted */
}
```

> 上面代码块的中文版：

```ts
type AgentId = string & { readonly __brand: "AgentId" };

function parseAgentId(input: string): AgentId {
  if (!isUUID(input)) throw new Error(`Invalid agent id: ${input}`);
  return input as AgentId;
}

function focusAgent(id: AgentId): void {
  /* 输入是可信的 */
}
```

Match the `readonly __brand: 'X'` shape. Don't invent a new convention.

> 照着 `readonly __brand: 'X'` 这个形状写。不要发明新的约定。

## Discriminated unions｜可辨识联合

Model variants with a literal discriminant. Every variant shares the field name and each variant's value is unique, so impossible combos can't be represented.

> 用字面量判别字段来建模变体。每个变体共享同一个字段名，而每个变体的值都是唯一的，这样不可能的组合就无法表示。

```ts
// Don't. Boolean + optionals lets contradictory states exist.
type DiffState = { loading: boolean; diff?: GitDiff; error?: string };

// Do. Only valid states exist.
type DiffState =
  | { kind: "loading" }
  | { kind: "ready"; diff: GitDiff }
  | { kind: "error"; error: string };
```

> 上面代码块的中文版：

```ts
// 不要这样。布尔值 + 可选字段会让矛盾的状态存在。
type DiffState = { loading: boolean; diff?: GitDiff; error?: string };

// 要这样。只有合法的状态存在。
type DiffState =
  | { kind: "loading" }
  | { kind: "ready"; diff: GitDiff }
  | { kind: "error"; error: string };
```

Pick one discriminant name (`kind`, `type`, `tag`) and stick to it.

> 选一个判别字段名（`kind`、`type`、`tag`）并坚持用它。

## Constructive modeling｜构造式建模

Build the type from parts that are all legal instead of restricting a loose type with runtime checks.

> 用全部合法的部件来构造类型，而不是用运行时检查去限制一个宽松的类型。

Non-empty, via a variadic tuple:

> 非空，用可变参数元组：

```ts
type NonEmpty<T> = [T, ...T[]];

// Don't: T[] plus a length check every caller must repeat
function pickWinner(entries: string[]): string {
  if (entries.length === 0) throw new Error("no entries");
  return entries[Math.floor(Math.random() * entries.length)];
}

// Do: an empty value of the type can't exist
function pickWinner(entries: NonEmpty<string>): string {
  return entries[Math.floor(Math.random() * entries.length)];
}
```

> 上面代码块的中文版：

```ts
type NonEmpty<T> = [T, ...T[]];

// 不要这样：T[] 加上一个每个调用方都得重复的长度检查
function pickWinner(entries: string[]): string {
  if (entries.length === 0) throw new Error("no entries");
  return entries[Math.floor(Math.random() * entries.length)];
}

// 要这样：这个类型的空值根本不可能存在
function pickWinner(entries: NonEmpty<string>): string {
  return entries[Math.floor(Math.random() * entries.length)];
}
```

Where a plain `T[]` arrives, narrow once with a guard. The fact then travels in the type:

> 当传进来的是普通的 `T[]` 时，用一个守卫收窄一次。之后这个事实就随着类型一起传递：

```ts
const isNonEmpty = <T>(arr: T[]): arr is NonEmpty<T> => arr.length > 0;
```

Even length, as pairs:

> 偶数长度，用成对的形式：

```ts
type Pairs<T> = [T, T][];
```

A time range, as start plus duration:

> 时间范围，用起点加时长：

```ts
// Don't: a comment holds the invariant
type TimeRange = { start: Date; end: Date }; // start <= end

// Do: a negative range can't be written; derive end when needed
type TimeRange = { start: Date; durationMs: number };
```

> 上面代码块的中文版：

```ts
// 不要这样：不变量靠一条注释维持
type TimeRange = { start: Date; end: Date }; // start <= end

// 要这样：负的范围根本写不出来；需要时再推导出 end
type TimeRange = { start: Date; durationMs: number };
```

Keep `durationMs` a plain number. Brand it (per Branded types) only if a raw number could be passed where a duration is expected, not by reflex. Pick the representation that makes the bad state unconstructable, then expose the reading you need on top (`pairs.flat()`, a `rangeEnd()` helper).

> `durationMs` 保持为普通数字。只有当一个原始数字可能被传到本该是时长的地方时，才（按“品牌类型”）给它加品牌，而不是出于条件反射。选择让坏状态无法被构造的表示，然后在上面暴露你需要的读取方式（`pairs.flat()`、一个 `rangeEnd()` 辅助函数）。

## Simplest total type｜最简单的全类型

Don't strengthen everything. Keep `T[]` when every operation on it is total:

> 不要什么都加强。当对 `T[]` 的每个操作都是完全的时，就保留它：

```ts
const sum = (xs: number[]) => xs.reduce((a, b) => a + b, 0); // [] is 0, fine
```

> 上面代码块的中文版：

```ts
const sum = (xs: number[]) => xs.reduce((a, b) => a + b, 0); // [] 的结果是 0，没问题
```

Strengthen when the loose type forces a lie at a use site. The tells are `!`, `arr[0] as T`, and a "should never happen" throw:

> 当宽松的类型迫使你在某个使用点撒谎时，才加强它。迹象是 `!`、`arr[0] as T`，以及“不应该发生”的抛出：

```ts
// Don't: partiality smuggled past the compiler
function newestSession(sessions: Session[]): Session {
  return sessions.at(0)!;
}

// Do: strengthen the input; the assertion disappears
function newestSession(sessions: NonEmpty<Session>): Session {
  return sessions[0];
}
```

> 上面代码块的中文版：

```ts
// 不要这样：不完全性被偷偷带过了编译器
function newestSession(sessions: Session[]): Session {
  return sessions.at(0)!;
}

// 要这样：加强输入；断言随之消失
function newestSession(sessions: NonEmpty<Session>): Session {
  return sessions[0];
}
```

Weakening the result to `Session | undefined` is the other total signature.

> 把结果弱化为 `Session | undefined`，是另一种完全的签名。

## `unknown` over `any`｜用 `unknown` 而不是 `any`

External data is always `unknown`. Narrow before use.

> 外部数据永远是 `unknown`。使用之前先收窄。

```ts
// Don't
function handle(input: any) {
  return input.foo.bar;
}

// Do
function handle(input: unknown) {
  if (typeof input === "object" && input !== null && "foo" in input) {
    // narrowed; compiler verifies access
  }
}
```

> 上面代码块的中文版：

```ts
// 不要这样
function handle(input: any) {
  return input.foo.bar;
}

// 要这样
function handle(input: unknown) {
  if (typeof input === "object" && input !== null && "foo" in input) {
    // 已收窄；编译器会验证访问
  }
}
```

External sources include RPC payloads, `JSON.parse`, `postMessage`, IPC, file contents, environment variables, database results.

> 外部来源包括 RPC 载荷、`JSON.parse`、`postMessage`、IPC、文件内容、环境变量、数据库查询结果。

## Schemas before hand-rolled guards｜先用 schema，再考虑手写守卫

Before writing a property-by-property type guard for external data, look for the repository's runtime schema library and existing schemas. Let one schema own validation and derive the TypeScript type from it. Do not maintain a schema, a duplicate interface, and a guard that can drift apart.

> 在为外部数据写一个逐个属性检查的类型守卫之前，先找仓库里的运行时 schema 库和已有的 schema。让一个 schema 负责校验，并从它推导出 TypeScript 类型。不要同时维护一个 schema、一个重复的接口和一个守卫，它们会逐渐对不上。

```ts
import { z } from "zod";

const UserSchema = z.object({
  id: z.string().uuid(),
  role: z.enum(["admin", "member"]),
});

type User = z.infer<typeof UserSchema>;

function parseUser(input: unknown): User {
  return UserSchema.parse(input);
}
```

Use `safeParse` when failure is an expected branch. Use the equivalent inference helper when the repository uses another schema library. Do not add a new schema dependency for one guard. This rule prefers the schema system the codebase already trusts.

> 当失败是预期中的一个分支时，用 `safeParse`。当仓库用的是别的 schema 库时，用它对应的推导辅助函数。不要为了一个守卫引入新的 schema 依赖。这条规则偏向代码库已经信任的那套 schema 系统。

## No `as` casts｜不用 `as` 强转

Every `as` is a potential runtime crash. Cast only after the type system has verified the claim.

> 每个 `as` 都是一个潜在的运行时崩溃。只有在类型系统已经验证了这个说法之后才强转。

```ts
import { z } from "zod";

// Don't
const user = data as User;

// Don't
function isUser(data: unknown): data is User {
  return typeof data === "object" && data !== null && "id" in data;
}

// Do
const userSchema = z.object({ id: z.string(), name: z.string() });
type User = z.infer<typeof userSchema>;

function parseUser(data: unknown): User {
  return userSchema.parse(data);
}
```

> 上面代码块的中文版：

```ts
import { z } from "zod";

// 不要这样
const user = data as User;

// 不要这样
function isUser(data: unknown): data is User {
  return typeof data === "object" && data !== null && "id" in data;
}

// 要这样
const userSchema = z.object({ id: z.string(), name: z.string() });
type User = z.infer<typeof userSchema>;

function parseUser(data: unknown): User {
  return userSchema.parse(data);
}
```

When the type comes first, annotate the validator with the type it proves. The compiler then rejects a validator that proves less than the type. Remove `name` from the object below and the assignment fails to compile.

> 当类型先存在时，用校验器所证明的类型来标注它。这样一来，如果校验器证明的比类型少，编译器就会拒绝它。把下面对象里的 `name` 去掉，这个赋值就会编译失败。

```ts
type User = { id: string; name: string };

const userSchema: z.ZodType<User> = z.object({ id: z.string(), name: z.string() });
```

When refactoring an `as` out of existing code, identify why TypeScript can't infer:

> 从现有代码里重构掉一个 `as` 时，先找出 TypeScript 为什么推断不出来：

- Missing discriminant: add one, switch to a discriminated union.

> - 缺少判别字段：加一个，改成可辨识联合。

- Overly wide source type (e.g. `Record<string, unknown>`): narrow it.

> - 源类型太宽（例如 `Record<string, unknown>`）：收窄它。

- Untyped boundary: parse with the schema that owns the shape. Add a schema only where none exists.

> - 没有类型的边界：用拥有这个形状的 schema 去解析。只在没有 schema 的地方才新加一个。

- Genuinely inexpressible: use a branded type or `satisfies`.

> - 确实无法表达：用品牌类型或 `satisfies`。

## Narrowing hierarchy｜收窄的优先级

From best to last-resort:

> 从最好到最后的手段：

1. **Discriminated union switch / if.** Compiler narrows automatically.

> 1. **可辨识联合的 switch / if。** 编译器会自动收窄。

2. **`in` operator.** `"key" in obj` narrows to variants containing that key.

> 2. **`in` 运算符。** `"key" in obj` 会收窄到包含那个键的变体。

3. **`typeof` / `instanceof`.** For primitives and class instances.

> 3. **`typeof` / `instanceof`。** 用于基本类型和类的实例。

4. **User-defined type guard.** When the above aren't enough.

> 4. **自定义类型守卫。** 当上面这些都不够时。

5. **`as` cast.** Only after validation.

> 5. **`as` 强转。** 只在校验之后。

```ts
function area(s: Shape): number {
  if ("radius" in s) return Math.PI * s.radius ** 2; // narrowed to circle
  return s.width * s.height; // narrowed to rect
}
```

> 上面代码块的中文版：

```ts
function area(s: Shape): number {
  if ("radius" in s) return Math.PI * s.radius ** 2; // 收窄为圆形
  return s.width * s.height; // 收窄为矩形
}
```

## Type guards｜类型守卫

A guard must actually verify the claim. A lying guard is worse than `as`.

> 守卫必须真正验证它所声称的内容。说谎的守卫比 `as` 更糟。

```ts
function isCircle(s: Shape): s is Shape & { kind: "circle" } {
  return s.kind === "circle";
}
```

Prefer discriminant narrowing when possible.

> 能用判别字段收窄时，优先用它。

## Exhaustiveness｜穷尽检查

In default arms, assign the discriminant to a `never`-typed local.

> 在 default 分支里，把判别对象赋给一个 `never` 类型的局部变量。

```ts
// Value-returning switch
function area(s: Shape): number {
  switch (s.kind) {
    case "circle":
      return Math.PI * s.radius ** 2;
    case "rect":
      return s.width * s.height;
    default: {
      const _exhaustive: never = s;
      return _exhaustive;
    }
  }
}

// Void switch
function handle(s: Shape): void {
  switch (s.kind) {
    case "circle":
      drawCircle(s);
      break;
    case "rect":
      drawRect(s);
      break;
    default: {
      const _exhaustive: never = s;
      void _exhaustive;
    }
  }
}
```

> 上面代码块的中文版：

```ts
// 有返回值的 switch
function area(s: Shape): number {
  switch (s.kind) {
    case "circle":
      return Math.PI * s.radius ** 2;
    case "rect":
      return s.width * s.height;
    default: {
      const _exhaustive: never = s;
      return _exhaustive;
    }
  }
}

// 没有返回值的 switch
function handle(s: Shape): void {
  switch (s.kind) {
    case "circle":
      drawCircle(s);
      break;
    case "rect":
      drawRect(s);
      break;
    default: {
      const _exhaustive: never = s;
      void _exhaustive;
    }
  }
}
```

Return-style in value-returning switches, void-style in statement switches.

> 有返回值的 switch 用 return 的写法，语句式的 switch 用 void 的写法。

## `satisfies` over `as`｜用 `satisfies` 而不是 `as`

`satisfies` validates without widening literal types.

> `satisfies` 能校验值，而不会拓宽字面量类型。

```ts
// Don't. Widens, loses literal types.
const config = { theme: "dark", cols: 3 } as Config;

// Do. Validates AND preserves literal types.
const config = { theme: "dark", cols: 3 } satisfies Config;
// config.theme is "dark" (literal), not string
```

> 上面代码块的中文版：

```ts
// 不要这样。会拓宽类型，丢掉字面量类型。
const config = { theme: "dark", cols: 3 } as Config;

// 要这样。既校验，又保留字面量类型。
const config = { theme: "dark", cols: 3 } satisfies Config;
// config.theme 的类型是 "dark"（字面量），而不是 string
```

## Boundary validation｜边界校验

Validate once where data crosses in. Trust types inside. See the **boundary-discipline** principle skill.

> 在数据进入的地方校验一次。内部信任类型。见 **boundary-discipline** 原则技能。

- **Wire formats** (proto, JSON-RPC): parse with `ignoreUnknownFields` so forward-compatible changes don't break old clients.

> - **线路格式**（proto、JSON-RPC）：用 `ignoreUnknownFields` 解析，这样向前兼容的改动不会弄坏旧客户端。

- **Persisted JSON:** versioned blob with a try/catch around the parse.

> - **持久化的 JSON：** 带版本的数据块，解析时包一层 try/catch。

- **Don't re-validate** deep in call chains.

> - **不要在调用链深处重复校验。**

## Schema-derived types｜从 schema 推导的类型

When a `.proto`, OpenAPI spec, GraphQL schema, or database migration already defines a shape, derive from the generated types instead of duplicating them.

> 当 `.proto`、OpenAPI 规范、GraphQL schema 或数据库迁移已经定义了某个形状时，从生成的类型推导，而不是复制一份。

```ts
// Don't. Duplicate shape, drifts when the schema changes.
type CheckSummary = {
  totalCount: number;
  checks: { name: string; status: string }[];
};
function renderChecks(s: CheckSummary) {
  /* ... */
}

// Do. Derive from the generated schema type.
import type { ChecksMessage } from "<generated module>";
function renderChecks(s: Pick<ChecksMessage, "totalCount" | "checks">) {
  /* ... */
}
```

> 上面代码块的中文版：

```ts
// 不要这样。重复的形状，schema 一变就会对不上。
type CheckSummary = {
  totalCount: number;
  checks: { name: string; status: string }[];
};
function renderChecks(s: CheckSummary) {
  /* ... */
}

// 要这样。从生成的 schema 类型推导。
import type { ChecksMessage } from "<generated module>";
function renderChecks(s: Pick<ChecksMessage, "totalCount" | "checks">) {
  /* ... */
}
```

Reach for `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited`, `typeof` before writing a new interface.

> 在写新接口之前，先考虑 `Pick`、`Omit`、`Parameters`、`ReturnType`、`Awaited`、`typeof`。

## Object args｜对象参数

```ts
// Don't. Swap two args, still compiles.
openFile(uri, {
  startLineNumber: 10,
  startColumn: 1,
  endLineNumber: 10,
  endColumn: 1,
});

// Do. Order-independent, self-documenting.
openFile({
  uri,
  selection: {
    startLineNumber: 10,
    startColumn: 1,
    endLineNumber: 10,
    endColumn: 1,
  },
});
```

> 上面代码块的中文版：

```ts
// 不要这样。交换两个参数，照样能编译。
openFile(uri, {
  startLineNumber: 10,
  startColumn: 1,
  endLineNumber: 10,
  endColumn: 1,
});

// 要这样。与顺序无关，自带说明。
openFile({
  uri,
  selection: {
    startLineNumber: 10,
    startColumn: 1,
    endLineNumber: 10,
    endColumn: 1,
  },
});
```

Skip on hot paths: per-frame render, tokenizers, parsers, anything in a tight loop where the allocation cost matters.

> 在热路径上跳过：每帧渲染、分词器、解析器，以及任何分配开销很重要的紧密循环。
