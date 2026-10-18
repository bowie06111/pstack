<!-- 中英对照译文。原文：skills/principle-test-behavior-not-implementation/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-test-behavior-not-implementation/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-test-behavior-not-implementation/SKILL.md)

```yaml
name: principle-test-behavior-not-implementation
description: "Apply when you write, change, or keep a test. Assert what the code's users can observe, so that the test fails when that behavior breaks and keeps passing when only the internals change."
disable-model-invocation: true
```

> **说明（description）：** 在编写、修改或保留测试时使用。断言代码使用者能观察到的东西，这样行为坏了测试会失败，只有内部改变时测试仍然通过。

# Test Behavior, Not Implementation｜测试行为而非实现

A test is a claim about what the code does for its users: a caller, an end user, or another service. Behavior is what those users can observe through the interface they use. That includes return values, errors, state they can read back, and effects on systems outside the code, such as a request sent, a row written, or a message emitted. Implementation is what they cannot observe: private helpers, internal call order, intermediate data, and which internal collaborator did the work.

> 测试是关于代码为它的使用者做了什么的一个论断：使用者可以是调用方、终端用户或另一个服务。行为是这些使用者通过他们所用的接口能观察到的东西。包括返回值、错误、他们能读回的状态，以及对代码之外系统的影响，比如发出的请求、写入的行或发送的消息。实现是他们观察不到的东西：私有辅助函数、内部调用顺序、中间数据，以及到底是哪个内部协作者完成了工作。

Judge each test against two imagined edits:

> 用两个想象中的修改来评判每一个测试：

- **Break the behavior.** Make the code return a wrong value, skip an effect, or do the effect twice. If the test still passes, it proves nothing. The usual causes are an assertion looser than the contract, an expected value computed by the code under test, a restated constant or prompt string that never runs the code that reads it, and an assertion on data that the test built itself.

> - **弄坏行为。** 让代码返回一个错误的值、跳过一个效果，或者把效果做两次。如果测试仍然通过，它什么都证明不了。常见的原因有：断言比约定更宽松、期望值是由被测代码算出来的、重复写了一个常量或提示词字符串却从没运行读取它的代码，以及断言的是测试自己构造的数据。

- **Change the internals.** Keep every observable result the same and restructure the code underneath. If the test now fails, it is pinned to the implementation. It will need a rewrite on every refactor and still catches no bug.

> - **改变内部。** 保持每一个可观察的结果不变，重构底下的代码。如果测试现在失败了，它就被钉死在实现上了。每次重构它都要重写，而且仍然抓不到任何 bug。

Keep a test that catches the first edit and survives the second. Rewrite or delete the rest. To write a test that does both, drive the code through its users' interface with a concrete input. Derive the expected value from the requirement, not from a run of the code. Assert what the contract promises, no less and no more.

> 保留那些能抓住第一种修改、又能挺过第二种修改的测试。其余的重写或删除。要写出两者都做到的测试，就用一个具体的输入，通过使用者的接口驱动代码。期望值从需求推导，而不是从代码的运行结果推导。断言约定承诺的内容，不多也不少。

The contract decides, not the shape of the assertion. When "an empty cart never charges" is the promise, an assertion that the payment client got no call is behavior. Pair it with a case where the charge happens, so that the test shows it can see a call. The payload that a mocked external service receives is behavior, because that service observes it. A call to an internal helper is implementation, because no user can see it. An empty list is the exact answer when no record matches, and a hollow check when the test only needs some result. An exact error message pins wording that no caller reads. The error type that callers branch on is the contract.

> 决定权在约定，而不在断言的形式。当“空购物车永远不会扣款”是承诺时，断言支付客户端没有收到调用就是行为。再配一个确实会扣款的用例，这样测试能证明它看得到调用。被模拟的外部服务收到的请求内容是行为，因为那个服务观察得到它。对内部辅助函数的调用是实现，因为没有使用者能看到它。当没有记录匹配时，空列表是确切的答案；当测试只需要有某个结果时，它就是一个空洞的检查。确切的错误信息钉死的是没有调用方会读的措辞。调用方据以分支的错误类型才是约定。

A test can pin an invariant instead of a call, such as a key that must exist in two tables or a compile-time type check. The invariant is the behavior.

> 测试也可以钉住一个不变量而不是一次调用，比如某个键必须同时存在于两张表里，或者一个编译期的类型检查。这个不变量就是行为。
