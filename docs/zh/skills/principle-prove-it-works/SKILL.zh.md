<!-- 中英对照译文。原文：skills/principle-prove-it-works/SKILL.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/principle-prove-it-works/SKILL.md](https://github.com/bowie06111/pstack/blob/main/skills/principle-prove-it-works/SKILL.md)

```yaml
name: principle-prove-it-works
description: "Apply after completing a task, before declaring done. Verify against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, self-report, or 'it compiles.'"
disable-model-invocation: true
```

> **说明（description）：** 在完成任务后、宣布完成之前使用。对着真实产物验证（运行功能、读取真实的值、检查 diff），而不是代理指标、自我报告或“能编译”。

# Prove It Works｜证明它能用

Verify every task output by checking the real thing directly. Do not infer from proxies, self-reports, or "it compiles."

> 通过直接检查真实的东西来验证每一个任务产出。不要从代理指标、自我报告或“能编译”去推断。

**Why:** Unverified work has unknown correctness. Indirect verification (file mtimes, output freshness, agent self-reports, cached screenshots) feels cheaper than direct observation. Acting on a wrong inference costs far more than checking the source.

> **原因：** 未经验证的工作，正确性是未知的。间接验证（文件修改时间、输出新鲜度、代理的自我报告、缓存的截图）感觉比直接观察便宜。但依据一个错误的推断行动，代价远远高于去查源头。

Check the real thing, not a proxy:

> 检查真实的东西，而不是代理指标：

- Check process liveness directly, not indirectly through derived state

> - 直接检查进程是否存活，而不是通过推导出来的状态间接判断

- Read the actual value, not a cached or derived representation

> - 读取真实的值，而不是缓存的或推导出来的表示

- When verification fails, suspect the observation method before suspecting the system

> - 验证失败时，先怀疑观察方法，再怀疑系统

## Script the check when you can｜能写成脚本就写成脚本

The strongest proof is a deterministic script that re-runs the same comparison, not a one-time eyeball. Write the script, run it, and keep its output as an artifact a reviewer can re-run instead of trusting your word.

> 最有力的证明是一个能重跑同样比较的确定性脚本，而不是一次性的肉眼查看。写好脚本，运行它，把它的输出作为审查者可以重跑的产物保留下来，而不是让人相信你的话。

Keep the artifact visible for the human. Commit it only for large or complex work where the trail has to be auditable later, like a big port or migration (the **show-me-your-work** skill).

> 让人能看到这个产物。只有对于大型或复杂的工作、之后需要可审计的记录时（比如一次大规模移植或迁移），才提交它（**show-me-your-work** 技能）。
