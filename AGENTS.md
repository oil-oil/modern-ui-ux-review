# oiloil-ui-ux-guide Agent Instructions

This repository uses `AGENTS.md` as the shared instruction entry for multi-agent tools.

## Scope

- Applies to the whole repository.
- Used when maintaining `skills/oiloil-ui-ux-guide/SKILL.md`, `README.md`, `index.html`, reference files, and the style-family library.

## Repository goals

- Keep this Skill **style-neutral**. It is not a "modern minimal" advocacy tool — it's a consultation tool that supports multiple style families.
- Keep outputs implementable, not generic slogans.
- Keep design guidance applicable across products of very different audiences (B2B SaaS, education, hospitality, indie tools, etc.).

## Two-layer rule architecture (do not collapse this)

The skill maintains a strict separation:

- **UX Hard Rules** — perceptual / cognitive / task-level facts that hold across all visual styles. Examples: task-first hierarchy, state coverage, affordance, error prevention. These are global.
- **Style Lens** — a chosen "style family" supplies font / color / spacing / radius / shadow / motion defaults *for that family only*. Each family has its own anti-patterns.

When editing the skill, **never move a Style Lens preference into the Hard Rules layer**, and never let one family's anti-patterns appear as a global rule. Specific things that belong in style families and not in hard rules:

- Font allow/deny lists (Inter vs Plus Jakarta vs Spectral)
- Pure black / pure white usage
- OKLCH vs HSL preference
- Specific radius ceilings or floors
- Bounce / no-bounce motion
- Lucide / Phosphor / Heroicons specifically

## Behavioral rules for the consultant role

- **Listen before recommending.** Open with questions, not opinions. Recommendations only when asked.
- **Neutral options.** When presenting choices, no starred recommendation unless the user asks "what do you think?".
- **No loaded labels** ("premium" / "efficient" steers the answer; use neutral descriptors and concrete references).
- **One question at a time** with a default the user can accept silently.
- **Defer to the user's stated preference**, unless it violates a UX Hard Rule. Even then, name the rule and offer one alternative — don't block.
- **Do not impose a style family** the user hasn't chosen.

## Content rules

- Keep the term `Skill` in docs and UI copy unless a request explicitly asks otherwise.
- Default docs language is Simplified Chinese, with minimal unnecessary English mixing in user-facing docs (READMEs, install guides). The internal SKILL.md and reference files are in English to keep the agent prompt in one language.
- Do not leak internal style constraints into end-user product copy.
- Avoid overfitting guidance to one page title or one business scenario.

## UX rules to preserve (these are the Hard Rules)

1. Task-first hierarchy and primary CTA clarity
2. State coverage (loading / empty / error / success / permission)
3. Affordance + signifier
4. Error prevention + recoverability
5. Feedback loop closure
6. Project-internal consistency
7. CRAP visual hierarchy
8. Spacing scale discipline
9. Help text layering L0–L3
10. UI copy source discipline

## Editing rules

- Keep updates small and coherent; avoid broad rewrites without clear value.
- When changing `skills/oiloil-ui-ux-guide/SKILL.md`, make sure `README.md` and `index.html` stay aligned.
- When adding a new style family, write a full `references/style-families/<name>.md` covering: signature, references, best for, token defaults (color/typography/radius/spacing/shadow/motion), anti-patterns within the family, typical surfaces, notes. Update `style-families/index.md`.
- When adding a new UX Hard Rule, justify it with a perceptual/cognitive/task-level argument. If the justification is "I think it looks better", it belongs in a style family, not in Hard Rules.
- When changing the preview template, keep the chrome neutral — the template should never compete with the design being previewed.
