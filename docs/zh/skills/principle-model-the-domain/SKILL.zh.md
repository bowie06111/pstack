<!-- 中英对照译文。原文：skills/principle-model-the-domain/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-model-the-domain/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-model-the-domain/SKILL.md)

```yaml
name: principle-model-the-domain
description: "Apply when writing stateful logic, or when code branches a lot or repeats a shape assumption across files."
disable-model-invocation: true
```

> **说明（description）：** 在写有状态的逻辑，或代码分支很多、在多个文件里重复同一形状假设时使用。

# Model the Domain｜建模领域

Encode the real domain in a data structure instead of scattering it across conditionals. A structure that matches the domain makes invalid states unrepresentable and deletes branches. Choosing it at write time is cheap. Recovering it later reads as a refactor and gets deferred.

> 把真实的领域编码进一个数据结构，而不是把它散落在各处的条件判断里。与领域匹配的结构会让非法状态无法表示，并删掉分支。在写代码时选好它很便宜。事后再找回来就成了一次重构，然后被一再推迟。

Reach for:

> 优先考虑：

- A state machine instead of scattered booleans, phases, or lifecycle checks.

> - 用状态机代替散落的布尔值、阶段标记或生命周期检查。

- A typed model instead of loose parameters or repeated shape assumptions.

> - 用类型化模型代替松散的参数或重复的形状假设。

- A map, registry, lookup table, or discriminated union instead of branching spread across files.

> - 用映射、注册表、查找表或可辨识联合，代替分散在多个文件里的分支。

- A reducer or command/event model instead of ad hoc state mutations.

> - 用 reducer 或命令/事件模型代替临时拼凑的状态修改。

- A module organized around one body of domain knowledge instead of a sequence such as load, validate, transform, and save. Execution order is not ownership.

> - 围绕一块领域知识来组织模块，而不是按加载、校验、转换、保存这样的顺序来组织。执行顺序不等于归属。

- A small module boundary that gathers repeated behavior, ownership, or invariants.

> - 用一个小的模块边界把重复的行为、归属或不变量聚在一起。

- A queue, cache, index, graph, tree, or normalized collection where the access pattern calls for it.

> - 在访问模式需要时，用队列、缓存、索引、图、树或规范化的集合。

- Any other structure that fits. When none fits, work out what the code must never allow and how the data gets read, then find the structure that encodes exactly that.

> - 任何其他合适的结构。没有合适的时候，先弄清代码绝不能允许什么、数据是怎么被读取的，然后找一个恰好能编码这些的结构。

Do not force an abstraction. Prefer boring code if the current shape is already clear, local, and unlikely to grow. Be skeptical of an abstraction that adds indirection without removing branches, duplicated rules, invalid states, or lifecycle risk.

> 不要强行抽象。如果当前的形状已经清楚、局部并且不太可能增长，就宁可用平淡的代码。对一个只增加了间接层、却没有消除分支、重复规则、非法状态或生命周期风险的抽象，要保持怀疑。

The sign that you skipped this is a new feature that grows an existing if/else chain by one more branch, or a second boolean that must stay in sync with the first. Temporal decomposition is another sign. Phase-named modules repeat the same domain rules across steps.

> 你跳过了这一步的迹象是：一个新功能让现有的 if/else 链又多了一个分支，或者出现了第二个必须和第一个保持同步的布尔值。按时间拆分是另一个迹象。以阶段命名的模块会在各个步骤里重复同样的领域规则。
