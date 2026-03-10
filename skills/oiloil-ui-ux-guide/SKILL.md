---
name: oiloil-ui-ux-guide
description: Modern, clean UI/UX guidance and design review for web/app interfaces. Use when you need UX recommendations, design principles, or reviewing existing UI (screenshots, mockups, HTML). Focus on visual hierarchy, task-first UX, feedback states, consistency, and error prevention. Enforce minimal style (spacious, typography-led) and forbid emoji as icons.
---

# OilOil UI/UX Guide (Modern Minimal)

Use this skill in two modes:

- `guide`: Provide compact principles and concrete do/don't rules for modern clean UI/UX.
- `review`: Review an existing UI (screenshot / mock / HTML / PR) and output prioritized, actionable fixes.

Keep outputs concise. Prefer bullets, not long paragraphs.

## Workflow (pick one)

### 1) `guide` workflow
1. Identify the surface: marketing page / dashboard / settings / creation flow / list-detail / form.
2. Identify the primary user task and primary CTA.
3. Apply the system-level guiding principles first (mental model and interaction logic).
4. Then apply the core principles below (start from UX, then refine with CRAP).
5. If icons are involved: apply `references/icons.md`.

### 2) `review` workflow
1. State assumptions (platform, target user, primary task).
2. List findings as `P0/P1/P2` (blocker / important / polish) with short evidence.
3. For each major issue, label the diagnosis: execution vs evaluation gulf; slip vs mistake (see `references/design-psych.md`).
4. Propose fixes that are implementable (layout, hierarchy, components, copy, states).
5. End with a short checklist to verify changes.

Use `references/review-template.md` when you need a stable output format.

## Non-negotiables (hard rules)
- No emoji used as icons (or as UI decoration). If an emoji appears, replace it with a proper icon.
- Icons must be intuitive and refined. Use a single consistent icon set for the product (avoid mixing styles).
- Minimize copy by default. Add explanatory text only when it prevents errors, reduces ambiguity, or improves trust.

## System-Level Guiding Principles

Apply these as first-order constraints before choosing components or page patterns.
Full definitions and review questions: `references/system-principles.md`.

Key principles: concept constancy · primary task focus · UI copy source discipline · state perceptibility · help text layering (L0–L3) · feedback loop closure · prevention + recoverability · progressive complexity · action perceptibility · cognitive load budget · evolution with semantic continuity.

## Core Principles (minimal set)

### A) Task-first UX
- Make the primary task obvious in <3 seconds.
- Allow exactly one primary CTA per screen/section.
- Optimize the happy path; hide advanced controls behind progressive disclosure.

### B) Information architecture (grouping & findability)
- Group by user mental model (goal/object/time/status), not by backend fields.
- Use clear section titles; keep navigation patterns stable across similar screens.
- When item count grows: add search/filter/sort early, not late.

### C) Feedback & system status
- Cover all states: loading, empty, error, success, permission. Details in `references/checklists.md`.
- After any action, answer: "did it work?" + "what changed?" + "what can I do next?"
- Prefer inline, contextual feedback over global toasts (except for cross-page actions).

### D) Consistency & predictability
- Same interaction = same component + same wording + same placement.
- Use a small, stable set of component variants; avoid one-off styles.

### E) Affordance + Signifiers (make actions obvious)
- Clickable things must look clickable (button/link styling + hover/focus + cursor). On web, custom clickable elements need `cursor: pointer` and focus styles.
- Primary actions need a label; icon-only is reserved for universally-known actions.
- Show constraints before submit (format, units, required), not only after errors.
- For deeper theory (affordances, signifiers, mapping, constraints): see `references/design-psych.md`.

### F) Error prevention & recovery
- Prevent errors with constraints, defaults, and inline validation.
- Make destructive actions reversible when possible; otherwise require deliberate confirmation.
- Error messages must be actionable (what happened + how to fix).

### G) Cognitive load control
- Reduce choices: sensible defaults, presets, and progressive disclosure.
- Break long tasks into steps only when it reduces thinking (not just to look "enterprise").
- Keep visual noise low: fewer borders, fewer colors, fewer competing highlights.

### H) CRAP (visual hierarchy & layout)
- Contrast: emphasize the few things that matter (CTA, current state, key numbers).
- Repetition: tokens/components/spacing follow a scale; avoid "almost the same" styles.
- Alignment: align to a clear grid; fix 2px drift; align baselines where text matters.
- Proximity: tight within a group, loose between groups; spacing is the primary grouping tool.

## Spacing & layout discipline (compact rule set)

Use this when implementing or reviewing layouts. Keep it short, but enforce it strictly.

- Rule 1 - One spacing scale:
  - Base unit: 4px.
  - Allowed spacing set (recommended): 4 / 8 / 12 / 16 / 24 / 32 / 40 / 48.
  - New gaps/padding should use this set; off-scale values need a clear reason.
- Rule 2 - Repetition first:
  - Same component type keeps the same internal spacing (cards, list rows, form groups, section blocks).
  - Components with the same visual role should not have different spacing patterns.
- Rule 3 - Alignment + grouping:
  - Align to one grid and fix 1-2px drift.
  - Tight spacing within a group, looser spacing between groups.
