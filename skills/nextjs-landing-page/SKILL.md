---
name: nextjs-landing-page
description: Implements a landing page in Next.js App Router with React + shadcn/ui + Tailwind - RSC/client-component boundaries, shadcn setup, next/image & next/font, and the streaming-metadata gotcha. Use when asked to build, scaffold, or fix a Next.js landing page, resolve 'use client' errors, set up shadcn, or fix a blank-head/unindexed page. Not for copy, visual design, animation, or SEO strategy.
---

# nextjs-landing-page

Build the page in Next.js App Router with client boundaries at the leaves, so the
bundle stays small and the copy stays server-rendered.

## The failure this fixes

The single most-documented Next.js failure: putting `"use client"` too high — at
a `layout.tsx` or page shell. Because the directive is a *module boundary*,
everything imported below it (and everything those imports pull in) gets bundled
for the browser — community reports describe 200-400KB+ of dead client JS. Worse
for a landing page: a client-boundary layout strips server-rendered copy and
blocks `metadata`/`generateMetadata` (mutually exclusive with `"use client"` in
one file), so the initial HTML ships with a blank/late `<head>` and unindexed
content — the whole SEO point of Next.js, gone.

## When to use / when NOT to use

Use to implement/scaffold/fix a Next.js App Router landing page: RSC boundaries,
shadcn/Tailwind setup, `next/image`/`next/font`, rendering strategy, and the
framework gotchas.

Not for: copy (`landing-page-copywriting`), section order
(`landing-page-structure`), palette/type (`visual-design-system`), animation
design (`scroll-motion` — this skill only wires the `"use client"` boundary it
needs), Core Web Vitals / SEO strategy (`web-vitals-and-seo`), or WCAG
(`landing-page-accessibility`).

## Workflow

1. **Default to Server Components; push `"use client"` to leaves.** Every `app/`
   file is a Server Component (0 KB JS) unless marked. Keep hero copy, proof,
   pricing, and FAQ server-rendered; mark only the interactive leaf (a button, a
   form, the animated element). Auditing boundary placement is the highest-leverage
   bundle fix.
2. **Keep server subtrees server-rendered inside client parents.** Pass Server
   Components as `children` to a Client Component and they stay on the server — the
   escape hatch for heavy/server-only logic. Use `next/dynamic` for heavy
   client-only libs (charts, the `motion` API); `optimizePackageImports` for
   many-export packages.
3. **Choose rendering: SSG/ISR by default** for marketing; SSR only for
   genuinely personalized content; PPR / `use cache` (Next 16) for mixed
   static+dynamic. **Stream content, never stream metadata** — an async
   `generateMetadata` that resolves late injects the `<head>` after the initial
   HTML (an SEO bug). Keep metadata synchronous/fast.
4. **Set up shadcn/Tailwind correctly.** shadcn is *open code* (`npx shadcn add`
   vendors source into your repo — you own it, upstream sync is manual). The July
   2026 Base UI default, Tailwind v4 `@theme inline`, and the layering model are
   version-dependent — see `references/nextjs-implementation.md` and verify against
   the changelog.
5. **Wire images and fonts for zero CLS.** `next/image` with `priority` on the LCP
   hero and `sizes` set; `next/font` self-hosts and generates fallback metrics.
   Details in the reference.
6. **Patch the critical CVE.** Verify the app is on a Next.js/React version fixed
   for **CVE-2025-55182 ("React2Shell")** — see the reference and the official
   advisory.

## The rules

- **`"use client"` is contagious up the import tree** — it bundles the whole
  subtree, not just the leaf.
- **`"use client"` and `metadata`/`generateMetadata` cannot coexist** in one file.
- **Children-as-props keeps subtrees server-rendered** even inside a client parent.
- **SSG/ISR is the marketing default;** server-only libs (markdown, formatting)
  ship zero bytes when kept in Server Components.
- **Stream content, not metadata.**
- **shadcn output needs de-defaulting** — v0/Cursor emit stock "grayscale PDF"
  shadcn; theme the tokens (the aesthetic choices are `visual-design-system`).
- **Vercel coupling is real** — PPR/edge features are Vercel-tuned; note portability
  cost. For a zero-interactivity page, Astro's islands (~0-15 KB) beat Next's
  baseline — point there when the page won't grow into an app.

## Output

A Next.js App Router landing page with client boundaries at the leaves, metadata
server-rendered and synchronous, `next/image`/`next/font` wired for zero CLS,
shadcn/Tailwind set up with themed tokens, SSG/ISR rendering, and version-gated
claims (Base UI, Tailwind v4, PPR, the CVE) checked against current docs.

## References

- `references/nextjs-implementation.md` - RSC boundary patterns, shadcn open-code
  + Base UI switch + Tailwind v4 gotchas, image/font specifics, PPR/rendering,
  CVE-2025-55182 details with fixed versions, and the fact ledger.
