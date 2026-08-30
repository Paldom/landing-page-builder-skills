# Changelog

All notable changes to this repository's skills are documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning: [SemVer](https://semver.org) on the plugin manifest
(breaking skill-interface change → major, new skill → minor, fix → patch).

## [Unreleased]

### Added
- `ux-guardrails` — deterministic UX enforcement as Claude Code hooks: a
  zero-dependency Node lint (`ux_lint.mjs`, 20 rules with `--self-test`)
  checking spacing-scale drift, typography budgets, WCAG contrast on
  `--x`/`--x-foreground` token pairs (hex/rgb/hsl/oklch), and AI-slop tells;
  PostToolUse blocks criticals via stderr, Stop gates the session once
  (honors `stop_hook_active`), advisories inform without blocking.
- `landing-page-art-direction` — commits a page to a named direction before
  code (macrostructure, type personality, palette family, motion character),
  names the rejected AI default, writes the commitment into DESIGN.md, ships a
  numeric modern-minimalist spec, and keeps cross-project variety via an
  optional design log. Distilled from an 18-repo analysis of the top public
  design skills (impeccable, hallmark, taste-skill, ui-ux-pro-max, shadcn,
  vercel, emil-design-eng, …).

### Changed
- `visual-design-system` — enforcement now points at the shipped
  `ux-guardrails` hook; description narrowed so art-direction and hook
  triggers route to the new sibling skills.
- Repository scaffolded from the skills template.
- Eight landing-page skills authored from a verified research pack:
  - `landing-page-copywriting` — VoC-sourced conversion copy, no AI slop.
  - `landing-page-structure` — the page as one sequenced argument (hero, ordering, forms, CTA/proof placement).
  - `visual-design-system` — distinctive aesthetic budgets + DESIGN.md tokens + lint/visual/a11y enforcement.
  - `scroll-motion` — the CSS / GSAP / Motion tier-by-purpose stack, INP-safe and reduced-motion-aware.
  - `nextjs-landing-page` — App Router RSC boundaries, shadcn/Tailwind setup, metadata & CVE-2025-55182 gotchas.
  - `web-vitals-and-seo` — server-rendered HTML for crawlers/AI bots, Core Web Vitals, field-vs-lab, structured data.
  - `landing-page-accessibility` — WCAG 2.2 AA: semantic HTML over ARIA, contrast, focus, target size, forms, no overlays.
  - `landing-page-experimentation` — flicker-free server/edge A/B testing, sample-size discipline, bandit-vs-A/B, bot filtering.
- `docs/setup-prompt.md` — paste-ready orchestration prompt composing the eight skills into a full landing-page build.
