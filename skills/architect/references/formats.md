# Artifact Formats

Shared format definitions for the architect skill pipeline. Every phase reads this file to know how to read its inputs and write its outputs.

## Folder Convention

- **First phase** asks the user for the artifact folder, or the user specifies it in chat (e.g. `docs/plans/user-auth/`).
- **Subsequent phases** save to the same folder. Detect the folder from the path of the input artifact they read.
- **Each feature** gets its own folder. Never mix artifacts from different features.

## Artifact Files

| Order | Artifact | File name | Produced by | Consumed by |
|-------|----------|-----------|-------------|-------------|
| 0 | Context Map | `context-map.md` | research (on demand) | all phases |
| 1 | Requirements | `requirements.md` | Phase 1 | Phase 2, Phase 3 |
| 2 | Design | `design.md` | Phase 2 | Phase 3 |
| 3 | Tasks | `tasks.md` | Phase 3 | (end of pipeline) |

## Input/Output Contract

Every phase in the pipeline:

1. **Checks** that required input artifacts exist in the folder before starting. If missing, tell the user which phase to run first.
2. **Reads** input artifacts using the templates defined below.
3. **Writes** its output artifact to the same folder using the template defined below.
4. **Announces** the artifact path after saving.
5. **Asks for confirmation** before continuing to the next phase.

---

## Template: context-map.md

This is a **living document** — updated every time research is performed at any phase.

The context-map for the architect pipeline is **focused** — it only contains context relevant to the feature being planned, not a full project scan. It should contain:

- Existing APIs and their signatures (relevant to the feature)
- Relevant DTOs and their fields
- Existing components that will be reused or modified
- Patterns and conventions to follow
- Things that are missing (untyped fields, missing endpoints, etc.)
- Prior art (similar existing features)

```markdown
# Context Map: [Feature Name]

## Relevant APIs

### [ApiName]
- **Location:** `[path/to/file]`
- **Methods used:**
  - `methodName(params): ReturnType` — [what it does]
  - `methodName(params): ReturnType` — [what it does]

### [ApiName]
[same structure]

## Relevant DTOs / Types

### [DtoName]
- **Location:** `[path/to/file]`
- **Fields:**
  - `fieldName: type` — [what it means]
  - `fieldName: type` — [what it means]
- **Notes:** [anything missing or notable]

### [DtoName]
[same structure]

## Relevant Components

### [ComponentName]
- **Location:** `[path/to/file]`
- **Purpose:** [what it does]
- **Props/Interface:** [key props or interface]
- **Notes:** [how it relates to this feature]

### [ComponentName]
[same structure]

## Relevant Utilities

- `functionName` at `[path]` — [what it does]
- `functionName` at `[path]` — [what it does]

## Relevant Store Modules

### [ModuleName]
- **Location:** `[path/to/file]`
- **Getters used:** `getterName` — [what it returns]
- **Actions used:** `actionName` — [what it does]

## Patterns and Conventions

- [Pattern 1 observed in the codebase relevant to this feature]
- [Pattern 2]
- [Convention to follow]

## Missing / Gaps

- [Thing that should exist but doesn't — e.g. "CartItemListDto does not type cart_options field"]
- [Field that is untyped]
- [API that lacks frontend wrapper]
```

---

## Template: requirements.md

