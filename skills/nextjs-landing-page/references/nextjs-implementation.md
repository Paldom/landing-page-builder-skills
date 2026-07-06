# Next.js Landing Page Implementation

Reference for `nextjs-landing-page`. Version-gate every framework-version claim
against the linked official docs — App Router, shadcn, and Tailwind move fast.

**Contents:** [RSC boundaries](#rsc-boundaries) · [shadcn & Tailwind v4](#shadcn--tailwind-v4) ·
[Images & fonts](#images--fonts) · [Rendering strategy](#rendering-strategy) ·
[CVE-2025-55182 (React2Shell)](#cve-2025-55182-react2shell) · [Fact ledger](#fact-ledger) · [Sources](#sources)

## RSC boundaries

- **Server-first default.** Every file under `app/` is a Server Component shipping
  0 KB JS unless it has `"use client"`. Keep hero copy, proof, pricing, FAQ server.
- **Client components at leaf nodes** — buttons, toggles, the animated CTA, a
  chat widget — never shared layouts or page shells.
- **`"use client"` is contagious.** The directive bundles the whole imported
  subtree; auditing its placement is the top bundle fix (reported 40-81%
  first-load-JS cuts — directional).
- **Children-as-props escape hatch.** Pass Server Components as `children` to a
  Client Component and they stay server-rendered.
- **`next/dynamic`** for heavy client-only libs (charts, rich text, the `motion`
  API); `dynamic(() => import(...), { ssr: false })` also fixes strict-CSP breakage.
- **`optimizePackageImports`** in `next.config` for large icon/util packages.
- **`"use client"` ≠ metadata:** a file with the directive cannot export
  `metadata`/`generateMetadata`; keep metadata in server files.

## shadcn & Tailwind v4

- **Open code, not a dependency.** `npx shadcn add button` vendors `button.tsx`
  into `components/ui/` — you own and edit it; there is no version bump. The
  tradeoff is a real maintenance tax: upstream security/bug fixes need manual
  diff-and-merge.
- **[VERSION-GATE] Base UI default (July 2026).** New `shadcn init` scaffolds Base
  UI; Radix stays supported via `npx shadcn init -b radix`. Key API change:
  Radix's `asChild`/`Slot` → Base UI's explicit `render` prop; `data-state` →
  native ARIA; positioning via Floating UI. Existing apps are not force-migrated;
  a coding-agent migration skill goes component-by-component. Verify at the
  changelog.
- **[VERSION-GATE] Tailwind v4.** Config lives in CSS (`@import "tailwindcss"`,
  no `tailwind.config.js`). Use **`@theme inline`** when tokens reference other CSS
  vars (the standard shadcn pattern) — plain `@theme` inlines values at build time
  and *silently breaks dark-mode overrides* (the most common "why is dark mode
  broken" bug). Don't mix leftover HSL with v4's OKLCH default. Monorepos need a
  `@source` directive pointing at shared UI packages (the JIT scanner skips
  `node_modules`).
- **Production layering:** `components/ui/` (raw) → `components/primitives/`
  (product-aware wrappers) → `components/blocks/` (page sections). Import through
  the wrapper layer so re-syncs have a small blast radius.
- **Escape the default look** with a theme generator (tweakcn) or CLI preset — the
  actual aesthetic decisions belong to `visual-design-system`. shadcn's default
  *styling* can fail WCAG AA (focus ring contrast); depth is
  `landing-page-accessibility`.

## Images & fonts

- **`next/image`:** `priority` on the LCP hero (else it lazy-loads and delays LCP —
  the #1 image regression); `sizes` so mobile doesn't fetch desktop images;
  explicit dimensions or a sized parent for `fill` to avoid CLS. Auto WebP/AVIF.
- **`next/font`:** self-hosts at build time and generates `size-adjust` /
  `ascent-override` metrics matching the fallback — this (not `display: swap`
  alone) drives font-swap CLS to near zero.

## Rendering strategy

- **SSG/ISR by default** for marketing ("public pages"); SSR only for genuinely
  personalized content; CSR only for authenticated tools with no SEO value.
- **[VERSION-GATE] Next 16 mixed pages:** PPR (Partial Prerendering) serves a
  static edge shell with dynamic slots streamed behind `<Suspense>`; `use cache` /
  Cache Components handle mixed static/dynamic. API names are moving — verify.
- **Stream content, never stream metadata** (the async `generateMetadata` SEO bug).
- **Vercel coupling:** PPR/edge features are Vercel-tuned; teams porting off hit
  pricing and the Cloudflare 25MB Edge Runtime cap. For a static page, Astro
  islands (~0-15 KB) beat Next's baseline JS.

## CVE-2025-55182 (React2Shell)

**Verified, real, critical.** An unsafe-deserialization RCE in the React Server
Components Flight protocol (CVSS 10.0), disclosed Dec 3 2025 and actively
exploited in the wild.

- **Affected:** React 19.0, 19.1.0, 19.1.1, 19.2.0; Next.js 15.x and 16.x using
  App Router. A default `create-next-app` production build is exploitable with **no
  developer code changes** — even apps that don't explicitly use server functions,
  as long as they support RSC.
- **Fixed in:** React 19.0.1 / 19.1.2 / 19.2.1; Next.js 15.0.5, 15.1.9, 15.2.6,
  15.3.6, 15.4.8, 15.5.7, and 16.0.7 (Next 13 → 14.2.35). Rebuild and redeploy;
  WAF rules are a stopgap, not a substitute for patching.
- Always confirm against the official advisory before acting (versions may have
  extended since).

## Fact ledger

- **STABLE:** RSC server-first default; `"use client"` contagion;
  children-as-props; SSG/ISR marketing default; `next/image`/`next/font` behavior;
  shadcn open-code + manual sync; stream-content-not-metadata; **CVE-2025-55182 is
  real** with the fixed versions above.
- **DRIFT-PRONE (version-gate):** Base UI default + `asChild`→`render`; Tailwind v4
  `@theme inline`/OKLCH; Next 16 PPR / Cache Components / `use cache` API names;
  the exact CVE fixed-version list (may extend).
- **UNVERIFIABLE-STAT (don't hard-cite):** bundle-cut % (40-81%), "~85KB JS floor",
  named case-study figures (Stripe/Sonos/Netflix/Preply), "14/48 shadcn components
  fail", all sentiment/download numbers.

## Sources

- Server & Client Components: https://nextjs.org/docs/app/getting-started/server-and-client-components
- Images: https://nextjs.org/docs/app/getting-started/images · Fonts: https://nextjs.org/docs/app/getting-started/fonts
- Package bundling / dynamic imports: https://nextjs.org/docs/app/guides/package-bundling
- shadcn Base UI default (changelog): https://ui.shadcn.com/docs/changelog · Tailwind v4: https://ui.shadcn.com/docs/tailwind-v4
- Tailwind v4: https://tailwindcss.com/blog/tailwindcss-v4
- CVE-2025-55182 — React advisory: https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components