- Rule 4 - No decorative nesting:
  - Extra wrappers must add real function (grouping, state, scroll, affordance).
  - If a wrapper only adds border/background, remove it and group with spacing instead.
- Quick review pass:
  - Any off-scale spacing values?
  - Any baseline/edge misalignment?
  - Any wrapper layer removable without losing meaning?

## Modern minimal style guidance (taste with rules)
- Use whitespace + typography to create hierarchy; avoid decoration-first design.
- Prefer subtle surfaces (light elevation, low-contrast borders). Avoid heavy shadows.
- Keep color palette small; use one accent color for primary actions and key states.
- Copy: short, direct labels; add helper text only when it reduces mistakes or increases trust.

## Motion (animation) guidance (content/creator-friendly, not flashy)
- Motion explains **hierarchy** (what is a layer/panel) and **state change** (what just happened). Avoid motion as decoration.
- Default motion vocabulary: fade; then small translate+fade; allow tiny scale+fade for overlays. Avoid big bouncy motion.
- Keep the canvas/content area stable. Panels/overlays can move; the work surface should not "float."
- Prefer consistency over variety: same component type uses the same motion pattern.
- Avoid layout jumps. Use placeholders/skeletons to keep layout stable while loading.

## Anti-AI Defaults (强制约束)

AI 生成 UI 时有固定倾向。以下是反模式清单，违反必须修复。

### 字体
- ❌ **禁止**: Inter, Roboto, Arial, Open Sans, system-ui
- ✅ **推荐**: Plus Jakarta Sans, Outfit, Manrope, DM Sans, Geist

### 颜色
- ❌ **禁止**:
  - 纯黑 `#000`, 纯白 `#fff`, 纯灰 `#888`
  - 在有色背景上使用灰色文字
  - purple-to-blue 渐变, cyan-on-dark
  - `#6366f1` (generic indigo)
- ✅ **推荐**:
  - **Tinted neutrals**: 给所有灰色添加品牌色调 (例: 品牌色 `#0066cc` → 灰色用 `#1a2a3a` 而非 `#333`)
  - **OKLCH 色彩空间**: 替代 HSL，感知均匀，调整明度/彩度时更自然
  - 深色模式需要更高的对比度和彩度，不是简单反转

### 布局
- ❌ **禁止**:
  - 所有内容包在卡片里
  - 卡片嵌套卡片
  - 相同的 3 列卡片网格
  - 圆角过大 (>12px) 的 "pill" 风格
- ✅ **推荐**:
  - 用留白和排版建立层次，而非边框/卡片
  - 卡片只用于真正需要分组的内容
  - 网格布局使用 `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))`

### 动效
- ❌ **禁止**: bounce/easing, 入场动画过多, 装饰性持续动画
- ✅ **推荐**: fade → translate+fade → scale+fade (仅 overlay)

### 图标
- ❌ **禁止**: emoji 作为图标, 混合多种图标风格
- ✅ **推荐**: Lucide, Phosphor, Heroicons 等现代图标库

---

## Bold Typography (大胆排版)

AI 生成的 UI 倾向于"安全"的布局——居中对齐、均匀网格、可预测的层次。鼓励探索更有个性的排版方向。

### 原则
- **排版即装饰**: 用字体大小/粗细/位置的对比代替边框/阴影/卡片
- **留白是设计元素**: 大面积留白本身就是视觉语言
- **不对称可以创造动态感**: 保持视觉重量平衡的前提下
- **一个焦点就够了**: 让关键元素突出，其他自然退后
- **网格是工具不是规则**: 必要时打破，但要有意图

### 何时大胆
Marketing 页面、产品介绍、Landing page、Hero 区域、作品展示——需要吸引注意力、传递品牌个性的场景。

### 何时克制
表单填写、数据录入、复杂操作流程、需要快速扫描的列表——效率和清晰度优先的场景。

---

## Anti-AI Self-Check (生成后必查)

- **Gradient restraint** — 装饰性渐变每页最多 1 个。背景、按钮、边框同时用渐变 = 过度。
- **No emoji as UI** — 检查 section icons、状态指示、按钮标签是否混入 emoji。
- **Copy necessity** — 删除这段文字后，用户能通过布局/图标/位置理解吗？能 → 删除。
- **Decoration justification** — 每个视觉特效 (blur/glow/动画) 必须回答："帮助用户理解什么？" 无答案 → 删除。
- **Font check** — 是否使用了 Inter/Roboto？替换为 Plus Jakarta Sans/Outfit/Manrope。
- **Color check** — 是否有纯黑纯白纯灰？是否在彩色背景上用灰色文字？添加色调。

## References
- System-level guiding principles (concept constancy, copy discipline, state perceptibility, etc.): `references/system-principles.md`
- Interaction psychology (Fitts/Hick/Miller, cognitive biases, flow, attention): `references/interaction-psychology.md`
- Design psychology (affordances, signifiers, mapping, constraints, gulfs, slips vs mistakes): `references/design-psych.md`
- Icon rules and "intuitive refined" guidance: `references/icons.md`
- Review output template and scoring: `references/review-template.md`
- Expanded checklists (states, affordance, lists, forms, settings, motion, dashboards, copy): `references/checklists.md`
