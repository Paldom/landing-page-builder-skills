# Performance & SEO Engineering

Reference for `web-vitals-and-seo`. The server-rendered HTML is the single
substrate that search, AI answer engines, and assistive tech all read.

**Contents:** [Rendering & CWV](#rendering--cwv) · [Third-party scripts](#third-party-scripts) ·
[On-page SEO & structured data](#on-page-seo--structured-data) · [AI crawlers & GEO](#ai-crawlers--geo) ·
[Fact ledger](#fact-ledger) · [Sources](#sources)

## Rendering & CWV

- **Initial server HTML carries everything rankable:** body copy, headings,
  title/description, canonical, hreflang, JSON-LD. SSG default; ISR for periodic
  freshness; SSR for per-user; CSR only for auth'd dashboards.
- **LCP element discoverable in initial HTML** — never client-rendered, never
  lazy-loaded.
- **Thresholds at p75:** LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1. **INP replaced FID,
  March 2024** — it measures the worst interaction across the session, which is
  why it's hardest to pass. Mobile INP is 2-3x worse (weaker CPUs run the same
  hydration far slower); test on real mid-range Android.
- **Workflow: field is truth, lab is diagnostic.** CrUX (Search Console / PSI
  field) = what Google sees → RUM (`web-vitals`) = why/which segments →
  Lighthouse/DevTools = how to fix. Gate CI on **TBT** (the lab proxy for INP — a
  lab run can't generate a real interaction) plus numeric LCP/CLS budgets; treat
  the Lighthouse score as a regression detector, not a KPI.
- **Verify the raw response:** `curl -A "GPTBot" <url>` and "View Source" — the
  rendered DOM in DevTools can disagree with what gets indexed.

## Third-party scripts

The primary INP killer — main-thread work no clean app code offsets. **Move,
don't delete:**

- **Server-side tagging (sGTM / Cloudflare Zaraz):** move Meta CAPI, GA4, Ads
  pixels into a server container — removes the JS from the browser and preserves
  attribution under ITP.
- **Edge A/B testing:** choose the variant at the CDN edge (Cloudflare Workers,
  Vercel/Next Middleware) so the correct HTML ships first — eliminates the
  client-side **anti-flicker snippet** that hides the body and inflates LCP/CLS.
  (Test *design* is `landing-page-experimentation`.)
- **Defer / idle-load** chat and heatmaps via `requestIdleCallback` or post-`load`
  — never synchronous in `<head>`. Trade-off: deferring risks attribution loss if
  users bounce first.
- **Partytown:** relocates scripts to a web worker; effective for analytics,
  **breaks on scripts needing synchronous DOM access**.

## On-page SEO & structured data

- Title < 60 chars (keyword near front); description < ~155; one H1 + semantic
  heading hierarchy; **server-rendered canonical** (a JS-injected canonical can be
  missed on Wave 1).
- **JSON-LD server-rendered and matching visible content exactly** — schema for
  content not on the page is a guideline violation, and JS-injected schema is
  invisible to AI crawlers. Common landing-page types: `Organization`, `Product`,
  `Service`, `LocalBusiness`, `BreadcrumbList`. Escape any CMS/user-provided
  values before embedding JSON-LD via `dangerouslySetInnerHTML` (XSS risk).
- **[VERSION-GATE] FAQ rich results stopped appearing May 7, 2026** (Search Console
  filters removed ~June 2026, API ~Aug 2026). The SERP dropdown is gone, but
  `FAQPage` JSON-LD is still valid and parsed by Google and LLMs for entity
  understanding — keeping it is fine, no need to rip it out. Verify against
  Google's current docs.
- **Schema is NOT a direct ranking factor** (Google/Mueller, repeatedly); its
  value is entity disambiguation and AI-citation legibility.

## AI crawlers & GEO

- GPTBot / ClaudeBot / PerplexityBot run **no JavaScript** — GEO/AEO is not a
  separate implementation, it's the same server-HTML discipline with zero
  tolerance for JS-dependent content. One contested finding: visible semantic HTML
  may matter more to LLMs than deep JSON-LD blocks.
- **[VERSION-GATE / stay skeptical]** `llms.txt` (reported unreliable / unused by
  Google), WebMCP, and a Lighthouse "Agentic Browsing" category are emerging —
  treat as directional and verify before relying on them.

## Fact ledger

- **STABLE:** content/metadata/schema in initial server HTML; AI crawlers run no
  JS; CWV thresholds LCP≤2.5s/INP≤200ms/CLS≤0.1 at p75; INP replaced FID (Mar
  2024); field (CrUX) > lab (Lighthouse); CWV is a tie-breaker; third-party scripts
  are the top INP cause; move to sGTM/edge/defer.
- **DRIFT-PRONE (version-gate + link a verify URL):** FAQ deprecation dates;
  Lighthouse "Agentic Browsing"; `llms.txt`/WebMCP; Astro-vs-Next bundle numbers;
  Googlebot 2MB truncation; EAA/ADA dates (see `landing-page-accessibility`).
- **UNVERIFIABLE-STAT (discard or heavily caveat):** "66% Astro vs 30% Next CrUX
  pass"; "70% smaller bundles"; all conversion/revenue-lift %; the "~300ms
  AI-crawler timeout" (single-source — discard).

## Sources

- web.dev — INP: https://web.dev/articles/inp · Core Web Vitals: https://web.dev/articles/vitals
- Google Search Central — Core Web Vitals: https://developers.google.com/search/docs/appearance/core-web-vitals
- Google Search Central — structured data intro: https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
- Vercel — how Core Web Vitals affect SEO: https://vercel.com/blog/how-core-web-vitals-affect-seo
- DebugBear — lab vs field data: https://www.debugbear.com/blog/lighthouse-lab-data-not-matching-field-data
- Philip Walton — performant A/B testing at the edge: https://philipwalton.com/articles/performant-a-b-testing-with-cloudflare-workers/
- Simo Ahava — server-side tagging: https://www.simoahava.com/analytics/server-side-tagging-google-tag-manager/
