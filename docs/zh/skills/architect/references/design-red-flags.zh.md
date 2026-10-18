<!-- 中英对照译文。原文：skills/architect/references/design-red-flags.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/architect/references/design-red-flags.md](https://github.com/bowie06111/pstack/blob/main/skills/architect/references/design-red-flags.md)

# Design red flags｜设计危险信号

Screen every candidate before synthesis. A red flag is a reason to revise or reject the shape.

> 在综合之前筛查每一个候选方案。危险信号是修改或否决这个形状的理由。

## Shallow module｜浅模块

A shallow module exposes a large interface while hiding little complexity. Judge depth by the capability and policy hidden behind the public surface relative to the size of that surface. Prefer a simple interface backed by substantial behavior.

> 浅模块暴露一个很大的接口，却只隐藏了很少的复杂度。判断深度，看的是公开接口背后隐藏的能力和策略，相对于接口本身的大小有多少。优先选择背后有大量行为支撑的简单接口。

Do not confuse a deep module with a deep call chain. A deep call chain scatters understanding across layers. A deep module concentrates capability behind one interface.

> 不要把深模块和深调用链混为一谈。深调用链把理解分散到各层。深模块把能力集中在一个接口后面。

Look for these signs:

> 留意这些迹象：

- Callers coordinate several methods to complete one operation.

> - 调用方要协调好几个方法才能完成一个操作。

- Public options expose internal stages or implementation choices.

> - 公开选项暴露了内部阶段或实现上的选择。

- Learning the interface does not save the caller from learning the implementation.

> - 学会了接口，调用方仍然免不了要去学实现。

## Information leakage｜信息泄漏

Information leakage makes multiple modules depend on the same internal decision. A representation, policy, or protocol detail appears in more than one place, so changing it requires coordinated edits.

> 信息泄漏让多个模块依赖同一个内部决定。一种表示、策略或协议细节出现在不止一个地方，所以改它需要协调好的多处修改。

Public re-exports of transport or wire types are leakage. Parse external data into domain types behind the interface. Keep storage schemas, framework objects, and protocol details private.

> 公开重新导出传输或线路格式的类型就是泄漏。在接口背后把外部数据解析成领域类型。存储结构、框架对象和协议细节保持私有。

## Temporal decomposition｜按时间拆分

Temporal decomposition organizes modules by execution order instead of the knowledge they own. Separate load, validate, transform, and save stages often repeat one representation and its invariants across several boundaries.

> 按时间拆分是按执行顺序来组织模块，而不是按它们所掌握的知识。分开的加载、校验、转换和保存阶段，经常让同一种表示及其不变量在好几个边界上重复。

Group code around domain knowledge and ownership. Methods that run at different times can still belong to one module when they protect the same decisions.

> 围绕领域知识和归属来组织代码。在不同时间运行的方法，只要守护的是同一组决定，仍然可以属于同一个模块。

## Pass-through method｜透传方法

A pass-through method forwards the same arguments to another method with the same shape. It adds a layer without hiding complexity.

> 透传方法把同样的参数转发给另一个形状相同的方法。它增加了一层，却没有隐藏任何复杂度。

Remove it or move responsibility to the module that can complete the operation. Keep a forwarding boundary only when it adds policy, adaptation, or a distinct abstraction.

> 删掉它，或者把职责移到能完成这个操作的模块里。只有当转发边界增加了策略、适配或一个独立的抽象时，才保留它。

## Split ownership｜归属分裂

More than one module writes the same state or keeps its own copy of it. An agent that edits one writer can't see the others, so their rules diverge.

> 不止一个模块写同一份状态，或者各自保留一份拷贝。编辑其中一个写入者的代理看不到其他写入者，于是它们的规则逐渐分歧。

Give each piece of state one owner. Other modules read it or ask the owner to change it.

> 每块状态只有一个所有者。其他模块读取它，或者请所有者修改它。

## Two ways to do one task｜一件事两种做法

The design supports more than one way to do the same task. An agent copies whichever way it finds first, so every way keeps gaining callers.

> 设计支持用不止一种方式完成同一件事。代理会照抄它最先找到的那种，所以每种做法都在不断增加调用方。

Keep one way. Move callers off the others and delete them in the same change.

> 只保留一种做法。在同一个改动里把调用方从其他做法上迁走，并删掉它们。

## Importable internals｜可导入的内部实现

A caller can import a module's internals. An agent takes the shortest path that compiles, so it imports them directly and they become part of the interface.

> 调用方可以导入一个模块的内部实现。代理会走能编译通过的最短路径，所以它会直接导入它们，于是它们就成了接口的一部分。

Make internals unreachable from outside the module, so an import from outside fails the build.

> 让内部实现从模块外部无法访问，这样从外部导入会让构建失败。

## Hand-synced list｜手工同步的列表

Two or more places list the same items, and adding an item means editing every list. An agent that sees one list updates only that one.

> 两个或更多地方列着同样的条目，新增一项就要改每一个列表。只看到一个列表的代理只会更新那一个。

Keep one list and derive the others from it. If a list can't be derived, make the build fail when the lists disagree.

> 只保留一个列表，其他的从它推导。如果某个列表推导不出来，就让构建在列表不一致时失败。
