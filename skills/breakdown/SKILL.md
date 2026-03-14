---
name: breakdown
description: Use when you have a PRD and architecture document and need to slice work into ordered, interlinked development tasks with acceptance criteria and verification steps.
---

# Breakdown

Decompose a PRD and architecture into small, ordered, verifiable development tasks — the final output of the analytics pipeline.

## When to Use

- A PRD and architecture document exist and are approved
- You need to turn requirements into implementable work items
- The work is too large to start without a task list
- Tasks need to be parallelized across agents or sessions
- You need to communicate scope and order to a developer or team

**When NOT to use:** The work is a single-file change with obvious scope, or a task list already exists and is still accurate.

## Input

- `prd.md` from the artifact folder (required)
- `architecture.md` from the artifact folder (required)
- `context-map.md` from the artifact folder (recommended)

## Output

- `tasks.md` saved to the artifact folder (see `references/formats.md` for template)

<HARD-GATE>
This is a READ-ONLY planning phase. Do NOT write any code or modify the codebase. The output is a task list document, not implementation.
</HARD-GATE>

## The Process

### Step 1: Load Upstream Artifacts

Read `prd.md`, `architecture.md`, and optionally `context-map.md` from the artifact folder. If either required artifact is missing, tell the user which skill to run first.

Also check for project documentation listed in `references/context-sources.md` (`docs/ProjectStructure.md`). If available, use it to verify file paths and directory locations when writing tasks.

Extract:
- User stories and acceptance criteria (from PRD)
- Component list, dependency graph, and tech decisions (from architecture)
- Existing patterns and file paths (from context map)

### Step 2: Map the Dependency Graph to Build Order

Use the dependency graph from the architecture document to determine bottom-up implementation order:

1. **Foundation first** — database schema, types, shared utilities
2. **Services next** — business logic, validation
3. **API layer** — endpoints, route handlers
4. **Frontend** — components, pages, integration
5. **Polish** — error handling, edge cases, optimization

High-risk items go early in their phase. Fail fast — discover problems before building on top of them.

### Step 3: Slice Vertically

Prefer vertical slices over horizontal layers. Each task should deliver a complete, testable piece of functionality:

**Bad (horizontal):**
```
Task 1: Create all database schemas
Task 2: Build all API endpoints
Task 3: Build all UI components
Task 4: Connect everything
```

**Good (vertical):**
```
Task 1: User can register (schema + API + basic UI)
Task 2: User can log in (auth schema + API + UI)
Task 3: User can create an item (item schema + API + UI)
```

Each vertical slice leaves the system in a working, testable state.

### Step 4: Size Each Task

| Size | Files | Scope | Action |
|------|-------|-------|--------|
| **XS** | 1 | Single function or config change | Good to go |
| **S** | 1-2 | One component or endpoint | Good to go |
| **M** | 3-5 | One feature slice | Good to go |
| **L** | 5-8 | Multi-component feature | Consider splitting |
| **XL** | 8+ | Too large | Must split further |

Target S and M tasks. If a task is L or larger, break it into smaller tasks. No task should touch more than ~5 files.

### Step 5: Write Tasks

For each task, use this structure:

```markdown
### Task N: [Short descriptive title]
- **Description:** [One paragraph — what this task accomplishes]
- **Acceptance Criteria:**
  - [ ] [Specific, testable condition]
  - [ ] [Specific, testable condition]
- **Verification:**
  - [ ] [How to confirm — test command, build check, manual verification]
- **Dependencies:** [Task numbers this depends on, or "None"]
- **Files:**
  - `[path/to/file]`
  - `[path/to/file]`
- **Size:** [XS / S / M / L]
```

Rules:
- Every task has acceptance criteria — no exceptions
- Every task has a verification step — how do you know it's done?
- Dependencies are explicit — no task depends on a task that comes later
- File paths are specific — not "somewhere in src/"
- Acceptance criteria are verifiable — never "works correctly" or "looks good"

### Step 6: Group into Phases with Checkpoints

Organize tasks into phases. Add a checkpoint after every 2-3 tasks:

```markdown
## Phase 1: Foundation
- Task 1: ...
- Task 2: ...

### Checkpoint: Foundation
- [ ] All tests pass
- [ ] Application builds without errors
- [ ] Core data model works
- [ ] Review before proceeding

## Phase 2: Core Features
- Task 3: ...
- Task 4: ...
- Task 5: ...

### Checkpoint: Core Features
- [ ] Primary user flow works end-to-end
- [ ] Review before proceeding
```

Checkpoints are human review gates. The implementer should stop and verify before moving to the next phase.

### Step 7: Identify Parallelization Opportunities

Classify tasks:

- **Safe to parallelize:** Independent feature slices, tests for already-implemented features, documentation
- **Must be sequential:** Database migrations, shared state changes, dependency chains
- **Needs coordination:** Tasks that share an API contract (define the contract first, then parallelize)

Include this classification in the task list document.

### Step 8: Document Risks

Carry over risks from the architecture document and add task-specific risks:

| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk] | [High/Med/Low] | [Strategy] |

### Step 9: Write and Save Task List

Write `tasks.md` to the artifact folder using the template from `references/formats.md`.

Present the task list to the user for review. Highlight:
- Total number of tasks and phases
- Estimated scope distribution (how many S, M, L tasks)
- High-risk tasks and when they're scheduled
- Parallelization opportunities

Apply requested changes, then save.

Announce the saved path:

> "Task list saved to `[path]/tasks.md`."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll figure out the order as I go" | Wrong order means building on foundations that don't exist yet. 10 minutes of ordering saves hours. |
| "The tasks are obvious from the PRD" | PRD defines WHAT. Breakdown defines HOW MUCH per step, in WHAT ORDER, verified HOW. Different concerns. |
| "Planning is overhead" | Planning IS the task. Implementation without a plan is just typing with extra debugging. |
| "I can hold it all in my head" | Context is finite. Written task lists survive session boundaries, compaction, and team handoffs. |
| "Everything is high priority" | Dependency order defines the real priority. Build foundations first, not the most exciting feature. |
| "Small tasks are too granular" | Small tasks complete reliably. Large tasks produce tangled messes and partial progress. |

## Red Flags

- Tasks without acceptance criteria
- Tasks without verification steps
- No dependency ordering — tasks listed randomly
- All tasks are L or XL sized
- No checkpoints between phases
- Horizontal slicing instead of vertical
- File paths missing or vague ("update the frontend")
- No parallelization classification
- Starting implementation without user reviewing the task list

## Verification

Before declaring the pipeline complete, confirm:

- [ ] All upstream artifacts loaded (PRD, architecture)
- [ ] Every task has acceptance criteria
- [ ] Every task has a verification step
- [ ] Dependencies identified and ordered correctly
- [ ] No task touches more than ~5 files
- [ ] Most tasks are S or M sized
- [ ] Phases with checkpoints every 2-3 tasks
- [ ] High-risk tasks scheduled early in their phase
- [ ] Parallelization opportunities classified
- [ ] Risks documented with mitigations
- [ ] User has reviewed and approved the task list
- [ ] `tasks.md` saved to artifact folder

## End of Pipeline

> "Task list complete. The analytics pipeline is finished. The artifact folder now contains:
> - `context-map.md` — codebase context
> - `goal-definition.md` — problem and success criteria
> - `brainstorming.md` — approaches evaluated and direction chosen
> - `prd.md` — formal requirements with acceptance criteria
> - `architecture.md` — technical design and dependency graph
> - `tasks.md` — ordered, interlinked development tasks
>
> Ready for implementation."
