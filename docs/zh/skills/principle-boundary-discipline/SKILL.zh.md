<!-- 中英对照译文。原文：skills/principle-boundary-discipline/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-boundary-discipline/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-boundary-discipline/SKILL.md)

```yaml
name: principle-boundary-discipline
description: "Apply when wiring validation, error handling, or framework adapters. Concentrate guards at system boundaries (CLI, config, network, external APIs); trust internal types and keep business logic in pure functions."
disable-model-invocation: true
```

> **说明（description）：** 在接入校验、错误处理或框架适配器时使用。把防护集中在系统边界（命令行、配置、网络、外部 API）；内部信任类型，业务逻辑放在纯函数里。

# Boundary Discipline｜边界纪律

Place validation, type narrowing, and error handling at system boundaries. Trust internal code unconditionally. Business logic lives in pure functions. The shell is thin and mechanical.

> 把校验、类型收窄和错误处理放在系统边界。无条件地信任内部代码。业务逻辑放在纯函数里。外壳保持薄而机械。

**Why:** Scattered validation is noisy, redundant, and gives a false sense of safety. Keep logic out of framework wiring so it can be tested without the framework.

> **原因：** 散落各处的校验嘈杂、重复，还会给人一种虚假的安全感。把逻辑放在框架的接线代码之外，这样不用框架也能测试它。

**The pattern:**

> **做法：**

- **At boundaries** (CLI args, config files, external APIs, network protocols): validate, return errors, handle defensively.

> - **在边界上**（命令行参数、配置文件、外部 API、网络协议）：校验，返回错误，防御性地处理。

- **Inside the system:** typed data, error propagation, no re-validation. Trust the types.

> - **在系统内部：** 有类型的数据，错误向上传递，不重复校验。信任类型。

- **Across the boundary.** Expose domain concepts, not the boundary's private representation. Keep general-purpose mechanism inside and special-purpose policy at the edge.

> - **跨越边界时。** 暴露领域概念，而不是边界的私有表示。通用的机制放在内部，特定用途的策略放在边缘。

**Applications:**

> **应用：**

Validation and error handling:

> 校验和错误处理：

- Validate config at parse time (the boundary), not inside business logic

> - 在解析时（边界）校验配置，而不是在业务逻辑里

- Parse raw data into domain types at the boundary

> - 在边界把原始数据解析成领域类型

- Do not re-export transport, storage, framework, or wire types through the public surface

> - 不要通过公开接口重新导出传输、存储、框架或线路格式的类型

- No redundant nil checks deep in call chains if the boundary already validated

> - 如果边界已经校验过，就不要在调用链深处做多余的空值检查

Code organization:

> 代码组织：

- Business logic in pure functions with no framework dependencies

> - 业务逻辑放在不依赖框架的纯函数里

- Parse functions: pure transforms from raw bytes to typed state

> - 解析函数：从原始字节到有类型状态的纯转换

- Prompt construction: structured state in, string out

> - 提示词构建：输入结构化状态，输出字符串

- Scoring and assessment: pure transforms from state to results

> - 评分和评估：从状态到结果的纯转换

**The tests:**

> **检验方法：**

- "Is this data crossing a system boundary right now?" If not, validation is redundant.

> - “这份数据此刻正在跨越系统边界吗？”如果不是，校验就是多余的。

- "Can this be a pure function that the shell just calls?" If yes, extract it.

> - “这能不能是一个外壳只管调用的纯函数？”如果能，就把它提取出来。
