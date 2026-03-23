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

### Step 3: Implementation Hints

For each task, decide the level of implementation detail based on task size:

- **Small tasks** — a sentence or two describing the expected change is enough
- **Medium tasks** — point out what changes are expected in each file, with rough examples
- **Large tasks** — rough code examples showing the approach

If the user explicitly asks for full code in implementation hints, provide it. Otherwise keep it at the "rough example" level.

### Step 4: Save and Announce

Save all compiled files. Announce:

> "Compiled [N] tasks to `[path]/tasks/`. Each issue is self-contained."

## Compiled Issue Format

See `formats.md` for the template. The key sections:

### Goal
Starts with scope (where in the app), then what's being done. One or two sentences, human language. If scope is short, merge it into the goal sentence.

Example: "At checkout page, Step 3 — prefill the contact form from the user's default contact when the step opens."

### Links
One-line placeholder for user to fill in: figma designs, slicing, screenshots, related PRs. Skip the section entirely if nothing to link.

### Dependencies
Other tasks/issues this one depends on. Written as linkable references for issue trackers.

### Context
The relevant slice of the codebase for this task, pulled from `context-map.md`:
- Files that will be touched, with paths
- API methods and their signatures
- DTO fields and types
- Response examples (if collected during Phase 1)

Only what's relevant to this task — not the full context-map dump.

### Implementation
Rough approach — what changes are expected in which files. Level of detail scales with task size. Can include code examples if the user asked for them.

### Acceptance Criteria
Humanized version of the user stories' acceptance criteria that this task covers. Skip anything already covered by CI (linting, type checks, build).

### Testing Notes
Manual verification steps. "Open checkout with a domain item in cart. Step 3 should show the contact form prefilled with default contact data. Change the name, click Confirm. Verify the contact is updated."

Written for someone who will click through the app, not read code.

### Out of Scope (optional)
Only present if `[not-planned]` or `[deferred]` questions from `requirements.md` intersect this task. Prevents the implementer from going down a rabbit hole that was explicitly excluded.

Example: "WHOIS privacy for .uk domains is not handled here — those domains don't support registrar-level privacy."

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
- Structured field labels where a sentence works
- Dumping the entire context-map into every issue
- Missing scope in the goal — reader doesn't know where in the app this is
- Implementation hints that are full implementations — keep them rough unless user asked for full code
- Acceptance criteria that repeat CI checks ("code compiles", "no linter errors")
- Missing testing notes — if it has UI, it needs manual test steps

## Verification

For each compiled issue, confirm:

- [ ] Goal includes scope (where) and purpose (what)
- [ ] Context contains only what's relevant to this task
- [ ] API signatures and DTO fields are from context-map (not assumed)
- [ ] Acceptance criteria are humanized and skip CI-covered items
- [ ] Testing notes describe manual steps a tester can follow
- [ ] No references to other planning artifacts — issue is self-contained
- [ ] Written in human language throughout
- [ ] Out of scope section present only if relevant exclusions exist
