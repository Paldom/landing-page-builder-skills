---
name: landing-page-accessibility
description: Makes a marketing landing page WCAG 2.2 AA accessible - semantic HTML over ARIA, color contrast, visible focus, touch-target size, labeled forms with linked errors, no overlay widgets, reduced motion. Use when asked to make a page accessible, fix WCAG/a11y issues, run an accessibility audit, or support screen readers and keyboard users. Not for copy, design, page speed/SEO, or A/B testing.
---

# landing-page-accessibility

Native semantic HTML first, automated scanners as a floor, a real manual pass
before ship.

## The failure this fixes

AI website builders scaffold a page in seconds, but the output is an
"accessibility black box": div-soup with no semantic landmarks, silent keyboard
traps, DOM/focus order that doesn't match the visual order, hallucinated or
generic alt text (`alt="image"` passes a scanner but fails the requirement), and
either missing or *over*-applied ARIA. The governing correction — verbatim in the
W3C ARIA spec — is **"No ARIA is better than bad ARIA"**: pages *with* ARIA
average more errors than pages without, because devs reach for
`<div role="button">` and never wire up the keyboard, focus, and state that
`<button>` gives for free. And **accessibility overlays are a liability, not a
fix.**

## When to use / when NOT to use

Use for WCAG 2.2 AA compliance on a marketing page: semantics, contrast, focus,
target size, forms, keyboard, reduced motion, and the legal drivers.

Not for: copy (`landing-page-copywriting`, though plain language helps here),
palette/type choices (`visual-design-system` — this skill owns *contrast
compliance*, not the palette), page speed/SEO (`web-vitals-and-seo`), motion
internals (`scroll-motion` — this skill owns the reduced-motion *requirement*),
Next.js implementation (`nextjs-landing-page`), or testing
(`landing-page-experimentation`).

## Workflow

1. **Semantic HTML before ARIA.** Use `<button>`, `<a>`, `<nav>`, `<main>`,
   `<header>`, `<footer>`, `<label for>` — not ARIA'd divs. Semantic HTML alone
   delivers ~80% of accessibility value. ARIA only where no native element
   expresses the behavior (live regions, custom tabs/accordions/dialogs,
   `aria-label` on icon-only buttons). Full rule set: `references/wcag-checklist.md`.
2. **Structure and contrast.** One `<h1>`, sequential headings; contrast 4.5:1
   normal text, 3:1 large text and non-text UI; visible `:focus-visible`
   indicators clearing 3:1 (the shadcn default subtle ring commonly fails this).
3. **Interactive targets and forms.** Target size ≥ 24x24 CSS px (44x44
   recommended). Every input has a real `<label>` (placeholder is not a label);
   errors in text, inline near the field, *and* summarized in a linked
   `role="alert"` region with focus moved to it on submit. Mark optional fields;
   WCAG 2.2 adds 3.3.7 Redundant Entry and 3.3.8 Accessible Authentication (no
   puzzle CAPTCHAs).
4. **Keyboard, alt text, motion.** Tab through the whole page (logical order, no
   traps, skip link); meaningful alt for content images, `alt=""` for decorative;
   honor `prefers-reduced-motion` (reduce, don't delete) and provide pause for
   anything auto-playing over 5s.
5. **Verify beyond the scanner.** axe-core / Lighthouse a11y catch only ~30-40%
   of real issues — a **floor, not proof**. Do one keyboard-only walkthrough + one
   screen-reader spot check (NVDA/VoiceOver) before ship; gate axe-core/Pa11y in
   CI. **Never install an overlay widget.**
6. **State legal drivers as version-gated.** EAA and ADA dates drift — see
   `references/wcag-checklist.md` and verify against the authoritative source.

## The rules

- **Native element over ARIA.** "No ARIA is better than bad ARIA." Don't put
  `aria-label` on something that already has visible text.
- **Contrast 4.5:1 / 3:1**, visible focus, never `outline: none` without a
  replacement.
- **Target size ≥ 24x24 CSS px** (2.5.8), 44x44 for real touch comfort.
- **Real labels + inline AND summarized-linked errors;** mark optional, not
  required; no color-only signaling.
- **Automated scans are a floor (~30-40% coverage)** — a 100/100 is a temperature
  check, not compliance. Manual keyboard + screen-reader testing is mandatory.
- **No overlay widgets** — they patch cosmetically, break assistive tech, and are
  a lawsuit/FTC red flag.
- **The accessibility tree is also the machine substrate** screen readers,
  crawlers, and AI agents read — so semantic HTML doubles as discoverability
  infrastructure (a byproduct of clean code, *not* a confirmed ranking factor;
  don't ARIA-stuff for agents).

## Output

A page that passes WCAG 2.2 AA on semantics, contrast, focus, target size, forms,
keyboard, and reduced motion; a CI axe-core gate plus a documented manual
keyboard + screen-reader pass; no overlay widget; and legal-deadline claims
version-gated against the authoritative source.

## References

- `references/wcag-checklist.md` - the full WCAG 2.2 AA rule list for marketing
  pages, the ARIA rules, the overlay case, the EAA/ADA legal drivers (version-
  gated), and the fact ledger.
