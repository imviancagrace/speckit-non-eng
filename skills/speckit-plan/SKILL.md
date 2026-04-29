---
name: speckit-plan
description: >
  Create an action plan for an initiative that already has a spec. Use this skill
  for /speckit.plan, or whenever someone has defined an initiative and needs to
  figure out HOW to execute it — approach, phases, what gets delivered, what could
  go wrong, who's involved, what's needed. Produces a single plan.md. Works for
  any domain; no technical knowledge required.
---

## User Input

```text
$ARGUMENTS
```

Consider any context provided (e.g., constraints, approach preferences, team size, timeline).

---

## What You're Doing

Translate the spec (WHAT and WHY) into a plan (HOW). A good plan answers:
- What's the overall approach?
- What happens in each phase, and what does each phase deliver?
- What could go wrong?
- What's needed to execute?

One document. No unnecessary artifacts.

---

## Step 1: Find the Spec

1. Read `.speckit/feature.json` → `feature_directory`.
2. If not found, scan `specs/` for the most recently modified directory.
3. Read `FEATURE_DIR/spec.md`. If it doesn't exist, tell the user to run `/speckit.specify` first.
4. Extract these fields from the spec's **Context** section — they drive the plan:
   - **Deadline** → sets phase target dates and overall timeline
   - **Team** → populates "What's Needed → People" and informs realistic phase durations
   - **Constraints** → shapes the Approach and Risks table
   - **Priority (MVP)** → identifies which phase is the minimum viable delivery; phases beyond MVP are "nice to have"

If any of these are missing or marked `[NEEDS CLARIFICATION]`, note it in the plan under What's Needed and make a reasonable assumption rather than stopping.

---

## Step 2: Write the Plan

Write `FEATURE_DIR/plan.md`:

```markdown
# Plan: [Initiative Name]

**Date**: [DATE]
**Spec**: [link to spec.md]

## Summary

[2–3 sentences: what this initiative does and the core approach.]

## Context

**Why now**: [Background — what prompted this initiative, from the spec's Overview]
**Deadline**: [From spec Context — the date or milestone that drives the plan's timeline]
**Team**: [From spec Context — who's doing this work]
**Constraints**: [From spec Context — budget, tools, approvals, dependencies]

## Approach

[3–5 sentences describing the overall strategy. HOW will this be executed?
What's the sequencing logic? What makes this approach the right one for the constraints?]

## Phases

### Phase 1: [Name] — [Duration or target date]

**Goal**: [What this phase achieves]
**Activities**:
- [Key activity]
- [Key activity]
**Done when**: [Observable completion signal]

---

### Phase 2: [Name] — [Duration or target date]

[Same structure]

---

### Phase 3: [Name] — [Duration or target date]

[Same structure]

*Add or remove phases as needed. Most initiatives fit 2–4 phases.*

## Risks

| Risk | Mitigation |
|------|-----------|
| [What could go wrong] | [How to prevent or respond] |
| [Another risk] | [Mitigation] |

## What's Needed

**People**: [Roles or skills required]
**Time**: [Rough estimate]
**Other**: [Budget, tools, approvals, dependencies]
```

**Writing guidance**:
- **Use the spec's Context, don't re-invent it**: deadline from the spec sets phase target dates; team from the spec answers "who does this"; constraints from the spec limit the approach
- **Mark the MVP phase**: the spec's Priority field tells you which phase is the minimum viable delivery — label it `*(MVP)*` so the plan is clear about what's required vs. what's ideal
- Each phase should end with something reviewable or demonstrable — avoid phases that just produce "more planning"
- Keep the plan readable in 5 minutes — if a section is getting long, it's probably going into too much detail
- The approach section should explain the *logic* of the sequencing, not just list what happens
- Risks are better named now than discovered later; fold in risks from the spec's Risks section

---

## Step 3: Report

Tell the user:
- `FEATURE_DIR/plan.md` created
- 2-sentence summary of the plan
- Any open items that need team input
- Next step: `/speckit.tasks` to break the plan into work items
