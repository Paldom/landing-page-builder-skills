---
name: ux-guardrails
description: Enforces UX quality mechanically - a zero-dep lint (spacing scale, typography budget, WCAG token contrast, AI-slop tells) wired as Claude Code PostToolUse/Stop hooks that block criticals. Use when asked to enforce design rules automatically, lint UI code, add a UX/design hook, or stop off-scale spacing and raw palette classes. Not for choosing tokens, full WCAG audits, or CI test hooks.
---

# ux-guardrails

Prose design rules are followed probabilistically and dropped under iteration
pressure; a deterministic check is not. This skill ships that check —
`scripts/ux_lint.mjs`, a single-file, zero-dependency Node lint — plus the hook
wiring that runs it on every UI write and refuses to finish a session with
critical violations outstanding.

## When to use / when NOT to use

Use to install or run mechanical UX enforcement in a landing-page project:
spacing on the grid, arbitrary-value and raw-palette bans, WCAG contrast on
design-token pairs, focus-visibility, slop tells (purple gradients, gradient
text, glassmorphism, `transition: all`).

Not for: choosing the palette/type/tokens (`visual-design-system`), picking the
overall look (`landing-page-art-direction`), a full WCAG 2.2 audit with manual
passes (`landing-page-accessibility`), motion design (`scroll-motion`), or
generic test/format hooks (that's ordinary repo tooling).

## Severity model (two tiers, on purpose)

- **critical** — objectively wrong, blocks: disabled zoom, `<img>` without
  `alt`, `outline-none` with no focus-visible replacement, positive tabindex,
  token pairs below 4.5:1. The agent must fix these.
- **advisory** — drift and slop tells, informs but never blocks: off-scale
  spacing, arbitrary text sizes, raw palette classes, gradient text, slow
  transitions. Per-edit nagging on taste rules has been observed to make agents
  conservative and samey — so taste stays advisory, and **the brief wins**: an
  intentional exception is suppressed explicitly, never fought.

Escape hatches (use these; never weaken the script): a
`ux-guardrails-ignore <rule-id>` comment on or above the line, or
`ignoreRules`/`ignoreFiles` in `.ux-guardrails.json`.

## Workflow

1. **Install into the consumer project.** Copy the script and wire both hooks:

   ```bash
   mkdir -p .claude/hooks
   cp "${CLAUDE_SKILL_DIR}/scripts/ux_lint.mjs" .claude/hooks/ux_lint.mjs
   node .claude/hooks/ux_lint.mjs --self-test   # must print SELF-TEST PASSED
   ```

   Merge into `.claude/settings.json` (merge — do not clobber existing hooks):

   ```json
   {
     "hooks": {
       "PostToolUse": [
         { "matcher": "Edit|Write|MultiEdit",
           "hooks": [{ "type": "command",
             "command": "node \"$CLAUDE_PROJECT_DIR/.claude/hooks/ux_lint.mjs\"",
             "timeout": 10 }] }
       ],
       "Stop": [
         { "hooks": [{ "type": "command",
             "command": "node \"$CLAUDE_PROJECT_DIR/.claude/hooks/ux_lint.mjs\"",
             "timeout": 30 }] }
       ]
     }
   }
   ```

2. **Configure only if the project's scale differs.** Optional
   `.ux-guardrails.json` at the project root:

   ```json
   { "spacingStep": 4, "contrastMin": 4.5, "maxFontFamilies": 3,
     "transitionMaxMs": 500, "ignoreRules": [], "ignoreFiles": [] }
   ```

   Defaults match this repo's `visual-design-system` budgets. WCAG floors are
   the one thing config should never be used to relax.

3. **Run it in CI too** (same script, exit 1 on criticals):

   ```bash
   node .claude/hooks/ux_lint.mjs src/ app/ styles/
   ```

4. **Token contrast needs the pairing convention.** Contrast is computed only
   for CSS custom properties following the shadcn `--x` / `--x-foreground`
   convention (plus `--background`/`--foreground`), with resolvable literal
   values (hex, rgb, hsl, oklch — no `var()` chains, `color-mix`, or alpha).
   Anything unresolvable is skipped **silently by design** — a lint that
   false-positives on values it can't parse gets uninstalled.

## How the hooks behave (what to tell the user)

- **PostToolUse** lints only the file just written. Criticals → exit 2, stderr
  becomes the agent's repair instruction (the write already landed —
  PostToolUse steers the next step, it cannot undo). Advisories → injected as
  context, never block.
- **Stop** lints all git-changed UI files (capped at 20). Criticals → exit 2
  blocks finishing, **once** — it honors `stop_hook_active`, so a stuck
  false positive can never loop the session.
- Cross-file budgets (font-family count) run only at Stop/CLI where the whole
  changeset is visible; per-file they'd be meaningless.

## Output spec

Findings as `file:line [SEVERITY] [rule-id] message`, grouped by file, ending
with a `N critical, M advisory` budget line and the ignore instruction. The
full rule catalog with thresholds and rationale: `references/rules.md`.

## Gotchas

- **Never "fix" a finding by editing the lint script, the hook config, or by
  scattering ignore comments.** An intentional design exception gets one
  documented ignore; three ignores of the same rule mean the config (or the
  design) is wrong.
- The Stop gate reads `git diff` — in a project without git it silently checks
  nothing (PostToolUse still covers every write).
- `--self-test` exercises every rule against embedded fixtures; run it after
  any update of the script copy.
- Advisory findings are signals, not noise: resolve each one by either fixing
  the code or ignoring it explicitly with a reason.
- The purple-gradient / gradient-text tells are heuristics about *defaults* —
  a brand that genuinely wants purple overrides them (ignore + a line in
  DESIGN.md), which is exactly the "brief wins" rule.

## References

- `references/rules.md` — every rule id, severity, exact trigger/threshold, and
  the research it's derived from.
