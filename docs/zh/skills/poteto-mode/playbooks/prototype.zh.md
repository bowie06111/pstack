<!-- 中英对照译文。原文：skills/poteto-mode/playbooks/prototype.md。英文为原文，引用块内为中文译文。 -->

> 原文链接：[skills/poteto-mode/playbooks/prototype.md](https://github.com/bowie06111/pstack/blob/main/skills/poteto-mode/playbooks/prototype.md)

### Prototype｜原型

**You own the design decision, not the code. The prototype is a throwaway instrument. The real build follows Feature.**

> **设计决定由你负责，不是代码。原型是用完即弃的工具。真正的构建走 Feature。**

The one playbook where the Laziness Protocol's "smallest change" and the verification bar invert. Speed over polish, code quality does not matter, no planning. The rigor is in picking the right design cheaply. Propose variations the user didn't ask for, throw an approach away and try another.

> 这是唯一一个把懒惰协议的“最小改动”和验证标准倒过来的剧本。速度优先于打磨，代码质量无所谓，不做规划。严谨体现在用低成本选对设计。主动提出用户没要求的变体，扔掉一种做法，再试另一种。

1. Scope the decision the prototype exists to make: which layout, which interaction, which density, or for an empirical fork which behavior, timing, or approach. No decision means no prototype. Route to Feature.

> 1. 界定这个原型要做的决定：哪种布局、哪种交互、哪种密度，或者对于经验性的分岔，哪种行为、时序或做法。没有要做的决定，就不做原型。转到 Feature。

2. Gather references when the design space is open. Search for prior art, summarize a moodboard of themes, palettes, and layouts, let the user pick directions before building. Skip when the direction is set.

> 2. 设计空间开放时收集参考。搜索已有作品，总结出主题、配色和布局的情绪板，让用户在构建前选方向。方向已定时跳过。

3. Build throwaway in an isolated scratch dir, separate from production source. For a visual decision, vanilla HTML/CSS/JS or the lightest stack that renders the idea, CDN deps, a dev server with hot reload. For a behavioral or timing decision, the smallest script that exercises the question. No production framework, no tests, no abstractions.

> 3. 在一个独立的临时目录里做一次性的东西，和生产代码分开。视觉决定用原生 HTML/CSS/JS 或能表现想法的最轻量技术栈，CDN 依赖，带热重载的开发服务器。行为或时序决定，用能检验问题的最小脚本。不用生产框架，不写测试，不做抽象。

4. When comparing alternatives, build them behind one switcher (buttons or a keypress), each variant labeled. This is the **exhaust-the-design-space** principle skill made cheap.

> 4. 比较多个方案时，把它们放在同一个切换器后面（按钮或按键），每个变体都标上名字。这就是低成本化的 **exhaust-the-design-space** 原则技能。

5. Verify on the matching surface. For a visual decision, screenshot each variant via the control skill and drive the interaction. For a behavioral or timing decision, observe the thing you are deciding by logging the timing, printing the output, or watching the render. The observation is the test here, not an assertion.

> 5. 在对应的界面上验证。视觉决定，通过控制技能给每个变体截图并操作交互。行为或时序决定，通过记录时序、打印输出或观察渲染来观察你要决定的东西。这里观察就是测试，而不是断言。

6. Present alternatives, tradeoffs, and a recommendation. The output is the decision plus the throwaway artifact, not shippable code. Hand the chosen direction to **Feature** (or `architect` for the shape) for the real build.

> 6. 给出备选方案、取舍和一个建议。产出是决定加上一次性的产物，不是可发布的代码。把选定的方向交给 **Feature**（或用 `architect` 定形状）做真正的构建。

**Reply:** the variants explored, the evidence (screenshots for a visual decision, the observed output or timing for a behavioral one), tradeoffs, your recommendation, and the scratch path. Say plainly that the prototype is throwaway.

> **回复：** 探索过的变体、证据（视觉决定给截图，行为决定给观察到的输出或时序）、取舍、你的建议，以及临时目录路径。明确说明原型是用完即弃的。
