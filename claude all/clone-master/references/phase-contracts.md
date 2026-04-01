# Phase Contracts — What Each Phase Does

Quick reference for clone-master to determine what a phase needs and produces.

---

## Phase 0 — Infrastructure
Input:   Nothing
Does:    Cloudflare setup, nameserver switch, Vercel account, GitHub repo
Output:  Live domain on Cloudflare + Vercel account ready
Gate:    Infrastructure checklist 100% complete before Phase 1

## Phase 1 — Audit (/clone-audit)
Input:   Target URL
Does:    Forensic scrape of target site — tokens, components, copy, IA
Output:  clone-audit_[domain].md
Gate:    Human reviews audit before Phase 2
Token cost: Medium (fetches 3-4 pages)

## Phase 2 — Design System (/clone-extract)
Input:   clone-audit_[domain].md (Layer 4)
Does:    Extracts design tokens, component specs, stack decisions
Output:  tokens.json + components.md + stack.md (all become Layer 3)
Gate:    HUMAN MUST APPROVE all [PROPOSED] tokens before Phase 3
Token cost: Low (reads one file, produces three)

## Phase 3 — Build Plan (/clone-plan)
Input:   tokens.json + components.md + stack.md (Layer 3)
Does:    Generates sitemap, page-by-page section breakdown, build order
Output:  sitemap.md + pages.md + build-order.md
Gate:    MOST CRITICAL — human approves full architecture before any code
Token cost: Low-Medium

## Phase 4 — Build (/clone-build)
Input:   All Layer 3 files + pages.md + build-order.md
Does:    Generates ONE component or page per invocation
Output:  .tsx files per component/page
Gate:    Human reviews each component before next is built
Token cost: Medium per component (keep sessions focused)
Note:    Run /clone-build [name] for specific component
         Run /clone-build for next in build-order.md

## Phase 5 — SEO + Performance
Input:   All built pages
Does:    Adds metadata, schema, sitemap, GA4, performance optimizations
Output:  Updated page files + next.config.js + sitemap.xml + robots.txt
Gate:    Lighthouse audit passes 90+ before Phase 6
Token cost: Medium

## Phase 6 — Deploy + QA (/clone-qa)
Input:   Built code + Layer 3 files + live target site re-fetch
Does:    Fidelity comparison, source-traces every gap
Output:  qa-report.md with output fixes + source fixes
Gate:    All critical blockers resolved before handover
Token cost: Medium-High (re-fetches target site)
