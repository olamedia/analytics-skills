---
name: architect
description: Use when you have a raw idea or feature request and want to go from idea to implementation-ready task list through a structured 3-phase pipeline — requirements, design, tasks. Uses research on demand to gather codebase context whenever needed.
---

# Architect (Orchestrator)

Run a 3-phase pipeline to turn a raw idea into an implementation-ready task list: requirements, design, tasks. Research is invoked on demand at any phase to gather codebase context.

## When to Use

- A feature request or idea needs structured planning before implementation
- You want requirements, architecture, and tasks as separate reviewable artifacts
- The problem benefits from iterative refinement with the user before coding

**When NOT to use:**
- The task is a single-file change with obvious scope — just do it
- Only one phase is needed — invoke that phase's skill directly
- Upstream artifacts already exist — pick up from the appropriate phase

## Input

- A raw idea, feature request, or problem description from the user
- Optional: artifact folder path (e.g. `docs/plans/my-feature/`)

## Output

An artifact folder containing:
- `context-map.md` (living document, updated throughout)
- `requirements.md`
- `design.md`
- `tasks.md`
- `tasks/task-N.md` (optional, from Phase 4 — standalone issue documents)

## The Pipeline

```
Phase 1: Requirements  →  Phase 2: Design  →  Phase 3: Tasks  →  Phase 4: Compile (optional)
         ↑                        ↑                     ↑
         └── research ────────── research ────────── research
```

Research is not a phase — it's a tool invoked whenever context is missing. Phase 4 is optional — only runs if the user wants standalone issue documents.

## The Process

### Step 1: Determine Artifact Folder

Ask the user for the artifact folder, or accept one specified in chat:

> "Where should I save the planning artifacts? Provide a folder path (e.g. `docs/plans/contact-picker/`), or I'll use `docs/plans/` as default."

Create the folder if it doesn't exist.

### Step 2: Run Phase 1 — Requirements

Follow the requirements process (see `references/phase-1-requirements.md`):

1. Ask user for the idea/goal
2. Compile intent block in user's own words
3. When user confirms initial idea is complete → run research → save `context-map.md`
4. Convert intent into initial user stories (extract what's clear, note gaps)
5. Ask clarification questions about gaps, refine stories
6. Track all questions with statuses
7. Loop until no open questions remain and user has nothing to add
8. Save `requirements.md`

Present summary and confirm before continuing:

> "Requirements complete: [intent summary], [N user stories], [N questions resolved]. Continue to design?"

### Step 3: Run Phase 2 — Design

Follow the design process (see `references/phase-2-design.md`):

- Load `requirements.md` and `context-map.md`
- Identify components, map data flow, build dependency graph
- Document tech decisions, integration points, risks
- Save `design.md`

Present architecture summary and confirm before continuing:

> "Design complete: [N components], [key decisions]. Continue to tasks?"

### Step 4: Run Phase 3 — Tasks

Follow the tasks process (see `references/phase-3-tasks.md`):

- Write the overview and task graph first — this reveals groupable tasks
- Slice into ordered tasks with acceptance criteria
- Merge overly atomic tasks (grouping pass)
- Group into phases
- Save `tasks.md`

Present summary and offer Phase 4:

> "Tasks complete: [N tasks] in [N phases]. Artifact folder `[path]` contains:
> - `context-map.md` — codebase context (living document)
> - `requirements.md` — intent, user stories, questions log
> - `design.md` — architecture and technical design
> - `tasks.md` — task overview with dependency graph
>
> Want me to compile tasks into standalone issue documents for an issue tracker?"

### Step 5 (Optional): Run Phase 4 — Compile

Only if the user asks for it. Follow the compile process (see `references/phase-4-compile.md`):

- Ask which tasks to compile (all or specific ones)
- Create `tasks/` folder, one `task-N.md` per task
- Each issue is self-contained: goal with scope, proposed changes checklist, humanized acceptance criteria with "don't touch" constraints, testing notes, optional notes for uncommon knowledge
- Written in human language for developers and testers

> "Compiled [N] tasks to `[path]/tasks/`. Each issue is self-contained and ready for your issue tracker."

## Research Rules

Research is invoked on demand, not as a phase. The full research process is defined in `references/research.md`. Triggers:

1. **After user confirms initial idea is complete** — mandatory first research to produce `context-map.md`
2. **When new requirements mention things not in context-map** (types, APIs, components, DTOs) — research before proceeding
3. **When design references unknown code** — research before documenting it
4. **Never assume** — if something is not in `context-map.md`, discover it first via codebase scanning

After every research run, update `context-map.md` in the artifact folder. The context-map is a **living document** — it grows throughout the pipeline as new context is discovered.

## Change Propagation Rules

When a new requirement is added at **any phase** (even during design or tasks):

1. **Propose changes first** — describe what would change in each affected doc (requirements → design → tasks)
2. **Wait for user agreement** — user reviews and confirms (or corrects)
3. **Apply all changes in one batch** — update all affected docs together

Never edit docs one at a time without confirming the full set of changes. This prevents endless correction loops.

## Human Review Gates

Between every phase, ask the user to confirm before proceeding. Do not skip gates.

If the user wants to revise an earlier phase:
1. Go back to that phase
2. Propose changes to the artifact and all downstream artifacts
3. Get user agreement
4. Apply changes in one batch
5. Continue from the revised phase forward

If the user wants to stop mid-pipeline, that's fine. The artifacts saved so far are valid. The user can resume from any point by invoking the appropriate phase directly.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This is simple, just give me the tasks" | Simple problems still benefit from explicit requirements. The pipeline moves quickly through simple cases. |
| "The review gates slow things down" | Review gates catch wrong directions early. Fixing requirements is cheaper than fixing a task list. |
| "I already know the codebase" | Context maps surface patterns you take for granted. Research prevents assumptions that explode during implementation. |
| "I'll research as I go" | On-demand research is fine — that's what this pipeline does. But never skip the initial research after the idea is confirmed. |

## Red Flags

- Skipping research when context is missing — leads to assumptions
- Making assumptions about APIs, DTOs, or components not in context-map
- Editing docs one at a time without proposing full change set
- Writing intent/goal in AI language instead of user's own words
- Proceeding when user expressed doubt or confusion
- Producing tasks without user confirming the design
- Not updating context-map when new research is done
- Starting implementation during any phase — all phases are READ-ONLY

## Verification

At the end of the pipeline, confirm:

- [ ] All artifacts exist in the artifact folder
- [ ] `context-map.md` reflects all research done throughout the pipeline
- [ ] `requirements.md` has intent in user's words, user stories, and questions log
- [ ] `design.md` has components, data flow, dependency graph, and tech decisions
- [ ] `tasks.md` has ordered tasks with acceptance criteria and verification
- [ ] No overly atomic tasks remain (grouping pass done)
- [ ] User confirmed at each review gate
- [ ] Artifact folder path communicated to user
