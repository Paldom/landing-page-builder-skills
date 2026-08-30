# Direction axes

Reference for `landing-page-art-direction`. The four axes a direction commits
to, with menus. Menus are starting vocabularies, not closed sets — a direction
may name something off-menu if the brief justifies it.

**Contents:** [Macrostructure](#axis-1--macrostructure) ·
[Type personality](#axis-2--type-personality) ·
[Palette family](#axis-3--palette-family) ·
[Motion character](#axis-4--motion-character) · [Design log](#design-log)

## Axis 1 — Macrostructure

The page's compositional shape, chosen **first** — it bundles heading
placement, body composition, divider language, and image treatment into one
decision, which produces categorically more variety than tuning six small
knobs. Menu (each with the brief it suits):

| Shape | First viewport reads as | Suits |
| --- | --- | --- |
| **Split studio** | title-left / lede-and-visual-right two-column | product with one strong screenshot |
| **Long document** | a designed document that starts reading immediately | dev tools, docs-led, trust-first |
| **Marquee hero** | one full-bleed visual plane, narrow text column | venues, lifestyle, photography-led |
| **Stat-led** | one large number or live artifact as the thesis | metrics products, "proof up front" |
| **Editorial column** | single measured column, generous margins | essays, manifestos, founder letters |
| **Bento grid** | asymmetric cell mosaic (with an anchor cell) | multi-feature products; needs discipline |
| **Workbench** | the actual product UI as the hero | apps confident in their interface |
| **Quote-led** | a customer's words as the opening | social-proof-rich B2B |
| **Index-first** | a table of contents / catalogue opening | content libraries, agencies |
| **Narrative workflow** | a step sequence the scroll walks through | process products, onboarding-led |

Guardrails: the first viewport is **one composition**, not a dashboard; the
hero carries brand + one headline + one supporting line + one CTA group + one
dominant visual and nothing else; a shape used as the workspace default (bento
especially) is the first thing to rotate away from.

## Axis 2 — Type personality

The pairing's voice — pick one, name it in DESIGN.md, let
`visual-design-system` choose the actual faces within its budgets (1–2
families, 2–3 weights):

- **Geometric-neutral** — quiet geometric sans throughout; hierarchy from
  size/weight only. The safest minimalist voice.
- **Grotesk-technical** — a grotesk with a mono for data/labels; developer
  register.
- **Serif-editorial** — expressive serif display over sans body; use
  deliberately, it is also the most common "AI tries to be classy" default.
- **Mono-utilitarian** — mono-led, document energy; pairs with long-document
  shapes.
- **Display-condensed** — loud compressed display for one-word-per-line
  heroes; sports/launch energy.

Reflexive Inter/Roboto/system-default reads as unchosen; so does reflexive
high-contrast serif + cream. Either can still be *chosen* — the axis exists so
the choice is explicit.

## Axis 3 — Palette family

Temperature + neutrals + the single accent. Two families are saturated AI
defaults — **indigo/violet on white** and **cream + brass/terracotta
("premium artisan")** — treat both as the rejected default unless the brand
demands them. Rotation menu:

| Family | Neutrals | Accent register |
| --- | --- | --- |
| **Cool paper** | near-white cool grays, near-black ink | one electric accent (cobalt, signal green) |
| **Warm paper** | bone/oat, espresso ink | one earthen accent (clay, moss, oxblood) |
| **Cold luxury** | silver/chrome/smoke | almost none — chrome is the accent |
| **Dark technical** | 10–16% L charcoal (never #000) | one phosphor accent, ≤5% of viewport |
| **Monochrome + pop** | strict grayscale | one saturated pop used ~3 times |
| **Forest/field** | deep green + bone | amber or brick |

Rules that travel across all families: one accent, saturation < 80%, reserved
for links + primary CTA; neutrals tinted (chroma ≥ 0.005 in OKLCH) unless the
direction is strict-minimal; contrast floors are non-negotiable
(`ux-guardrails` enforces token pairs).

## Axis 4 — Motion character

- **None** — static page, `:hover`/`:active` states only. A legitimate
  direction, not a failure.
- **Restrained** — entrance reveals and micro-feedback only: UI transitions
  ≤ 300ms, ease-out enters, stagger 30–80ms, everything guarded by
  `prefers-reduced-motion`. The minimalist default.
- **Expressive** — scroll-driven sequences, pinning, parallax; budget 2–3
  intentional motions per page, each justified in one sentence. Implementation
  belongs to `scroll-motion`.

## Design log

Cross-project variety mechanism (optional; for workspaces that ship many
pages). A small JSON file — workspace-level
(`~/.landing-skills/design-log.json`) or repo-level for a monorepo of sites:

```json
[
  { "date": "2026-07-22", "project": "acme-analytics",
    "macrostructure": "split-studio", "type": "grotesk-technical",
    "palette": "cool-paper", "motion": "restrained" }
]
```

Protocol: read before choosing a direction for a **new** project; the new
direction must differ from the last three entries on macrostructure plus at
least one other axis; state the comparison in plain text ("last three:
bento/bento/split-studio — picking long-document, mono-utilitarian") before
writing code; append the new entry. Newest first, keep ~20. Never consult the
log when editing an existing project — within a project, consistency wins.
