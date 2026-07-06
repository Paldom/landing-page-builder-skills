# Landing Page Builder Skills

[![CI](https://github.com/Paldom/landing-page-builder-skills/actions/workflows/ci.yml/badge.svg)](https://github.com/Paldom/landing-page-builder-skills/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![skills.sh](https://skills.sh/b/Paldom/landing-page-builder-skills)](https://skills.sh/Paldom/landing-page-builder-skills)

Agent Skills for building high-converting, modern, performant landing pages: design tokens, conversion copywriting, marketing UX, layout patterns, Next.js/React + shadcn implementation, scroll motion, and performance/SEO/accessibility.

Agent Skills for [Claude Code](https://code.claude.com/docs/en/skills) (and any
[Agent Skills](https://agentskills.io)-compatible tool). Each skill is a folder under
[`skills/`](skills/) with a single-purpose `SKILL.md`, trigger evals, and optional
scripts/references — validated on every write, commit, and PR.

## Quick start

Install with the [skills CLI](https://skills.sh) — auto-detects 70+ agents
(Claude Code, Codex, Cursor, Copilot, pi, …):

```bash
npx skills add Paldom/landing-page-builder-skills                  # all detected agents
npx skills add Paldom/landing-page-builder-skills -a codex -a pi   # or target specific agents
```

Or with the [GitHub CLI](https://cli.github.com/manual/gh_skill_install) (≥ 2.90),
including version-pinned installs from releases:

```bash
gh skill install Paldom/landing-page-builder-skills
gh skill install Paldom/landing-page-builder-skills <skill> --pin <tag>
```

Or as a Claude Code plugin:

```
/plugin marketplace add Paldom/landing-page-builder-skills
/plugin install landing-page-builder-skills@landing-page-builder-skills
```

Or copy a single skill into a project:

```bash
git clone https://github.com/Paldom/landing-page-builder-skills.git
cp -r landing-page-builder-skills/skills/<skill-name> your-project/.claude/skills/
```

Then just describe the task — the skill activates on its description — or invoke it
explicitly with `/<skill-name>`.

## Skills

These eight skills compose into a full landing-page build. A paste-ready
orchestration prompt lives in [docs/setup-prompt.md](docs/setup-prompt.md).

| Skill | Description |
| --- | --- |
| [landing-page-copywriting](skills/landing-page-copywriting/) | Conversion copy that reads human, not AI slop — VoC mining, outcome-first headlines, CTA labels, specific social proof. |
| [landing-page-structure](skills/landing-page-structure/) | The section skeleton as one sequenced argument — hero anatomy, ordering, form-field count, CTA/proof placement. |
| [visual-design-system](skills/visual-design-system/) | A distinctive on-brand look plus DESIGN.md tokens and lint/visual/a11y gates that keep AI output off the generic "slop" aesthetic. |
| [scroll-motion](skills/scroll-motion/) | Scroll animation at the right tier (CSS / GSAP / Motion) that protects INP and honors prefers-reduced-motion. |
| [nextjs-landing-page](skills/nextjs-landing-page/) | Next.js App Router build with leaf-level client boundaries, shadcn/Tailwind setup, and the metadata & React2Shell CVE gotchas. |
| [web-vitals-and-seo](skills/web-vitals-and-seo/) | Server-rendered HTML for crawlers and AI bots, Core Web Vitals, field-vs-lab, and structured data. |
| [landing-page-accessibility](skills/landing-page-accessibility/) | WCAG 2.2 AA — semantic HTML over ARIA, contrast, focus, target size, labeled forms, no overlay widgets. |
| [landing-page-experimentation](skills/landing-page-experimentation/) | Flicker-free server/edge A/B testing, sample-size discipline, bandit-vs-A/B, and bot-traffic filtering. |

## Repository structure

```
skills/                  # distributed skills, one folder per skill (SKILL.md + evals/ + scripts/)
docs/                    # skill-authoring guide, eval methodology, deployment guide
scripts/                 # deterministic validator used by hooks and CI
skills.sh.json           # skills.sh repo-page customization (groupings)
.claude/                 # agentic dev setup: hooks + bundled add-skill / publish-repo skills
.claude-plugin/          # plugin + marketplace manifests (makes this repo installable)
.local/                  # gitignored working area: sources, research, PROMPT.md (see below)
```

## Working on this repo with an agent

This repo is agent-native: canonical agent instructions live in
[AGENTS.md](AGENTS.md) (CLAUDE.md imports it), hooks validate every `SKILL.md` on
write, `make check` runs the full validator, and CI enforces the same gate on every
PR. The bundled `add-skill` skill walks the eval-first authoring workflow described
in [docs/skill-authoring.md](docs/skill-authoring.md). Maintainers drive sessions
with their own (gitignored, personal) `.local/PROMPT.md` goal prompt.

## Contributing

Contributions welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the skill-proposal
process, the authoring workflow, and the PR checklist. Please note the
[Code of Conduct](CODE_OF_CONDUCT.md).

## Support

Questions, ideas, or something not working? Start with [SUPPORT.md](SUPPORT.md) —
bugs and skill proposals have [issue templates](../../issues/new/choose), and
security concerns go through [SECURITY.md](SECURITY.md) (never a public issue).

## License

[MIT](LICENSE) © 2026 Paldom