Three sections: **Intent** (user's words), **User Stories** (AI-generated clarification), **Questions** (running log).

```markdown
# Requirements: [Feature Name]

## Intent

[User's idea/goal written as close as possible in their own words.
Do NOT rephrase in corporate or AI language.
If the user said "we should also update privacy since BE might not do it", write exactly that.
This block is the source of truth. Update it whenever the user refines or extends the goal.]

## User Stories

User stories clarify the intent. They take ambiguous or implicit parts of the idea and make every edge case and decision point explicit.

### US-001: [Title]
**As a** [user type], **I want** [action] **so that** [benefit].

**Acceptance Criteria:**
- [ ] [Specific, testable condition]
- [ ] [Specific, testable condition]

### US-002: [Title]
[same structure]

### US-003: [Title]
[same structure]

## Questions

Running log of all questions that arose during requirements gathering.
Each question has a status and, if resolved, the answer or clarification below it.

- [resolved] Question text here
  Answer or clarification text here
- [to-ask] Question text here
- [not-planned] Question about related but out-of-scope concern
  Clarification of why it's not planned
- [resolved] Another resolved question
  The answer provided by user
```

### Question Statuses

| Status | Meaning |
|--------|---------|
| `[resolved]` | Question was answered. Answer/clarification follows on the next line. |
| `[to-ask]` | Question needs to be asked to the user. |
| `[not-planned]` | Related concern, explicitly excluded. Clarification of why follows on next line. |
| `[deferred]` | Will be addressed in a later phase or feature. Reason follows on next line. |

---

## Template: design.md

```markdown
# Design: [Feature Name]

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
[User Action] → [Frontend Component] → [API Call] → [Backend] → [Response] → [UI Update]
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

## Tech Decisions
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
```

---

## Template: tasks.md

```markdown
# Tasks: [Feature Name]

## Overview
[One sentence: what this task list implements]

**Source Requirements:** [path to requirements.md]
**Source Design:** [path to design.md]

### Task Graph

` ` `mermaid
graph LR
    T1[Task 1: Cart DTOs] --> T3[Task 3: useContactPicker]
    T2[Task 2: canItemHaveContact] --> T3
    T3 --> T5[Task 5: Wire CheckoutContactStep]
    T4[Task 4: ContactAutocomplete] --> T5
` ` `

---

## Phase 1: [Phase Name]

### Task 1: [Short descriptive title]
- **Covers:** US-001, US-003
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

---

## Template: compiled issue (task-N.md)

Used by Phase 4 (Compile). Each file is a standalone, self-contained issue for an issue tracker. Written in **human language** throughout — no structured field labels where a sentence works, no AI-facing instructions. The reader is a developer or tester picking up a ticket.

```markdown
# [Short descriptive title]

## Goal
[Starts with scope (where in the app), then what's being done. One or two sentences, human language. If scope is short, merge it into the goal sentence.]

Example: "At checkout page, Step 3 — prefill the contact form from the user's default contact when the step opens."

## Links
[Optional. figma, screenshots, slicing, related PRs — to be filled in. Skip section if nothing to link.]

## Dependencies
- [Task/issue this depends on — linkable for issue trackers]

## Proposed changes
[Checklist of concrete changes, file by file. Use sub-headers to group when the task spans multiple areas. Flat list for single-area tasks. Add a motivation line below a group when the reason isn't obvious.]

### [Area name, e.g. "API layer"]
- [ ] `path/to/file.ts` — what to change
- [ ] `path/to/other-file.ts` — what to change

### [Area name, e.g. "UI"]
- [ ] `path/to/component.vue` — what to change
- [ ] `path/to/other-component.vue` — what to change

Motivation: [why this group of changes is needed, if not obvious]

## Acceptance criteria
[Humanized version of user stories' acceptance criteria. Skip anything covered by CI. Include "don't touch" constraints — things that must stay unchanged.]

- Contact form prefills first name, last name, email, phone from the default contact
- Changing a field and clicking Confirm updates the existing contact
- getContactsPage stays paginated — not changed to load all at once

## Testing notes
[Manual verification steps. Written for someone clicking through the app.]

Open checkout with a domain item in cart. Step 3 should show with the contact form prefilled. Change the name, click Confirm. Verify the name updated. Remove all domain items from cart. Step 3 should disappear.

## Notes
[Optional. Uncommon knowledge that helps the implementer — prior art, dead code, non-obvious relationships. Skip if nothing non-obvious. Do not state things obvious from reading the code.]

## Out of scope
[Optional. Only present if [not-planned] or [deferred] questions from requirements intersect this task.]

WHOIS privacy for .uk domains is not handled here — those domains don't support registrar-level privacy.
```
