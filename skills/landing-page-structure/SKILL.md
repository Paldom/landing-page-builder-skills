---
name: landing-page-structure
description: Designs the section skeleton of a marketing landing page - the page as one sequenced argument, hero anatomy, section ordering, form-field count, and CTA/proof placement. Use when the user asks how to structure, lay out, sequence, or wireframe a landing page, which sections to include, or how many form fields. Not for the words/copy, visual styling, animation, or code.
---

# landing-page-structure

Structure a landing page as one argument, not a stack of default sections.

## The failure this fixes

Asked to build a landing page, a model drops in a **reorderable stack of default
sections** - hero, feature grid, testimonials block, pricing, FAQ, footer - in
template order, with no argument connecting them. The tells: proof exiled to a
lone "What our customers say" section (or the footer) instead of sitting next to
the claim it backs; a hero carrying three or four competing CTAs; a uniform
icon-row feature grid; and a 9-11-field form because the fields were available,
not needed. It passes visual inspection but has **no story** - you can shuffle
the sections without hurting comprehension, which proves it is parts, not a
sequence.

## When to use / when NOT to use

Use for the *skeleton*: which sections exist, in what order, the hero's internal
anatomy, form-field decisions, and where CTAs and proof go.

Not for: the words in those sections (`landing-page-copywriting`), palette/type
(`visual-design-system`), motion between sections (`scroll-motion`), building the
sections in code (`nextjs-landing-page`), or testing variants
(`landing-page-experimentation`).

## Workflow

1. **State the one argument.** Before placing any section, write the page's
   single claim and its support as a sentence: problem → stakes → mechanism →
   proof → CTA. Sections exist to advance that argument; features are *narrated
   inside* it, never dumped as a standalone grid.
2. **Lay out the hero (first screen).** It must answer four questions without
   scrolling: *what is this, who is it for, why trust it, what do I do next* -
   i.e. one benefit headline, one supporting subhead, ONE primary CTA, one trust
   cue (a specific number, one review, or one known logo), and a real product
   shot (screenshot / short muted loop / interactive demo), not stock art. The
   majority of visitors never scroll past this screen (cited figures ~57%
   desktop / ~64% mobile - directional, not a guarantee).
3. **Sequence the body.** Order sections as the argument, with proof adjacent to
   each claim. Use a canonical spine (`references/structure-patterns.md`) and
   adapt - do not ship the default order unexamined. Scale length to purchase
   risk: ~5-7 sections for simple/low-consideration offers, ~10-12 for
   high-ticket B2B (more objection-handling, dual-stage CTA).
4. **Place CTAs and proof.** One primary action, repeated every ~400-600px and
   once more at the bottom (final CTA carries reassurance microcopy); at most one
   lower-friction secondary action; sticky/floating CTA on mobile. Cap distinct
   proof *types* at 3-5; never centralize all proof in one block or the footer.
5. **Design forms for the lowest necessary friction.** Default ~3 fields B2C,
   ~4-5 B2B; single-step under ~5 fields, multi-step at 5+ or genuine
   conditional logic; capture email on step one so abandonment still yields a
   lead. Details: `references/structure-patterns.md`.
6. **Run the reorder test.** If the sections can be shuffled without hurting
   comprehension, there is no argument - fix ordering before adding anything.

## The rules

- **The page is one sequenced argument, not a section list.** Out-of-expected
  order (pricing before the problem, features before trust) imposes
  "reconciliation cost" that kills scroll momentum.
- **Hero answers the four questions in the first screen** - one headline, one
  subhead, one primary CTA, one trust cue, real product shot.
- **One primary CTA, repeated - not multiple competing CTAs.** A single soft
  secondary ("Watch 2-min tour") is fine; three co-equal buttons is the
  anti-pattern. Sticky CTA and above-fold CTA do not stack additively.
- **Proof goes adjacent to the claim it supports and to the decision point**
  (beside the CTA, the form, the pricing) - not centralized, not in the footer
  (footer proof ≈ no proof).
- **Form field-count discipline.** Field count is the highest-leverage,
  most-replicated form lever; below ~5 fields, *sequencing* beats subtraction.
  Match length to traffic intent (cold → minimal, warm/high-intent → progressive
  profiling).
- **Bento grid is for feature density, not personality.** Use it for a
  multi-feature product's features/proof section with a clear anchor cell (~2x
  the next largest), 5-9 cells, and a mobile re-prioritization pass. Skip it for
  a single dominant offer - use a linear sequence.
- **Above the fold is a trust signal, not a literal boundary.** What matters is
  whether the first screen instantly establishes relevance and message match.

## Output

A section list in order (each section named with its job in the argument), the
hero's internal layout, CTA and proof placements, and form-field decisions -
plus the reorder-test verdict. Words, styling, motion, and code are deferred to
their sibling skills.

## References

- `references/structure-patterns.md` - canonical section-order spines (B2B SaaS,
  narrative CRO arc), hero-pattern-by-funnel-temperature, the form
  single-vs-multi-step decision, and the directional-vs-fact ledger.
