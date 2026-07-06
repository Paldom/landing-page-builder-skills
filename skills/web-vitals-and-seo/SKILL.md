---
name: web-vitals-and-seo
description: Engineers a landing page to rank and load fast - server-rendered HTML so crawlers and AI bots see content, Core Web Vitals (LCP/INP/CLS), field-vs-lab, structured data, off-main-thread scripts. Use when asked to improve Core Web Vitals, page speed, technical SEO, get indexed, or fix a slow page. Not for copy, design, accessibility, A/B testing, or Next.js 'use client'/build bugs.
---

# web-vitals-and-seo

Put everything that must rank or be cited into the initial server HTML, and
optimize the field data, not the lab score.

## The failure this fixes

Two recurring failures. **First:** content, metadata, canonical, and JSON-LD are
rendered *after* load via `useEffect` / client components — so the raw HTTP
response is a blank shell. AI crawlers (GPTBot, ClaudeBot, PerplexityBot) run
**zero JavaScript, ever** and see nothing; Googlebot's Wave-1 crawl also sees
nothing and bets on a Wave-2 render queue that lags hours to weeks. **Second:**
teams burn a sprint chasing Lighthouse 100 — a single simulated lab load on a
throttled phone — while Google ranks on **CrUX field data** (real users, p75, 28
days). A page hits 95+ in the lab and still fails CWV in the field; the effort
went to the wrong number.

## When to use / when NOT to use

Use for ranking + loading: rendering strategy for SEO, Core Web Vitals,
lab-vs-field, structured data, AI-crawler/GEO visibility, and third-party-script
cost.

Not for: copy (`landing-page-copywriting`), palette/type
(`visual-design-system`), WCAG compliance (`landing-page-accessibility` — though
the accessibility tree *is* the crawler's substrate), Next.js implementation
bugs like `"use client"` breaking metadata (`nextjs-landing-page`), motion's INP
cost (`scroll-motion`), or test setup (`landing-page-experimentation`).

## Workflow

1. **Put rankable content in the initial server HTML.** Body copy, headings,
   title/description, canonical, hreflang, and JSON-LD must be in the raw response.
   Default **SSG** (or ISR for freshness); SSR only for genuinely per-user content;
   CSR only for authenticated tools with no SEO value. The **LCP element must be
   discoverable in the initial HTML** — never client-rendered, never lazy-loaded.
2. **Verify what crawlers actually get.** `curl -A "GPTBot" <url>` and plain "View
   Source" — DevTools and Search Console live-tests render JS and can disagree with
   what gets indexed.
3. **Optimize the field, gate the lab.** Workflow: **CrUX** (what Google sees) →
   **RUM** (`web-vitals` lib — why / which segments) → **Lighthouse/DevTools**
   (how to fix locally). Thresholds at p75: **LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1**
   (INP replaced FID in March 2024). Gate CI on **TBT** as the lab proxy for INP,
   then confirm real INP against CrUX post-release. Mobile INP runs 2-3x worse —
   test on real mid-range Android.
4. **Move third-party scripts off the main thread** (the primary INP killer) —
   don't delete, relocate: server-side tagging (sGTM), edge A/B assignment (no
   anti-flicker snippet), defer/idle-load chat & heatmaps, Partytown web worker.
   Details: `references/perf-seo.md`.
5. **Get on-page SEO + structured data right.** Title < 60, description < ~155,
   one H1, server-rendered canonical; JSON-LD server-rendered and matching visible
   content. Schema is not a direct ranking factor; GEO/AEO is the same
   server-HTML discipline. Version-gate volatile items (FAQ deprecation, Agentic
   Browsing, `llms.txt`) — see the reference.

## The rules

- **Everything rankable in the initial server HTML.** AI crawlers execute no JS;
  Googlebot Wave-2 is slow and not guaranteed.
- **Field data > lab score.** CrUX ranks; Lighthouse is a diagnostic and a CI
  regression gate. CWV is a tie-breaker, not a primary ranking factor — get into
  the green, then stop; perfecting 90→100 is near-zero SEO return.
- **The LCP element must be in the initial HTML** and not lazy-loaded.
- **Third-party scripts are the top INP cause** — move to sGTM / edge / deferred.
- **JSON-LD: server-rendered, matches visible content, sanitized** (escape `<`)
  when injected from CMS/user fields.
- **The lazy fix is the correct fix** — native `<button>`, less hydration, SSG
  over a JS framework, a pixel at the edge over an anti-flicker hack: less code,
  better on every axis.

## Output

A page whose rankable content, metadata, and schema are in the initial server
response; CWV optimized against field data with a TBT-gated CI budget;
third-party cost relocated off the main thread; and every drift-prone SEO claim
(FAQ deprecation, AI-crawler behavior, framework bundle numbers) version-gated or
dropped — never stated from an unverified single source.

## References

- `references/perf-seo.md` - the CrUX→RUM→Lighthouse workflow, third-party-script
  relocation options, on-page SEO + structured-data rules, AI-crawler/GEO notes,
  and the fact ledger (stable vs version-gate vs discard).
