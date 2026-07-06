# Setup prompt — build a landing page end to end

The eight skills in this repo compose into one workflow. Paste the `/goal` block
below into a fresh session (with this repo's skills installed), replacing the
**Brief** line with your product, audience, and goal. It sequences the skills,
parallelizes only the steps whose file surfaces are disjoint, and puts a
verification gate after each build/harden step.

Keep it honest: every percentage lift the skills mention is directional, not a
promise, and version-dependent facts (shadcn Base UI, Tailwind v4, PPR, FAQ
schema, EAA/ADA dates, the React2Shell CVE fix list) must be re-checked against
current docs. If you have the `/cross` skill installed, cross-validate the
strategy artifacts (step 1) before writing code.

```
/goal Build a complete, high-converting, accessible, fast landing page by composing this repo's eight skills. Work autonomously; invoke each skill by name. NEVER run git commit or git push - leave all changes in the working tree for me to review.

Brief: <describe the product, target audience, and primary conversion goal - replace this line>.

Order and gates (do not skip a gate):

1. Strategy (run in parallel - disjoint artifacts, no shared files):
   - /landing-page-structure -> the section plan: one sequenced argument, hero anatomy, form-field count, CTA/proof placement.
   - /landing-page-copywriting -> the copy for those sections, voice-of-customer-sourced, no invented claims/stats. If no customer research exists, gather a lightweight sample first.
   - /visual-design-system -> a DESIGN.md (role-based tokens + a Don'ts list) and the palette/type/spacing budget; wire the lint/visual/a11y enforcement gates.
   Gate: a written section plan, a copy doc, and a DESIGN.md exist before any code.

2. Build (sequential - shared code):
   - /nextjs-landing-page -> implement the page in Next.js App Router from the plan + copy + tokens. Client boundaries at leaves; metadata server-rendered; next/image priority on the LCP hero. Verify the app builds and the page renders; confirm React/Next versions are patched for CVE-2025-55182.
   - /scroll-motion -> add motion only where it earns it, lowest tier first (CSS). Gate: page still builds; animation honors prefers-reduced-motion.

3. Harden (sequential - shared code):
   - /web-vitals-and-seo -> server-render content/metadata/schema; verify with curl -A "GPTBot" <url> and View Source; measure Core Web Vitals. Gate: rankable content in initial HTML; LCP/INP/CLS budgets met locally.
   - /landing-page-accessibility -> WCAG 2.2 AA. Gate: axe-core clean in CI + one manual keyboard and one screen-reader walkthrough; no overlay widget.

4. Measure:
   - /landing-page-experimentation -> server/edge variant assignment (no anti-flicker snippet), server-side exposure logging, real-event instrumentation. For a low-traffic page, set up qualitative review instead of an underpowered A/B test.

Rules:
- Treat every % lift figure in the skills as directional, never a promise.
- Version-gate volatile facts against current docs before relying on them.
- Parallelize only step 1 (disjoint files); the rest is sequential because it edits shared code.
- Verify after each build/harden step by actually running the app, not just reading the diff.

Definition of done: the page builds; content/metadata/schema are server-rendered; Core Web Vitals budgets met; WCAG 2.2 AA passes (automated + manual); copy is customer-sourced and slop-free; the design system is enforced by gates; measurement is wired. No git commits/pushes - summarize what changed for my review.
```
