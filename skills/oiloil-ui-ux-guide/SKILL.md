---
name: oiloil-ui-ux-guide
description: Run a structured UI/UX consultation to either (a) co-design a project-specific design system and emit `design-spec.md`, (b) review an existing UI with prioritized fixes, or (c) emit compact do/don't rules for a surface. Triggers when the user wants to define / build / refine a design system or design tokens, asks for a design spec, asks for a full UI review of a screen / mockup / PR, or wants design rules for a surface type. Do NOT trigger for narrow one-off questions ("is this color OK?", "should this button be larger?") — answer those directly without invoking the consultation flow.
---

# OilOil UI/UX Guide

A style-neutral UI/UX consultation skill. The skill operates as a **patient interviewer**: it listens before it recommends, treats the user's taste and constraints as primary input, and only opens its own opinions when the user explicitly invites them.

## Default behavior

When triggered without an explicit mode, run `design`. Switch only when the user is explicit:

| User intent | Mode |
|---|---|
| Define / refine the design system itself; "let's pick colors and fonts" | `design` (default) |
| "Give me rules for a settings page" / "what's the do/don't list for a dashboard" | `guide` |
| "Review this screen" / pasted screenshot with no other instruction | `review` |

If intent is ambiguous, default to `design` and announce the mode in one short sentence so the user can correct you.

## Hard precondition for `design`: explore the code first, then listen

Before asking the user anything, you **must** run Phase 0 (codebase scan). After Phase 0, you **listen first** — ask open questions about the user's product, brand, and taste; do **not** open with recommendations.

Phase 0 — what "explored" means:
1. Glob the project for design tokens (Tailwind config, theme files, CSS variables, design-system folders).
2. Detect the UI framework from `package.json`.
3. Sample 2–3 representative UI files to see how tokens are actually used.
4. Form a factual summary of what exists — *not* a verdict on whether it's good.

Open with one paragraph: what tokens / framework / consistency level you found. Then ask the user what they want to keep, change, or define from scratch. Recommendations come later, only when invited or when the user is clearly stuck.

Full Phase 0 patterns and the listening-first interview script: `references/design-interview.md`.

---

## Operating principles (all modes)

These shape *how* the skill talks, not *what* it produces.

### Listen first, recommend last
- Open with questions, not opinions. Find out the user's product, brand, references, constraints.
- When presenting options, give 2–3 **without** a starred recommendation. Let the user choose. Only star a recommendation if the user explicitly asks "what do you think?" or "what would you pick?".
- Don't ascribe value labels to options ("premium" vs "efficient" is loaded). Use neutral descriptors and concrete references.

### Imagery over jargon
- "Closer to Linear" beats "sharp + dense + monochrome".
- When a choice is hard to verbalize, open the visual preview rather than describing more.

### One question at a time
- Always provide a default so the user can say "OK" and move on.
- Don't bundle multiple decisions into one prompt.

### Challenge mismatches *gently*
- If the user's choices contradict their stated product or audience, name the tension and offer two paths — don't simply override.

---

## Mode workflows

### `design` (default) — Design Spec Builder

Output: `design-spec.md` in the project root, validated against a project-specific business mockup.

1. **Phase 0 · Scan code** (silent, mandatory) — see precondition above.
2. **Phase 1 · Listen** — open questions about product, audience, brand assets, references the user admires, hard constraints, **primary locale**. No recommendations yet.
3. **Phase 2 · Style Family selection** — if the user already named a clear direction, confirm it and skip ahead. Otherwise present 2–4 candidate **style families** (`references/style-families/index.md`) as neutral options. The chosen family supplies a token starting point — the user can override anything.
4. **Phase 3 · Visual choices** — for each remaining unknown, present 2–3 options drawn from the chosen family. Token dimensions: color · type · radius · spacing · shadow · motion · **container strategy · icon system · decoration · locale**. The last four come from `references/extended-dimensions.md` and are real visual decisions; don't skip them.
5. **Phase 4a · Generic preview** — render tokens on the static template's 5 surfaces (dashboard / marketing / content / form / pricing) for *exploration*. Switchers for container strategy / icon set / decoration / viewport / dark / locale. Iterate by rewriting the JSON config only.
6. **Phase 4b · Business mockup** ← **the gating artifact**. Generate a standalone HTML of the user's *actual product surface*, in *their language*, applying the *full token set including extended dimensions*. The user makes the final ship/iterate decision here, not in the generic preview. Strict contract: `references/business-mockup-contract.md`.
7. **Phase 5 · Output** — generate `design-spec.md` only after the user has signed off on the business mockup. Template: `references/design-spec-template.md`.

Full flow: `references/design-interview.md`
Style family library: `references/style-families/`
Extended token dimensions (containerStrategy / iconSystem / decoration / locale): `references/extended-dimensions.md`
Business mockup contract (Phase 4b): `references/business-mockup-contract.md`
Preview template: `references/design-preview-template.html`

### `guide` — Compact rules for a surface

1. Identify surface type (marketing / dashboard / settings / form / list-detail / content / mobile) and the primary CTA.
2. Apply the **UX Hard Rules** below.
3. Apply system-level constraints (`references/system-principles.md`).
4. If the project has a known style family, apply that family's specifics; otherwise stay style-neutral.
5. If icons are involved: `references/icons.md`.

Output: bullet do/don't list, no long paragraphs.

### `review` — Prioritized fixes for an existing UI

