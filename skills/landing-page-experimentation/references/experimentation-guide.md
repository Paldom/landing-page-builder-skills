# Landing Page Experimentation Guide

Reference for `landing-page-experimentation`. Every lift percentage in the source
research traces to a single case study or vendor blog — the ~13% test-win-rate
baseline means published wins are survivors, not the average. **All magnitudes are
directional, never planning inputs.**

**Contents:** [Server/edge assignment](#serveredge-assignment) ·
[Sample size & significance](#sample-size--significance) · [A/B vs bandit](#ab-vs-bandit) ·
[Bot filtering](#bot-filtering) · [Instrumentation](#instrumentation) ·
[Tooling (version-gate)](#tooling-version-gate) · [Fact ledger](#fact-ledger) · [Sources](#sources)

## Server/edge assignment

- **Assign the variant server-side / at the edge** (Next.js middleware, Cloudflare
  Workers) and render the correct HTML before it ships. No client-side DOM swap,
  no anti-flicker snippet — zero flicker, no hydration mismatch, neutral-to-positive
  CWV.
- **Persist assignment in a cookie** (anonymous ID) so the same visitor always
  sees the same variant.
- **Log exposure server-side, at assignment time** — not in a later client
  `useEffect`. Ad-blockers/privacy settings drop 10-30% of client events.
- **Precompute pattern:** don't call `cookies()`/`headers()` in a root layout
  (forces whole-app dynamic rendering, kills CDN caching). Resolve the variant in
  middleware and encode it into a hidden URL segment so each variant pre-renders
  statically.
- **Join exposure and conversion on the same key** (`experiment_key`, `variant`,
  `subject_id`, `exposure_id`) so lift computes cleanly and dedupes.

## Sample size & significance

- **~300+ conversions/variant** (≈600 total for a 50/50) before any conclusion.
- Run **≥2 weeks** covering multiple weekday+weekend cycles to average out
  day-of-week and novelty effects.
- Run an **A/A test first** to catch tooling bugs (an A/A test still swings a
  meaningful amount by chance).
- **Pre-commit the sample size; never peek / call early** — early "wins" evaporate.
- Only ~13% of tests reach a significant winner (directional); public case studies
  are survivorship-biased by construction.

## A/B vs bandit

- **A/B (fixed split)** for permanent structural decisions (pricing, core UX)
  where you need clean causal certainty and long-term learning.
- **Multi-armed bandit (Thompson Sampling is the production default)** for
  many-variant (5+), short-lived, high-traffic optimization (Black Friday, paid
  campaigns) to minimize regret. Bandits trade clean data for short-term revenue,
  and need a conversion floor before routing meaningfully (below it they behave
  like round-robin).

## Bot filtering

Filter at the edge/server before trusting any delta: exclude known crawler UAs;
watch for 0-second-session / ~100%-bounce spikes and metronome hit patterns.
Unfiltered, a bandit can "declare a winner" optimized for scraper behavior. Some
ad-driven landing-page tests have been reported at >98% bot traffic (directional).

## Instrumentation

- Instrument **real events**: GA4 recommended events (`generate_lead`, `purchase`,
  `add_to_cart`) plus custom scroll-depth at 25/50/75/90 (GA4's default scroll
  event fires only at 90%).
- GA4 **bounce rate is now the inverse of engagement rate** (<10s, no key event,
  <2 pageviews = not engaged) — a different definition than Universal Analytics.
- Heatmaps/session recordings **generate hypotheses**; A/B **validates** them.
  Prioritize with RICE/ICE. Clean up feature-flag/experiment branches within 1-2
  sprints to avoid flag rot.

## Tooling (version-gate)

Names, tiers, and pricing drift — verify before recommending:

- **Edge/framework:** Vercel Flags SDK + Edge Config and the precompute pattern
  (tightly coupled to Vercel; OpenFeature adapters give portability); Cloudflare
  Workers for platform-agnostic edge assignment.
- **Experiment/flag platforms:** GrowthBook (warehouse-native, keeps data in your
  infra), Statsig (warehouse + bandits), PostHog (analytics + flags + replay +
  experiments). Flags and A/B tests have converged into one primitive.
- **Cookieless analytics:** Plausible, Umami, Fathom (no consent banner; you lose
  returning-user recognition).
- **Qualitative:** Microsoft Clarity (free) — Consent API reported mandatory for
  EEA visitors since ~Oct 31, 2025; carries US CIPA wiretapping risk if recording
  without consent. Verify current consent requirements.

## Fact ledger

- **STABLE (method):** server/edge assignment + server-rendered HTML kills flicker
  and protects CWV; server-side exposure logging; ~300/variant floor; ≥2-week
  multi-cycle; A/A first; bandit-vs-A/B decision + bandit conversion floor; bot
  contamination is real; ad-blockers eat client events; real-event instrumentation.
- **DRIFT-PRONE (version-gate):** tool names/pricing; Vercel Flags GA date & Vercel
  coupling; Clarity Consent API EEA date; GA4 default scroll threshold and bounce
  definition; the MAB engagement floor number.
- **UNVERIFIABLE-STAT (directional only):** 13% win rate; DIY vs expert lift gap;
  >98% bot traffic; vendor deal sizes; all specific lift % (Going +104%, etc.);
  median CVR (4.0 vs 6.6 depending on dataset); "96% accuracy" predictive claims.

## Sources

- Aurora Scharff — the precompute pattern (edge A/B in Next.js): https://aurorascharff.no/posts/the-precompute-pattern-encoding-dynamic-data-into-urls-in-nextjs/
- Philip Walton — performant A/B testing at the edge: https://philipwalton.com/articles/performant-a-b-testing-with-cloudflare-workers/
- Vercel — A/B testing on Vercel: https://vercel.com/docs/workflow-collaboration/feature-flags
- GrowthBook vs PostHog: https://posthog.com/blog/posthog-vs-growthbook
- Braze — multi-armed bandit method: https://www.braze.com/resources/articles/multi-armed-bandit
- GA4 — measure & report conversions: https://support.google.com/analytics/answer/9267568
- web.dev — Core Web Vitals (guardrails for tests): https://web.dev/articles/vitals
