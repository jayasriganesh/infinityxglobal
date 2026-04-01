---
name: clone-extract
description: >
  Trigger this skill immediately when the user types /clone-extract. This is Stage 2 of the
  MWP Website Clone Pipeline. It reads the clone-audit output (Layer 4 working artifact) and
  produces three structured files: design-tokens.json (all colors, typography, spacing as
  production-ready CSS variables), components.md (full component spec with states and variants),
  and stack.md (technology decisions for the clone build). This stage requires a clone-audit .md
  file as input — if missing, ask the user to run /clone-audit first. Always produce all three
  output files. Also trigger when user says "extract design tokens", "generate component spec",
  "what components do I need to build", or "I've done the audit, now what".
---

# /clone-extract — Design Token & Component Extraction (MWP Stage 2)

## Trigger
Activate when the user types `/clone-extract`.

**Required input:** The `clone-audit_[domain].md` file from Stage 1 (`/clone-audit`).
If it's not in the conversation or referenced, ask: "Please paste or share your clone-audit output first."

---

## Role — What This Stage Does
This is **MWP Stage 02_extract**. You read the audit (Layer 4) and produce three structured files that
become the **Layer 3 reference material** for all downstream stages (clone-plan, clone-build, clone-qa).

These files are the factory configuration. Once set, they should not change per-page — they are the
design system that governs every build decision downstream.

---

## Layer 3 vs Layer 4 Distinction (Critical)

| What you're reading | What you're producing |
|---|---|
| `clone-audit_[domain].md` → Layer 4 (working artifact from this run) | `design-tokens.json` → Layer 3 (stable reference for all future stages) |
| | `components.md` → Layer 3 (stable component spec) |
| | `stack.md` → Layer 3 (stable technology decisions) |

**The audit is the raw material. The three output files are the refined factory spec.**

---

## Pre-Extraction Protocol

Before writing any file, read the entire audit and build these internal maps:

**Token Map:** Every observed color → proposed CSS variable name
**Font Map:** Every observed font → role (display / heading / body / mono)
**Spacing Map:** Every observed Tailwind spacing class → 8px-base scale equivalent
**Component Map:** Every component from the audit → its required props, states, and variants

Gaps in the audit (tokens marked **(requires browser verification)**) → make a design decision,
label it `[PROPOSED — not in audit]`, and document your reasoning.

---

## Output 1 — `design-tokens.json`

Save to: `/mnt/user-data/outputs/clone-extract_[domain]_tokens.json`

```json
{
  "meta": {
    "source_audit": "clone-audit_[domain].md",
    "target_site": "[url]",
    "extraction_date": "[date]",
    "confidence": "observed | partially-observed | proposed"
  },
  "colors": {
    "primary": {
      "value": "#[hex]",
      "source": "observed inline / observed class / [PROPOSED — not in audit]",
      "css_var": "--color-primary",
      "usage": "primary CTA buttons, active nav, accent elements"
    },
    "primary_dark": {
      "value": "#[hex]",
      "source": "[source]",
      "css_var": "--color-primary-dark",
      "usage": "hover state for primary"
    },
    "secondary": { ... },
    "background": { ... },
    "surface": { ... },
    "text_primary": {
      "value": "#[hex]",
      "css_var": "--color-text-primary",
      "wcag_aa_on_bg": "[X:1 — pass/fail]"
    },
    "text_muted": { ... },
    "border": { ... },
    "success": { ... },
    "error": { ... }
  },
  "typography": {
    "font_display": {
      "family": "[font name]",
      "source": "observed <link> / [PROPOSED — not in audit]",
      "weights": ["400", "600", "700"],
      "css_var": "--font-display"
    },
    "font_body": { ... },
    "scale": {
      "xs":   { "size": "12px", "line_height": "16px" },
      "sm":   { "size": "14px", "line_height": "20px" },
      "base": { "size": "16px", "line_height": "24px" },
      "lg":   { "size": "18px", "line_height": "28px" },
      "xl":   { "size": "20px", "line_height": "30px" },
      "2xl":  { "size": "24px", "line_height": "32px" },
      "3xl":  { "size": "30px", "line_height": "36px" },
      "4xl":  { "size": "36px", "line_height": "40px" },
      "5xl":  { "size": "48px", "line_height": "1" },
      "6xl":  { "size": "60px", "line_height": "1" }
    }
  },
  "spacing": {
    "base": "8px",
    "scale": {
      "1": "4px",
      "2": "8px",
      "3": "12px",
      "4": "16px",
      "6": "24px",
      "8": "32px",
      "10": "40px",
      "12": "48px",
      "16": "64px",
      "20": "80px",
      "24": "96px"
    },
    "container_max_width": "[observed or proposed — e.g. 1280px]",
    "container_padding": "[observed — e.g. 24px]"
  },
  "border_radius": {
    "sm": "[observed class or proposed]",
    "md": "[observed class or proposed]",
    "lg": "[observed class or proposed]",
    "full": "9999px"
  },
  "shadows": {
    "sm": "[proposed or observed]",
    "md": "[proposed or observed]",
    "lg": "[proposed or observed]"
  },
  "breakpoints": {
    "sm": "640px",
    "md": "768px",
    "lg": "1024px",
    "xl": "1280px",
    "2xl": "1536px"
  }
}
```

