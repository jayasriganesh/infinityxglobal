# InfinityX Global — Clone Project State
# ─────────────────────────────────────────────────────────────
# HOW TO USE THIS FILE
# Lives at project root: D:\infinityxglobal-claude\CLAUDE.md
# Claude CLI reads it automatically every session.
# Never re-explain the project. Just type /clone-master.
# Tick completed items and save at the end of every session.
# ─────────────────────────────────────────────────────────────

## Status
Current phase:  PHASE 3 — Complete (build plan delivered)
Last completed: sitemap.md + pages.md + build-order.md — 16 sprints defined, ~48hr build estimate
Next action:    Human review build-order.md → approve sprint sequence → /clone-build
Blocked by:     Human approval of build order and sprint priorities

## Project
Target site:    https://www.maxhub.com/in/
Client:         InfinityX Global
Client site:    https://infinityxglobal.com
GitHub repo:    https://github.com/jayasriganesh/infinityxglobal.git
Start date:     28-03-26
Deadline:       25-04-26 (4 weeks from sign-off)
Stack:          Next.js 14 · TypeScript · Tailwind CSS · shadcn/ui · Vercel
Local path:     D:\infinityxglobal-claude\

## Session Rules (read every session)
- ONE session = ONE goal only (one component, one phase step)
- Never re-discuss locked decisions
- Never build without checking Phase Completion checklist first
- Update this file at the end of every session
- Browser automation for audit: claude --mcp-server playwright
- All other sessions: plain claude

## Decisions Locked (never re-discuss)
- Hosting:        Vercel free (under client account)
- Email:          Hostinger Premium — NEVER touch MX records
- DNS + CDN:      Cloudflare free plan
- SMTP:           smtp.hostinger.com:587 STARTTLS
- Forms:          Cloudflare Turnstile + Nodemailer + Google Sheets logging
- Analytics:      GA4 + Google Search Console
- CMS:            Dropped V1 — quote separately as Phase 2
- Admin panel:    Dropped V1 — quote separately
- Language:       English only V1
- Performance:    Lighthouse 90+ mobile
- Images:         Real client photos + Unsplash placeholders until assets arrive
- Pricing:        All contact-for-pricing — no fixed prices V1
- Agents:         Single focused sessions only — no parallel agents

## Scope V1 (Locked — ₹30,000 fixed)
IN:
  Website clone (MAXHUB India structure)
  Product pages — IFP (priority) + Kiosks + CCTV + Display Solutions catalogue
  Contact form + Get Quote form (separate)
  File attachments on forms (RFQ docs)
  Mobile responsive (320 / 768 / 1024 / 1440)
  404 page · GA4 · Privacy policy
  Cloudflare + Vercel deployment
  Handover package (README + credentials + .env.example)

OUT (change order if requested):
  Admin CMS · Analytics dashboard · Partner portal · Multilingual
  Blog CMS (shell page only) · Careers CMS (static form only)
  WhatsApp Business button (TBD after trip)
  Product brochure PDF downloads (TBD after assets)
  Own-brand display launch page (confidential — mid-2026)

## Client Facts (Verified — questionnaire 28-03-26)
Founded:            2014
Installations:      4000+
Certifications:     MSME, ISO, GeM
Channel partners:   DO NOT DISPLAY
Govt projects:      DO NOT DISPLAY
Case studies:       DO NOT DISPLAY
Primary contact:    Mahesh Rekapalli — CEO — 9666305689
Approval authority: Mahesh Rekapalli
Phone (public):     8228822849 / 9640778582
Email (public):     contact@infinityxglobal.com
Form submissions:   contact@infinityxglobal.com
GST number:         PENDING — Laxman sir
Address:            Developer has it
Social media:       None — not V1
WhatsApp button:    TBD after trip — change order
Feedback SLA:       48hr max

## Competitors (SEO positioning)
BenQ · MAXHUB · Newline · EyeRIS · Sensus

