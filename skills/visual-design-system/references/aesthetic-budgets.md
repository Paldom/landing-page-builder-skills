# Aesthetic Budgets & the AI-Slop Ban-List

Reference for `visual-design-system`. Concrete constraints that produce a
distinctive, on-brand look instead of the generic AI aesthetic. Fashion-cycle
labels ("warm minimalism") drift; the *budgets and the ban-list* are the durable
part.

**Contents:** [Palette budget](#palette-budget) · [Typography budget](#typography-budget) ·
[Spacing & layout budget](#spacing--layout-budget) · [The AI-slop ban-list](#the-ai-slop-ban-list) ·
[Sources](#sources)

## Palette budget

- **≤4 roles:** two neutrals + one ink + one accent.
- **Background:** warm off-white `#fafaf9` — never pure `#fff` (reads clinical).
- **Ink:** near-black `#1c1917` — never pure `#000` (needlessly harsh). Muted ink
  `#57534e` for secondary text (holds ~7.4:1 on the off-white).
- **Accent:** reserved for links + the primary CTA only, appearing ~3-4x per
  page. If it shows up ten times it stops meaning "act here".
- Prefer **opacity tiers** (100 / 65 / 40 / 10%) of ink/accent over inventing new
  brand colors.
- **The "safe green" trap:** ban indigo/blue-purple and models default to emerald
  — ban the substitute too, and name the accent you *do* want.
- Contrast is a hard floor: 4.5:1 body, 3:1 large text & non-text UI (target 7:1).

## Typography budget

- **1-2 typefaces, 2-3 weights.** Pattern: a neutral/geometric sans for body + one
  expressive serif or display face for headlines that carry the hierarchy.
- **Body floor 16px** — below it, iOS zooms on input focus.
- Line-height ~1.5-1.6 body, ~1.2 headings; measure ~65ch; 1.25 "major-third"
  modular scale; hero display 72-96px is now normal ("type-as-hero" replaces
  decorative imagery).
- Reflexive Inter/Poppins/Helvetica now reads as AI-sameness. This is an
  opinionated stance, not a hard ban — but choose type deliberately.

## Spacing & layout budget

- **8px base scale** (8 / 16 / 24 / 32 / 48 / 64 / 96).
- **~96px (6rem) between sections** — resist shrinking it.
- Separate elements with **whitespace, not shadows**: if you need a shadow to see
  the card, add whitespace instead.
- One primary CTA per screen; nav 3-5 items.
- **Show the product:** real screenshots / product photography that demonstrate
  the headline's promise — not stock photos, 3D renders, abstract illustration,
  or sparkle icons.

## The AI-slop ban-list

Put these in the DESIGN.md Don'ts section verbatim; they are the recognizable
"AI built this" tells:

- Blue → purple / indigo / aurora gradients.
- Glassmorphism, glow shadows, glowing rounded cards.
- The "2026 hype look": dark mode + floating dashboard mockup + purple gradient +
  sparkle icon.
- Three-card or bento grids with equal-weight cells (no anchor).
- Centered hero + eyebrow dot/pill above the headline.
- Marquee logo strips with no names.
- Oversized uniform corner radii on everything.
- The canonical `nav → hero → features → testimonials → pricing → faq → cta →
  footer` order shipped unexamined (real pages vary far more — see
  `landing-page-structure`).

Add one "don't" for every specific bad output your own tools produce; the ban-list
is only as good as it is real.

## Sources

- Google Labs — DESIGN.md announcement: https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/
- Why AI design converges (root cause): https://uxdesign.cc/ai-design-isnt-ugly-it-s-fluent-and-that-s-the-problem-131b2f4eb78c
- Concrete warm-neutral token set: https://www.superdesign.dev/blog/why-ai-design-looks-generic
- AI design-slop tells (prose-ignored evidence): https://solodesign.cc/blog/ai-design-slop-the-tells/
- NN/g — aesthetic-minimalist design (cognitive load): https://www.nngroup.com/articles/aesthetic-minimalist-design/
