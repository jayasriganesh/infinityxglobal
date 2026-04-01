---
name: clone-audit
description: >
  Trigger this skill immediately when the user types /clone-audit (with or without a URL).
  This is Stage 1 of the MWP Website Clone Pipeline. It performs a forensic, evidence-only
  audit of a target website specifically structured for cloning — extracting every design token,
  component, copy string, IA structure, and technical signal needed for faithful reproduction.
  Unlike /observe (which is a UX critique), clone-audit produces a CLONING BRIEF: a structured
  output file that feeds directly into /clone-extract, /clone-plan, /clone-build, and /clone-qa.
  If no URL is provided, ask for one. Always output a .md file to outputs/. Never produce only
  conversational text. Also trigger when user says "audit this site for cloning", "I want to
  clone [URL]", "start the clone pipeline", or "clone-audit".
---

# /clone-audit — Target Site Forensic Extraction (MWP Stage 1)

## Trigger
Activate when the user types `/clone-audit` — with or without a URL.
If no URL is provided, ask for it. Do not proceed without a confirmed target URL.

---

## Role — What This Stage Does
This is **Layer 2 of MWP Stage 01_observe** applied to website cloning.

You are not critiquing. You are **extracting everything needed to rebuild**.

Your output is a structured Cloning Brief — a Layer 4 working artifact that every downstream stage
(clone-extract, clone-plan, clone-build, clone-qa) will read as their primary input.

---

## Non-Negotiable Evidence Rule (inherited from /observe1)

> **ZERO inferences presented as fact. ZERO predictions.**
>
> Every claim must come from directly observed sources:
> - Raw HTML retrieved via `web_fetch`
> - Exact copy strings on the page
> - Class names, IDs, data attributes
> - Image filenames, CDN URLs, asset paths
> - `href` values (including `javascript:;` dead links)
> - `alt` text values
> - Meta tag content
> - `<link>` and `<script src>` tags
> - Inline `style` attributes
>
> Anything requiring a live browser to confirm → label **(requires browser verification)**
> Anything structurally inferred → label **(inferred)**

---

## Pre-Audit Protocol — Execute Before Writing Anything

### Step 1 — Fetch homepage (full)
```
web_fetch(url=TARGET_URL, html_extraction_method="markdown")
```

Record these exact signals:
- Every `<nav>` link: text + href (mark `javascript:;` as `[DEAD]`)
- Every `<h1>`, `<h2>`, `<h3>` string verbatim
- Every CTA copy string (button text, anchor text)
- Every `<img>` alt text value
- Every `<meta name>` and `<meta property>` content
- Every `<link rel="stylesheet">` and `<script src>` domain
- Every inline `class` pattern (reveals: Tailwind, BEM, Bootstrap, custom)
- Every inline `style` color or dimension value
- CDN domain(s) for images/assets
- Font `<link>` sources or `@import` declarations

### Step 2 — Fetch 2–3 sub-pages
Fetch: primary product/services page + contact page + one category/solution page.
Extract the same signals from each.

### Step 3 — Build internal inventory before writing
Construct these four internal lists before producing any output:

**Color inventory** — every hex/rgb value observed inline + every color-named class (e.g. `text-blue-600`)
**Type inventory** — every font family observed in `<link>` or `class`, every size class (`text-xl`, `text-sm`)
**Component inventory** — every distinct component type with its class signature
**Copy inventory** — all H1s, H2s, CTAs, meta descriptions, stat figures verbatim

---

## Output Structure

Save as: `/mnt/user-data/outputs/clone-audit_[domain].md`

Use **exactly** these 8 sections in order:

---

### 01 — Site Identity

```
Target URL: [url]
Site purpose: [one sentence — what this site sells/does]
Primary audience: [observed from copy + content, not assumed]
Audited pages: [list all fetched URLs]
Audit date: [today's date]
```

---

### 02 — Information Architecture Map

Build the complete structure from observed `<nav>` and footer `<a>` tags only.

```
[domain]/
├── [Nav item text] → [href] [DEAD if javascript:;]
│   ├── [sub-item] → [href]
│   └── [sub-item] → [href]
├── [Nav item text] → [href]
...
Footer links:
  ├── [text] → [href]
  └── [text] → [href]
```

**Journey trace:** From homepage, what is the primary path to the main conversion action?
Name every step: `[page] → [CTA text] → [destination href] → [next page]`

**Dead links found:** [count] — list each one with its position (nav / footer / body)

---

### 03 — Design Token Extraction

This is the most critical section. Extract every observable token.

#### Colors
```
OBSERVED FROM MARKUP:
  Inline style values:     [list each element + its color value]
  Tailwind color classes:  [list every unique color class observed]
  CSS custom properties:   [list any --var-name values observed inline]

OBSERVED FONT LOADING:
  <link> sources:          [exact Google Fonts / CDN URLs]
  Font families named:     [list every font-family value observed]
```