## Expansion Markets (SEO)
Current: Pan India
Target:  Nepal · Sri Lanka · Bhutan · Saudi Arabia / UAE · Africa

## Product Catalogue (Full — confirmed by client)
Priority V1:  IFP (3 series × 3 sizes — spec pending) · Kiosks · CCTV
After trip:   All display products below (change order — additional billing)
Pricing:      ALL contact-for-pricing — no exceptions

### Category A — LED Video Walls
- COB LED Video Wall               P0.9–P1.87 · 2K/4K/8K
- Transparent LED Video Wall       indoor P1.9–P3.9 IP45 · outdoor P3.9–P15.6 IP65
- All-in-One LED Display           120/138/150/169 inches · Android OS
- Scape Indoor LED Wall            P0.7–P4 · 100K hr
- Scape Outdoor LED Wall           P2.5–P10 · IP65 · 100K hr
- 3D Anamorphic Active LED Wall    indoor P1.2–P1.4 · outdoor P2.5–P8
- Scape Flexible / Curved Indoor
- LCD Video Wall                   55 inch · bezel 0.88/1.78/3.5mm

### Category B — Kiosks & Standees
- Ultra Slim Kiosk Touch/Non-Touch  32/43/55/65 inches · Android ← V1
- Dual Side Hanging Kiosk           37/43 inches
- Dual Side Standing Kiosk          55 inches
- A-Frame Standee                   32/43/55 inches
- Outdoor High Brightness Display   43/55/65/75 inches · IP65
- Health Kiosk                      75 health checkups

### Category C — Specialty Displays
- Waterproof Mirror TV              custom · IP65 · 4K · anti-fog
- Interactive Smart Mirror TV       24–98 inches · capacitive touch
- Customized Touch Table            32/43/55 inches · Android/Windows
- Holographic Display               24–86 inches
- Flat Wall Display                 24–55 inches
- Digi Board                        32–65 inches · airports/govt
- Smart Frame                       21.5 inches
- Professional Monitors             24–110 inches · 400 nits
- Pane                              P2.5 · cascade · <35kg
- LED Cubes                         P1.8 & P2.5 · 100K hr
- Square TV                         33 inches
- Canvas TV                         43 inches · digital painting
- Lift & Learn Solution             sensor-based
- Stitch Television                 24/32 inches · leather framing
- Cast Television                   24/32 inches · metal framing
- Cascade Display                   24/32 inches

### Category D — Software
- CAST CMS                          web-based · multi-zone · 24/7

### CCTV
- Confirmed in scope · spec PENDING after trip

## Site Structure & URLs (MAXHUB nav → InfinityX mapping)

### Navigation Structure
MAXHUB India nav:
  Products → Interactive Flat Panel · Commercial Display · LED Display
           · Unified Communication · Capture System · Accessories · Software

InfinityX nav (mirrors MAXHUB structure):
  Products → Interactive Flat Panels   [V1]
           → Commercial Display        [AFTER TRIP]
           → LED Display               [AFTER TRIP]
           → Kiosks & Signage          [V1]
           → CCTV & Security           [V1]
           → Software (CAST CMS)       [AFTER TRIP]

### URL Structure by Category

#### Category 1 — Interactive Flat Panels [V1]
/products/interactive-flat-panels/              category landing
/products/interactive-flat-panels/[series-1]/   placeholder (names pending)
/products/interactive-flat-panels/[series-2]/   placeholder
/products/interactive-flat-panels/[series-3]/   placeholder

Build with placeholder series names (Series A/B/C) → swap after trip
MAXHUB reference structure: V6 Classic/ViewPro/Transcend series

#### Category 2 — Commercial Display [AFTER TRIP]
/products/commercial-display/professional-monitors/
/products/commercial-display/flat-wall-display/
/products/commercial-display/square-tv/
/products/commercial-display/canvas-tv/
/products/commercial-display/digi-board/
/products/commercial-display/smart-frame/
/products/commercial-display/waterproof-mirror-tv/
/products/commercial-display/interactive-smart-mirror-tv/
/products/commercial-display/stitch-television/
/products/commercial-display/cast-television/

