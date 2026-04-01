---
name: clone-plan
description: >
  Trigger this skill immediately when the user types /clone-plan. This is Stage 3 of the MWP
  Website Clone Pipeline. It reads the clone-extract outputs (design-tokens.json, components.md,
  stack.md) and produces a complete build plan: sitemap.md (every page with its component list
  and copy), page-plan.md (section-by-section breakdown for each page), and build-order.md
  (the exact sequence to code components and pages). Requires clone-extract files as input.
  Also trigger when user says "plan the build", "what pages do I build", "give me the build
  order", "create a sitemap for the clone", or "I've extracted the tokens, now plan".
---

# /clone-plan — Build Architecture & Page Planning (MWP Stage 3)

## Trigger
Activate when the user types `/clone-plan`.

**Required inputs (Layer 3 from Stage 2):**
- `clone-extract_[domain]_tokens.json`
- `clone-extract_[domain]_components.md`
- `clone-extract_[domain]_stack.md`

If these are missing, ask the user to run `/clone-extract` first.
Also accept: the `clone-audit_[domain].md` if extract hasn't been run yet (pull IA map from it).

---

## Role — What This Stage Does
This is **MWP Stage 03_architecture**. You read the stable Layer 3 reference files and produce
the **build architecture** — the complete structural plan for what to code, in what order, with
what components on each page.

This is the highest-leverage human review gate in the pipeline. The paper's U-shape intervention
finding applies here: **heavy human editing at this stage is expected and correct.** Getting the
architecture wrong means everything built in Stage 4 is wrong.

Your output should be edited by the human before `/clone-build` begins.

---

## Pre-Planning Protocol

Before writing any file, extract from the Layer 3 inputs:

1. **From components.md:** Full component inventory with confidence levels
2. **From tokens.json:** Design system + any [PROPOSED] tokens needing human confirmation
3. **From stack.md:** Technology choices + Tailwind config
4. **From audit IA map (if available):** Every observed page + nav structure

Build an internal page × component matrix:
```
Page → [list of components it needs] → [copy source] → [complexity]
```

---

## Output 1 — `sitemap.md`

Save to: `/mnt/user-data/outputs/clone-plan_[domain]_sitemap.md`

```markdown
# Site Architecture — [domain] Clone

Source files: clone-extract_[domain]_*.* 
Date: [date]

---

## Route Map

```
app/
├── page.tsx                    → Homepage
├── [observed-slug]/
│   └── page.tsx               → [Page name from nav]
├── [observed-slug]/
│   └── page.tsx               → [Page name from nav]
...
└── contact/
    └── page.tsx               → Contact page
```

## Page Index

| Route | Page Name | Source Nav Item | Priority | Components Needed |
|---|---|---|---|---|
| `/` | Homepage | — | 1 (build first) | Navbar, Hero, Stats, Features, Logos, Testimonials, CTA, Footer |
| `/[slug]` | [Name] | [nav text] → [observed href] | 2 | [list] |
| ... | | | | |

## Shared Components (built once, used everywhere)
- **Navbar** — appears on all pages
- **Footer** — appears on all pages
- **[any other shared components from audit]**

## Page-Specific Components
| Page | Unique Components |
|---|---|
| Homepage | [list] |
| [page] | [list] |

---

## Component Dependency Map

Build shared components before page-specific ones:

```
Phase 1 — Design System (no deps)
  └── tailwind.config.js tokens
  └── globals.css variables

Phase 2 — Atoms (no deps)
  └── Button (variants: primary, secondary, ghost)
  └── Badge
  └── Typography components (H1, H2, H3, Body, Caption)

Phase 3 — Molecules (depend on atoms)
  └── NavItem
  └── Card base
  └── FormField

Phase 4 — Organisms (depend on molecules)
  └── Navbar
  └── Footer
  └── Hero
  └── StatsStrip
  └── FeatureCards
  └── LogoStrip
  └── TestimonialRow
  └── ContactForm
  └── [any other observed organisms]

Phase 5 — Pages (depend on all organisms)
  └── Homepage (assemble organisms)
  └── [other pages in priority order]
```
```

---

## Output 2 — `page-plan.md`

Save to: `/mnt/user-data/outputs/clone-plan_[domain]_pages.md`

For every page in the sitemap, produce a section-by-section plan:

