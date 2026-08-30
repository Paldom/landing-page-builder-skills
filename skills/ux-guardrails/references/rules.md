# ux-guardrails rule catalog

Reference for `ux-guardrails`. Every rule `scripts/ux_lint.mjs` enforces, with
exact triggers, thresholds, and provenance. Severity: **critical** blocks
(exit 2 / CI exit 1); **advisory** informs.

**Contents:** [Accessibility](#accessibility-critical-unless-noted) ·
[Spacing](#spacing) · [Typography](#typography) · [Color](#color) ·
[Slop tells](#slop-tells) · [Motion](#motion) · [Budgets](#cross-file-budgets) ·
[Config & ignores](#config--ignores) · [Provenance](#provenance)

## Accessibility (critical unless noted)

| id | fires on | why |
| --- | --- | --- |
| `a11y.viewport-zoom` | `user-scalable=no` or `maximum-scale=1` | WCAG 1.4.4 — never disable pinch zoom |
| `a11y.img-alt` | `<img>` with no `alt=` (spread-props `{...}` tags skipped) | screen readers announce the filename; `alt=""` if decorative |
| `a11y.focus-visible` | `outline-none`/`outline: none` in a file with no `focus-visible`, `focus:ring`, `focus:outline`, or `focus-within` anywhere | keyboard users lose the focus indicator entirely |
| `a11y.positive-tabindex` | `tabindex`/`tabIndex` ≥ 1 | breaks natural focus order; use 0 or restructure |
| `a11y.div-onclick` (advisory) | `<div>`/`<span>` with `onClick` and no `role`/`onKeyDown`/`tabIndex` in the tag | not keyboard-operable; prefer `<button>`/`<a>`. Advisory because wrappers sometimes delegate |

## Spacing

| id | severity | fires on |
| --- | --- | --- |
| `spacing.arbitrary` | advisory | Tailwind arbitrary spacing (`p-[13px]`, `gap-[7px]`, `mt-[22px]`… on p/m/gap/space/inset/top-style utilities) whose px value is > 2 and not a multiple of `spacingStep` (default 4) |
| `spacing.css-offscale` | advisory | `padding`/`margin`/`gap` CSS declarations containing a px value > 2 off the grid |

Standard Tailwind scale classes (`p-4`, `gap-6`) are deliberately never
flagged — the rule targets invented values, the highest-signal drift tell
("padding: 17px"), not the framework's own scale. Tighten via `spacingStep: 8`
if the project commits to a strict 8px grid.

## Typography

| id | severity | fires on |
| --- | --- | --- |
| `type.arbitrary-size` | advisory | `text-[15px]`-style arbitrary font sizes — bypasses the type scale |
| `type.tiny-text` | advisory | CSS `font-size` < 12px |
| `type.italic-heading` | advisory | `italic` class on `h1–h6` / CSS `font-style: italic` on heading selectors — a top AI tell |

## Color

| id | severity | fires on |
| --- | --- | --- |
| `color.raw-scale` | advisory | raw Tailwind palette classes (`bg-blue-500`, `text-neutral-500`, `from-purple-400`, all hues incl. grays) — semantic tokens (`bg-primary`, `text-muted-foreground`) are the shadcn canon |
| `color.raw-hex-class` | advisory | `bg-[#ff0000]`-style hardcoded hex in classes |
| `color.contrast-pair` | **critical** | any CSS custom-property pair `--x` / `--x-foreground` (plus `--background`/`--foreground`) in the same rule block, both resolvable to literal hex/rgb/hsl/oklch, with WCAG contrast < `contrastMin` (default 4.5:1) |

Contrast implementation: OKLCH → OKLab → linear sRGB (gamut-clamped), hex/rgb
gamma-decoded, relative luminance `0.2126R+0.7152G+0.0722B`, ratio
`(L1+0.05)/(L2+0.05)`. Values containing `var()`, `color-mix`, gradients, or
alpha are skipped silently — unresolvable ≠ failing.

## Slop tells

All advisory — heuristics about AI defaults, overridable when the brief
genuinely wants them ("the brief wins").

| id | fires on |
| --- | --- |
| `slop.gradient-text` | `bg-clip-text` / `background-clip: text` |
| `slop.purple-gradient` | `from/via/to-purple|violet|indigo|fuchsia-*` classes, or the known AI gradient hex set (`#7c3aed #8b5cf6 #a855f7 #9333ea #6d28d9 #667eea #764ba2 #6366f1`) inside a gradient |
| `slop.glassmorphism` | `backdrop-blur` combined with translucent fills (`bg-white/10`, `bg-opacity-*`) |

## Motion

| id | severity | fires on |
| --- | --- | --- |
| `motion.transition-all` | advisory | `transition-all` / `transition: all` — animates layout properties too |
| `motion.slow-transition` | advisory | transition durations > `transitionMaxMs` (default 500ms; ~200ms feels responsive). Animations (`@keyframes` durations) are exempt — ambient loops are legitimately long |
| `motion.ease-in-ui` | advisory | standalone `ease-in` (not `ease-in-out`, not `--ease-in` token definitions) — delays the visible start; enters want ease-out |
| `motion.reduced-motion` | advisory | a CSS file adding `@keyframes` with no `prefers-reduced-motion` in the same file — verify a global guard exists |

## Cross-file budgets

Run only in Stop/CLI mode (whole changeset visible):

| id | severity | fires on |
| --- | --- | --- |
| `type.family-budget` | advisory | more than `maxFontFamilies` (default 3) distinct first families across `font-family:` declarations — display + body + one utility face |

## Config & ignores

`.ux-guardrails.json` at the project root: `spacingStep`, `contrastMin`,
`maxFontFamilies`, `transitionMaxMs`, `ignoreRules` (rule ids), `ignoreFiles`
(path substrings). Inline: a `ux-guardrails-ignore <rule-id …>` comment on the
finding's line or the line above (no id = suppress all rules on that line).
WCAG floors (`contrastMin`, the a11y criticals) should never be relaxed —
config exists to fit the project's scale, not to lower the floor.

## Provenance

Thresholds distilled from public design-skill engines and standards
(mid-2026): WCAG 2.x contrast (4.5:1 / 3:1) and zoom rules; the pbakaus
"impeccable" detector engine (two-tier hook, per-edit mechanical vs Stop-time
taste, ignore-with-reason escape hatch, "the brief wins"); the nutlope
"hallmark" gate spec (4px spacing grid, token-only colors, italic-heading and
gradient-text tells); shadcn/ui skill canon (semantic tokens over raw palette
classes); Vercel web-interface-guidelines (`transition: all`, focus-visible,
zoom, img dimensions); Emil Kowalski's motion standards (sub-300–500ms UI,
ease-out enters). Where sources conflicted, the less false-positive-prone
variant won.
