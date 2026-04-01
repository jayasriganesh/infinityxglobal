# Review Gates — Exact Prompts

These are the human review gate prompts clone-master uses.
Never skip a gate. Never auto-proceed through a gate.

---

## Gate 1 — After Phase 2 (Design System)

> **Phase 2 complete — review before planning.**
>
> Open `clone-extract_[domain]_tokens.json` and check:
> - Every token marked `[PROPOSED — not in audit]` needs your sign-off
> - Primary color correct?
> - Font families correct?
> - Any token you want to change?
>
> Open `clone-extract_[domain]_components.md` and check:
> - Are all components from the target site captured?
> - Any components I missed?
> - Any structural details that look wrong?
>
> Edit the files directly if needed.
> Type **"approved"** or **/clone** when ready to generate the build plan.

---

## Gate 2 — After Phase 3 (Build Plan) — MOST CRITICAL

> **Phase 3 complete — THIS IS THE MOST IMPORTANT REVIEW.**
>
> Everything built in Phase 4 comes from this plan.
> Wrong plan = wrong build. Changes here are free. Changes in Phase 4 cost time.
>
> Open `clone-plan_[domain]_pages.md` and verify:
> - Is every page in the sitemap correct?
> - Is the section order on each page correct?
> - Is the copy for each section right?
> - Any open questions marked in the plan that need your answer?
>
> Open `clone-plan_[domain]_build-order.md` and verify:
> - Does the build sequence make sense?
> - Any component missing from the list?
>
> Edit the files directly.
> Type **"approved"** or **/clone** when ready to start building.

---

## Gate 3 — After Each Component (Phase 4)

> **[Component name] built.**
>
> Before the next component — check:
> - Does the copy match the target site exactly?
> - Does the layout match?
> - Any token (color, font, spacing) that looks off?
> - Mobile layout correct at 375px?
>
> Edit the .tsx file directly if needed.
> Type **/clone** to build the next component: **[next item]**
> Or type **/clone-build [name]** to jump to a specific one.

---

## Gate 4 — After Phase 6 (QA Report)

> **QA complete. Fidelity score: [X]/10**
>
> Critical blockers: [N] — must fix before delivery
> Medium issues: [N] — fix before client review
> Low issues: [N] — optional polish
>
> Your fix list has two types:
> - **Output fixes** ([N]): edit the component files directly
> - **Source fixes** ([N]): edit pipeline files so the bug never recurs
>
> Recommended order: source fixes first → re-run affected /clone-build steps → output fixes last.
>
> When all critical blockers are cleared:
> Type **"ship ready"** to get the handover checklist.

---

## Gate 5 — Handover

> **Site is ship-ready.**
>
> Before handing over, complete:
> - [ ] GitHub repo transferred to client account
> - [ ] .env.example created with all variable names (no values)
> - [ ] README.md written (how to run + how to deploy)
> - [ ] Credentials document prepared
> - [ ] GA4 + Search Console access given to client
> - [ ] Cloudflare access given to client
> - [ ] Vercel access given to client
> - [ ] Phase 2 quote (admin panel) prepared
> - [ ] Client walkthrough scheduled
>
> Type **"handover complete"** to close the project.
