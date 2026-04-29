---
name: speckit-tasks
description: >
  Break an initiative's plan into a concrete, prioritized task list. Use this skill
  for /speckit.tasks, or whenever someone has a plan and wants to turn it into
  trackable work items. Each task gets an ID, an optional parallel marker, and a
  named deliverable. Organizes work into phases from the plan. Works for any domain;
  no technical knowledge required.
---

## User Input

```text
$ARGUMENTS
```

Consider any context (e.g., team size, MVP scope, priority focus).

---

## What You're Doing

Take the plan and spec and turn them into a task list someone can actually execute. Every task should be specific enough to pick up and start without asking clarifying questions.

---

## Step 1: Find the Documents

1. Read `.speckit/feature.json` → `feature_directory`.
2. If not found, scan `specs/` for the most recently modified directory.
3. Read:
   - **Required**: `FEATURE_DIR/plan.md`, `FEATURE_DIR/spec.md`
   - If either is missing, tell the user which command to run first.

---

## Step 2: Extract the Structure

From `plan.md`: phases, activities, done-when criteria, constraints, team size
From `spec.md`: requirements (R-001, etc.), success criteria (SC-001, etc.), any scenarios

Map each activity and requirement to a concrete task.

---

## Step 3: Write tasks.md

Write `FEATURE_DIR/tasks.md`:

```markdown
# Tasks: [Initiative Name]

**Source**: spec.md, plan.md
**Directory**: [FEATURE_DIR]

## Format: `[ID] [P?] Description → Deliverable`

- **[P]**: Task can run in parallel with others in the same phase
- **→ Deliverable**: The named output — a document, decision, completed activity, or result

---

## Phase 1: Setup

*Kick-off, alignment, shared workspace — anything needed before real work begins.*

- [ ] T001 [Brief, specific action] → [Named deliverable]
- [ ] T002 [P] [Another action] → [Named deliverable]

---

## Phase 2: [Phase Name from plan]

*[What this phase is for — 1 sentence]*

- [ ] T003 [Action] → [Deliverable]
- [ ] T004 [P] [Action that can run in parallel] → [Deliverable]
- [ ] T005 [Action that depends on T004] → [Deliverable]

**Done when**: [Restate the done-when signal from the plan]

---

## Phase 3: [Next phase]

[Same structure]

---

## Final: Polish & Wrap-Up

- [ ] TXXX [P] Consolidate documentation → [Final docs package]
- [ ] TXXX Share outcomes with stakeholders → [Stakeholder update]
- [ ] TXXX Capture lessons learned → [Retrospective notes]

---

## Execution Order

- Phases run sequentially (Phase 2 doesn't start until Phase 1 is done)
- Tasks marked [P] within a phase can run simultaneously
- The first phase is the MVP — stop and validate before proceeding

## MVP Strategy

Complete Phase 1 + Phase 2 only. Validate against the spec's success criteria.
Proceed to later phases once the core value is confirmed.
```

**Task writing rules**:
- Each task is one clear action + one named output
- "Named deliverable" means something that can be shown to someone — a doc, a decision, a completed activity, a number. Not vague like "progress" or "work"
- Mark [P] only when tasks genuinely don't block each other within the same phase
- Every requirement from the spec (R-001, etc.) should trace to at least one task
- Every success criterion from the spec (SC-001, etc.) should trace to at least one task that produces evidence for it
- Total task count: enough to be useful, not so many it becomes overwhelming (roughly 10–30 for most initiatives)

---

## Step 4: Report

Tell the user:
- `FEATURE_DIR/tasks.md` created
- Total tasks, how many can run in parallel
- Which tasks tie to which success criteria
- Next step: `/speckit.implement` to start executing
