---
name: visual-design-system
description: Sets up a landing page's visual design system and keeps AI output on-brand - palette/type/spacing budgets that avoid generic 'AI slop' aesthetics, plus a DESIGN.md (tokens-as-roles, a Don'ts list) and lint/visual/a11y enforcement gates. Use when asked to choose colors, fonts, or visual style, stop AI design sameness, or set up design tokens. Not for copy, layout structure, motion, or SEO.
---

# visual-design-system

Give a landing page a distinctive, on-brand look — and make the agent actually
hold to it.

## The failure this fixes

AI generators converge on one identical aesthetic: Inter/Poppins type, a
blue-to-purple (indigo/aurora) gradient hero, glassmorphism cards, a centered
`mx-auto` hero with an "eyebrow pill", a three-card feature grid, and the default
shadcn fingerprint — "AI slop", the "sea of sameness". It gets *worse* under
iteration: "make it premium" or "be distinctive" ratchets a model **toward** the
average, because it reads "distinctive" as "add more". And prose brand rules are
followed only probabilistically and silently dropped under deadline — banning a
color in writing has been observed to fail repeatedly while a lint rule fixed it
first try. So this skill does two things: **choose constrained, distinctive
budgets**, and **encode + enforce them mechanically.**

## When to use / when NOT to use

Use to choose palette / type / spacing, kill the generic AI look, and set up a
machine-readable design system (DESIGN.md, tokens, lint gates) that agents obey.

Not for: the words (`landing-page-copywriting`), section skeleton
(`landing-page-structure`), animation (`scroll-motion`), building components
(`nextjs-landing-page`), full WCAG compliance work (`landing-page-accessibility`
— this skill treats contrast only as a floor), or ranking/perf
(`web-vitals-and-seo`).

## Workflow

1. **Set the aesthetic budget** (hard constraints beat vibes). Palette: ≤4 roles
   — two neutrals + one ink + one accent, accent reserved for links/primary CTA
   only. Warm off-white background (`#fafaf9`), near-black ink (`#1c1917`) — never
   pure `#fff`/`#000`. Type: 1-2 typefaces, ≥16px body floor. Spacing: 8px scale,
   ~96px between sections. Show the real product, not stock. Full budgets +
   the ban-list: `references/aesthetic-budgets.md`.
2. **Ban the slop tells explicitly.** Write down what NOT to do (blue→purple
   gradients, glassmorphism, sparkle icons, three equal-weight cards, centered
   eyebrow-pill hero, marquee logo strips) — a Don'ts list built from bad output
   you have actually seen is the single highest-leverage artifact.
3. **Encode it as DESIGN.md.** Machine-readable YAML tokens + prose rationale,
   tokens defined as **roles not raw values**, checked into the repo root beside
   AGENTS.md. Structure, the 8 sections, and the tokens-vs-prose split:
   `references/design-md-and-enforcement.md`. DESIGN.md is a fast-converging 2026
   convention (Google Labs, alpha), not a ratified standard — verify against the
   current spec.
4. **Enforce with deterministic gates.** Prose is advisory; a failing lint rule
   is not. Stack: ESLint `no-restricted-imports` + custom `no-arbitrary-colors`;
   Stylelint `declaration-strict-value`; TypeScript discriminated unions for
   variants; visual-regression (Playwright `toHaveScreenshot()`); axe-core in CI.
   Cap the agent's self-correction loop and forbid it editing the lint config.
   Details: `references/design-md-and-enforcement.md`.

## The rules

- **Constrain the brand hard; keep creative exploration separate.** Over-specify
  the *brand tokens* (palette, type, spacing) but do not over-specify the
  *creative direction* — heavy prompt specification can suppress a model's design
  strengths and produce *more* monotonous output. Constrain what must be
  consistent; leave room to explore what should be distinctive.
- **Tokens define roles, not values.** "primary" is "the main ink color of the
  page", not a hex. Every consumer references the role, so a rebrand is a
  one-line edit. Tokens carry *what*; prose carries *when/why*. A pure token dump
  still yields off-brand output.
- **The Don'ts list is the highest-leverage section** — one "don't" per real bad
  output.
- **Enforcement beats persuasion.** Consistency you enforce survives the agent;
  consistency you request survives until the agent is in a hurry. The agent never
  opens Figma — it reads the repo, so the governing spec is tokens-as-code plus a
  gate it cannot pass without obeying.
- **Layer the instruction files.** Behavioral rules → AGENTS.md/CLAUDE.md; visual
  spec → a dedicated DESIGN.md loaded only when generating UI (avoids paying the
  token tax on every turn). Keep root instruction files lean.
- **Contrast floor (defer depth to accessibility skill):** body text 4.5:1,
  large text and non-text UI 3:1 — a hard gate, not a preference.

## Output

A defined palette/type/spacing budget with an explicit ban-list; a DESIGN.md with
role-based tokens, prose rationale, and a Don'ts section; and a wired enforcement
stack (lint + visual + a11y gates) the agent must pass — verified or version-gated
against the current DESIGN.md spec, never citing raw research paths.

## References

- `references/aesthetic-budgets.md` - concrete palette/type/spacing budgets, the
  warm-neutral token set, and the full AI-slop ban-list.
- `references/design-md-and-enforcement.md` - DESIGN.md structure and the 8
  sections, tokens-as-roles, authoring methods, Figma MCP/Code Connect, and the
  deterministic enforcement stack with the fact ledger.
