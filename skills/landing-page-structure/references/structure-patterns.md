# Landing Page Structure Patterns

Reference for `landing-page-structure`. Every percentage in the source research
traces to a single case study or vendor blog — treat all magnitudes as
**directional, never a promised outcome.**

**Contents:** [Section-order spines](#section-order-spines) ·
[Hero patterns by funnel temperature](#hero-patterns-by-funnel-temperature) ·
[Form: single vs multi-step](#form-single-vs-multi-step) ·
[Proof placement](#proof-placement) · [Fact ledger](#fact-ledger) · [Sources](#sources)

## Section-order spines

Adapt, don't ship verbatim. The order must express the page's argument.

**B2B SaaS conversion spine (single dominant offer):**
1. Hero — outcome + audience + why-trust + primary CTA
2. Proof strip — logo bar or one specific stat
3. Problem / stakes — the cost of inaction
4. Solution / mechanism — how it works
5. Product tour or bento feature grid
6. Role / use-case section — high-ticket only
7. Pricing — 3 tiers, "Most Popular" middle anchor
8. FAQ — objection handling
9. Final CTA — repeat the primary action + reassurance microcopy

**Narrative CRO arc (proof adjacent to claims):**
Hook (name their exact pain) → Stakes → Mechanism → Proof-for-mechanism →
Objection handling → CTA.

**9-block B2B variant (credibility-first):** Hook → **Proof** → Problem →
Solution → Mechanism → Results → Objection → CTA → FAQ. Moving proof *before* the
problem to establish credibility is a valid, deliberate reordering — still an
argument, not a default stack.

The diagnostic across all spines: **the reorder test.** If sections can be
shuffled without hurting comprehension, the page has no story.

## Hero patterns by funnel temperature

Match hero *style* to how warm the traffic is:

- **Product-shot hero** — real screenshot/UI; solution- and product-aware traffic.
- **Split-screen** — copy left, product right; general SaaS default.
- **Interactive demo / "try it" hero** — product-aware, high-intent.
- **Big-typographic hero** — brand/positioning play; needs strong copy to carry it.
- **Brutalist-minimal** — distinctive brand statement; weaker at explaining.

All heroes still answer *what / who / why-trust / what-next* in the first screen.
Video heroes are a legitimate *style* only if they don't blow the load-time
budget (they can underperform static/single-stat heroes, largely via LCP
penalties — see `web-vitals-and-seo`).

## Form: single vs multi-step

- **Field count first.** Default ~3 fields B2C, ~4-5 B2B. Each added field
  decays conversion roughly monotonically; this is the most reliable form lever.
- **Single-step under ~5 fields** — step-transition overhead exceeds the benefit.
- **Multi-step at ~5+ fields or genuine conditional/sequential logic.** Order:
  low-effort fields first (email, single choice), sensitive fields (phone,
  budget) last; always a labeled step indicator ("Step 2 of 3"); capture email
  on step one so abandonment still yields a lead.
- **Match to intent.** Cold traffic tolerates one field; warm/high-intent traffic
  tolerates progressive profiling. (Form *accessibility* — labels, error
  linking — is `landing-page-accessibility`; testing variants is
  `landing-page-experimentation`.)

## Proof placement

- Put at least one proof element in the first screen.
- Place each proof *adjacent to the specific claim or objection it answers* (ROI
  proof near pricing, security near the form).
- Proof placed too early reads generic; placed only at the bottom it arrives too
  late. Footer-only proof ≈ no proof.
- Cap distinct proof *types* at 3-5; stacking more badges depresses conversion
  (the "used-car-lot" effect). Copy for proof is `landing-page-copywriting`.

## Fact ledger

**STABLE (directionally reliable, cross-corroborated — use as principles):**
- Most visitors never scroll past the first screen; comprehension is a ~3-7s,
  one-shot event.
- Field count is the highest-leverage, most-replicated form lever.
- Sticky CTA and above-fold CTA do not stack additively.
- Message match between ad/query and hero is the top paid-traffic lever.
- Specific, named, quantified proof beats generic "trusted by thousands"; footer
  placement ≈ no proof.
- One dominant CTA beats multiple competing CTAs.
- 3 pricing tiers tend to beat 4-5 (choice overload).

**UNVERIFIABLE-STAT (single case studies / vendor blogs — directional only):**
Form 11→4 fields "+120-160%"; 1-field 13.4% vs 9-field 3.6% CVR; message-match
"+20-50%"; "Trial for free" vs "Sign up" "+104%"; competing CTAs "-266%"; inline
CTA "+121%"; proof adjacent to CTA "+68%"; "Most Popular" badge "+15-25%"; 1s vs
5s load 9.6% vs 3.3% CVR; median CVR ~6.6%; video hero "-7%". None are planning
inputs.

## Sources

- 2,000-page A/B study (CTA/proof placement, hero patterns): https://www.digitalapplied.com/blog/landing-page-conversion-study-2000-pages-tested-2026
- Reorder test / 5-part arc: https://roast.page/blog/landing-page-story-structure
- 9-block B2B sequence, reconciliation cost: https://witscode.com/blogs/high-converting-landing-page
- NN/g — scrolljacking & structure authority: https://www.nngroup.com/articles/scrolljacking-101/
- Section sequencing as continuous argument: https://vantage-design.com/resources/landing-page-content-flow/
- Multi-step form thresholds: https://blog.hubspot.com/marketing/multi-step-forms
