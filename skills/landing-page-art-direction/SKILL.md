---
name: landing-page-art-direction
description: Commits a landing page to a distinctive named art direction before code - macrostructure, type personality, palette family, motion character - rejecting the AI-default look, with a numeric modern-minimalist spec. Use when a page looks generic or AI-generated, all pages look the same, or to pick a visual direction, style, or vibe. Not for design tokens, section order, copy, or animation code.
---

# landing-page-art-direction

Sameness is a time-of-first-choice failure, not a memory failure. Every
AI-generated page that "looks the same" made the same unexamined defaults in
its first minute: centered hero, Inter, indigo accent, three equal cards. This
skill forces one explicit, named direction choice **before any code**, writes
it down, and keeps variety across projects — without ever restyling a shipped
page.

## When to use / when NOT to use

Use at the start of a landing-page build, when output looks generic /
AI-generated / like every other site, or when successive projects converge on
one look.

Not for: tokens, palette values, DESIGN.md structure and enforcement gates
(`visual-design-system` — it consumes the direction chosen here), section
sequencing (`landing-page-structure`), the words (`landing-page-copywriting`),
building components (`nextjs-landing-page`), animation implementation
(`scroll-motion`), or mechanical rule enforcement (`ux-guardrails`).

## Workflow

1. **Design Read (one line, before anything visual).** Subject, audience, and
   the page's single job — e.g. "monitoring tool for SREs who distrust
   marketing; job: get a docs click". If the brief doesn't say, decide and
   state it. Every later choice must be justifiable from this line.
2. **Commit on the four axes** (menu + definitions:
   `references/direction-axes.md`):
   - **Macrostructure** — the page's compositional shape (split studio,
     long document, stat-led, editorial column, bento…), chosen first; it
     constrains everything else.
   - **Type personality** — the pairing's voice (geometric-neutral,
     grotesk-technical, serif-editorial, mono-utilitarian…).
   - **Palette family** — temperature + neutrals + the one accent
     (see the rotation table; never default to indigo-on-white or cream+brass).
   - **Motion character** — none / restrained / expressive, with budgets.
3. **Name the rejected default.** One sentence: "Rejecting: centered hero +
   Inter + purple gradient + three-card grid." Naming the default is what
   prevents drifting back into it; a direction defined only positively erodes
   under iteration.
4. **Write the commitment into DESIGN.md** as a short "Direction" block
   (direction name, the four axes, the rejected default, one signature
   element), then hand off to `visual-design-system` to turn it into tokens
   and budgets. The commitment is per-project and permanent: later edits obey
   it; only an explicit user-requested redesign reopens it.
5. **Execute with restraint.** One signature element carries the
   distinctiveness (a typographic hero, an unexpected macrostructure, one
   bold color move); everything else stays quiet and disciplined. Distinctive
   ≠ decorated: asking a model to "be more distinctive" ratchets it *toward*
   the mean — change axes, don't add effects.
6. **Modern-minimalist is the default direction when the brief is quiet** —
   executed with numbers, not vibes: `references/modern-minimal.md` (spacing
   rhythm, type scale and contrast pairing, single accent, depth mechanisms
   that keep minimal from reading as empty).
7. **Across projects, rotate.** Keep an optional design log
   (`references/direction-axes.md#design-log`) of past directions; a new
   project must differ from the last three on macrostructure plus at least one
   other axis, and the comparison is stated in text before coding. Never
   applied to edits of an existing page.

## Output spec

A stated Design Read; a named direction with all four axes; the rejected
default named; a Direction block written into DESIGN.md before any component
code; for new projects in a multi-project workspace, a one-line rotation
statement referencing the log.

## Gotchas

- **Never rotate within a project.** A landing page is one brand living for
  months; forced variety inside it is churn, not craft. Rotation is for the
  *next* project.
- **Minimal needs scaffolding.** "Clean and simple" without numbers produces
  empty flat pages; precision (spacing rhythm, type contrast, one accent,
  deliberate texture) is what reads as premium.
- **Don't ban yourself into a new monoculture.** The rejected-default list is
  per-project, derived from the brief — a fixed universal ban list just
  converges on a different same look. If the brand genuinely wants purple,
  purple wins.
- **Editorial-bold is not the only escape from generic.** Restraint is a
  direction, not a failure to choose one.
- Naming vivid looks to avoid can *prime* them — state the rejected default
  once in DESIGN.md, then stop repeating it in prompts.

## References

- `references/direction-axes.md` — the four axes with menus (macrostructures,
  type personalities, palette families with rotation, motion characters) and
  the cross-project design-log convention.
- `references/modern-minimal.md` — the numeric modern-minimalist spec:
  budgets, OKLCH token skeleton, anti-boring mechanisms, motion numbers.