#### Category 3 — LED Display [AFTER TRIP]
/products/led-display/                          category landing
/products/led-display/cob-led-video-wall/
/products/led-display/transparent-led-video-wall/
/products/led-display/all-in-one-led/
/products/led-display/scape-indoor/
/products/led-display/scape-outdoor/
/products/led-display/scape-flexible-curved/
/products/led-display/3d-anamorphic-led-wall/
/products/led-display/lcd-video-wall/
/products/led-display/led-cubes/
/products/led-display/pane/
/products/led-display/cascade-display/

#### Category 4 — Kiosks & Signage [V1 + AFTER TRIP]
/products/kiosks/                               category landing
/products/kiosks/ultra-slim-kiosk/              [V1] Touch + Non-Touch · 32/43/55/65"
/products/kiosks/dual-side-hanging-kiosk/       [AFTER TRIP]
/products/kiosks/dual-side-standing-kiosk/      [AFTER TRIP]
/products/kiosks/a-frame-standee/               [AFTER TRIP]
/products/kiosks/outdoor-high-brightness-display/ [AFTER TRIP]
/products/kiosks/health-kiosk/                  [AFTER TRIP]
/products/kiosks/holographic-display/           [AFTER TRIP]
/products/kiosks/customized-touch-table/        [AFTER TRIP]
/products/kiosks/lift-and-learn/                [AFTER TRIP]

#### Category 5 — CCTV & Security [V1]
/products/cctv/                                 category landing
/products/cctv/[product-1]/                     spec PENDING (build placeholder)
/products/cctv/[product-2]/                     spec PENDING (build placeholder)

#### Category 6 — Software [AFTER TRIP]
/products/software/                             category landing
/products/software/cast-cms/                    CAST CMS page

### Page Count Summary
V1 BUILD (now):
  /products/ (all products listing)
  IFP:    4 pages (category + 3 series)
  Kiosks: 2 pages (category + 1 product)
  CCTV:   3 pages (category + 2 products)
  ─────────────────────────────────────
  Total V1 product pages: ~9 pages

AFTER TRIP (change order):
  Commercial Display:  10 pages
  LED Display:         12 pages
  Kiosks remaining:     9 pages
  Software:             2 pages
  ─────────────────────────────────────
  Total additional:    ~33 pages

GRAND TOTAL:          ~42 product pages

### URL Slug Rules
- All lowercase, hyphenated
- No special characters
- Category always first: /products/[category]/[product]/
- Series pages: /products/interactive-flat-panels/[series-name]/
- Never use query strings for products

### Product Page Build Notes
1. Product listing page (/products/) uses category cards — mirrors MAXHUB nav dropdown
2. Category pages show all products: image + name + brief spec + Get Quote CTA
3. Product page structure: hero image → key specs → applications → Get Quote form
4. IFP pages: placeholder series names (Series A/B/C) → client fills after trip
5. CCTV pages: generic surveillance content → client fills after trip
6. "Get Quote" button on every product page → links to /get-quote with product name pre-filled
7. No pricing anywhere on product pages — all contact-for-pricing

## Infrastructure Checklist
- [x] GitHub repo — https://github.com/jayasriganesh/infinityxglobal.git
- [x] Next.js 14 boilerplate initialized
- [x] Tailwind CSS + shadcn/ui installed
- [x] .claude/commands/ — all 6 slash commands in place
- [x] .claude/settings.json configured
- [x] CLAUDE.md placed in project root
- [x] Playwright MCP server installed (@playwright/mcp@latest)
- [ ] Cloudflare account created (friend — weekend)
- [ ] DNS records verified in Cloudflare
- [ ] DMARC updated to p=quarantine
- [ ] Nameservers switched at registrar
- [ ] Email tested post-migration
- [ ] Vercel account created (client email)
- [ ] Vercel connected to GitHub repo
- [ ] Domain connected in Vercel
- [ ] A record → 76.76.21.21 in Cloudflare

