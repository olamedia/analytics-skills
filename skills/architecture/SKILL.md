---
name: architecture
description: Use when you have a PRD and need to design the technical approach — components, data flow, dependencies, and tech decisions — before breaking work into tasks.
---

# Architecture

Design the technical architecture for a set of requirements: components, data flow, dependency graph, tech stack decisions, integration points, and risks.

## When to Use

- A PRD exists and the technical approach needs to be designed
- Before task breakdown — architecture defines WHAT components exist; breakdown defines the ORDER to build them
- When architectural decisions need to be documented (data model, API design, component structure)
- When integration with existing systems requires explicit mapping

**When NOT to use:** The technical approach is already documented in an `architecture.md`, or the change is trivial (single file, no architectural decisions).

## Input

- `prd.md` from the artifact folder (required)
- `context-map.md` from the artifact folder (required — for tech stack and existing patterns)
- `brainstorming.md` from the artifact folder (recommended — for chosen direction rationale)

## Output

- `architecture.md` saved to the artifact folder (see `references/formats.md` for template)

<HARD-GATE>
This is a READ-ONLY design phase. Do NOT write any code, create any source files, or modify the codebase. The output is an architecture document, not implementation.
</HARD-GATE>

## The Process

### Step 1: Load Upstream Artifacts

Read `prd.md`, `context-map.md`, and optionally `brainstorming.md` from the artifact folder. If `prd.md` or `context-map.md` is missing, tell the user which skill to run first.

Also check for project documentation listed in `references/context-sources.md` (`docs/TechStack.md`, `docs/ProjectStructure.md`). If available, use them for tech stack details and project structure when designing components and mapping integration points.

Extract:
- User stories and functional requirements (from PRD)
- Tech stack, patterns, and conventions (from context map)
- Chosen direction and trade-offs accepted (from brainstorming)

### Step 2: Identify Components

From the PRD requirements, identify the distinct components needed:

- **Data layer:** schemas, models, migrations
- **Backend logic:** services, business rules, validation
- **API layer:** endpoints, routes, middleware
- **Frontend components:** views, forms, widgets
- **Shared utilities:** helpers, types, constants

For each component, define:
- **Responsibility** — what it does (single responsibility)
- **Location** — where it lives in the project (follow existing conventions from context map)
- **Interface** — key functions, endpoints, or props it exposes
- **Dependencies** — what other components it needs

Follow existing codebase patterns. If the project uses a service layer pattern, use it. If components are colocated with tests, follow that. Do not invent new patterns when existing ones work.

### Step 3: Map Data Flow

Describe how data moves through the system for the key user stories:

```
[User Action]
  → [Frontend Component] (handles UI state)
    → [API Call] (request to backend)
      → [Route Handler] (validation, auth)
        → [Service] (business logic)
          → [Database] (persistence)
        ← [Response]
      ← [API Response]
    ← [UI Update] (re-render with new data)
```

Cover the primary happy path and the most important error path.

### Step 4: Build Dependency Graph

Map what must be built before what. This directly feeds the task breakdown:

```
[Database Schema / Migrations]
    ├── [Types / Interfaces]
    │       ├── [Service Layer]
    │       │       ├── [API Endpoints]
    │       │       │       └── [Frontend API Client]
    │       │       │               └── [UI Components]
    │       │       └── [Validation Logic]
    │       └── [Test Utilities]
    └── [Seed Data]
```

Implementation order follows the graph bottom-up: foundations first.

### Step 5: Document Tech Decisions

For every non-trivial technical choice, record:

| Decision | Choice | Rationale |
|----------|--------|-----------|
| State management | [e.g. URL params + React state] | [Why — aligns with existing pattern, simpler than Redux for this scope] |
| Data fetching | [e.g. SWR with optimistic updates] | [Why — already in the stack, handles caching] |
| Validation | [e.g. Zod schemas shared client/server] | [Why — single source of truth, TypeScript inference] |

Do not introduce new dependencies without justification. Check the existing stack first (context map).

### Step 6: Map Integration Points

How does this feature connect to what already exists?

- Which existing files will be modified?
- Which existing APIs will be consumed?
- Which existing components will be extended or composed?
- Are there database schema changes that affect other features?

### Step 7: Define Boundaries

State what is and isn't allowed during implementation:

- **Always:** [e.g. follow existing naming conventions, write tests for new logic, use existing error handling patterns]
- **Ask first:** [e.g. adding new dependencies, changing shared types, modifying database schema]
- **Never:** [e.g. bypass validation, commit secrets, modify unrelated code]

### Step 8: Identify Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| [e.g. Schema migration breaks existing queries] | High | [Run migration in staging first, write rollback] |
| [e.g. New endpoint conflicts with existing route] | Medium | [Check route registry before adding] |

Flag high-risk items — these should be tackled early in the task breakdown (fail fast).

### Step 9: Write and Save Architecture Document

Write `architecture.md` to the artifact folder using the template from `references/formats.md`.

Present the architecture to the user for review. Walk through:
1. Component overview
2. Data flow for the primary user story
3. Key tech decisions and their rationale
4. Any risks or open questions

Apply requested changes, then save.

Announce the saved path:

> "Architecture saved to `[path]/architecture.md`."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll figure out the architecture while coding" | Architecture decisions made under implementation pressure are worse. 15 minutes of design saves hours of refactoring. |
| "This is too small for an architecture document" | If it touches 3+ files or makes a tech decision, write it down. A short document is fine. |
| "The PRD already describes the technical approach" | PRD describes WHAT. Architecture describes HOW — components, data flow, dependencies. Different concerns. |
| "I know the codebase, I don't need to document integration points" | The task breakdown skill and any agent doing implementation will need this documented. Write it for them. |
| "We can refactor later if the architecture is wrong" | Wrong architecture is the most expensive kind of rework. Get it right in a document before committing to code. |

## Red Flags

- No dependency graph — implementation order will be wrong
- Components that don't follow existing codebase patterns
- New dependencies added without checking what's already in the stack
- No integration points mapped — feature will break existing functionality
- Empty risks section — every architecture has risks
- Writing code during the architecture phase

## Verification

Before handing off, confirm:

- [ ] All upstream artifacts loaded (PRD, context map)
- [ ] Components identified with responsibilities, locations, and interfaces
- [ ] Data flow documented for primary user story
- [ ] Dependency graph shows implementation order
- [ ] Tech decisions documented with rationale
- [ ] Integration points with existing system mapped
- [ ] Boundaries defined (Always / Ask first / Never)
- [ ] Risks identified with mitigations
- [ ] User has reviewed and approved the architecture
- [ ] `architecture.md` saved to artifact folder

## Next

> "Architecture complete. Next recommended skill: **breakdown** — slice this into ordered, interlinked development tasks."
