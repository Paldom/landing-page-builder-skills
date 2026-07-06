# DESIGN.md & Deterministic Enforcement

Reference for `visual-design-system`. How to encode a design system so an agent
ingests it, and how to enforce it so the agent cannot route around it.

**Contents:** [DESIGN.md structure](#designmd-structure) · [Tokens as roles](#tokens-as-roles) ·
[Authoring methods](#authoring-methods) · [Figma MCP & Code Connect](#figma-mcp--code-connect) ·
[Enforcement stack](#enforcement-stack) · [Fact ledger](#fact-ledger) · [Sources](#sources)

## DESIGN.md structure

A single repo-root file pairing machine-readable tokens with human-readable
rationale — the design-context counterpart to AGENTS.md/CLAUDE.md.

1. **YAML front matter** — normative tokens: colors, typography, spacing,
   border-radius, components, with typed values and `{path.to.token}`
   cross-references. Follows the **W3C DTCG** token structure, so it can export to
   `tokens.json` or a Tailwind config.
2. **Markdown body** — eight canonical `##` sections, **in order** (the model
   reads top-to-bottom): Overview, Colors, Typography, Layout, Elevation & Depth,
   Shapes, Components, Do's and Don'ts.

Google Labs published the draft spec (Apache-2.0) in **April 2026**; it ships a
CLI — `npx @google/design.md lint DESIGN.md` catches broken token refs, WCAG AA
contrast failures, orphaned tokens, and section-order violations, plus `diff` and
`export` (Tailwind / DTCG). **The format is alpha and actively changing** — verify
the current spec at the repo below before relying on exact field names.

## Tokens as roles

- "primary" is "the main ink color of the page", not `#1c1917`. Every consumer
  (button, header, link) references the role; a rebrand propagates by editing one
  line.
- Tokens carry *what*; prose carries *when/why*. A pure token dump still produces
  off-brand output because the agent has values but no judgment.
- The **Do's and Don'ts** section is the highest-leverage part — one "don't" per
  real bad output (see `aesthetic-budgets.md` for the ban-list).

## Authoring methods

- **Manual** — hand-write for greenfield brands.
- **Extract** (preferred when tokens already exist) — read `tokens.json` /
  Tailwind config / CSS variables and emit the file; don't re-type values.
- **Generate** — from a Figma file or a live URL; good for bootstrapping,
  drift-prone as a source of truth.

Keep the visual spec in DESIGN.md, *separate* from behavioral rules in
AGENTS.md/CLAUDE.md, and reference it from AGENTS.md so it loads only when UI is
being generated (avoids paying the token cost on every turn). Root instruction
files degrade past ~150-200 discrete instructions — keep them lean.

## Figma MCP & Code Connect

Figma Dev Mode MCP + Code Connect feed the agent real component/prop/import
mappings, so it emits `<Button variant="primary">` instead of guessing a Tailwind
div. Caveats: requires a paid Figma tier, and it still collapses to generic output
without a project rules file instructing the agent to reuse existing components.
It complements DESIGN.md (tokens); it does not replace the enforcement layer.

## Enforcement stack

Enforcement beats prose because rules under deadline get dropped. Stack the gates
an agent cannot argue with:

- **Lint:** ESLint `no-restricted-imports` (block legacy components) + custom
  `no-arbitrary-colors` / `no-hardcoded-colors` (raw hex → error),
  `no-arbitrary-spacing`, `no-inline-styles`.
- **Style:** Stylelint `declaration-strict-value` — off-scale hex/px rejected at
  compile.
- **Types:** TypeScript **discriminated unions** so an invalid variant/prop combo
  is a *type* error.
- **Visual:** screenshot diff — Chromatic, Percy, or Playwright
  `toHaveScreenshot()` (agents misreport pixels they reason over as code; force a
  real-browser "prove it").
- **Accessibility:** axe-core in CI (`@axe-core/playwright` or the Storybook a11y
  addon) — the shared floor with `landing-page-accessibility`.

Operational rules:

- Prefer **semantic tokens** (`bg-card`, `variant="primary"`) — models reason
  about meaning, not hex math.
- Surface violations via a `PostToolUse` hook that **blocks the write**, so stderr
  becomes a deterministic repair instruction.
- **Cap the self-correction loop** (~7 iterations) to avoid whack-a-mole and cost
  blowup.
- **Meta-rules:** forbid the agent editing lint/CI config, adding
  `eslint-disable`, casting to `any`, deleting failing tests, or "solving" a
  violation by minting a new token instead of using an existing one. Two-tier
  lint (loose for humans, zero-tolerance on AI diffs) plus protected config paths.

## Fact ledger

- **STABLE:** DESIGN.md dual-layer format & 8-section order; tokens-as-roles; the
  budgets in `aesthetic-budgets.md`; all named tooling (ESLint/Stylelint rules,
  Chromatic/Percy/Playwright, axe-core, Figma MCP + Code Connect); enforcement
  over prose; cap-the-loop; layered agent files; W3C DTCG token spec.
- **DRIFT-PRONE (version-gate):** DESIGN.md is an alpha convention, not a ratified
  cross-tool spec — Google Labs open-sourced it ~April 2026; CLI rule set,
  section aliases, and component-property list may shift. Verify the current spec.
  The static-file-vs-live-MCP-server scaling debate (Atlassian) is unsettled.
- **UNVERIFIABLE-STAT (directional only):** all slop-scale numbers, token-coverage
  %, adoption %, "instruction quality +4%/-3%", "78-98% token savings", axe-core
  coverage %, regression-rate %. Never state as fact.

## Sources

- DESIGN.md spec & CLI: https://github.com/google-labs-code/design.md
- Vercel — design-systems-to-agent-skills (source-verified extraction): https://github.com/vercel-labs/design-systems-to-agent-skills
- Making AI agents follow your design system: https://www.builder.io/blog/how-to-make-ai-agents-follow-your-design-system
- Figma Dev Mode MCP + Code Connect: https://developers.figma.com/docs/figma-mcp-server/create-skills/
- W3C Design Tokens (DTCG): https://www.w3.org/community/design-tokens/
