# speckit-non-eng

Custom skills for Claude Cowork that bring a structured, specification-driven workflow to any non-engineering initiative — campaigns, programs, reports, events, process changes, strategy work, and more.

## The workflow

```
/speckit.specify  →  /speckit.plan  →  /speckit.tasks  →  /speckit.implement
```

Each step builds on the last. You can stop at any stage, or jump in mid-flow if you already have a spec or plan.

| Skill | What it does |
|-------|-------------|
| `/speckit.specify` | Turns a description into a structured spec: requirements and measurable success criteria |
| `/speckit.plan` | Creates a phased action plan from the spec |
| `/speckit.tasks` | Breaks the plan into a prioritized, trackable task list |
| `/speckit.implement` | Executes tasks and produces real deliverables |
| `/speckit.clarify` | *(optional)* Asks up to 5 questions to sharpen an existing spec |
| `/speckit.analyze` | *(optional)* Checks that spec, plan, and tasks are consistent before execution |

## How to use in Claude Cowork

1. Open the `skills/` folder
2. Upload the .md file **Customize → Skills** in Claude Cowork
3. Repeat for whichever skills you want

### Using the skills

Once installed, start a new conversation in Claude Cowork and type:

```
/speckit.specify [describe your initiative in plain language]
```

**Example:**
```
/speckit.specify We need to plan our Q3 all-hands event. It's a half-day in-person gathering 
for 200 people. Goal is to align the company on H2 priorities and boost morale after a tough Q2.
```

Claude will create a `specs/001-q3-all-hands/spec.md` in your project with requirements and success criteria. Then run:

```
/speckit.plan
/speckit.tasks
/speckit.implement
```

Each command reads the previous output automatically — no copy-pasting needed.

## What gets created

All initiative files are saved under `specs/[NNN]-[initiative-name]/`:

```
specs/
└── 001-q3-all-hands/
    ├── spec.md        ← requirements + success criteria
    ├── plan.md        ← phased action plan
    ├── tasks.md       ← prioritized task list
    └── progress.md    ← execution log (created by /speckit.implement)
```

A `.speckit/feature.json` file keeps track of which initiative is active so each command knows where to look.
