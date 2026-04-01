# CLAUDE.md — Project State Template

Paste this at the start of every session. Fill in the brackets.

---

```markdown
# [CLIENT NAME] Clone — Project State

## Status
Current phase: PHASE 0 — Infrastructure setup
Last completed: Project scoping
Next action: Cloudflare setup → nameserver switch → Vercel account

## Project
Target site:   [URL to clone]
Client:        [Client name]
Your site:     [Client's domain]
Start date:    [date]
Deadline:      [date — 4 weeks from start]
Stack:         Next.js 14 · TypeScript · Tailwind · shadcn/ui · Vercel

## Decisions Locked (never re-discuss)
- Website hosting: Vercel free (under client account)
- Email hosting: Hostinger Premium (unchanged)
- DNS + CDN: Cloudflare free plan
- SMTP: smtp.hostinger.com:587 STARTTLS
- Forms: Cloudflare Turnstile + Nodemailer + Google Sheets
- Analytics: GA4 + Google Search Console
- CMS: Dropped V1 — quoted separately
- Admin panel: Dropped V1 — quoted separately
- Language: English only V1
- Performance: Lighthouse 90+ mobile
- Images: Real client photos + Unsplash placeholders

## Scope V1 (Locked)
IN:
  Website clone · Product templates · Forms · SEO
  Mobile responsive · 404 pages · GA4 · Privacy policy
  Cloudflare + Vercel deployment · Handover package
OUT:
  Admin CMS · Analytics dashboard · Partner portal · Multilingual

## Infrastructure Checklist
- [ ] Cloudflare account created
- [ ] All DNS records verified in Cloudflare
- [ ] DMARC updated to p=quarantine
- [ ] Nameservers switched at registrar
- [ ] Email tested post-migration
- [ ] Vercel account created (client email)
- [ ] GitHub repo created (private)
- [ ] Vercel connected to GitHub repo
- [ ] Domain connected in Vercel
- [ ] A record updated in Cloudflare → Vercel IP

## Content Checklist
- [ ] Client questionnaire sent
- [ ] Logo received (SVG/PNG)
- [ ] Navigation structure confirmed
- [ ] Product list received ([N] products, [N] categories)
- [ ] Company facts verified (year, installs, partners, projects)
- [ ] Product images received
- [ ] Product descriptions received
- [ ] Inquiry email address confirmed
- [ ] GST number received
- [ ] Office address confirmed

## Phase Completion
- [ ] Phase 0 — Infrastructure + DNS
- [ ] Phase 1 — Audit (/clone-audit)
- [ ] Phase 2 — Design System (/clone-extract)
- [ ] Phase 3 — Build Plan (/clone-plan)
- [ ] Phase 4 — Build (/clone-build)
- [ ] Phase 5 — SEO + Performance
- [ ] Phase 6 — Deploy + QA (/clone-qa)

## Build Progress (Phase 4)
Sprint 1 — Foundation
  - [ ] Next.js init + TypeScript + Tailwind + shadcn
  - [ ] tailwind.config.js tokens
  - [ ] globals.css variables
  - [ ] Fonts installed
Sprint 2 — Atoms
  - [ ] Button (primary/secondary/ghost)
  - [ ] Badge
  - [ ] Typography components
Sprint 3 — Shared Organisms
  - [ ] Navbar
  - [ ] Footer
Sprint 4 — Homepage Sections
  - [ ] Hero
  - [ ] Stats strip
  - [ ] Feature cards
  - [ ] Logo strip
  - [ ] Testimonials
  - [ ] CTA section
Sprint 5 — Homepage Assembly
  - [ ] Homepage page.tsx
  - [ ] Responsive test (320/768/1024/1440)
Sprint 6 — Product Templates
  - [ ] Template A ([layout name])
  - [ ] Template B ([layout name])
  - [ ] Template C ([layout name])
  [add more as discovered in audit]
Sprint 7 — Content Pages
  - [ ] [page name] /[slug]
  - [ ] [page name] /[slug]
  [add after clone-plan]
Sprint 8 — Forms + Functions
  - [ ] Contact form + server action
  - [ ] Turnstile integration
  - [ ] Nodemailer setup
  - [ ] Google Sheets logging
  - [ ] Rate limiting
Sprint 9 — SEO
  - [ ] generateMetadata() per page
  - [ ] JSON-LD schema (Organization)
  - [ ] JSON-LD schema (Product pages)
  - [ ] Sitemap.xml
  - [ ] robots.txt
  - [ ] GA4 integration
  - [ ] Search Console verified
  - [ ] Alt text all images
Sprint 10 — Polish
  - [ ] 404 page
  - [ ] 500 error page
  - [ ] Privacy policy page
  - [ ] Cookie consent banner
  - [ ] Loading states

## Security Checklist
- [ ] .gitignore created before first commit
- [ ] .env.local created (never committed)
- [ ] Security headers in next.config.js
- [ ] Turnstile on all forms
- [ ] Server-side validation on all forms
- [ ] Rate limiting configured
- [ ] TypeScript strict mode on

## Files Produced
Phase 1: clone-audit_[domain].md
Phase 2: clone-extract_[domain]_tokens.json
         clone-extract_[domain]_components.md
         clone-extract_[domain]_stack.md
Phase 3: clone-plan_[domain]_sitemap.md
         clone-plan_[domain]_pages.md
         clone-plan_[domain]_build-order.md
Phase 6: clone-qa_[domain]_report.md

## Handover Checklist (Week 4)
- [ ] GitHub repo transferred to client account
- [ ] .env.example file created
- [ ] README.md written
- [ ] Credentials document prepared
- [ ] Phase 2 quote (admin panel) prepared
- [ ] Client walkthrough done

## DNS Reference (infinityxglobal.com)
A      @         → 76.76.21.21 (Vercel — update after first deploy)
CNAME  www       → cname.vercel-dns.com (Vercel)
MX     @ pri 5   → mx1.hostinger.com (NEVER change)
MX     @ pri 10  → mx2.hostinger.com (NEVER change)
SMTP             → smtp.hostinger.com:587 STARTTLS

## Notes
[Add project-specific notes here as you go]
```
