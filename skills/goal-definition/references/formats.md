# Artifact Formats

Shared format definitions for the analytics skill pipeline. Every skill reads this file to know how to read its inputs and write its outputs.

## Folder Convention

- **First skill called** in the chain asks the user for the artifact folder, or the user specifies it in chat (e.g. `docs/analytics/user-auth/`).
- **Subsequent skills** save to the same folder. Detect the folder from the path of the input artifact they read.
- **Each feature** gets its own folder. Never mix artifacts from different features.

## Artifact Files

| Order | Artifact | File name | Produced by | Consumed by |
|-------|----------|-----------|-------------|-------------|
| 0 | Context Map | `context-map.md` | `research` | all subsequent skills |
| 1 | Goal Definition | `goal-definition.md` | `goal-definition` | `brainstorming`, `prd` |
| 2 | Brainstorming | `brainstorming.md` | `brainstorming` | `prd`, `architecture` |
| 3 | PRD | `prd.md` | `prd` | `architecture`, `breakdown` |
| 4 | Architecture | `architecture.md` | `architecture` | `breakdown` |
| 5 | Task List | `tasks.md` | `breakdown` | (end of pipeline) |

## Input/Output Contract

Every skill in the pipeline:

1. **Checks** that required input artifacts exist in the folder before starting. If missing, tell the user which skill to run first.
2. **Reads** input artifacts using the templates defined below.
3. **Writes** its output artifact to the same folder using the template defined below.
4. **Announces** the artifact path after saving.
5. **Recommends** the next skill at the end.

---

## Template: context-map.md

```markdown
# Context Map

## Project
- **Name:** [project name]
- **Path:** [absolute path to project root]
- **Type:** [web app / API / library / CLI / monorepo / other]

## Tech Stack
- **Languages:** [e.g. TypeScript 5, Python 3.12]
- **Frameworks:** [e.g. React 18, Express, Django]
- **Database:** [e.g. PostgreSQL via Prisma, SQLite]
- **Build/Dev:** [e.g. Vite, Webpack, npm scripts]
- **Testing:** [e.g. Vitest, Jest, Playwright]
- **Other:** [CI, deployment, notable tools]

## Project Structure
[Directory tree with descriptions of key directories]

## Key Patterns
- **Component pattern:** [how components are structured]
- **State management:** [approach]
- **API pattern:** [REST / GraphQL / tRPC, how routes are organized]
- **Error handling:** [approach]
- **Naming conventions:** [files, functions, variables]

## Existing Conventions
- [Convention 1 observed in the codebase]
- [Convention 2]
- [Convention 3]

## Rules Files Found
- [List any CLAUDE.md, .cursorrules, AGENTS.md, etc. and key contents]

## Constraints
- [Technical constraint 1]
- [Technical constraint 2]

## Prior Art
- [Relevant existing features or patterns that relate to the upcoming work]

## Codebase Health Notes
- [Any notable issues: large files, unclear boundaries, missing tests, etc.]
```

---

## Template: goal-definition.md

```markdown
# Goal Definition

## Problem Statement
[One-sentence "How Might We" framing of the problem]

## Goal
[What we want to achieve — clear, specific, measurable where possible]

## Who Is This For
- **Primary user:** [who benefits most]
- **Secondary users:** [others affected]

## Success Criteria
- [ ] [Specific, testable condition 1]
- [ ] [Specific, testable condition 2]
- [ ] [Specific, testable condition 3]

## Assumptions
- [Assumption 1 — and how to validate it]
- [Assumption 2 — and how to validate it]

## Constraints
- [Technical constraint]
- [Time/resource constraint]
- [Business constraint]

## Not Doing (and Why)
- [Thing 1] — [reason]
- [Thing 2] — [reason]

## Open Questions
- [Question needing answer before proceeding]
```

---

## Template: brainstorming.md

```markdown
# Brainstorming

## Problem Recap
[One-sentence recap from goal-definition.md]

## Approaches Explored

### Approach 1: [Name]
- **Description:** [2-3 sentences]
- **Pros:** [bullet list]
- **Cons:** [bullet list]
- **Feasibility:** [High / Medium / Low]
- **User Value:** [High / Medium / Low]
- **Risk:** [High / Medium / Low]

### Approach 2: [Name]
- **Description:** [2-3 sentences]
- **Pros:** [bullet list]
- **Cons:** [bullet list]
- **Feasibility:** [High / Medium / Low]
- **User Value:** [High / Medium / Low]
- **Risk:** [High / Medium / Low]

### Approach 3: [Name] (optional)
[same structure]

## Comparison

| Criterion | Approach 1 | Approach 2 | Approach 3 |
|-----------|-----------|-----------|-----------|
| User Value | | | |
| Feasibility | | | |
| Differentiation | | | |
| Risk | | | |

## Chosen Direction
- **Selected:** [Approach N]
- **Rationale:** [Why this approach over others — 2-3 sentences]
- **Trade-offs accepted:** [What we're giving up]

## Assumptions to Validate
- [ ] [Assumption from chosen approach]
- [ ] [Assumption from chosen approach]
```