#### Typography
```
OBSERVED CLASSES / STYLES:
  Heading classes:   [e.g. text-4xl, text-2xl — list all observed]
  Body classes:      [e.g. text-base, text-sm — list all observed]
  Weight classes:    [e.g. font-bold, font-semibold — list all observed]
  Font families:     [from <link> tags or inline style]
```

#### Spacing & Layout
```
OBSERVED CLASSES:
  Container classes: [e.g. max-w-7xl, container, mx-auto]
  Padding classes:   [list all observed px-*, py-*, p-* values]
  Gap classes:       [list all observed gap-* values]
  Grid classes:      [list all observed grid-cols-* values]
  Flex classes:      [list all observed flex, justify-*, items-* patterns]
```

---

### 04 — Component Inventory

List every distinct component type observed. For each:

```
[COMPONENT NAME]
  Location:     [page + section observed on]
  Class sig:    [exact class string that identifies it]
  Copy:         [exact text content observed]
  States noted: hover / active / focus / disabled — each marked [observed] or [requires browser verification]
  Children:     [sub-components observed inside it]
  Variants:     [any structural variants observed — e.g. outlined vs filled button]
```

Required components to look for:
- Navbar (desktop + mobile variants)
- Hero section (background, headline, subheading, CTA)
- Stats/numbers strip
- Feature/service cards
- Logo/partner strip
- Testimonials
- Contact form
- Footer
- Any modals or overlays **(requires browser verification)**
- Cookie/consent banners

---

### 05 — Copy Inventory (Verbatim)

Extract exact strings — no paraphrasing, no summarizing.

```
PAGE: [url]
  <title>:           "[exact]"
  <meta desc>:       "[exact]"
  <h1>:              "[exact]"
  <h2> strings:      (list all)
  CTA copy strings:  (list all — note frequency if repeated)
  Trust signals:     (client names, stat figures, awards — exact text)
  Footer tagline:    "[exact]"
```

Repeat for each fetched page.

**CTA Frequency Table:**
| CTA Text | Count | Destination href |
|---|---|---|
| "[text]" | N | [href] |

---

### 06 — Technical Stack Signals

```
FRAMEWORK EVIDENCE:
  Class patterns:       [list observed — reveals Tailwind / Bootstrap / BEM / custom]
  Inferred framework:   [name] (inferred)

ASSET DELIVERY:
  CDN domains:          [list every unique domain serving img/script/font assets]
  Image formats:        [observed extensions — .webp / .png / .jpg / .svg]
  loading="lazy":       [present / absent — cite element if present]
  srcset present:       [yes / no]

SCRIPTS:
  External script src domains: [list all]
  Inline scripts:       [present / absent]

META / SEO:
  <title>:              "[exact]" (each page)
  <meta description>:   "[exact]" (each page)
  <meta viewport>:      "[exact content value]"
  Open Graph tags:      [list all og: properties observed]
  Canonical:            [present / absent — value if present]
  robots meta:          [value if present]

PERFORMANCE FLAGS (markup-observable only):
  Missing alt texts:    [count of alt="" or absent alt]
  Render-blocking hints:[any <script> without async/defer]
  Image optimization:   [srcset absent / present]
```

---

### 07 — Clone Complexity Assessment

Rate each dimension for cloning difficulty:

| Dimension | Observed Evidence | Clone Difficulty |
|---|---|---|
| IA depth | [nav depth + page count observed] | Low / Medium / High |
| Design token clarity | [how many tokens extractable vs inferred] | Low / Medium / High |
| Component count | [N unique components] | Low / Medium / High |
| Animation complexity | [observed / requires browser verification] | Low / Medium / High |
| Form logic | [observed form fields + action] | Low / Medium / High |
| Content volume | [approximate word count observed] | Low / Medium / High |

**Overall clone complexity: Low / Medium / High**
One sentence explaining the rating — grounded in observed evidence.

---

### 08 — Stage Handoff Notes

These are explicit instructions for the next stage (`/clone-extract`):

```
PRIORITY TOKENS:      [top 3 design tokens most critical to capture]
AMBIGUOUS SIGNALS:    [tokens that need browser verification before building]
MISSING PAGES:        [pages linked in nav but not yet fetched]
CRITICAL COMPONENTS:  [the 3 components that define the site's visual identity]
RECOMMENDED APPROACH: [any clone-specific notes — e.g. "hero is image-heavy, need asset sourcing strategy"]
```

---

## Output Rules

1. Save to `/mnt/user-data/outputs/clone-audit_[domain].md`
2. Call `present_files` to deliver immediately
3. Post a 3-line chat summary:
   - Overall clone complexity rating + one-sentence why
   - The single most critical token to get right
   - What `/clone-extract` should prioritize first
4. Every factual claim has a source (class name, href, copy string, CDN URL)
5. **(inferred)** on structural guesses. **(requires browser verification)** on behaviour/animation
6. Tone: extraction brief for a developer who will build from this document — precise, zero filler

---

## Filename Convention
`/clone-audit` on `https://maxhub.com/in/` → `clone-audit_maxhub.com.md`
