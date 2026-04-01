---
name: clone-master
description: >
  Trigger this skill immediately when the user types /clone or /clone-master (with or without
  a URL). This is the MWP master orchestrator for the complete website clone pipeline. It reads
  the project CLAUDE.md state file, determines the current phase, and either runs the correct
  sub-skill automatically or tells the user exactly what to do next. Manages all 5 stages:
  clone-audit → clone-extract → clone-plan → clone-build → clone-qa. Tracks what is done,
  what is in progress, and what is next. One trigger handles the entire project from first audit
  to final QA. Also trigger when user says "where are we", "what's next", "continue the project",
  "resume the clone", "start the clone pipeline", or pastes a CLAUDE.md into the conversation.
---

# /clone-master — MWP Pipeline Orchestrator

## Trigger
Activate when the user types `/clone` or `/clone-master`.
Also activate when the user pastes a CLAUDE.md block into the conversation.

---

## What This Skill Does

This is the **Layer 0 + Layer 1 orchestrator** of the entire clone pipeline.

It does three things every time it runs:

1. **Reads state** — from CLAUDE.md in the conversation or asks for it
2. **Determines position** — which phase is active, what's done, what's blocked
3. **Acts** — either runs the next sub-skill automatically or gives the human the exact next action

You never need to remember which skill to run. `/clone` always knows.

---

## First Run Protocol (No CLAUDE.md Yet)

If no CLAUDE.md exists in the conversation:

Ask the user:
> "Is this a new project or are you resuming an existing one?"

**New project:**
→ Ask for the target URL
→ Generate a fresh CLAUDE.md (see template in references/claude-md-template.md)
→ Immediately run `/clone-audit [url]`

**Resuming:**
→ Ask them to paste their CLAUDE.md
→ Read it → determine current phase → continue from there

---

## State Reading Protocol

When CLAUDE.md is present, extract:

```
Current phase:     which PHASE is active
Last completed:    what was just finished
Next action:       what the CLAUDE.md says to do next
Blocked by:        any open checklist items marked [ ]
Content status:    what's received vs waiting
Infrastructure:    what's configured vs pending
```

Then decide: **auto-run** or **report + prompt**.

---

## Decision Logic

```
IF current phase = PHASE 0 (Infrastructure)
  AND Cloudflare not set up
  → Report infrastructure checklist status
  → Tell user exact next step
  → Do NOT run audit yet (infrastructure must be done first)

IF current phase = PHASE 0
  AND infrastructure complete
  → Auto-run: /clone-audit [url from CLAUDE.md]
  → Update CLAUDE.md phase to PHASE 1

IF current phase = PHASE 1 (Audit complete)
  → Auto-run: /clone-extract
  → Update CLAUDE.md phase to PHASE 2

IF current phase = PHASE 2 (Extract complete)
  → Human review gate (tokens.json + components.md)
  → Wait for user confirmation before proceeding
  → On confirmation: auto-run /clone-plan
  → Update CLAUDE.md phase to PHASE 3

IF current phase = PHASE 3 (Plan complete)
  → CRITICAL human review gate (page plan)
  → Wait for explicit user approval
  → On approval: auto-run /clone-build (first item in build-order.md)
  → Update CLAUDE.md phase to PHASE 4

IF current phase = PHASE 4 (Building)
  → Check build-order.md for next unchecked item
  → Auto-run: /clone-build [next item]
  → After each build: prompt human review before next item
  → When all items checked: auto-run /clone-qa

IF current phase = PHASE 5 (QA)
  → Auto-run: /clone-qa
  → Report: output fixes vs source fixes
  → When all critical blockers resolved: declare ship-ready
```

---

## After Every Sub-Skill Runs

After any sub-skill completes, `/clone-master` must:

1. **Update the CLAUDE.md** — mark completed items, update current phase
2. **Print the updated CLAUDE.md block** — so user can copy-paste and save it
3. **State clearly:**
   ```
   ✓ DONE:     [what just completed]
   ⚡ DOING:   [what's active right now]
   → NEXT:     [exact next action]
   ⏸ WAITING:  [anything blocked on human input]
   ```
4. **Prompt the human review gate if required**

---

## Human Review Gates (Never Skip These)

These are MWP's non-negotiable pause points. Auto-running through them is wrong.

```
Gate 1 — After clone-extract:
  Human must approve tokens.json and components.md
  Specifically: any [PROPOSED — not in audit] tokens
  Prompt: "Review the token file. Any changes before planning?"

Gate 2 — After clone-plan (MOST CRITICAL):
  Human must approve page structure and section order
  Prompt: "This is the cheapest point to change anything.
           Is every page and section correct?"

Gate 3 — After each clone-build component:
  Human reviews before next component is built
  Prompt: "Does this match the target site? Any fixes?"

Gate 4 — After clone-qa:
  Human decides: fix now or ship
  Prompt: "N critical blockers found. Fix or ship?"
```

---

## Token Efficiency Mode

To save tokens across sessions:

**At session start:** User pastes CLAUDE.md → `/clone-master` reads it → continues immediately. No re-explaining the project.

**At session end:** `/clone-master` prints the updated CLAUDE.md → user saves it → next session starts clean.

**During build phase:** `/clone-build` runs one component per invocation. User reviews. Continues. This keeps context windows small and focused per stage — exactly the MWP principle.

**Never load all context at once.** Each stage only sees its own Layer 3 files.

---

## The Status Dashboard

When the user types `/clone` with no URL and no specific action, print this dashboard:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  INFINITYXGLOBAL.COM — Clone Pipeline
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROJECT
  Target:    https://www.maxhub.com/in/
  Client:    InfinityX Global
  Deadline:  [from CLAUDE.md]
  Week:      [calculate from start date]

PHASE PROGRESS
  [✓] Phase 0 — Infrastructure
  [✓] Phase 1 — Audit
  [~] Phase 2 — Design System      ← ACTIVE
  [ ] Phase 3 — Templates
  [ ] Phase 4 — Content Pages
  [ ] Phase 5 — SEO + Performance
  [ ] Phase 6 — Deploy + QA

CURRENT ACTION
  Running: /clone-extract
  File ready: clone-audit_maxhub.com.md ✓

WAITING ON HUMAN
  → Review tokens.json after extract completes

CONTENT STATUS
  Logo:           [✓ received / ✗ waiting]
  Product list:   [✓ received / ✗ waiting]
  Product images: [✓ received / ✗ waiting]

INFRASTRUCTURE
  Cloudflare:  [✓ / ✗]
  Vercel:      [✓ / ✗]
  DNS switch:  [✓ / ✗]
  Email test:  [✓ / ✗]

SKILLS AVAILABLE
  /clone-audit    /clone-extract    /clone-plan
  /clone-build    /clone-qa         /clone-master

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Type /clone to continue from current position.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CLAUDE.md Update Protocol

After every action, output the full updated CLAUDE.md inside a code block so the user can copy it:

```
━━━ SAVE THIS — Updated CLAUDE.md ━━━
[full CLAUDE.md content here]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Tell the user: "Copy this and save it. Paste it at the start of your next session."

---

## References

Read these when needed — do not load all at once:

- `references/claude-md-template.md` — Full CLAUDE.md template for new projects
- `references/phase-contracts.md` — What each phase inputs, does, and outputs
- `references/review-gates.md` — Exact prompts for each human review gate
- `references/status-codes.md` — What ✓ / ~ / ✗ / ⏸ mean in the dashboard