## Content Checklist
- [x] Questionnaire signed (28-03-26)
- [x] Full product catalogue received (PDF)
- [x] Logo exists — SVG + dark variant (not received yet)
- [ ] Logo files received
- [ ] IFP series names + exact sizes
- [ ] Product images (after trip)
- [ ] Product spec sheets (after trip)
- [ ] CCTV details (after trip)
- [ ] GST number (Laxman sir)
- [ ] Privacy policy document
- [ ] Testimonials (3–5 · name + company + designation)
- [ ] Brochure PDF decision (after trip)

## Phase Completion
- [ ] Phase 0 — Infrastructure + DNS
- [x] Phase 1 — /clone-audit https://www.maxhub.com/in/
- [x] Phase 2 — /clone-extract
- [x] Phase 3 — /clone-plan
- [ ] Phase 4 — /clone-build
- [ ] Phase 5 — SEO + Performance
- [ ] Phase 6 — /clone-qa

## Pipeline Trigger Prompts

### Phase 1 + 2 Combined (run with: claude --mcp-server playwright)
```
Read CLAUDE.md and the Site Structure & URLs section carefully.
Then read .claude/commands/references/phase-contracts.md.

PHASE 1: Run /clone-audit on https://www.maxhub.com/in/
Output → docs/research/clone-audit_maxhub-india.md
Pause and confirm file is written before continuing.

PHASE 2: Run /clone-extract using docs/research/clone-audit_maxhub-india.md as input.
Skip: Unified Communication, Capture System, Accessories (InfinityX does not sell these).
Output → docs/research/tokens.json + docs/research/components.md + docs/research/stack.md
Print all [PROPOSED] tokens for review when done.

Update CLAUDE.md after both phases — tick Phase 1 and 2 complete, next action = human review tokens.json before /clone-plan.
```

### Phase 3 (run with: claude)
```
Read CLAUDE.md and Site Structure & URLs section.
Read docs/research/tokens.json + components.md + stack.md.
Read .claude/commands/references/phase-contracts.md.

Run /clone-plan.
Output → docs/research/sitemap.md + docs/research/pages.md + docs/research/build-order.md

Update CLAUDE.md — tick Phase 3 complete, next action = /clone-build.
```

### Phase 4 (run with: claude)
```
Read CLAUDE.md Build Progress checklist.
Read docs/research/tokens.json + components.md + stack.md + build-order.md.
Read .claude/commands/references/phase-contracts.md.

Run /clone-build — ONE SPRINT AT A TIME.
Follow build-order.md exactly — tick items in CLAUDE.md as you complete them.

After each sprint: commit, update CLAUDE.md, stop.
Next session: resume from next unchecked sprint.
```

### Phase 6 (run with: claude --mcp-server playwright)
```
Read CLAUDE.md.
Read docs/research/clone-audit_maxhub-india.md (original audit).

Run /clone-qa.
Output → docs/research/qa-report.md

Update CLAUDE.md — tick Phase 6 complete, project status = READY FOR HANDOVER.
```

## Build Progress (Phase 4)
Sprint 1 — Foundation
  - [x] Next.js 14 + TypeScript + Tailwind + shadcn init
  - [ ] tailwind.config.ts — tokens from audit
  - [ ] globals.css — CSS variables from audit
  - [ ] Fonts installed

Sprint 2 — Atoms
  - [ ] Button (primary / secondary / ghost)
  - [ ] Badge
  - [ ] Typography components

Sprint 3 — Shared Organisms
  - [ ] Navbar (desktop + mobile)
  - [ ] Footer

Sprint 4 — Homepage Sections
  - [ ] Hero
  - [ ] Stats strip (Founded 2014 · 4000+ Installs · MSME · ISO · GeM)
  - [ ] Feature cards
  - [ ] Product category strip
  - [ ] Testimonials
  - [ ] CTA section

