# Phase 4: Compile (Optional)

Compile tasks into standalone, human-readable issue documents suitable for issue trackers (Jira, Linear, GitHub Issues) where no other planning artifacts are present.

This phase is **optional**. The pipeline is complete after Phase 3. Phase 4 runs only when the user asks for it.

## When to Use

- User wants to create issues in an issue tracker
- Tasks need to be handed off to developers or a team who won't read the planning artifacts
- Each task needs to be self-contained with all context embedded

**When NOT to use:** The implementer has access to the artifact folder and can read `tasks.md`, `design.md`, `requirements.md`, and `context-map.md` directly.

## Input

- `tasks.md` from the artifact folder (required)
- `requirements.md` from the artifact folder (required — for user stories and questions)
- `design.md` from the artifact folder (required — for context and implementation approach)
- `context-map.md` from the artifact folder (required — for API signatures, DTOs, response examples)

## Output

- `tasks/` folder inside the artifact folder
- One `task-N.md` file per task (or per group of tasks if user selects specific ones)

## The Process

### Step 1: Ask What to Compile

After Phase 3 completes (or when user requests compilation), ask:

> "Want me to compile tasks into standalone issue documents? All tasks, or specific ones?"

The user may want all tasks compiled, or only a subset.

### Step 2: Compile Each Task

For each task, create a `task-N.md` file in the `tasks/` folder. Pull all relevant context from the upstream artifacts so the issue is **self-contained** — the reader should not need to open any other document.

Write in **human language**. No structured field labels where a sentence works. No AI-facing instructions. The reader is a developer or tester picking up a ticket.

Before writing any text, read `references/humanizer.md` and apply its rules. All goal descriptions, acceptance criteria, and testing notes must pass the humanizer filter — no AI vocabulary, no structural tells, no puffery.

### Step 3: Save and Announce

Save all compiled files. Announce:

> "Compiled [N] tasks to `[path]/tasks/`. Each issue is self-contained."

## Compiled Issue Format

See `formats.md` for the template. The key sections:

### Goal
Starts with scope (where in the app), then what's being done. One or two sentences, human language. If scope is short, merge it into the goal sentence.

Example: "At checkout page, Step 3 — prefill the contact form from the user's default contact when the step opens."

### Links (optional)
One-line placeholder for user to fill in: figma designs, slicing, screenshots, related PRs. Skip the section entirely if nothing to link.

### Dependencies
Other tasks/issues this one depends on. Written as linkable references for issue trackers.

### Proposed changes
A checklist of concrete changes, file by file. Use sub-headers to group related changes when the task has multiple areas (e.g. "Foundation", "UI", "API"). For a single-area task, a flat list is fine.

Each item is a file path + what to change. Add a motivation line below a group when the reason for the change isn't obvious from the change itself.

"Don't touch" constraints belong in acceptance criteria, not here. This section is only what TO do.

Detail scales with task size:
- **Small tasks** — one line per file is enough
- **Medium tasks** — file path + what changes in that file
- **Large tasks** — can include rough code examples if the user asked for them

### Acceptance criteria
Humanized version of the user stories' acceptance criteria that this task covers. Skip anything already covered by CI (linting, type checks, build).

Include "don't touch" constraints here — things that must stay unchanged (e.g. "getActiveJobs stays numeric", "existing links keep working"). These prevent the implementer from over-reaching.

### Testing notes
Manual verification steps. Written for someone who will click through the app, not read code.

### Notes (optional)
Uncommon knowledge that helps the implementer but doesn't fit elsewhere — things like "this API already accepts both types (prior art)", "this variable is dead code", "these pages are always navigated to with numeric IDs". Skip if nothing non-obvious to say. Do not state things that are obvious from reading the code.

### Out of scope (optional)
Only present if `[not-planned]` or `[deferred]` questions from `requirements.md` intersect this task. Prevents the implementer from going down a rabbit hole that was explicitly excluded.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The developer can just read the planning docs" | Issue trackers are where work gets done. If the context isn't in the ticket, it doesn't exist for the implementer. |
| "This is just copy-pasting from other artifacts" | It's not copying — it's extracting and humanizing the relevant slice for each task. Different tasks need different context. |
| "Implementation hints will get stale" | They're hints, not contracts. They point the developer in the right direction. The design doc is the source of truth. |
| "All tasks need the same level of detail" | Small tasks need a sentence. Large tasks need examples. Match detail to complexity. |

## Red Flags

- Issues that reference other planning artifacts ("see design.md for details") — they must be self-contained
- AI-language in the issue text — run through `references/humanizer.md` filter
- Proposed changes that duplicate the goal or acceptance criteria — each section has one job
- Dumping the entire context-map into proposed changes — only include files that change
- Missing scope in the goal — reader doesn't know where in the app this is
- Proposed changes that are full implementations — keep them rough unless user asked for full code
- Acceptance criteria that repeat CI checks ("code compiles", "no linter errors")
- Missing testing notes — if it has UI, it needs manual test steps
- Notes section stating things obvious from reading the code — only uncommon knowledge
- Copying from competitor/reference material without verifying against actual codebase research

## Verification

For each compiled issue, confirm:

- [ ] Goal includes scope (where) and purpose (what)
- [ ] Proposed changes are a concrete checklist — file paths and what to change
- [ ] Acceptance criteria are humanized, skip CI checks, and include "don't touch" constraints
- [ ] Testing notes describe manual steps a tester can follow
- [ ] No references to other planning artifacts — issue is self-contained
- [ ] Written in human language throughout
- [ ] Notes section only contains non-obvious knowledge (or is omitted)
- [ ] Out of scope section present only if relevant exclusions exist
- [ ] Everything traces back to user stories — nothing copied from external sources without verification
