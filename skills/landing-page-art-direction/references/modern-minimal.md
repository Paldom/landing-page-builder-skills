# Modern-minimal, by the numbers

Reference for `landing-page-art-direction`. The default direction when the
brief is quiet — specified numerically because "clean and simple" without
numbers produces empty flat pages. Minimal is precision, not absence.

**Contents:** [Budgets](#budgets) · [Token skeleton](#oklch-token-skeleton) ·
[Anti-boring mechanisms](#anti-boring-mechanisms) · [Motion](#motion-numbers) ·
[What minimal bans — and permits](#what-minimal-bans--and-permits) ·
[Provenance](#provenance)

## Budgets

- **Spacing:** 4px grid, generous at section level — ~96px (6rem) between
  major sections, 24–40px inside cards. Whitespace does the separating;
  if a shadow is needed to see a card, add whitespace instead.
- **Type scale:** 1.2–1.25 ratio, ≤5 sizes per page. Body ≥16px,
  line-height 1.5–1.65; display line-height 1.05–1.2, tracking −0.02 to
  −0.035em; measure 45–75ch (65ch default). Heading-to-body weight gap
  ≥300 units. Display caps around 5.5rem — minimal heroes are confident,
  not shouting.
- **Palette:** two neutrals + one ink + one accent. Accent on links and the
  primary CTA only (~3–4 appearances). Pure `#fff` paper is *allowed* in
  strict minimal (elsewhere it reads clinical); ink stays near-black
  (`oklch(0.15–0.2 …)`), never `#000`.
- **Radius:** one system — 6–8px controls, 8–12px cards, or a committed 0px
  sharp system. Never mixed, never oversized-uniform on everything.
- **Borders:** hairlines — `1px` at low contrast (`oklch(0.91 0 0)` on white)
  — or none; separation by rhythm, not boxes.

## OKLCH token skeleton

CSS-first (Tailwind v4 `@theme`) starting point — near-neutral single-hue,
chroma ≤ 0.025 for every neutral, saturation reserved for one accent and
destructive states:

```css
@theme {
  --color-background: oklch(1 0 0);
  --color-foreground: oklch(0.145 0.02 var(--hue));
  --color-muted-foreground: oklch(0.46 0.015 var(--hue));
  --color-border: oklch(0.91 0.005 var(--hue));
  --color-primary: oklch(0.205 0.02 var(--hue));
  --color-primary-foreground: oklch(0.985 0.005 var(--hue));
  --color-accent: /* the ONE saturated choice, chroma 0.12-0.22 */;
  --color-accent-foreground: /* verified >= 4.5:1 against accent */;
}
```

Dark mode = flip lightness in `.dark {}` (fg 0.145 ↔ 0.985), lower accent
chroma slightly. Keep every `--x`/`--x-foreground` pair resolvable to literal
values so `ux-guardrails` can verify contrast mechanically.

## Anti-boring mechanisms

Minimal fails as "sterile and unfinished" when it's only subtraction. Pick
**one or two**, not all:

1. **Typographic contrast as the interest.** Three distinct roles — display,
   body, mono for meta/data — from ≤2 families (mono may be the third). The
   hierarchy jump *is* the visual event.
2. **One signature element.** A single place the page is remembered by: an
   oversized typographic hero, one unexpected layout move, one bold color
   block. Spend boldness exactly once; everything else stays quiet.
3. **Quiet depth.** Texture at the threshold of perception: grain overlay at
   0.03–0.05 opacity, one warm radial light at 0.03, or real photography
   desaturated — never gradients-as-decoration, never glassmorphism.
4. **Materially real imagery.** One in-situ product screenshot or photograph
   doing narrative work beats any abstract blob. Crop to a calm tonal area if
   text sits on it.
5. **Restrained motion as polish** (see below) — a 12px fade-rise on entry
   reads as craft without becoming a look.

## Motion numbers

Restrained character by default: entrance = translateY(12px) + fade over
~600ms ease-out for content blocks, UI feedback ≤ 200–300ms, stagger
30–80ms, `cubic-bezier(0.16, 1, 0.3, 1)` as the house ease-out, everything
inside `prefers-reduced-motion` guards. No bounce, no parallax, nothing
infinite except loaders.

## What minimal bans — and permits

Bans (these read as templated minimal): uniform rounded cards in equal grids,
centered hero + eyebrow pill, thin-line icon walls, gradient text, glass
panels, decorative dots and section-number eyebrows, italic headings.
Permits (that stricter styles ban): pure-white paper, zero-chroma neutrals,
a committed 0-radius system, pages with almost no images when the typography
carries.

## Provenance

Distilled (mid-2026) from converging public specs: the hallmark
"modern-minimal" genre rules and gate thresholds; the most-starred minimalist
taste skills (Notion-register minimalism: hairline borders, pastel-spot
accents, depth mandates); the effective-ui-design ruleset (8pt grid, 1.2
scale, contrast floors); Tailwind-v4 CSS-first OKLCH token architecture; and
Emil Kowalski's motion standards. Values were cross-checked across sources;
where they disagreed, the WCAG-safe or lower-variance value was kept.