```markdown
# Page-by-Page Build Plan — [domain] Clone

---

## PAGE: Homepage (`/`)

**Build priority:** 1
**Estimated complexity:** [Low / Medium / High]

### Section Breakdown

| # | Section | Component | Copy Source | Animation | Confidence |
|---|---|---|---|---|---|
| 1 | Navigation | Navbar | [exact observed nav items] | Floating on scroll (requires browser verification) | High |
| 2 | Hero | Hero | H1: "[observed text]" / CTA: "[observed text]" | [observed or proposed] | High |
| 3 | Stats | StatsStrip | "[observed stat 1]", "[observed stat 2]" | NumberTicker on scroll | High |
| 4 | Features | FeatureCards | [section heading from audit] | [observed or proposed] | Medium |
| 5 | Logos | LogoStrip | [observed client/partner names] | Marquee (if 8+) | High |
| 6 | Testimonials | TestimonialRow | [observed or absent] | [proposed] | Low |
| 7 | CTA Band | CTASection | "[observed CTA text]" | [proposed] | High |
| 8 | Footer | Footer | [from audit footer links] | None | High |

**Confidence note:** Sections marked Low confidence need human input before building.

### Exact Copy for This Page
```
Hero H1:       "[observed exact text]"
Hero sub:      "[observed exact text or ABSENT — write new copy]"
Hero CTA:      "[observed text]" → [href]
Stats:         [list each stat + label exactly as observed]
Section H2s:   [list in order]
Footer tagline:"[observed]"
```

### Open Questions Before Building
- [ ] [Any ambiguous component that needs human decision]
- [ ] [Any proposed token that needs sign-off]

---

## PAGE: [Next page slug]

[Same format]
```

---

## Output 3 — `build-order.md`

Save to: `/mnt/user-data/outputs/clone-plan_[domain]_build-order.md`

```markdown
# Build Order — [domain] Clone

This is the exact sequence for /clone-build to follow.
Complete each item before moving to the next. Check off as done.

---

## Sprint 1 — Foundation (no UI yet)

- [ ] Initialize Next.js project with TypeScript + Tailwind + shadcn/ui
- [ ] Paste `tailwind.config.js` extensions from `stack.md`
- [ ] Set `globals.css` with CSS custom properties from `tokens.json`
- [ ] Install animation library: [from stack.md]
- [ ] Install fonts: [exact Google Fonts import from tokens.json]

## Sprint 2 — Atoms

- [ ] Button component (variants: primary / secondary / ghost / icon)
      → Ref: components.md #BUTTON
- [ ] Badge component
- [ ] Typography wrappers (H1, H2, H3, Body, Caption)

## Sprint 3 — Shared Organisms

- [ ] Navbar
      → Ref: components.md #NAVBAR
      → Copy: [observed nav items from page-plan.md homepage]
- [ ] Footer
      → Ref: components.md #FOOTER
      → Copy: [observed footer links + copyright]

## Sprint 4 — Homepage Organisms (in section order)

- [ ] Hero
      → Ref: components.md #HERO
      → Copy: page-plan.md #Homepage
- [ ] StatsStrip
      → Ref: components.md #STATS STRIP
      → Copy: [exact stats from page-plan.md]
- [ ] FeatureCards
- [ ] LogoStrip
- [ ] TestimonialRow (if observed)
- [ ] CTASection

## Sprint 5 — Homepage Assembly

- [ ] Assemble homepage from Sprint 3+4 components
- [ ] Test responsive at 320px / 768px / 1024px / 1440px

## Sprint 6 — Secondary Pages

[List remaining pages in priority order from sitemap.md]

## Sprint 7 — QA

- [ ] Run /clone-qa
```

---

## Output Rules

1. Save all three files to `/mnt/user-data/outputs/`
2. Call `present_files` with all three files
3. Post a 3-line chat summary:
   - Total page count + total component count to build
   - The one architectural decision that most needs human review
   - Estimated build sprints to completion
4. Confidence levels must flow from the audit — low-confidence sections = human review required

---

## Human Review Gate (MWP Principle — Most Critical Gate)

After delivering files, explicitly prompt:

> **Stage 3 complete — PLEASE REVIEW BEFORE BUILDING.**
>
> This is the cheapest point to make changes. Check `clone-plan_[domain]_pages.md`:
> - Is every page in the sitemap correct?
> - Are the section orders right on each page?
> - Any sections missing or wrong?
> - Any open questions in the page plans that need your answer?
>
> Edit `pages.md` and `sitemap.md` directly.
> When you're satisfied, run `/clone-build [component-or-page-name]` to start building.
