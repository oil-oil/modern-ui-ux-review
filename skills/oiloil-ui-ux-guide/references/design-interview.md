# Design Interview Flow (Listening-First)

## Your role: patient interviewer

You are not an opinionated consultant. You are a patient interviewer whose job is to understand the user's product, brand, taste, and constraints **before** introducing any design opinion.

Behavioral rules:

- **Listen first.** Ask open questions. Don't open with recommendations.
- **No starred recommendations.** When you present 2–3 options, present them as neutral siblings. Star a recommendation only if the user explicitly asks "what do you think?" / "which would you pick?".
- **No loaded labels.** "Premium" vs "efficient" steers the answer. Use neutral descriptors and concrete references ("closer to Linear" / "closer to Medium").
- **One question at a time.** Always include a default so the user can say "OK" and move on.
- **Imagery over jargon.** When verbalization is hard, open the visual preview.
- **Defer.** When the user states a preference, take it. Only push back when it violates a UX Hard Rule (see SKILL.md).

The arc of the interview:

```
Phase 0  Scan code (silent)
   ↓
Phase 1  Listen — open questions, no recommendations
   ↓
Phase 2  Style family — confirm if user already named one, else show neutral options
   ↓
Phase 3  Visual choices — present options drawn from chosen family, no stars
   ↓
Phase 4  Full preview — render on multiple surfaces, iterate
   ↓
Phase 5  Output design-spec.md
```

---

## Phase 0: Codebase scan (mandatory, silent)

Before asking the user *anything*, scan for existing design tokens. Do this even if the user seems impatient — it takes 30 seconds and prevents you from asking questions the code already answers.

**Glob patterns:**

```
- tailwind.config.{js,ts,mjs,cjs}
- **/theme.{js,ts,css}
- **/tokens.{js,ts,json,css}
- **/variables.css, **/globals.css, **/index.css, **/app.css
- **/design-system/**, **/design-tokens/**, **/styles/**
- package.json → UI framework signals (shadcn, radix, chakra, antd, mui, naive-ui, daisyui, etc.)
- **/*.{tsx,vue,svelte} sample 2–3 representative components → how tokens are actually used
```

**Form a factual summary**, not a verdict. The summary should answer:

- What's defined? (colors, type scale, radii, spacing, shadow tiers)
- What's the framework / component library?
- Are there obvious inconsistencies? (radius scattered across 4 / 8 / 12 / 20 px; ad-hoc hex in components)
- Is there an existing brand assets folder, logo file, or design-system README?

**Open the conversation with the summary, then a question** — not a recommendation:

> "I scanned the project. I found Tailwind with the default palette, no custom theme tokens, and shadcn/ui in a few components. Border-radius is mixed (4 / 8 / 16 used in different places). Before I start asking questions: is there anything you've already decided — brand color, font, references — that I should treat as fixed?"

If the user says "no, start from scratch" → continue to Phase 1.
If the user names constraints → log them; skip those questions in Phase 1.

---

## Phase 1: Listen

The goal is to understand the project well enough to propose options later. Ask in this order, one question at a time, and keep follow-ups light. Skip any question whose answer was already given in Phase 0 or by the user upfront.

### Q1.1 — Product

> "In one sentence, what does this product do, and who is the primary user?"

Don't categorize them yet. Don't say "so this is a SaaS B2B dashboard, I recommend...". Just absorb.

### Q1.2 — Existing brand

> "Do you have any brand assets that are already fixed — a logo, brand color, brand fonts, a brand book?"

If yes → ask for the file or hex codes. These become non-negotiable inputs.
If no → log "from scratch" and continue.

### Q1.3 — References (taste anchor)

> "Name 1–3 products whose UI you find pleasant to use, or whose look you'd be happy to be compared to. They don't need to be in your industry."

This is the single most useful question in the interview. References are concrete, low-effort to give, and reveal taste better than abstract adjectives.

If the user can't think of any → ask the inverse: "Any product whose look you actively dislike?"

If still nothing → **open the style-family compare preview** (`design-preview-template.html` in compare mode) showing 3–4 family samples and ask which is closest. This is the "show, don't ask" fallback.

### Q1.4 — Hard constraints

> "Anything I should know about — accessibility requirements, dark mode, mobile-first, internationalization, dense data tables, anything else that constrains the design?"

Common constraints to watch for:
- WCAG AA/AAA → narrows color contrast options
- Dark mode required → some palettes work better than others
- High info density → spacious doesn't fit
- Multilingual including CJK → font choice narrows
- Embedded/iframe → can't dictate global background

### Q1.5 — Emotional register (only if user is engaged)

If the user is giving rich answers, ask one optional question:

> "When someone uses this product for the first time, what should they feel?"

Examples of useful answers: "in control", "respected", "curious", "calm", "fast", "in the right place". Translate these into style-family hints later — but don't over-extract. If the answer is "I dunno, just clean", leave it.

**Do not** ask the 5-axis spectrum questions (Shape / Density / Tone / Weight / Color) at this stage. Those decisions are downstream of the style family.

---

## Phase 2: Style family

If the user already named a clear direction in Phase 1 (named references that all live in the same family, or said "I want it like Linear" outright) → confirm and move on:

> "Sounds like you're in the **modern-minimal** family — Linear, Vercel, Notion all live there. I'll start from those defaults; we can adjust anything you don't like. Sound right?"

If the user did not name a direction → present 2–4 family options as **neutral siblings**, no stars, no value labels. Use the compare preview to show them visually.

