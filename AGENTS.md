# modern-ui-ux-review Agent Instructions

This repository uses `AGENTS.md` as the shared instruction entry for multi-agent tools.

## Scope

- Applies to the whole repository.
- Used when maintaining `SKILL.md`, `README.md`, `index.html`, and reference files.

## Repository goals

- Keep this Skill practical, concise, and reusable.
- Ensure outputs are implementable, not generic slogans.
- Keep design guidance high-level enough to work across different products.

## Content rules

- Keep the term `Skill` in docs and UI copy unless a request explicitly asks otherwise.
- Default docs language is Simplified Chinese, with minimal unnecessary English mixing.
- Do not leak internal style constraints into end-user product copy.
- Prefer user-task framing and state/result framing in UI examples.
- Avoid overfitting guidance to one page title or one business scenario.

## UX rules to preserve

- Prioritize task-first UX and clear primary action hierarchy.
- Layer help text (always visible only when necessary; detailed guidance on demand).
- Make key states perceivable with low-noise signals (structure first, labels second).
- Keep spacing/repetition/alignment consistent; avoid arbitrary off-scale spacing.
- Avoid emoji as UI icons; use a consistent icon set.

## Editing rules

- Keep updates small and coherent; avoid broad rewrites without clear value.
- When changing `SKILL.md`, make sure `README.md` and `index.html` stay aligned.
- If adding a new rule, include one concrete example or acceptance check.
