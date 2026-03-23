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
- User stories → added as clarification questions are answered
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

### Step 2: Initial Research

Once the user confirms the idea draft is complete, run research on the codebase. This is **mandatory** — never skip it.

Follow the research process defined in `references/research.md`. This is a "first research" run — broad scope, scanning everything the idea touches.

Save findings to `context-map.md` in the artifact folder.

Present a brief summary:

> "Context mapped. Found: [key APIs], [key DTOs], [key components]. [Any notable gaps]."

### Step 3: Requirements Loop

After initial research, the requirements loop begins. It is driven by **new requirements** from the user and **questions** from the AI. There are no separate stages — questions, user stories, and research all happen as a natural consequence of new information arriving.

**The trigger is always: a new requirement or clarification from the user.**

When a new requirement arrives (user adds something, or answers a question):

1. **Update Intent** in `requirements.md` if it refines or extends the goal
2. **Ask questions** if anything is unclear — add them to Questions section as `[to-ask]`
3. **Write user stories** from the parts that are clear — add to User Stories section
4. **Run research** if the requirement mentions things not in context-map (see `references/research.md`, "subsequent research") → update `context-map.md`
5. **Update questions** when answered — change status to `[resolved]`, add answer on next line

This loop runs until no `[to-ask]` questions remain and the user has nothing to add.

#### Asking Questions

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

Do NOT assume answers. If you don't know, add a `[to-ask]` question.

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

### Phase 1 Ends

When no `[to-ask]` questions remain and the user has nothing to add, Phase 1 is done. `requirements.md` is already complete.

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
- Writing code during the requirements phase

## Verification

Before handing off, confirm:

- [ ] Artifact folder exists and path is known
- [ ] Intent block uses user's exact words
- [ ] Initial research completed and `context-map.md` saved
- [ ] All clarification questions asked and answered
- [ ] Assumptions listed and confirmed by user
- [ ] Every user story has verifiable acceptance criteria
- [ ] Questions log has no `[to-ask]` items remaining
- [ ] User has reviewed and nothing left to add
- [ ] `requirements.md` is up to date (has been updated incrementally throughout)

## Next

> "Requirements complete. Next phase: **design** — design the technical architecture for these requirements."