1. State assumptions (platform, target user, primary task) — one line each.
2. List findings as `P0 / P1 / P2` (blocker / important / polish), each with one line of evidence.
3. For major issues, label the diagnosis using `references/design-psych.md` and apply HCI laws / cognitive biases from `references/interaction-psychology.md` when relevant.
4. Propose implementable fixes (layout, component, copy, state).
5. End with a short verification checklist.

Output format: `references/review-template.md`. Per-surface checklists: `references/checklists.md`.

**Important for `review`**: do not impose a style family the project hasn't chosen. Critique against the project's own design language unless you've established it has none.

---

## UX Hard Rules (style-independent — apply to every project)

These are not aesthetic preferences. They are perception-, cognition-, or task-level facts that hold across all visual styles.

1. **Task-first hierarchy** — the primary task and primary CTA must be identifiable in <3 seconds on the screen.
2. **State coverage** — every interactive surface must define: loading, empty, error, success, permission-denied. Missing any one is a real bug, not polish. See `references/checklists.md`.
3. **Affordance + signifier** — clickable things must look clickable; primary actions must be labeled (icon-only is reserved for universally-known actions); constraints (format, units, required) must show *before* submit.
4. **Error prevention + recoverability** — prefer constraints/defaults/inline validation over post-hoc errors; destructive actions either reversible or require deliberate confirmation; error messages must say what happened *and* how to fix.
5. **Feedback loop closure** — after any action, the UI must answer: "did it work?" + "what changed?" + "what's next?". See `references/system-principles.md`.
6. **Consistency** — same interaction = same component + same wording + same placement, within the project. Cross-project consistency is *not* a hard rule.
7. **CRAP for visual hierarchy** — Contrast / Repetition / Alignment / Proximity. These are perceptual constants, not style choices.
8. **Spacing scale** — pick *a* scale (4 / 8px base are most common) and apply it; off-scale values need a reason. The specific scale is a project choice; the discipline is a hard rule.
9. **Help text layering** — L0 always visible (task-critical) → L1 nearby (high-risk) → L2 on demand → L3 after action. Many L0 hints = fix IA, not add more text.
10. **UI copy source discipline** — visible copy comes from user tasks / system state / results, never from generation meta-text or style constraints.

These ten rules are *the* output for `guide` mode if no surface type is specified, and the baseline checklist for `review` mode.

---

## Style Lens (project-chosen — never default-imposed)

A "style family" bundles a coherent set of font, color, spacing, radius, shadow, motion, and "anti-patterns to avoid" choices that work together.

The skill ships with eight families. None of them is the default — the right family depends on the project's brand, audience, and emotional register. See `references/style-families/index.md` for the catalog and `references/style-families/<family>.md` for each family's specifics.

| Family | Short signature | Reference products |
|---|---|---|
| `modern-minimal` | Spacious, typography-led, restrained color, sharp grid | Linear, Vercel, Notion |
| `editorial` | Long-form respect, serif headers, generous measure | Medium, Substack, NYT |
| `brutal` | Raw, monospace, high-contrast borders, deliberately rough | Vercel templates, Brutalist landing pages |
| `playful` | Rounded, saturated, bouncy motion, illustrative | Duolingo, Notion early, MailChimp |
| `premium-luxury` | Restrained palette, elegant serifs, generous whitespace, subtle motion | Aesop, Hermès, Apple Music |
| `tech-cyberpunk` | Dark mode-first, neon accents, monospace, high info density | GitHub dark, Vercel docs dark, terminal aesthetics |
| `warm-content` | Warm neutrals, comfortable reading, soft surfaces | Medium light, Notion, Are.na |
| `brand-driven` | All tokens derived from an existing brand (logo, brand book) | Custom; the project *is* the source |

**Important**: families are starting points, not cages. A user can pick `modern-minimal` and still want 16px radius. The family supplies defaults; the user always wins.

**Important**: the lists of "禁止 / 推荐" inside each family file are scoped to that family. They are not global UX rules. `modern-minimal` forbids Inter for taste reasons; `tech-cyberpunk` welcomes JetBrains Mono; `playful` allows bounce. Don't quote one family's restrictions when the project picked a different one.

---

## When the user pushes back on a suggestion

Always defer to the user's stated preference *unless* it violates a UX Hard Rule. If it does:
- Name the rule that's at risk.
- Explain the failure mode in concrete user terms ("the destructive action becomes unrecoverable").
- Offer one alternative that preserves the user's intent.
- If they still want it, do it. The hard rules are guidance, not gates.

## References

- Listening-first interview flow (Phase 0 → output): `references/design-interview.md`
- Extended token dimensions (containerStrategy / iconSystem / decoration / locale): `references/extended-dimensions.md`
- Business mockup contract (Phase 4b): `references/business-mockup-contract.md`
- Style family catalog: `references/style-families/index.md`
- Per-family details: `references/style-families/<family>.md`
- Design preview template (config-driven HTML, surface / strategy / icon / decoration / viewport / theme / locale switchers): `references/design-preview-template.html`
- `design-spec.md` output template: `references/design-spec-template.md`
- System-level principles: `references/system-principles.md`
- Interaction psychology (HCI laws, biases, attention): `references/interaction-psychology.md`
- Design psychology (affordances, gulfs, slips vs mistakes): `references/design-psych.md`
- Icon rules: `references/icons.md`
- Review output template: `references/review-template.md`
- Per-surface checklists: `references/checklists.md`
