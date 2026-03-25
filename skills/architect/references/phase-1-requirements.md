# Phase 1: Requirements

Take a raw idea and turn it into a structured requirements document with intent, user stories, and a questions log.

This phase combines what the original pipeline does in 4 separate skills (goal-definition → brainstorming → prd → research). Instead of 4 artifacts, it produces one `requirements.md` and one `context-map.md`.

## Input

- Raw idea or request from the user
- Artifact folder path
- See `context-sources.md` for additional project documentation that may provide context

## Output

- `requirements.md` in the artifact folder (see `formats.md` for template) — created immediately, updated incrementally
- `context-map.md` in the artifact folder (see `formats.md` for template)

<HARD-GATE>
This is a READ-ONLY requirements phase. Do NOT write any code, create any source files, or modify the codebase. The output is a requirements document, not implementation.
</HARD-GATE>

## Key Principle: `requirements.md` is a Living Document

`requirements.md` is created the moment the user says the first bit of the idea. It is updated incrementally throughout Phase 1 — not assembled at the end.

- Intent block → written on the first user message, updated on every subsequent one
- User stories → initial set derived from the intent after research, refined as questions are answered
- Questions log → entries added as questions arise, statuses updated as they're resolved

Every step below updates `requirements.md` in place. There is no "save at the end" step.

## The Process

### Step 1: Collect the Idea Draft

Ask the user what they want to build:

> "What's the idea or goal for this feature?"

On the **first user message**, create `requirements.md` in the artifact folder with the Intent block. Use the template from `formats.md`.

**CRITICAL:** Use the user's own language. Do NOT rephrase, do NOT add AI fluff, do NOT use corporate language. If the user said "we should update domain items privacy too since BE may not do it", write exactly that.

During this step, **only compile** — do NOT ask clarification questions, do NOT write user stories, do NOT run research. Just accumulate the user's idea into the Intent block as one big blob.

After each user message, update the Intent block in `requirements.md` and ask:

> "Anything to add to the idea?"

Continue until the user says the initial idea draft is complete.

### Step 1b: Design Slicing Check

Before running research, check if the feature involves **new UI components**. If it does, design slicing (a reference implementation, mockup component, or visual spec) is a **task dependency** — not something to search for.

Ask the user:

> "This feature needs new UI. Where is the design slicing? Provide a path, or point me to an existing component to use as a reference."

The user may:
- Provide a path to existing slicing
- Create a slicing file and provide the path
- Point to an existing component and say "do it like this one"
- Say there is no slicing — note it as a gap and proceed with requirements based on the user's verbal description

**Do NOT search the codebase hoping to find slicing.** If slicing exists, the user knows where it is. If it doesn't exist, the user will tell you how to proceed. Never design UI from scratch when the project has a design system.

When slicing is provided, read it during research (Step 2) and document it in `context-map.md` — it defines the target layout, props, emits, and UIKit dependencies.

### Step 2: Initial Research

Once the user confirms the idea draft is complete, run research on the codebase. This is **mandatory** — never skip it.

Follow the research process defined in `references/research.md`. This is a "first research" run — broad scope, scanning everything the idea touches.

Save findings to `context-map.md` in the artifact folder.

Present a brief summary:

> "Context mapped. Found: [key APIs], [key DTOs], [key components]. [Any notable gaps]."

### Step 3: Initial User Stories

After research, convert the intent into initial user stories. Read the intent block and `context-map.md` together — derive every user story you can from what's already clear. Write them into `requirements.md`.

This step is about extraction, not invention. Only write stories that trace directly to the intent. Don't ask questions yet — just convert what you have into structured form. Gaps and ambiguities will become obvious during this conversion; note them but save questions for Step 4.

After writing the stories, compile a **Goal** section in `requirements.md` — a short humanized summary that merges all user stories into readable prose. The Goal sits between Intent and User Stories. It is the essence of what the stories describe, written for a human reader who doesn't want to parse formal user story format.

**The Goal must be updated whenever user stories are added, removed, or changed** — it always reflects the current set of stories.

