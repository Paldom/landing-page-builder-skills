---
name: scroll-motion
description: Adds scroll-triggered animation and motion to a landing page with the right tier by purpose - native CSS scroll-driven animation, GSAP/ScrollTrigger, or Motion - protecting INP and honoring prefers-reduced-motion. Use when asked to add scroll animations, reveals, parallax, a pinned/scrubbed sequence, smooth scrolling, or pick an animation library. Not for copy, layout, palette, or SEO.
---

# scroll-motion

Add motion that runs on the compositor thread, points toward the action, and
degrades gracefully — not a heavy JS library for a fade.

## The failure this fixes

Asked to "add scroll animation", a model reaches for the heaviest tool it knows —
Framer Motion for a decorative fade, Three.js for a hero — when a few lines of
native CSS would run for free on the compositor thread. It then animates
layout-triggering properties (`width`, `height`, `top`, `box-shadow`), forcing
every frame onto the main thread, which competes with interaction handling and
tanks INP and CLS on the mobile devices that are 60%+ of traffic. The second half
of the scar is narrative: it scroll-jacks and gates content behind a scroll
"tunnel" with no exit to pricing/CTA — the most-hated anti-pattern (NN/g) and an
accessibility failure.

## When to use / when NOT to use

Use for any scroll/motion decision: reveals, parallax, progress bars,
pinned/scrubbed sequences, scrollytelling, smooth-scroll, and choosing between
CSS / GSAP / Motion.

Not for: the words (`landing-page-copywriting`), section order
(`landing-page-structure`), palette/type (`visual-design-system`), general
performance/SEO (`web-vitals-and-seo` — this skill owns motion's INP impact
specifically), or full WCAG work (`landing-page-accessibility` — this skill owns
the motion accessibility floor).

## Workflow

1. **Classify the animation by purpose, then pick the tier** (climb from the top;
   drop down only when the tier above structurally can't do it). CSS covers ~80%
   of what ScrollTrigger used to be needed for.

   | Need | Reach for | INP impact |
   |---|---|---|
   | Decorative reveals, fade/slide-in, progress bar, simple parallax, staggered grids | **Native CSS** `animation-timeline: scroll()` / `view()` | None (compositor thread) |
   | Pinned sections, scrubbed timelines, horizontal-in-vertical, SVG path | **GSAP + ScrollTrigger** (via `@gsap/react` `useGSAP()`) | Low if optimized |
   | React component state, gestures, layout animation, `AnimatePresence` exits | **Motion** (formerly Framer Motion) | Higher; use `LazyMotion` |
   | Immersive 3D scroll scenes (brand/premium only) | **react-three-fiber**, sparingly | High; prefer pre-rendered sequences |

   Hybrid stacking all tiers on one page is the normal professional pattern.

2. **Keep it compositor-safe.** Animate only `transform`, `opacity`, `filter`.
   Compositor eligibility is all-or-nothing per `@keyframes` block — one
   layout-triggering property drops the whole animation to the main thread.
3. **Handle the CSS gotchas.** `animation-fill-mode: both` is mandatory (or
   reveals flicker back to their start state); `scroll()` tracks container
   progress, `view()` tracks one element through the viewport; wrap in
   `@supports (animation-timeline: scroll())` with a static, fully-visible
   fallback (Firefox lacks it). Details: `references/motion-stack.md`.
4. **Gate motion for accessibility.** `prefers-reduced-motion` is the floor, not
   sufficient — reduce (swap displacement for an opacity cross-fade), don't
   delete; reveal content must be structurally present regardless of animation;
   auto-play >5s needs a pause control (WCAG 2.2.2). Full rules:
   `references/motion-stack.md`.
5. **Never gate content.** Motion points *toward* the action; keep visible exit
   points and CTAs; never scroll-jack. Cap scrollytelling at ~5 scenes / 2-3 min,
   and apply the litmus test: if removing the scroll-link still reads fine as a
   static page, it was decoration, not scrollytelling — write the narrative in
   prose first.

## The rules

- **Purpose picks the tier, not what's already imported.** A simple reveal in
  Framer Motion is the canonical waste.
- **Only `transform` / `opacity` / `filter`** animate cheaply; everything else
  triggers layout/paint every frame.
- **`animation-fill-mode: both`** on every scroll reveal.
- **`@supports` fallback** because Firefox lacks `animation-timeline`; unsupported
  browsers render the final visible state, never a hidden one.
- **Lenis smooth-scroll breaks `position: sticky`, `IntersectionObserver`, and CSS
  scroll-driven animations** unless run in native mode (`ReactLenis root`). Only
  add it to coordinate GSAP + Motion under one rAF loop.
- **GSAP is free** for commercial use (Webflow/GreenSock, since April 2025),
  including ScrollTrigger and SplitText — it is *not* MIT-licensed; it uses the
  GreenSock standard license. `useGSAP()` auto-cleans tweens on unmount.
- **In Next.js/RSC**, JS animation libraries force a contagious `"use client"`
  boundary — push it to the leaf; CSS scroll-driven animation forces none (another
  reason to default to it). Boundary mechanics live in `nextjs-landing-page`.

## Output

Motion implemented at the lowest-cost tier that fits the purpose, animating only
compositor-safe properties, with `@supports` fallbacks and reduced-motion gating,
no scroll-jacking, and any browser-support claim version-gated against caniuse.

## References

- `references/motion-stack.md` - the full tiered decision, CSS gotchas, the
  accessibility floor, per-library reduced-motion hooks, GSAP pin pitfalls, and
  the fact ledger (stable vs drift-prone vs unverifiable).
