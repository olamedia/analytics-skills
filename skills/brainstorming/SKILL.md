---
name: brainstorming
description: Use when you have a defined goal and need to explore different approaches before committing to a direction. Use when there are multiple valid solutions, when trade-offs need evaluation, or when the best approach is not obvious.
---

# Brainstorming

Explore 2-3 approaches to achieve a defined goal, evaluate trade-offs, and pick a direction with the user.

## When to Use

- A goal is defined but the approach is not decided
- Multiple valid solutions exist and trade-offs need evaluation
- The user wants to see options before committing
- The problem can be solved in fundamentally different ways
- You need to stress-test assumptions before investing in a PRD

**When NOT to use:** The approach is already decided and confirmed, or a `brainstorming.md` with a chosen direction already exists.

## Input

- `goal-definition.md` from the artifact folder (required)
- `context-map.md` from the artifact folder (recommended)

## Output

- `brainstorming.md` saved to the artifact folder (see `references/formats.md` for template)

<HARD-GATE>
Do NOT write any code, create any files outside the artifact folder, or take any implementation action during brainstorming. The output is a document with a chosen direction, not implementation.
</HARD-GATE>

## The Process

### Step 1: Load Context

Read `goal-definition.md` and `context-map.md` from the artifact folder. If `goal-definition.md` is missing, tell the user to run `goal-definition` first.

Also check for project documentation listed in `references/context-sources.md` (`docs/TechStack.md`, `docs/ProjectStructure.md`). If available, use them to ground feasibility assessments.

Recap the problem statement and success criteria from the goal definition to confirm alignment.

### Step 2: Scan for Prior Art

If a `context-map.md` exists, scan the codebase for:

- Existing patterns that could be extended or reused
- Similar features already implemented (how were they done?)
- Constraints that eliminate certain approaches
- Libraries already in the stack that could help

Ground your approaches in what actually exists. Don't propose approaches that ignore the codebase.

### Step 3: Generate 2-3 Approaches

For each approach, provide:

- **Name** — a short descriptive label
- **Description** — 2-3 sentences explaining the approach
- **Pros** — concrete advantages
- **Cons** — concrete disadvantages
- **Feasibility** — High / Medium / Low, considering the existing codebase
- **User Value** — High / Medium / Low
- **Risk** — High / Medium / Low

Apply ideation lenses selectively — pick the ones that fit the problem:

- **Simplification:** "What's the version that's 10x simpler?"
- **Inversion:** "What if we approached this from the opposite direction?"
- **Constraint removal:** "What if we removed the hardest constraint?"
- **Audience shift:** "What if this were for a different user?"
- **Combination:** "What if we combined two existing approaches?"

Do NOT generate more than 3 approaches. Quality over quantity.

### Step 4: Present Comparison

Present a side-by-side comparison table:

| Criterion | Approach 1 | Approach 2 | Approach 3 |
|-----------|-----------|-----------|-----------|
| User Value | | | |
| Feasibility | | | |
| Differentiation | | | |
| Risk | | | |

Lead with your recommendation and explain why:

> "I recommend Approach 2 because [rationale]. It trades off [X] for [Y], which aligns with the success criteria because [reason]."

### Step 5: Get User Decision

Ask the user which direction to take. One question, clear options:

```
Which approach should we proceed with?
  A. Approach 1: [name]
  B. Approach 2: [name] (recommended)
  C. Approach 3: [name]
  D. Hybrid or different direction — describe what you're thinking
```

Do NOT proceed until the user picks a direction.

### Step 6: Document Chosen Direction

After the user decides, write `brainstorming.md` to the artifact folder using the template from `references/formats.md`. Include all approaches explored, the comparison, the chosen direction with rationale, and assumptions to validate.

Announce the saved path:

> "Brainstorming saved to `[path]/brainstorming.md`."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The right approach is obvious" | Obvious approaches have hidden trade-offs. Exploring alternatives takes 10 minutes and often reveals a better path. |
| "We don't have time to explore options" | Picking the wrong approach wastes far more time than evaluating 2-3 options upfront. |
| "I'll just build the simplest thing" | Simplest is often right, but you should articulate WHY it's right, not just default to it. |
| "The user already told me how to build it" | Users describe solutions. Your job is to evaluate whether it's the best solution for their goal. |
| "More options means better analysis" | 2-3 well-evaluated approaches beat 10 shallow ones. Don't dilute attention. |

## Red Flags

- Jumping straight to one approach without considering alternatives
- Approaches that ignore the existing codebase or tech stack
- No recommendation — presenting options without an opinion
- All approaches look the same (variations, not genuine alternatives)
- Proceeding without user selecting a direction
- Writing code or creating implementation files during brainstorming

## Verification

Before handing off, confirm:

- [ ] Goal definition and context map loaded
- [ ] 2-3 genuinely different approaches explored
- [ ] Each approach has pros, cons, feasibility, value, and risk
- [ ] Comparison table included
- [ ] Recommendation given with rationale
- [ ] User has selected a direction
- [ ] `brainstorming.md` saved to artifact folder

## Next

> "Direction chosen. Next recommended skill: **prd** — formalize the requirements for this approach."
