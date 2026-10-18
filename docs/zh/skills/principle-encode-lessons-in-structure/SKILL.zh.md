<!-- 中英对照译文。原文：skills/principle-encode-lessons-in-structure/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-encode-lessons-in-structure/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-encode-lessons-in-structure/SKILL.md)

```yaml
name: principle-encode-lessons-in-structure
description: "Apply when you catch yourself writing the same instruction a second time, or notice a recurring correction. Encode the rule as a lint, metadata flag, runtime check, or script instead of more text."
disable-model-invocation: true
```

> **说明（description）：** 在发现自己第二次写同一条指示，或注意到反复出现的纠正时使用。把规则编码成 lint、元数据标记、运行时检查或脚本，而不是写更多文字。

# Encode Lessons in Structure｜把教训编码进结构

If the lesson needs judgment and cannot be encoded, make the instruction more prominent and add an example of the failure mode.

> 如果这条教训需要判断、无法编码，就让指示更醒目，并加上一个失败情形的例子。

**Corollary:** If the fix is structural, only use the structural fix. The instruction is the symptom.

> **推论：** 如果修复是结构性的，就只用结构性的修复。指示只是症状。
