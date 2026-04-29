---
name: speckit-specify
description: >
  Turn any initiative description into a structured spec document. Use this skill
  for /speckit.specify, or whenever someone wants to define an initiative before
  working on it — projects, campaigns, events, reports, process changes, strategies,
  programs, policies. Creates a numbered spec directory and saves state so the
  rest of the workflow (/speckit.plan, /speckit.tasks, /speckit.implement) can
  pick up where this left off. Works for any team or domain; no technical knowledge
  required.
---

## User Input

```text
$ARGUMENTS
```

Consider this carefully before proceeding. It's the initiative description.

---

## What You're Doing

Turn the user's description into a clear, reviewable spec. The spec has two jobs: define the initiative clearly enough to plan against, and give the plan the context it needs to make good decisions.

Focus on four things:

1. **What must be true** (requirements)
2. **How we know it worked** (success criteria — always measurable)
3. **The context that shapes the plan** (deadline, team, constraints)
4. **What we're assuming or risking** (so the plan can account for it)

Scenarios (who benefits and how) are optional — include them only if there are distinct groups whose different needs matter to how the initiative is shaped.

---

## Step 1: Set Up the Directory

1. Check if `.speckit/feature.json` exists and has a `feature_directory`. If yes, use it.
2. Otherwise, scan `specs/` to find the next available number (001, 002, 003…). If `specs/` is empty, start at 001.
3. Generate a 2–4 word hyphenated name from the description (e.g., `q3-report`, `brand-refresh`, `hiring-plan`).
4. Create: `specs/[NNN]-[short-name]/`
5. Save `.speckit/feature.json`:
   ```json
   { "feature_directory": "specs/[NNN]-[short-name]" }
   ```

---

## Step 2: Write the Spec

Write `FEATURE_DIR/spec.md`. Use the structure below — skip any section that genuinely doesn't apply, but always include Requirements and Success Criteria.

```markdown
# [Initiative Name]

**Date**: [DATE]
**Status**: Draft

## Overview

[2–3 sentences: what this is, why it matters, what a win looks like.]

## Context

The practical constraints the plan needs to work within.

- **Deadline**: [When this needs to be done, or the key milestone it's tied to]
- **Team**: [Who's doing this work — roles, number of people, or named individuals if known]
- **Constraints**: [Budget limits, tools available, approvals needed, dependencies on other work]
- **Priority**: [What's the minimum version that still counts as a success — the MVP]

## Requirements

What MUST be true or happen for this initiative to be complete.

- **R-001**: [Specific, testable requirement]
- **R-002**: [Another requirement]
- **R-003**: [Continue as needed]

## Success Criteria

How we measure whether this worked. Every criterion needs a number, percentage, date, or other concrete target.

- **SC-001**: [e.g., "Report delivered to all stakeholders by May 15"]
- **SC-002**: [e.g., "Campaign reaches 10,000 impressions in the first week"]
- **SC-003**: [Continue as needed]

## Scenarios *(include only if different groups have meaningfully different needs)*

### [Who] — [What they need]

[1–2 sentences on their situation and what success looks like for them.]

**This is working when**: [Specific observable signal]

---

## Assumptions

- [Something assumed to be true that wasn't stated]
- [Scope boundary or constraint]

## Risks

- [What could go wrong] → [How to address it]
- [Another risk] → [Mitigation]
```

**Writing guidance**:
- **Context**: Extract deadline, team, and constraints from the description. If not stated, use [NEEDS CLARIFICATION] for deadline (it always affects the plan) and make reasonable guesses for everything else, documented as assumptions.
- **Requirements**: Describe WHAT must happen, not HOW to do it
- **Success criteria**: Must be checkable — if you can't verify it, push for a proxy metric. A deadline is a valid success criterion (e.g., "SC-001: Delivered by April 30").
- **Priority/MVP**: If the user didn't specify, make a judgment call about what the smallest useful version looks like and document it in Context.
- Use [NEEDS CLARIFICATION: question] only for things that would meaningfully change the shape of the plan — maximum 3.

---

## Step 3: Validate

After writing, quickly self-check:

- [ ] Overview is clear to someone who wasn't in the conversation
- [ ] Context has a deadline (or a [NEEDS CLARIFICATION] marker)
- [ ] Context has enough team/constraint info for someone to build a realistic plan
- [ ] All requirements are specific and testable
- [ ] All success criteria have measurable targets
- [ ] Assumptions are documented, not buried
- [ ] No implementation details snuck in (requirements say WHAT, not HOW)

If anything fails the check, fix it before reporting.

---

## Step 4: Handle Clarifications

If you have [NEEDS CLARIFICATION] markers, present each one with a recommended answer and a short table of options. Wait for the user's response, then update the spec.

---

## Step 5: Report

Tell the user:
- The directory: `specs/[NNN]-[short-name]/`
- A 2-sentence summary of what was captured
- Next step: `/speckit.plan` to create the action plan