Present the initial stories and goal to the user for review.

### Step 4: Requirements Loop

After initial stories are written, the requirements loop begins. It is driven by **new requirements** from the user and **questions** from the AI. Questions arise naturally from gaps discovered during Step 3 and from new information arriving.

**The trigger is always: a new requirement or clarification from the user, or gaps found during story writing.**

When a new requirement arrives (user adds something, or answers a question):

1. **Update Intent** in `requirements.md` if it refines or extends the goal
2. **Write or update user stories** from the parts that are clear — add to User Stories section
3. **Ask questions** if anything is unclear — add them to Questions section as `[to-ask]`
4. **Run research** if the requirement mentions things not in context-map (see `references/research.md`, "subsequent research") → update `context-map.md`
5. **Update questions** when answered — change status to `[resolved]`, add answer on next line

This loop runs until no `[to-ask]` questions remain and the user has nothing to add.

#### Asking Questions

Before marking a question `[to-ask]`, check if the codebase can answer it. Run research (`references/research.md`, subsequent research) and update `context-map.md` with findings. Only mark `[to-ask]` if the answer requires a human decision — not missing context.

All questions go into the Questions section of `requirements.md` first — with the correct status (`[resolved]` if answered by research, `[to-ask]` if it needs the user). Then present only the `[to-ask]` items to the user. Never ask a question verbally without writing it into the document first.

**API contracts are not up for discussion.** Frontend DTOs mirror the backend contract. The contract is either already in the codebase (existing API), described by backend tasks, or defined in a parent task. If the contract exists — read it from OpenAPI spec, existing DTOs, or backend repositories (if running in a mixed environment with BE and FE repos, the contract may be discoverable via research). If it doesn't exist yet — it's a backend concern, not a frontend requirements question. Do not ask how the API should be shaped.

Ask targeted questions — one at a time. Focus on what's unclear in the intent:

- **Who is this for?** — The specific user and their situation
- **What does success look like?** — Concrete, observable outcomes
- **Scope Boundaries:** What should this specifically NOT do?
- **Edge Cases:** What happens when things go wrong?
- **Constraints:** Time, tech, resources, compatibility

Use structured options where possible for quick answers:

```
How should the system handle duplicate contacts?
  A. Allow duplicates silently
  B. Reject with a validation error
  C. Warn the user and let them decide
  D. Auto-merge duplicates
```

Users can answer with "A" or "1A, 2C" for quick iteration.

Do NOT assume answers. If you don't know and the codebase can't answer, add a `[to-ask]` question.

#### Writing User Stories

User stories are a **clarification layer** — they take ambiguous or implicit parts of the idea and make every edge case and decision point explicit. They appear as a natural result of answered questions and new requirements.

```markdown
### US-001: [Title]
**As a** [user type], **I want** [action] **so that** [benefit].

**Acceptance Criteria:**
- [ ] [Specific verifiable criterion]
- [ ] [Specific verifiable criterion]
```

Rules:
- Each story must be small enough to implement in one focused session
- Acceptance criteria must be verifiable, never vague
- **Bad:** "Works correctly", "Good UX", "Handles edge cases"
- **Good:** "Button shows confirmation dialog before deleting", "Response returns within 500ms"
- Number stories sequentially: US-001, US-002, etc.
- They must trace back to something in the Intent block
- Acceptance criteria should read naturally — apply `references/humanizer.md` rules, avoid AI vocabulary and structural tells

#### Question Statuses

```
- [resolved] How should privacy be sent to the API?
  As boolean true/false in the POST body
- [to-ask] Should we support bulk contact assignment?
- [not-planned] What about WHOIS privacy for .uk domains?
  UK domains don't support WHOIS privacy at registrar level
- [deferred] Contact validation against ICANN rules
  Will be addressed in Phase 2 of the feature
```

| Status | Meaning |
|--------|---------|
| `[resolved]` | Answered. Answer/clarification follows on next line. |
| `[to-ask]` | Needs to be asked to the user. |
| `[not-planned]` | Related concern, explicitly excluded. Reason follows. |
| `[deferred]` | Will be addressed later. Reason follows. |

