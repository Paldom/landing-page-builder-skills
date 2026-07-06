# WCAG 2.2 AA Checklist for Landing Pages

Reference for `landing-page-accessibility`. Target **WCAG 2.2 AA**; WCAG 2.1 AA is
the more conservative legal floor most laws still cite; WCAG 3.0 is draft, not a
compliance target.

**Contents:** [Semantics & ARIA](#semantics--aria) · [Contrast & focus](#contrast--focus) ·
[Targets & forms](#targets--forms) · [Keyboard, alt, motion](#keyboard-alt-motion) ·
[Verification & overlays](#verification--overlays) · [Legal drivers](#legal-drivers-version-gate) ·
[Fact ledger](#fact-ledger) · [Sources](#sources)

## Semantics & ARIA

- Native semantic HTML before ARIA: `<button>`, `<a href>`, `<nav>`, `<main>`,
  `<header>`, `<footer>`, `<ul>`, `<label for>`. Delivers ~80% of a11y value.
- **"No ARIA is better than bad ARIA"** (W3C). Pages with ARIA average *more*
  detected errors because `<div role="button">` skips the free keyboard/focus/
  state of `<button>`. Use ARIA only where no native element fits: live regions
  (`aria-live`), custom tabs/accordions/dialogs (follow the ARIA APG patterns),
  `aria-label` on icon-only controls.
- Don't put `aria-label` on an element that already has visible text — it
  overrides it.
- One `<h1>`; sequential H2/H3 with no skipped levels (screen-reader users
  navigate by heading).

## Contrast & focus

- Contrast: **4.5:1** normal text, **3:1** large text (≥18px, or ≥14px bold) and
  non-text UI (icons, borders, focus rings, chart strokes).
- Visible focus: use `:focus-visible`; never `outline: none` without a
  replacement; the ring must clear **3:1** against adjacent colors (the
  shadcn/default subtle ring commonly fails this).

## Targets & forms

- **Target size 2.5.8:** minimum **24x24 CSS px** (the interactive hit area, not
  the visible glyph — padding counts); 44x44 recommended for real touch. Exception:
  undersized targets pass if a 24px circle centered on each doesn't intersect
  another; inline text links are exempt.
- Every input has a real associated `<label>` — a placeholder is **not** a label.
  Hints via `aria-describedby`.
- Errors: in **text**, **inline near the field**, AND **summarized in a linked
  `role="alert"` region** with focus moved to it on submit. Field-level-only errors
  are a top audit failure.
- Mark **optional** fields (don't rely on color for required). WCAG 2.2 adds
  **3.3.7 Redundant Entry** (don't re-ask for data already given this session) and
  **3.3.8 Accessible Authentication** (no cognitive-function-test logins / puzzle
  CAPTCHAs — use honeypot or invisible verification).

## Keyboard, alt, motion

- Tab through the whole page: logical order matching the DOM, no traps; provide a
  skip-to-content link.
- Alt text: meaningful description for content images, `alt=""` for decorative; no
  "image of…" prefixes, no AI filler.
- Honor `prefers-reduced-motion` — **reduce** (swap movement for opacity), don't
  delete all feedback. Separately, **2.2.2** requires pause/stop for anything
  auto-playing or looping over 5s; reduced-motion alone doesn't satisfy it.
  (Motion implementation is `scroll-motion`.)
- Plain-language copy aids cognitive access and scanning (wording is
  `landing-page-copywriting`).

## Verification & overlays

- **axe-core / Lighthouse a11y are a floor (~30-40% coverage; axe-core ~57% at
  best).** A 100/100 is a temperature check. They can't judge whether alt text is
  meaningful, focus order logical, or an error message helpful.
- **Mandatory manual pass:** one keyboard-only walkthrough + one screen-reader
  spot check (NVDA/VoiceOver). Manual testing routinely finds far more than
  automated alone. Gate axe-core/Pa11y/Lighthouse CI so regressions fail the
  build — but treat the gate as the floor.
- **No overlay widgets** (accessiBe, EqualWeb): they patch the visual layer
  cosmetically, fix only ~15-40%, break real assistive tech, and are a plaintiff/
  FTC red flag (the FTC fined a major overlay vendor for deceptive claims).
- The accessibility tree is the shared machine-readable substrate for screen
  readers, crawlers, and AI agents — semantic HTML doubles as SEO/GEO
  infrastructure, but this is a byproduct, *not* a confirmed Google ranking
  factor. Don't ARIA-stuff for agents.

## Legal drivers (version-gate)

Dates and cited WCAG versions drift — **verify before repeating:**

- **EU — European Accessibility Act (EAA):** enforceable since **June 28, 2025**
  for B2C digital services, e-commerce, and banking sold into the EU (existing
  services have a grace period to ~June 28, 2030). Standard: EN 301 549 → WCAG 2.1
  AA.
- **US — ADA Title II:** codifies WCAG 2.1 AA for state/local government; compliance
  deadlines land ~mid-2026 (largest entities ~April 2026; smaller extended). ADA
  Title III continues to drive private-sector web lawsuits with WCAG as the
  de-facto benchmark.

## Fact ledger

- **STABLE:** semantic-HTML-first / first rule of ARIA; "no ARIA is better than
  bad ARIA"; contrast 4.5:1 & 3:1; target size 24x24 min / 44x44 recommended;
  automated tools ~30-40% coverage; overlay backlash (harmful, FTC-fined); manual
  keyboard + screen-reader testing required; accessibility tree = shared substrate.
- **DRIFT-PRONE (version-gate):** EAA date (June 28 2025) & 2030 grace; ADA Title
  II deadlines; which WCAG version each law cites; WebAIM Million year figures;
  Lighthouse "Agentic Browsing".
- **UNVERIFIABLE-STAT (do not assert as fact):** "95.9% / 96.3% of homepages fail
  WCAG"; "readable copy converts ~2x"; "14/48 shadcn components fail contrast";
  lawsuit-count and fine specifics from vendor blogs.

## Sources

- W3C WCAG 2.2: https://www.w3.org/TR/WCAG22/
- W3C — Understanding 2.5.8 Target Size (Minimum): https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- MDN — ARIA (the "first rule"): https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA
- MDN — HTML: a good basis for accessibility: https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/HTML
- WebAIM Million (ARIA-misuse dataset): https://webaim.org/projects/million/
- W3C ARIA Authoring Practices Guide: https://www.w3.org/WAI/ARIA/apg/
- ADA.gov — Title II Web Rule: https://www.ada.gov/resources/2024-03-08-web-rule/
- EU EAA (Directive 2019/882): https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32019L0882