**Confidence rules:**
- `"observed"` → directly extracted from audit's markup evidence
- `"partially-observed"` → class name seen but value requires browser verification
- `"proposed"` → design decision made to fill audit gap — documented in reasoning

---

## Output 2 — `components.md`

Save to: `/mnt/user-data/outputs/clone-extract_[domain]_components.md`

For every component identified in the audit, produce a full spec:

```markdown
# Component Spec — [domain] Clone

Source audit: clone-audit_[domain].md
Date: [date]

---

## [COMPONENT NAME] — e.g. NAVBAR

**Observed class signature:** [exact classes from audit]
**Confidence:** observed / partially-observed / proposed

### Structure
```html
<!-- Structural skeleton — observed from audit -->
<nav class="[classes]">
  <div class="[logo container classes]">...</div>
  <ul class="[nav list classes]">
    <li><a href="[observed href]">[observed text]</a></li>
    <!-- repeat for each observed nav item -->
  </ul>
  <div class="[CTA container classes]">[observed CTA text]</div>
</nav>
```

### Props / Variants
| Variant | Trigger | Classes | Source |
|---|---|---|---|
| Default | on load | [classes] | observed |
| Scrolled | on scroll | [classes] | (requires browser verification) |
| Mobile | <768px | [classes] | (requires browser verification) |

### States
- **Hover:** [observed class or requires browser verification]
- **Active link:** [observed class or requires browser verification]
- **Mobile open:** [requires browser verification]

### Content
- Logo: [observed alt text or src pattern]
- Nav items: [list all observed text + href pairs]
- CTA: "[observed text]" → [observed href]

### Clone Notes
[Any specific notes about what makes this component tricky to clone]

---

## HERO SECTION

[Same format — structure, props, states, content, clone notes]

---

## STATS STRIP

[Same format]

---

## FEATURE CARDS

[Same format]

---

## LOGO/PARTNER STRIP

[Same format]

---

## TESTIMONIALS

[Same format]

---

## CONTACT FORM
[Include: observed <input> types, <select> options, <textarea>, action attribute, submit button text]

---

## FOOTER

[Same format — include all observed footer link groups + copyright text]
```

---

## Output 3 — `stack.md`

Save to: `/mnt/user-data/outputs/clone-extract_[domain]_stack.md`

```markdown
# Technology Stack Decision — [domain] Clone

Source audit: clone-audit_[domain].md
Date: [date]

---

## Target Site Stack (Observed)

| Signal | Observed | Inferred Stack |
|---|---|---|
| Class patterns | [from audit] | [framework] |
| Script src domains | [from audit] | [libraries] |
| Font loading | [from audit] | [font strategy] |
| CDN pattern | [from audit] | [hosting hint] |
| Meta tags | [from audit] | [CMS hint] |

**Target site likely uses:** [inferred stack — labelled (inferred)]

---

## Recommended Clone Stack

Based on audit signals and the target site's complexity rating ([Low/Medium/High]).

```
Framework:      Next.js 14 (App Router)
Language:       TypeScript
Styling:        Tailwind CSS v3
Components:     shadcn/ui (functional base)
Animation:      [Aceternity UI / Magic UI / Framer Motion — chosen based on audit complexity]
Icons:          Lucide React
Fonts:          [exact Google Fonts observed or proposed]
Hosting:        Vercel (recommended)
```

### Stack Rationale

**Next.js:** [why — SSR for SEO? Static for speed? Based on what audit revealed]
**Tailwind:** [why — matches observed class pattern / proposed for audit-detected framework]
**Animation library choice:** [why this library fits the brand tone observed in audit]

---

## Tailwind Config Additions

Based on extracted tokens, add to `tailwind.config.js`:

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '[hex from tokens]',
        'primary-dark': '[hex from tokens]',
        secondary: '[hex from tokens]',
        // ... all from design-tokens.json
      },
      fontFamily: {
        display: ['[font]', 'sans-serif'],
        body: ['[font]', 'sans-serif'],
      },
      // Any custom spacing or border-radius from tokens
    }
  }
}
```

---

## Layer 3 Files Produced

These files are now **stable reference material** for downstream stages.
Do not regenerate them per-page. Edit the source files if design decisions change.

| File | Used by | Purpose |
|---|---|---|
| `clone-extract_[domain]_tokens.json` | clone-build, clone-qa | Design system tokens |
| `clone-extract_[domain]_components.md` | clone-build, clone-qa | Component spec |
| `clone-extract_[domain]_stack.md` | clone-build | Technology decisions |
```

---

## Output Rules

1. Save all three files to `/mnt/user-data/outputs/`
2. Call `present_files` with all three files
3. Post a 3-line chat summary:
   - Confidence rating on the token extraction (what % observed vs proposed)
   - The one component that will be hardest to clone and why
   - Signal for the user: "Review components.md before running /clone-plan — the [X] component needs your confirmation on [Y]"
4. Mark every gap in the audit with `[PROPOSED — not in audit]` and document reasoning
5. Never silently fill gaps — every design decision made to fill a missing token must be explicit

---

## Human Review Gate (MWP Principle)

After delivering files, explicitly prompt the human:

> **Stage 2 complete. Before running /clone-plan:**
> Review `clone-extract_[domain]_components.md` — specifically check:
> - Are the component structures correct?
> - Are there components I missed from the audit?
> - Any [PROPOSED] tokens you want to change?
>
> Edit the files directly, then run `/clone-plan` when ready.
