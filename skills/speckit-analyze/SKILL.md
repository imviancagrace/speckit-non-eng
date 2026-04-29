---
name: speckit-analyze
description: >
  Read-only consistency check across spec.md, plan.md, and tasks.md — finds gaps,
  contradictions, and coverage issues before execution begins. Use this skill for
  /speckit.analyze, or whenever someone wants to verify that the three documents
  are aligned before starting work. Does NOT modify any files. Produces a findings
  report with severity levels and suggested fixes.
---

## User Input

```text
$ARGUMENTS
```

If the user named a focus area (e.g., "check coverage" or "look at risks"), prioritize it.

---

## What You're Doing

Act as an independent reviewer reading the three documents cold. Find problems the original authors may have missed — gaps, vague criteria, tasks with no requirement behind them, requirements with no tasks, and inconsistent terminology.

**Read only. Do not change any files.**

---

## Step 1: Load Documents

1. Read `.speckit/feature.json` → `feature_directory`.
2. If not found, scan `specs/` for the most recently modified directory.
3. Check what's available:
   - **Required**: `spec.md`, `plan.md`, `tasks.md`
   - If `tasks.md` is missing, tell the user to run `/speckit.tasks` first.

---

## Step 2: Check for Issues

### A. Coverage — does every requirement have tasks?

For each R-### in spec.md:
- Is there at least one task in tasks.md that addresses it?
- If not: **Coverage Gap**

For each SC-### (success criterion):
- Is there at least one task that would produce evidence of meeting it?
- If not: **Coverage Gap**

### B. Measurability — are success criteria actually measurable?

For each SC-### entry:
- Does it have a number, percentage, date, or other specific target?
- Vague: "improved quality", "better results", "more efficient"
- Measurable: "delivered by May 15", "90% completion rate", "score ≥ 4/5"
- If vague: **Vague Metric**

### C. Orphaned tasks — do all tasks trace back to something?

For each task outside Setup and Polish phases:
- Does it connect to a requirement, success criterion, or plan activity?
- If nothing connects it: **Orphaned Task**

### D. Contradictions

- Does any requirement conflict with another?
- Does the plan's approach conflict with a spec constraint?
- If yes: **Conflict**

### E. Terminology drift

- Is the same concept called different things across documents?
- E.g., spec says "customers", plan says "clients", tasks say "users"
- If yes: **Terminology Drift**

### F. Missing deliverables

- Are there tasks without a named `→ Deliverable`?
- If yes: **Missing Deliverable**

---

## Step 3: Assign Severity

- **CRITICAL**: A requirement or SC has zero task coverage; a direct contradiction
- **HIGH**: Vague success criteria; major scope gap; significant terminology drift
- **MEDIUM**: Orphaned tasks; missing deliverable labels; minor inconsistency
- **LOW**: Style improvements; minor redundancy

---

## Step 4: Report

Output a structured findings report (do NOT write this to a file — display in the conversation):

```markdown
# Analysis: [Initiative Name]

**Documents reviewed**: spec.md, plan.md, tasks.md
**Date**: [DATE]

## Findings

| ID | Type | Severity | Location | Summary | Suggestion |
|----|------|----------|----------|---------|------------|
| G1 | Coverage Gap | CRITICAL | spec.md – R-002 | No tasks address this requirement | Add a task in Phase 2 |
| V1 | Vague Metric | HIGH | spec.md – SC-002 | "improved engagement" has no target | Add a number: e.g., "engagement rate ≥ 15%" |

## Coverage Summary

| Requirement / Criterion | Covered? | Task IDs |
|------------------------|----------|----------|
| R-001 | ✅ | T003, T007 |
| R-002 | ❌ | — |
| SC-001 | ✅ | T011 |
| SC-002 | ⚠️ Vague | T009 |

## Metrics

- Requirements checked: [N] | Covered: [N] ([%])
- Success criteria checked: [N] | Measurable: [N]
- CRITICAL: [N] | HIGH: [N] | MEDIUM: [N] | LOW: [N]

## Next Actions

[If CRITICAL issues]: Resolve before starting /speckit.implement:
- [Specific action for each critical issue]

[If only MEDIUM/LOW]: Ready to proceed. Optional improvements:
- [Suggestions]

---

Want me to suggest specific text edits for the top issues? Say "yes, suggest fixes."
```

---

## Operating Rules

- Never modify files — read and report only
- If everything looks good, say so clearly with the coverage stats
- Always cite the specific document and section for each finding
- Zero issues is a valid and good outcome — report it with the coverage summary
