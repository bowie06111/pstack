<!-- 中英对照译文。原文：skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md)

```yaml
name: principle-migrate-callers-then-delete-legacy-apis
description: "Apply when introducing a new internal API while old callers still exist. Migrate callers and delete the old API in the same wave instead of preserving compatibility layers."
disable-model-invocation: true
```

> **说明（description）：** 在旧调用方仍存在时引入新的内部接口时使用。在同一波里迁移调用方并删除旧接口，而不是保留兼容层。

# Migrate Callers Then Delete Legacy APIs｜先迁移调用方再删除旧接口

When we decide a new API is the right design, migrate callers and remove the old API in the same refactor wave instead of preserving compatibility layers.

> 当我们确定新接口是正确的设计时，在同一波重构里迁移调用方并删除旧接口，而不是保留兼容层。

**Rule:**

> **规则：**

- Do not keep legacy API paths only because internal callers still exist

> - 不要仅仅因为还有内部调用方，就保留旧接口的路径

- Inventory callers, migrate them, and delete the old API immediately

> - 清点调用方，迁移它们，然后立刻删除旧接口

- Treat temporary adapters as exceptional and time-boxed, not default architecture

> - 把临时适配器当作例外并设定时限，而不是默认的架构

- Update tests to assert the new contract, and delete tests that only protect pre-refactor implementation details

> - 更新测试，让它们断言新的约定；删除那些只保护重构前实现细节的测试

**When this applies:**

> **适用条件：**

- No external users depend on backward compatibility

> - 没有外部用户依赖向后兼容

- The project can absorb coordinated breaking changes

> - 项目能承受协调好的破坏性改动

- The new API is part of a simplification or refactor initiative

> - 新接口是一次简化或重构计划的一部分

Keeping both old and new APIs creates dual-path complexity, slows cleanup, and makes the codebase feel append-only.

> 同时保留新旧两套接口会带来双路径的复杂度，拖慢清理，让代码库感觉只能往上加东西。
