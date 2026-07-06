---
name: landing-page-experimentation
description: Sets up conversion measurement and A/B testing for a landing page - server/edge variant assignment (no flicker), A/B vs multi-armed bandit, statistical significance, filtering bot/AI traffic, analytics. Use when asked to A/B test, run an experiment, measure conversions, check significance, or set up landing-page analytics. Not for writing copy, page structure, or Core Web Vitals.
---

# landing-page-experimentation

Assign variants on the server, respect the sample-size floor, and distrust the
numbers until the bots are filtered.

## The failure this fixes

Teams bolt a client-side A/B script onto a page; it swaps the DOM after render
(flash of original content, 200-500ms delay), and the anti-flicker snippet that
hides the body to mask it directly tanks LCP/CLS — so the test *suppresses* the
conversion it measures. Then they run tests that can never reach significance
(only ~13% of tests produce a winner; low-traffic pages can't clear the ~300
conversions/variant floor), and trust analytics corrupted by bot/AI traffic
(0-second sessions, some ad-driven tests >98% bots) and ad-blockers that eat
10-30% of client events. The deepest scar: tooling isn't the lever — the same AI
CRO tools deliver a few % lift DIY vs several-fold more expert-guided, because
**human hypothesis quality is what moves the number.**

## When to use / when NOT to use

Use for measurement and testing: variant assignment, A/B vs bandit, sample size &
significance, bot filtering, event instrumentation, and consent.

Not for: writing the variants (`landing-page-copywriting`), section design
(`landing-page-structure`), the page's own Core Web Vitals (`web-vitals-and-seo`
— though this skill's edge-assignment protects them), palette
(`visual-design-system`), or building the page (`nextjs-landing-page`).

## Workflow

1. **Assign the variant server-side / at the edge** (Next.js middleware,
   Cloudflare Workers) and render the correct HTML — no client DOM swap, no
   anti-flicker snippet. Persist the assignment in a cookie so a visitor always
   sees the same variant; **log exposure server-side at assignment time**
   (ad-blockers drop client events). Use the **precompute pattern** (encode the
   variant into a URL segment) so each variant stays statically cached — don't
   call `cookies()`/`headers()` in a root layout. Details:
   `references/experimentation-guide.md`.
2. **Size the test before running it.** ~300+ conversions/variant floor; ≥2 weeks
   covering multiple weekday/weekend cycles; run an **A/A test first** to catch
   tooling bugs; pre-commit the sample size and **never peek / stop early**.
3. **Pick A/B vs bandit deliberately.** Fixed-split **A/B for permanent
   structural decisions** (pricing, core UX) needing clean causal certainty;
   **multi-armed bandit (Thompson Sampling) for many-variant, short-lived,
   high-traffic** optimization to minimize regret. Bandits need a conversion floor
   before they route meaningfully.
4. **Filter bots before trusting any delta.** Exclude known crawler UAs
   server-side; watch for 0-second-session / ~100%-bounce spikes and metronome hit
   patterns. Unfiltered, a bandit "wins" by optimizing for scraper behavior.
5. **Instrument real events, not vanity counts.** GA4 `generate_lead` /
   `purchase` / `add_to_cart` plus custom scroll-depth (GA4's default scroll event
   fires only at 90%; bounce is now the inverse of engagement). Heatmaps/session
   recordings generate hypotheses; A/B validates them.
6. **Know the honest limit.** Most pages lack the traffic to test the levers that
   matter — below the floor, use qualitative review (heatmaps, replay, user
   testing), not underpowered tests that produce false winners faster.

## The rules

- **Server/edge variant assignment + server-rendered HTML** = zero flicker,
  neutral-to-positive CWV, no anti-flicker snippet.
- **Server-side exposure logging** — client events are lost to ad-blockers.
- **~300 conversions/variant, ≥2-week multi-cycle, A/A first, no peeking.**
- **A/B for causal/permanent; bandit for many-variant/short-lived;** bandits need
  a conversion floor.
- **Bot/AI traffic is real and must be filtered** before trusting deltas.
- **Real-event instrumentation** over pageview vanity metrics.
- **Human hypothesis quality is the lever, not the tool** — more automation
  without discipline just accelerates false winners.
- **Every lift % in the research is directional, never a promise** (the 13% win
  rate baseline means published wins are survivors, not the average).

## Output

Server/edge variant assignment with server-side exposure logging; a pre-committed
sample size honoring the ~300/variant floor and multi-cycle duration; an explicit
A/B-vs-bandit choice; bot filtering; real-event instrumentation; and every tool
name, pricing, and lift number version-gated or flagged directional — with a clear
"use qualitative review instead" path for low-traffic pages.

## References

- `references/experimentation-guide.md` - server/edge assignment & the precompute
  pattern, sample-size and significance discipline, A/B-vs-bandit decision, bot
  filtering, the tooling landscape (version-gated), and the fact ledger.