Sprint 5 — Homepage Assembly
  - [ ] app/page.tsx assembled
  - [ ] Responsive test (320 / 768 / 1024 / 1440)

Sprint 6 — Product Templates
  - [ ] IFP product template
  - [ ] Kiosk product template
  - [ ] Generic display template
  - [ ] CCTV template

Sprint 7 — Content Pages
  - [ ] /products
  - [ ] /about
  - [ ] /contact
  - [ ] /get-quote
  - [ ] /blog (static shell)
  - [ ] /careers (static form)
  - [ ] /privacy-policy
  - [ ] /not-found

Sprint 8 — Forms + Functions
  - [ ] Contact form + server action
  - [ ] Get Quote form (separate)
  - [ ] File attachment (PDF/DOC max 10MB)
  - [ ] Cloudflare Turnstile on all forms
  - [ ] Nodemailer → contact@infinityxglobal.com
  - [ ] Google Sheets logging
  - [ ] Rate limiting

Sprint 9 — SEO
  - [ ] generateMetadata() per page
  - [ ] JSON-LD Organization schema
  - [ ] JSON-LD Product schema
  - [ ] sitemap.xml
  - [ ] robots.txt
  - [ ] GA4 integration
  - [ ] Search Console verified
  - [ ] Alt text all images
  - [ ] OG images per page

Sprint 10 — Polish
  - [ ] 404 page (branded)
  - [ ] 500 error page
  - [ ] Cookie consent banner
  - [ ] Loading states / skeletons
  - [ ] Security headers (next.config.mjs)

## Security Checklist
- [x] .gitignore in place
- [ ] .env.local created (never commit)
- [ ] .env.example created (commit — no secrets)
- [ ] Security headers in next.config.mjs
- [ ] Turnstile on all forms
- [ ] Server-side validation on all inputs
- [ ] Rate limiting on server actions
- [ ] TypeScript strict mode ON

## Research Files (produced by pipeline)
Phase 1:  docs/research/clone-audit_maxhub-india.md
Phase 2:  docs/research/tokens.json
          docs/research/components.md
          docs/research/stack.md
Phase 3:  docs/research/sitemap.md
          docs/research/pages.md
          docs/research/build-order.md
Phase 6:  docs/research/qa-report.md

## Handover Checklist (Week 4)
- [ ] GitHub repo transferred to client account
- [ ] .env.example complete
- [ ] README.md written
- [ ] Credentials document (Vercel + Cloudflare + GA4 + Turnstile)
- [ ] Phase 2 quote (CMS / admin panel)
- [ ] Client walkthrough with Mahesh Rekapalli

## DNS Reference — NEVER TOUCH MX RECORDS
A      @        →  76.76.21.21           (Vercel — after first deploy)
CNAME  www      →  cname.vercel-dns.com  (Vercel)
MX     @ pri 5  →  mx1.hostinger.com     (NEVER CHANGE)
MX     @ pri 10 →  mx2.hostinger.com     (NEVER CHANGE)
SMTP            →  smtp.hostinger.com:587 STARTTLS

## Change Order Tracker
- [ ] WhatsApp Business button — TBD after trip
- [ ] Product brochure downloads — TBD after assets
- [ ] 30+ display product pages (from PDF) — after trip
- [ ] Own-brand display launch page — CONFIDENTIAL · mid-2026

## Notes
- Client on business trip — returning end of month with product details + images
- All display products from PDF are confirmed in scope — added after trip as change order
- CCTV confirmed in scope — spec pending
- InfinityX SmartClass = education IFP product name
- Own display production mid-2026 — CONFIDENTIAL — not on V1
- Project clock: 28-03-26 (both parties signed)
- Logo: client has SVG + dark variant — chase before trip ends
- Friend: Cloudflare + Vercel — available weekend
