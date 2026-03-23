# Phase 2: Design

Design the technical architecture for a set of requirements: components, data flow, dependency graph, tech stack decisions, integration points, and risks.

This phase does what the original `architecture` skill does, but consumes `requirements.md` instead of separate `prd.md` + `brainstorming.md` + `goal-definition.md`.

## Input

- `requirements.md` from the artifact folder (required)
- `context-map.md` from the artifact folder (required — for tech stack and existing patterns)
- See `context-sources.md` for additional project documentation that may provide context

## Output

- `design.md` saved to the artifact folder (see `formats.md` for template)

<HARD-GATE>
This is a READ-ONLY design phase. Do NOT write any code, create any source files, or modify the codebase. The output is an architecture document, not implementation.
</HARD-GATE>

## The Process

### Step 1: Load Upstream Artifacts

Read `requirements.md` and `context-map.md` from the artifact folder. If either is missing, tell the user which phase to run first.

Also check for project documentation listed in `context-sources.md` (`docs/TechStack.md`, `docs/ProjectStructure.md`). If available, use them for tech stack details and project structure when designing components and mapping integration points.

Extract:
- Intent and user stories (from requirements)
- Success criteria (from requirements — user stories' acceptance criteria)
- Tech stack, patterns, and conventions (from context map)
- Existing components and APIs (from context map)

### Step 2: Identify Components

From the requirements, identify the distinct components needed:

- **Data layer:** schemas, models, DTOs, type definitions
- **API layer:** endpoints, route handlers, API client methods
- **Logic layer:** composables, services, business rules, validation
- **UI layer:** views, forms, widgets, wrapper components
- **Shared utilities:** helpers, type guards, formatters, constants

For each component, define:
- **Responsibility** — what it does (single responsibility)
- **Location** — where it lives in the project (follow existing conventions from context map)
- **Interface** — key functions, endpoints, or props it exposes
- **Dependencies** — what other components it needs

Follow existing codebase patterns. If the project uses a composable pattern, use it. If components are colocated with their logic, follow that. Do not invent new patterns when existing ones work.

### Step 3: Map Data Flow

Describe how data moves through the system for the key user stories:

```
[User Action]
  → [Frontend Component] (handles UI state)
    → [Composable/Service] (business logic)
      → [API Call] (request to backend)
        → [Backend] (processing)
      ← [Response]
    ← [State Update]
  ← [UI Update] (re-render with new data)
```

Cover the primary happy path and the most important error path. Use mermaid diagrams or text-based flow diagrams.

### Step 4: Build Dependency Graph

Map what must be built before what. This directly feeds the task breakdown in Phase 3:

```
[Types / DTOs]
    ├── [API Layer Methods]
    │       ├── [Composables]
    │       │       ├── [UI Components]
    │       │       │       └── [Integration / Wiring]
    │       │       └── [Store Updates]
    │       └── [Utilities]
    └── [Test Helpers]
```

Implementation order follows the graph bottom-up: foundations first.

### Step 5: Document Tech Decisions

For every non-trivial technical choice, record:

| Decision | Choice | Rationale |
|----------|--------|-----------|
| State management | [e.g. composable + local refs] | [Why — aligns with existing pattern, simpler than store for this scope] |
| Data fetching | [e.g. composable with debounced API call] | [Why — follows existing autocomplete pattern] |
| Error handling | [e.g. toast notifications via PrimeVue] | [Why — already used elsewhere in the checkout flow] |

Do not introduce new dependencies without justification. Check the existing stack first (context map).

### Step 6: Map Integration Points

How does this feature connect to what already exists?

- Which existing files will be modified?
- Which existing APIs will be consumed?
- Which existing components will be extended or composed?
- Are there type changes that affect other features?
- Which store modules are involved?

### Step 7: Define Boundaries

State what is and isn't allowed during implementation:

- **Always:** [e.g. follow existing naming conventions, use existing error handling patterns, keep existing DTO fields unchanged]
- **Ask first:** [e.g. adding new dependencies, changing shared types, modifying existing component props]
- **Never:** [e.g. modify existing DTO field types on production, bypass validation, commit secrets]

### Step 8: Identify Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| [e.g. DTO field missing from backend response] | High | [Check backend response shape first, add typing as needed] |
| [e.g. Autocomplete floods API with requests] | Medium | [Debounce search with 300ms delay + AbortController] |

Flag high-risk items — these should be tackled early in the task breakdown (fail fast).

### Step 9: Check for Missing Context

Before saving, scan the design for references to things not in `context-map.md`:
- New types or DTOs mentioned but not documented
- APIs called but not explored
- Components referenced but not examined

If anything is missing → run research (see `references/research.md`, "subsequent research") → update `context-map.md` → update design.

### Step 10: Write and Save Design Document

Write `design.md` to the artifact folder using the template from `formats.md`.

Present the architecture to the user for review. Walk through:
1. Component overview
2. Data flow for the primary user story
3. Key tech decisions and their rationale
4. Any risks or open questions

Apply requested changes, then save.

Announce the saved path:

> "Design saved to `[path]/design.md`."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll figure out the architecture while coding" | Architecture decisions made under implementation pressure are worse. 15 minutes of design saves hours of refactoring. |
| "This is too small for an architecture document" | If it touches 3+ files or makes a tech decision, write it down. A short document is fine. |
| "The requirements already describe the technical approach" | Requirements describe WHAT. Design describes HOW — components, data flow, dependencies. Different concerns. |
| "I know the codebase, I don't need to document integration points" | Phase 3 and any agent doing implementation will need this documented. Write it for them. |
| "We can refactor later if the architecture is wrong" | Wrong architecture is the most expensive kind of rework. Get it right in a document before committing to code. |

## Red Flags

- No dependency graph — implementation order will be wrong
- Components that don't follow existing codebase patterns
- New dependencies added without checking what's already in the stack
- No integration points mapped — feature will break existing functionality
- Empty risks section — every architecture has risks
- Writing code during the design phase
- References to APIs or DTOs not in context-map — research first
- Components that don't map to any user story in requirements

## Verification

Before handing off, confirm:

- [ ] All upstream artifacts loaded (requirements, context map)
- [ ] Components identified with responsibilities, locations, and interfaces
- [ ] Data flow documented for primary user story
- [ ] Dependency graph shows implementation order
- [ ] Tech decisions documented with rationale
- [ ] Integration points with existing system mapped
- [ ] Boundaries defined (Always / Ask first / Never)
- [ ] Risks identified with mitigations
- [ ] No references to unknown code (all researched and in context-map)
- [ ] User has reviewed and approved the design
- [ ] `design.md` saved to artifact folder

## Next

> "Design complete. Next phase: **tasks** — slice this into ordered, interlinked development tasks."
