---
name: analyze
description: Use when you have a raw idea or request and want to run the full analytics pipeline automatically — from research through to an interlinked task list. Best for straightforward problems where the full pipeline can flow with minimal back-and-forth.
---

# Analyze (Orchestrator)

Run the full analytics pipeline in sequence: research, goal definition, brainstorming, PRD, architecture, and task breakdown. Produces a complete set of interlinked artifacts from a raw idea.

## When to Use

- A feature request or idea needs the full treatment: analysis through to task list
- The problem is straightforward enough that all 6 steps can flow in one session
- You want to minimize manual skill invocation and let the pipeline run

**When NOT to use:**
- The problem is complex or ambiguous — run skills individually with more deliberation at each stage
- Only one phase is needed (e.g., just need a PRD for existing requirements) — invoke that skill directly
- Upstream artifacts already exist — pick up from the appropriate skill in the chain

## Input

- A raw idea, feature request, or problem description from the user
- A codebase to analyze (current workspace or specified path)

## Output

A complete artifact folder containing:
- `context-map.md`
- `goal-definition.md`
- `brainstorming.md`
- `prd.md`
- `architecture.md`
- `tasks.md`

## The Pipeline

```
research → goal-definition → brainstorming → prd → architecture → breakdown
```

Each step produces an artifact that the next step consumes. Human review gates occur between phases.

## The Process

### Step 1: Determine Artifact Folder

Ask the user for the artifact folder, or accept one specified in chat:

> "Where should I save the analysis artifacts? Provide a folder path (e.g. `docs/analytics/user-auth/`), or I'll use `docs/analytics/` as default."

Create the folder if it doesn't exist.

### Step 2: Run Research

Before scanning, check for existing project documentation listed in `references/context-sources.md` (`docs/TechStack.md`, `docs/ProjectStructure.md`). If they exist and are fresh, use them to accelerate the research phase.

Follow the `research` skill process:
- Scan codebase structure, tech stack, patterns, conventions
- Find rules files, note constraints
- Save `context-map.md`

Present a brief summary and confirm before continuing:

> "Context mapped. Key findings: [tech stack], [N patterns identified], [any notable constraints]. Continue to goal definition?"

### Step 3: Run Goal Definition

Follow the `goal-definition` skill process:
- Restate the idea as "How Might We"
- Ask 3-5 sharpening questions (one at a time)
- Surface assumptions
- Define testable success criteria
- Save `goal-definition.md`

Present the goal and confirm before continuing:

> "Goal defined: [problem statement]. [N success criteria]. Continue to brainstorming?"

### Step 4: Run Brainstorming

Follow the `brainstorming` skill process:
- Scan for prior art in the codebase
- Generate 2-3 approaches with trade-offs
- Present comparison and recommendation
- Get user to pick a direction
- Save `brainstorming.md`

Present the chosen direction and confirm before continuing:

> "Direction chosen: [approach name]. Continue to PRD?"

### Step 5: Run PRD

Follow the `prd` skill process:
- Ask 3-5 clarifying questions where gaps exist
- Write user stories with acceptance criteria
- Write functional requirements
- Define non-goals and success metrics
- Save `prd.md`

Present the PRD summary and confirm before continuing:

> "PRD complete: [N user stories], [N functional requirements], [N non-goals]. Continue to architecture?"

### Step 6: Run Architecture

Follow the `architecture` skill process:
- Identify components with responsibilities and locations
- Map data flow and dependency graph
- Document tech decisions and integration points
- Define boundaries, identify risks
- Save `architecture.md`

Present architecture summary and confirm before continuing:

> "Architecture designed: [N components], [key tech decisions]. Continue to task breakdown?"

### Step 7: Run Breakdown

Follow the `breakdown` skill process:
- Map dependency graph to build order
- Slice vertically, size each task
- Write tasks with acceptance criteria and verification
- Group into phases with checkpoints
- Classify parallelization opportunities
- Save `tasks.md`

Present the final summary:

> "Pipeline complete. Artifact folder `[path]` contains:
> - `context-map.md` — codebase context
> - `goal-definition.md` — problem and success criteria
> - `brainstorming.md` — approaches and chosen direction
> - `prd.md` — requirements with acceptance criteria
> - `architecture.md` — technical design
> - `tasks.md` — [N tasks] in [N phases], ready for implementation"

## Human Review Gates

Between every phase, ask the user to confirm before proceeding. Do not skip gates.

If the user wants to revise an earlier phase:
1. Go back to that phase
2. Update the artifact
3. Assess whether downstream artifacts need updating
4. Continue from the revised phase forward

If the user wants to stop mid-pipeline, that's fine. The artifacts saved so far are valid. The user can resume from any point by invoking the appropriate skill directly.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This is simple, just give me the tasks" | Simple problems still benefit from explicit goals and requirements. The orchestrator moves quickly through simple cases. |
| "The review gates slow things down" | Review gates catch wrong directions early. Fixing a goal is cheaper than fixing a task list. |
| "I'll skip brainstorming, I know the approach" | Even obvious approaches have trade-offs worth documenting. Brainstorming takes 5 minutes in the orchestrator. |

## Red Flags

- Skipping review gates between phases
- Proceeding when the user expressed doubt or confusion
- Producing a task list without the user confirming the PRD
- Artifacts that don't reference each other (broken chain)
- Not saving artifacts to the folder (keeping everything in conversation)

## Verification

At the end of the pipeline, confirm:

- [ ] All 6 artifacts exist in the artifact folder
- [ ] Each artifact references its upstream artifacts
- [ ] User confirmed at each review gate
- [ ] Task list has acceptance criteria and verification for every task
- [ ] Task dependencies are ordered correctly
- [ ] Artifact folder path communicated to user