---

## Template: prd.md

```markdown
# PRD: [Feature Name]

## Introduction
[Brief description of the feature and the problem it solves]

## Goals
- [Specific, measurable objective 1]
- [Specific, measurable objective 2]

## User Stories

### US-001: [Title]
**Description:** As a [user], I want [feature] so that [benefit].

**Acceptance Criteria:**
- [ ] [Specific verifiable criterion]
- [ ] [Specific verifiable criterion]

### US-002: [Title]
[same structure]

## Functional Requirements
- FR-1: [The system must...]
- FR-2: [When a user does X, the system must...]

## Non-Goals (Out of Scope)
- [What this feature will NOT include]

## Design Considerations
- [UI/UX requirements if applicable]
- [Relevant existing components to reuse]

## Technical Considerations
- [Known constraints or dependencies]
- [Integration points]
- [Performance requirements]

## Success Metrics
- [How success will be measured]

## Open Questions
- [Remaining questions or areas needing clarification]
```

---

## Template: architecture.md

```markdown
# Architecture: [Feature Name]

## Overview
[One paragraph: what we're building and the high-level approach]

## Components

### [Component 1 Name]
- **Responsibility:** [what it does]
- **Location:** [file path(s)]
- **Depends on:** [other components]
- **Interface:** [key functions/endpoints/props it exposes]

### [Component 2 Name]
[same structure]

## Data Flow
[Describe how data moves through the system, or use a diagram]

```
[User Action] → [Frontend Component] → [API Endpoint] → [Service] → [Database]
                                                                    ↓
                                                              [Response] → [UI Update]
```

## Dependency Graph
[What must be built before what]

```
[Foundation Layer]
    ├── [Component A]
    │       ├── [Component B]
    │       └── [Component C]
    └── [Component D]
```

## Tech Stack Decisions
| Decision | Choice | Rationale |
|----------|--------|-----------|
| [Decision 1] | [Choice] | [Why] |
| [Decision 2] | [Choice] | [Why] |

## Integration Points
- [How this connects to existing system part 1]
- [How this connects to existing system part 2]

## Boundaries
- **Always:** [rules that must be followed]
- **Ask first:** [decisions requiring approval]
- **Never:** [hard constraints]

## Risks and Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk 1] | [High/Med/Low] | [Strategy] |
| [Risk 2] | [High/Med/Low] | [Strategy] |

## Open Questions
- [Architectural question needing resolution]
```

---

## Template: tasks.md

```markdown
# Tasks: [Feature Name]

## Overview
[One sentence: what this task list implements]

**Source PRD:** [path to prd.md]
**Source Architecture:** [path to architecture.md]

---

## Phase 1: [Phase Name]

### Task 1: [Short descriptive title]
- **Description:** [One paragraph — what this task accomplishes]
- **Acceptance Criteria:**
  - [ ] [Specific, testable condition]
  - [ ] [Specific, testable condition]
- **Verification:**
  - [ ] [How to confirm — test command, build, manual check]
- **Dependencies:** [Task numbers this depends on, or "None"]
- **Files:**
  - `[path/to/file]`
  - `[path/to/file]`
- **Size:** [XS / S / M / L]

### Task 2: [Title]
[same structure]

### Checkpoint: Phase 1
- [ ] All tests pass
- [ ] Application builds without errors
- [ ] [Phase-specific verification]
- [ ] Review before proceeding

---

## Phase 2: [Phase Name]

### Task 3: [Title]
[same structure]

### Task 4: [Title]
[same structure]

### Checkpoint: Phase 2
- [ ] [Verification items]

---

## Parallelization Notes
- **Safe to parallelize:** [which tasks are independent]
- **Must be sequential:** [which tasks have strict ordering]
- **Needs coordination:** [which tasks share interfaces]

## Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk] | [High/Med/Low] | [Strategy] |

## Task Sizing Reference
| Size | Files | Scope |
|------|-------|-------|
| XS | 1 | Single function or config change |
| S | 1-2 | One component or endpoint |
| M | 3-5 | One feature slice |
| L | 5-8 | Multi-component feature — consider splitting |
```
