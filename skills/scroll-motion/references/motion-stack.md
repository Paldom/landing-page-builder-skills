# The Scroll-Motion Stack

Reference for `scroll-motion`. Choose by animation *purpose*; protect INP; honor
reduced motion.

**Contents:** [Tiered decision](#tiered-decision) · [CSS gotchas](#css-gotchas) ·
[GSAP pitfalls](#gsap-pitfalls) · [Accessibility floor](#accessibility-floor) ·
[Fact ledger](#fact-ledger) · [Sources](#sources)

## Tiered decision

```
needs pinning / multi-element scrub / SVG path?  -> GSAP + ScrollTrigger
React component state / gesture / exit transition? -> Motion
immersive 3D as the differentiator?              -> react-three-fiber (+ fallback)
else (reveal / progress / parallax)              -> CSS animation-timeline
```

| Need | Tool | Bundle | INP |
|---|---|---|---|
| Decorative reveals, fade/slide, progress bar, simple parallax, staggered grids | Native CSS `scroll()`/`view()` | 0 KB | None (compositor) |
| Pinned sections, scrubbed timelines, horizontal-in-vertical, SVG path/DrawSVG | GSAP + ScrollTrigger | ~9 KB core, ~18-30 KB w/ plugins | Low if optimized |
| React state, gestures, layout animation, `AnimatePresence` | Motion (ex-Framer Motion) | ~11-31 KB | Higher; `LazyMotion` trims it |
| Immersive 3D scroll scenes (premium only) | react-three-fiber | Heaviest (WebGL) | High; prefer pre-rendered image sequences |

Stacking all four on one page is normal, not a compromise: CSS for cheap
high-frequency effects, GSAP for one or two hero sequences, Motion for the
surrounding UI.

## CSS gotchas

- **Animate only `transform`, `opacity`, `filter`.** Never `width`/`height`/`top`/
  `left`/`box-shadow` — layout/paint every frame defeats the whole point.
- **Compositor eligibility is all-or-nothing per `@keyframes` block.** One
  layout-triggering property drops the entire animation to the main thread. Split
  mixed animations into separate keyframes on separate timelines.
- **`animation-fill-mode: both` is mandatory** — without it, revealed elements
  snap back to their pre-animation state (the #1 "why does my reveal flicker" bug).
- **`scroll()` vs `view()`:** `scroll()` tracks a container's total scroll progress
  (progress bars, sticky-header transforms); `view()` tracks one element's transit
  through the viewport (per-element reveals) via `animation-range: entry/cover/exit`.
- **Scroll-*triggered* vs scroll-*driven*:** Chrome 145+ can fire a time-based
  animation once at a scroll threshold (a declarative `IntersectionObserver`
  replacement, e.g. sticky-nav color change) — distinct from continuously scrubbed
  scroll-*driven* animation.
- **`@supports (animation-timeline: scroll())` fallback is required.** Firefox
  lacks it; unsupported browsers must render content in its final visible state,
  never hidden.

## GSAP pitfalls

- GSAP is **free for commercial use** (Webflow acquired GreenSock Oct 2024; all
  plugins free since April 2025) under the **GreenSock standard license — not
  MIT.** Verify current terms at gsap.com/pricing before relying on the license.
- **`pin: true`** inserts a pin-spacer that doubles element height in flow and
  switches to `position: fixed`; mobile dynamic viewport height + late fonts
  desync it. Prefer `scrub` without `pin` when a full-viewport takeover isn't
  essential.
- In React, use `useGSAP({ scope })` for auto-cleanup, or manually
  `ScrollTrigger.getAll().forEach(t => t.kill())` on unmount to avoid leaks.

## Accessibility floor

`prefers-reduced-motion` is the floor, **not** sufficient on its own:

- The **View Transitions API does NOT auto-respect** the preference — gate
  `document.startViewTransition()` explicitly.
- **Scroll-linked (scrubbed) motion carries vestibular risk** because movement
  couples to a physical gesture. Parallax/zoom on primary content are top
  offenders.
- **Reveal content must be structurally present regardless of animation** —
  compositor animations give assistive tech no signal; content that only appears
  via animation is invisible when animation is off. Wire focus/`aria-live`
  manually.
- **Reduce, don't delete:** swap displacement for an opacity cross-fade; consider
  a `--motion-scale-factor` token. Reset **both** `animation` and
  `animation-timeline` in the reduced-motion block, or scroll motion still fires.
- **WCAG 2.2.2 (Pause/Stop/Hide):** anything auto-playing over 5s needs a control.
- Per-tool hooks: **Motion** → `useReducedMotion()` + `<MotionConfig reducedMotion>`
  + `LazyMotion`; **GSAP** → `gsap.matchMedia()` /
  `window.matchMedia('(prefers-reduced-motion: reduce)')`; **Tailwind** →
  `motion-safe:` / `motion-reduce:`.

## Fact ledger

- **STABLE:** animate only transform/opacity/filter; compositor eligibility is
  per-keyframe all-or-nothing; the purpose-based tiering and hybrid-stack norm;
  `animation-fill-mode: both`; `scroll()` vs `view()`; GSAP free (standard
  license, not MIT) with `useGSAP()` cleanup; Framer Motion → "Motion" rebrand;
  scroll-jacking is an anti-pattern; the ~5-scene cap and narrative-first rule.
- **DRIFT-PRONE (version-gate + verify on caniuse):** `animation-timeline` support
  % (sources say 83-90%); Firefox status (partial / behind
  `dom.animations-api.scroll-driven.enabled`); Safari baseline; Chrome 145
  scroll-triggered timing.
- **UNVERIFIABLE-STAT (never quote as fact):** all ms INP deltas ("GSAP +8-25ms",
  "Motion +30-70ms", NRK "0.16ms tasks"); engagement/conversion lifts ("+62%
  dwell", "+40% conversion"); "CSS cuts CPU 40-60%"; ">50% of mobile users have
  reduce-motion on"; "CSS replaces ~80% of ScrollTrigger" (directional).

## Sources

- Chrome for Developers — NRK scroll-driven case study: https://developer.chrome.com/blog/nrk-casestudy
- MDN — CSS scroll-driven animations: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll-driven_animations
- WebKit — scroll-driven animations with just CSS: https://webkit.org/blog/17101/a-guide-to-scroll-driven-animations-with-just-css/
- NN/g — scroll-jacking 101: https://www.nngroup.com/articles/scrolljacking-101/
- GSAP — ScrollTrigger / React / accessibility: https://gsap.com/docs/v3/Plugins/ScrollTrigger/ · https://gsap.com/resources/React/
- Motion — scroll animations / GSAP vs Motion: https://motion.dev/docs/react-scroll-animations
- Lenis (native-mode caveat): https://github.com/darkroomengineering/lenis
- Support check: https://caniuse.com/mdn-css_properties_animation-timeline