### Step 5: Design Readiness Check

Before closing Phase 1, walk through **every step** of the Phase 2 process (`references/phase-2-design.md`) and for each, check if you can complete it using only `requirements.md` and `context-map.md`. Do this in one pass — collect all questions that surface, don't stop at the first one.

Checklist to walk through:

- **Components** — Can you name every component, its location, and its dependencies? Are all UIKit/shared components available in the project, or do any need porting/creating?
- **Data flow** — Can you trace the full flow from trigger to render to cleanup?
- **Dependencies** — Are all imports resolvable? Any missing packages or components?
- **Integration points** — Do you know every file that needs modification?
- **Risks** — Can you identify concrete risks with evidence?

If new context was discovered during this check (e.g. a component doesn't exist, a utility is missing), **update `context-map.md` first**, then add questions to `requirements.md`. Context-map is always updated before any other artifact.

If questions were collected — add them all as `[to-ask]` to `requirements.md`, present them to the user together, and loop back to Step 4. Run research if needed to fill gaps in `context-map.md`.

Only proceed when the full pass produces no new questions.

### Phase 1 Ends

When no `[to-ask]` questions remain, the design readiness check passed, and the user has nothing to add, Phase 1 is done. `requirements.md` is already complete.

> "Requirements complete at `[path]/requirements.md`. Continue to design?"

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The goal is obvious" | Obvious goals have hidden assumptions. Write them down — it takes 5 minutes. |
| "We'll figure out success criteria later" | Without success criteria, you can't know when you're done. Define them now. |
| "The user already told me what they want" | Users describe solutions. Your job is to uncover the underlying problem and edge cases. |
| "User stories are too formal" | User stories force you to name the user, the action, and the benefit. That's clarity, not ceremony. |
| "We'll figure out edge cases during implementation" | Edge cases found during implementation cost 10x more than edge cases found during requirements. |
| "I already know the codebase" | Context maps surface patterns you take for granted. Research prevents assumptions that explode during implementation. |
| "Assumptions are too early to surface" | The earlier you surface assumptions, the cheaper they are to correct. |

## Red Flags

- Intent written in AI language instead of user's words
- Assuming an API exists without checking via research
- Proceeding with questions still marked `[to-ask]`
- User stories that don't trace back to the intent
- context-map that's stale after user added new requirements
- Jumping to solutions before defining the problem
- No "who is this for" answer
- Success criteria that say "works correctly" or "is fast"
- Empty questions section — every feature has questions
- Not running initial research after idea is confirmed
- Jumping straight to questions without first deriving user stories from the intent — stories make gaps visible
- Goal section missing or stale after user stories were updated
- Asking the user a question that the codebase could answer — run research first
- Asking a question verbally without writing it into the Questions section first
- Asking how to design a DTO instead of asking what the backend returns — API contracts are backend decisions
- Closing Phase 1 without the design readiness check — questions will surface during Phase 2 and force backtracking
- Writing code during the requirements phase

## Verification

Before handing off, confirm:

- [ ] Artifact folder exists and path is known
- [ ] Intent block uses user's exact words
- [ ] Initial research completed and `context-map.md` saved
- [ ] Initial user stories derived from intent before questions were asked
- [ ] Goal section compiled from user stories and kept in sync with any story changes
- [ ] All questions written into document before being asked
- [ ] Research-answerable questions resolved via codebase, not asked to user
- [ ] All clarification questions asked and answered
- [ ] Assumptions listed and confirmed by user
- [ ] Every user story has verifiable acceptance criteria
- [ ] Questions log has no `[to-ask]` items remaining
- [ ] User has reviewed and nothing left to add
- [ ] Design readiness check performed — mental design draft produced no new questions
- [ ] `requirements.md` is up to date (has been updated incrementally throughout)

## Next

> "Requirements complete. Next phase: **design** — design the technical architecture for these requirements."
