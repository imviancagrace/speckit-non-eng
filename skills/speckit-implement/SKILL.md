---
name: speckit-implement
description: >
  Execute the tasks from an initiative's tasks.md, working through them phase by
  phase and marking each one complete. Use this skill for /speckit.implement, or
  whenever someone wants to start executing on a task list — drafting documents,
  producing deliverables, running analyses, writing content, building anything
  described in the tasks. Can focus on a specific phase or task range. Reports
  progress and what's still outstanding after each session.
---

## User Input

```text
$ARGUMENTS
```

If the user specified a phase, task ID, or focus area, prioritize that. Otherwise, start from the first incomplete task.

---

## What You're Doing

Work through the initiative's tasks and produce real outputs — drafted documents, completed deliverables, decisions, content, analyses. This is where the spec and plan become tangible work.

Think of yourself as a capable team member who has read the spec and plan and is now doing the work. Each task should be completed fully, not described.

---

## Step 1: Load Context

1. Read `.speckit/feature.json` → `feature_directory`.
2. If not found, scan `specs/` for the most recently modified directory.
3. Read all available documents in `FEATURE_DIR`:
   - `tasks.md` (required — if missing, tell user to run `/speckit.tasks` first)
   - `spec.md` (for requirements and success criteria)
   - `plan.md` (for approach and phase context)

---

## Step 2: Identify What to Execute

From tasks.md, find:
- The current phase (first phase with incomplete tasks)
- Which tasks are `- [ ]` (incomplete) vs `- [x]` (complete)
- Which tasks are marked [P] (can run in parallel)
- Any specific task or phase the user mentioned in `$ARGUMENTS`

If the user didn't specify a focus, default to: **complete the current phase before moving to the next**.

---

## Step 3: Execute Tasks

For each task in scope:

1. **Read the task carefully**: understand the action and the named deliverable
2. **Produce the deliverable**: write the document, draft the content, complete the analysis, make the decision — do the actual work
3. **Save the output**: write deliverables to `FEATURE_DIR/` with a clear filename matching the deliverable name
   - Documents → `FEATURE_DIR/[deliverable-name].md` (or appropriate format)
   - If a deliverable is a decision or action (not a document), note it in `FEATURE_DIR/progress.md`
4. **Mark the task complete**: update `tasks.md`, changing `- [ ] T00X` to `- [x] T00X`

**Execution order**:
- Complete non-parallel tasks sequentially
- For [P] tasks, produce all of them before moving on (they're independent but part of the same phase)
- Don't skip tasks without flagging the reason

**When you encounter a task that needs human input**:
- Document what's needed and why in `FEATURE_DIR/progress.md` under a "Blockers" section
- Mark the task with a note: `- [ ] T00X ⚠️ Blocked: [what's needed]`
- Continue with other tasks that don't depend on the blocked one

---

## Step 4: Maintain progress.md

Keep a running log at `FEATURE_DIR/progress.md`:

```markdown
# Progress: [Initiative Name]

**Last updated**: [DATE]

## Completed

- **T001** → [Deliverable name]: [1-sentence summary of what was produced]
- **T002** → [Deliverable name]: Done

## In Progress

- **T003**: [What's been started]

## Blocked

- **T004** ⚠️: [What's needed to unblock]

## Remaining

Phases not yet started: [list]
```

---

## Step 5: Report After Each Session

After executing the tasks in scope, report:

- **Completed this session**: List of task IDs and their deliverables
- **Files created**: List of new documents in `FEATURE_DIR/`
- **Blockers** (if any): What's needed and from whom
- **What's next**: The next phase or task ID to tackle
- **Progress toward success criteria**: Quick check — which SC-00X from the spec are now evidenced by completed work?

---

## Principles

- **Do the work, don't describe it**: If the task says "Draft the Q3 report outline," produce the outline — don't write a paragraph about what the outline would contain
- **Deliverables should be shareable**: What you produce should be something a real person could pick up and use
- **One task at a time**: Complete each task fully before moving to the next (unless they're [P] parallel tasks)
- **Stay in scope**: Execute what the tasks describe — don't expand scope or add unrequested features
- **When in doubt, do less and ask**: It's better to complete 3 tasks well than 8 tasks partially
