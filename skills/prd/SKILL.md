---
name: prd
description: Use when you have a chosen direction and need to formalize requirements into a Product Requirements Document. Use when user stories, acceptance criteria, and scope boundaries need to be written down before architecture or implementation.
---

# PRD

Create a structured Product Requirements Document with user stories, acceptance criteria, functional requirements, and scope boundaries.

## When to Use

- A direction has been chosen and needs to be formalized into requirements
- Before architecture or task breakdown — requirements must exist first
- When the team needs a shared source of truth for what "done" means
- When acceptance criteria need to be explicit and verifiable

**When NOT to use:** Requirements are already documented in a `prd.md` that is still accurate, or the change is a single-line fix with obvious scope.

## Input

- `brainstorming.md` from the artifact folder (required — contains chosen direction)
- `goal-definition.md` from the artifact folder (required — contains success criteria)
- `context-map.md` from the artifact folder (recommended — for technical grounding)

## Output

- `prd.md` saved to the artifact folder (see `references/formats.md` for template)

## The Process

### Step 1: Load Upstream Artifacts

Read `brainstorming.md`, `goal-definition.md`, and `context-map.md` from the artifact folder. If `brainstorming.md` or `goal-definition.md` is missing, tell the user which skill to run first.

Also check for project documentation listed in `references/context-sources.md` (`docs/TechStack.md`, `docs/ProjectStructure.md`). If available, use them to inform technical considerations.

Extract:
- The chosen direction and rationale (from brainstorming)
- Success criteria and constraints (from goal definition)
- Tech stack and existing patterns (from context map)

### Step 2: Ask Clarifying Questions

Ask 3-5 essential clarifying questions where the chosen direction leaves gaps. Focus on:

- **Core Functionality:** What are the key actions a user takes?
- **Scope Boundaries:** What should this specifically NOT do?
- **Edge Cases:** What happens when things go wrong?
- **Success Criteria:** How do we measure "done" beyond the goal definition?

Format questions with lettered options for quick answers:

```
1. How should the system handle duplicate entries?
   A. Allow duplicates silently
   B. Reject with a validation error
   C. Warn the user and let them decide
   D. Auto-merge duplicates

2. What is the scope for the first version?
   A. Minimal viable — core happy path only
   B. Full-featured with error handling
   C. Core path + most common edge cases
   D. Other: [please specify]
```

Users can answer with "1C, 2A" for quick iteration.

Only ask questions where the answer genuinely changes the PRD. Do not ask questions you can answer from the upstream artifacts.

### Step 3: Write User Stories

For each piece of functionality, write a user story:

```markdown
### US-001: [Title]
**Description:** As a [user], I want [feature] so that [benefit].

**Acceptance Criteria:**
- [ ] [Specific verifiable criterion]
- [ ] [Specific verifiable criterion]
```

Rules for user stories:
- Each story must be small enough to implement in one focused session
- Acceptance criteria must be verifiable, never vague
- Bad: "Works correctly", "Good UX", "Handles edge cases"
- Good: "Button shows confirmation dialog before deleting", "Response returns within 500ms"
- Number stories sequentially: US-001, US-002, etc.

### Step 4: Write Functional Requirements

Number each requirement for easy reference:

```
- FR-1: The system must allow users to create an account with email and password
- FR-2: When a user submits an invalid email, the system must display an inline error message
- FR-3: Passwords must be at least 8 characters with one number and one special character
```

Be explicit and unambiguous. The reader may be a junior developer or AI agent.

### Step 5: Define Non-Goals

Carry over the "Not Doing" list from goal definition and expand it based on the clarifying questions:

```
## Non-Goals (Out of Scope)
- No email verification in v1 — will be added later
- No social login — only email/password for now
- No admin dashboard — management via database directly
```

This section prevents scope creep. Every PRD must have non-goals.

### Step 6: Add Technical and Design Considerations

Based on the context map, note:
- Existing components that can be reused
- Integration points with existing systems
- Performance requirements
- Known constraints or dependencies

### Step 7: Define Success Metrics

Translate the success criteria from goal definition into measurable metrics:

```
## Success Metrics
- Registration completes in under 3 clicks
- Error rate for form submission < 5%
- Page load time < 2 seconds
```

### Step 8: Write and Save PRD

Write `prd.md` to the artifact folder using the template from `references/formats.md`. Include all sections from Steps 3-7, plus an Open Questions section for anything unresolved.

Present the PRD to the user for review before saving. Apply any requested changes.

Announce the saved path:

> "PRD saved to `[path]/prd.md`."

## Writing Quality

The PRD reader may be a junior developer or AI agent:

- Be explicit and unambiguous
- Avoid jargon or explain it
- Provide concrete examples where helpful
- Number requirements for easy reference
- Make acceptance criteria verifiable (not "works correctly")

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The requirements are obvious from the brainstorming" | Brainstorming picks a direction. PRD defines every detail within that direction. Different levels of specificity. |
| "User stories are too formal" | User stories force you to name the user, the action, and the benefit. That's clarity, not ceremony. |
| "We'll figure out edge cases during implementation" | Edge cases found during implementation cost 10x more than edge cases found during requirements. |
| "Non-goals section is unnecessary" | Without non-goals, scope creep is invisible. Every "quick addition" erodes focus. |
| "Acceptance criteria slow us down" | Acceptance criteria define "done." Without them, you're done when someone feels like it. |

## Red Flags

- User stories without acceptance criteria
- Acceptance criteria that say "works correctly" or "is intuitive"
- No non-goals section
- No functional requirements — only user stories (or vice versa)
- PRD that doesn't reference the chosen direction from brainstorming
- Proceeding to architecture without user reviewing the PRD

## Verification

Before handing off, confirm:

- [ ] Upstream artifacts loaded (brainstorming, goal definition)
- [ ] 3-5 clarifying questions asked where needed
- [ ] Every user story has verifiable acceptance criteria
- [ ] Functional requirements are numbered and unambiguous
- [ ] Non-goals section has at least 2 items
- [ ] Success metrics are measurable
- [ ] User has reviewed and approved the PRD
- [ ] `prd.md` saved to artifact folder

## Next

> "PRD complete. Next recommended skill: **architecture** — design the technical approach for these requirements."
