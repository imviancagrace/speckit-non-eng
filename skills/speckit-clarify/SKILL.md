---
name: speckit-clarify
description: >
  Sharpen a spec by finding ambiguities and asking targeted questions. Use this
  skill for /speckit.clarify, or whenever a spec exists but feels vague — unclear
  scope, unmeasured success criteria, unstated assumptions, missing edge cases.
  Asks up to 5 questions one at a time and writes the answers back into the spec.
  Optional step between /speckit.specify and /speckit.plan.
---

## User Input

```text
$ARGUMENTS
```

If the user named a specific area to focus on (e.g., "scope" or "success criteria"), prioritize that.

---

## What You're Doing

Read the spec, find the gaps that would cause problems during planning or execution, and ask the right questions to close them. Then update the spec with the answers.

---

## Step 1: Find the Spec

1. Read `.speckit/feature.json` → `feature_directory`.
2. If not found, scan `specs/` for the most recently modified directory.
3. Read `FEATURE_DIR/spec.md`. If missing, tell the user to run `/speckit.specify` first.

---

## Step 2: Find the Gaps

Scan the spec for:

| Gap type | What to look for |
|----------|-----------------|
| Vague scope | What's in vs. out isn't stated |
| Unmeasured criteria | Success criteria without numbers or dates |
| Undefined stakeholders | "the team" or "stakeholders" without being specific |
| Missing timeline | No sense of when this needs to be done |
| Hidden assumptions | Things treated as fact that aren't stated |
| Edge cases | Obvious "what if" scenarios not addressed |
| Conflicts | Two requirements that can't both be true |

Build a prioritized list of questions. Ask only questions whose answers would **materially change how this initiative is planned or executed**. Skip anything minor or stylistic.

Maximum 5 questions total.

---

## Step 3: Ask One Question at a Time

Never reveal future questions. Ask one, wait for the answer, then ask the next.

**Format for each question**:

```markdown
## Question [N]: [Topic]

**Context**: "[Quote the vague or missing part of the spec]"

**What we need to know**: [Why this matters for planning]

**Recommended answer**: [Your best suggestion based on context — 1 sentence reasoning]

| Option | Answer | What this means |
|--------|--------|-----------------|
| A | [Option] | [Implication] |
| B | [Option] | [Implication] |
| Custom | Your own answer | — |

Reply with A, B, "yes" to accept the recommendation, or your own answer.
```

After each answer: record it, update the spec immediately, then move to the next question.

If you find no meaningful gaps: say so clearly and suggest proceeding to `/speckit.plan`.

---

## Step 4: Update the Spec

After each accepted answer, update `spec.md` immediately:

- Vague success criterion → replace with a measurable one
- Undefined scope → add a clear in/out boundary to Assumptions
- Missing timeline → add to Assumptions or Success Criteria
- Edge case → add to Risks section
- Conflict resolved → update the conflicting requirements

Also add a `## Clarifications` section at the bottom:
```markdown
## Clarifications

### Session [DATE]
- Q: [Question] → A: [Answer]
```

---

## Step 5: Report

- How many questions asked and answered
- Which sections were updated
- Whether the spec is ready for `/speckit.plan`, or if anything major remains open