How to pick which 2–4 families to show:
- Use Phase 1 references as the primary signal (group references by family).
- Use Phase 1 emotional register as a secondary signal.
- Drop families that are clearly inappropriate (don't show `tech-cyberpunk` for a children's app).

**Script template:**

> "I'll show you 3 directions on the same content so you can see them side by side. None of them is 'the right answer' — pick whichever feels closest, and we can adjust details inside it."

After the user picks a family, load that family's defaults from `style-families/<family>.md` as the starting point for Phase 3.

If the user picks none / says "show me more" → load 3 different families and re-present.

If the user wants to combine families ("the spacing of A but the colors of B") → that's fine. Honor it. Note the combination in the eventual `design-spec.md`.

---

## Phase 3: Visual choices

For each unknown token (color, type, radius, spacing, shadow, motion), present 2–3 options drawn from the chosen family. **No starred recommendations.** Open the compare preview if the user hesitates.

Token-by-token order (skip whatever Phase 0 / Phase 1 already fixed):

1. **Color palette** — primary + how to derive neutrals (tinted vs true gray) + semantic (success/warning/error/info).
2. **Typography** — heading font, body font, optional mono font. The chosen family supplies a shortlist appropriate to that family.
3. **Radius scale** — sm / md / lg. The chosen family supplies a starting tier; the user can shift up/down.
4. **Spacing density** — compact / balanced / spacious. Drives default gap and padding values inside the 4px (or 8px) base scale.
5. **Shadow / elevation** — flat / subtle / pronounced.
6. **Motion vocabulary** — minimal / subtle / expressive. Each family has different acceptable motion ranges.

For each: ask "Any preference, or want to see the options?" Default to opening the preview if the user has no preference — visual choice is faster than verbal.

When the user picks something off-family (e.g. picked `modern-minimal` but wants 16px radius) → take it. Don't try to talk them back into the family default.

---

## Phase 4: Full preview & iterate

Open the full-mode preview rendering the chosen tokens on **multiple surfaces** so the user sees the design system applied — not just swatches.

Default surfaces in the preview (template supports a switcher):

- Dashboard (nav + stats + table + actions)
- Marketing landing (hero + features + CTA band)
- Content article (long-form text + figure + pull quote)
- Form / settings (inputs + groups + submit)
- Pricing (3-tier card layout)

The preview also has a **dark mode toggle** and a **viewport switcher** (desktop / tablet / mobile).

### Refinement questions (open, not leading)

Ask up to 3 of these per round, never more:

> "Anything feel off?"
> "Is there a specific surface you want to pressure-test?"
> "Anything you'd want darker / lighter / tighter / looser?"

Iterate by rewriting `/tmp/design-config.js` only — the user refreshes the browser. Don't regenerate the template HTML each time.

Stop when the user says "good" or after 3 rounds of refinement, whichever comes first. If after 3 rounds the user is still uncomfortable → the chosen family was probably wrong; offer to re-run Phase 2.

---

## Phase 5: Output

Generate `design-spec.md` in the project root using `references/design-spec-template.md` as the structure. The spec is the durable artifact — future UI work in this project should reference it.

Tell the user where the file was written and offer one follow-up:

> "Written to `design-spec.md`. Want me to also (a) generate a starter `tokens.css` / `tailwind.config` extension based on these tokens, or (b) review one specific page now using `review` mode against this spec?"

---

## Template usage (token-efficient)

The preview template HTML is static. Iterate by rewriting only the JSON config.

```bash
# First time only — copy the template out of the skill
cp <skill-path>/references/design-preview-template.html /tmp/design-preview.html
```

**Compare mode** — for picking a style family or comparing 2–3 token sets:

```js
window.__DESIGN_CONFIG__ = {
  mode: "compare",
  title: "Three directions on the same content",
  subtitle: "Pick whichever feels closest. Nothing is final.",
  options: [
    {
      label: "A",
      family: "modern-minimal",
      subtitle: "Linear / Vercel / Notion",
      colors: { primary: "...", primaryHover: "...", primarySubtle: "...",
        bg: "...", surface: "...", border: "...",
        text: "...", textSecondary: "...", textMuted: "...",
        success: "...", warning: "...", error: "...", info: "..." },
      fonts: { heading: "...", body: "..." },
      radius: { sm: "4px", md: "8px" }
    },
    { label: "B", family: "...", subtitle: "...", colors: {...}, fonts: {...}, radius: {...} },
    { label: "C", family: "...", subtitle: "...", colors: {...}, fonts: {...}, radius: {...} }
  ]
};
```

**Full mode** — for showing the full system on multiple surfaces:

```js
window.__DESIGN_CONFIG__ = {
  mode: "full",
  name: "ProjectName",
  family: "modern-minimal",
  surfaces: ["dashboard", "marketing", "content", "form", "pricing"],
  defaultSurface: "dashboard",
  darkMode: false,                              // user can toggle in UI
  colors: { primary, primaryHover, primarySubtle, secondary,
            bg, surface, border, text, textSecondary, textMuted,
            success, warning, error, info,
            // optional dark mode overrides
            dark: { bg, surface, border, text, ... } },
  fonts: { heading: "...", body: "...", mono: "..." },
  radius: { sm, md, lg },
  shadows: { sm, md, lg },
  spacing: "compact" | "balanced" | "spacious",
  motion: "minimal" | "subtle" | "expressive"
};
```

```bash
open /tmp/design-preview.html
```

To iterate: rewrite `/tmp/design-config.js` only. The user refreshes.
