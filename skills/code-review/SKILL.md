---
name: code-review
description: |
  Code review skill for Vue 3 and TypeScript projects.
  Discovers what to review (uncommitted, committed, or GitHub PR), applies structured review process
  with severity-labeled feedback, language-agnostic quality rules, and language-specific references.
  Use when: reviewing code, code review, MR review, quality check, reviewing changes.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Shell
---

# Code Review

Structured code review with severity-labeled feedback. Discovers scope from git state, loads language-specific rules on demand.

## When to Use

- Reviewing uncommitted or committed code changes
- Reviewing a GitHub PR
- MR review or quality check
- Establishing review standards

## Project Conventions

Before reviewing, check if the project has a `docs/CodeStyle.md` file (or similar convention document). If present, load it and treat project-specific rules as overrides to the general rules below.

## Review Process

### Phase 1: Scope Discovery

Determine what to review. The user specifies the mode, or the skill asks.

**Mode 1 — Uncommitted changes:**
1. Run `git status` to collect changed, staged, and untracked files
2. Present a compact file list grouped by status
3. Ask the user to confirm or exclude files

**Mode 2 — Committed changes:**
1. Ask the user if they are on the correct branch
2. Ask for the diff target: `master`, a branch name, or a commit ID
3. Run `git diff <target>...HEAD --name-status` to get the file list
4. Present the list, ask the user to confirm or exclude files

**Mode 3 — GitHub PR:**
1. Ask the user for the PR number
2. Run `gh pr diff <number> --name-only` to get changed files
3. Present the list, ask the user to confirm or exclude files

After confirmation, record the **confirmed file list** and the **diff mode**. Each file's diff will use the matching method:
- Uncommitted → working tree diff (`git diff` / `git diff --cached`)
- Committed → `git diff <target>...HEAD -- <file>`
- GitHub PR → `gh pr diff <number> -- <file>`

Also check for `docs/CodeStyle.md` and load if present.

Note the change size. If the confirmed list exceeds ~400 changed lines, suggest splitting.

### Phase 2: High-Level Review

1. Apply general code quality rules (see below)
2. Check file organization and architecture fit
3. Identify file extensions in the confirmed list
4. Load language references:
   - `.vue` files → read all files in `references/vue/`
   - `.ts` / `.js` files → read all files in `references/typescript/`
   - Security concerns → read `references/security.md`

### Phase 3: Detailed Review

For each file in the confirmed list:
1. Get the diff using the recorded mode
2. Read the full file for context where needed
3. Apply language-specific rules from loaded references
4. Label each finding with a severity (see below)
5. Use question-based feedback for non-blocking items

### Phase 4: Summary & Report

1. Run through checklists (general + language-specific)
2. Write the review report to `docs/reviews/<branch-name>.md` (create folder if needed)
   - Get branch name: `git branch --show-current`
   - If uncommitted mode with no branch (detached HEAD), use the date: `review-YYYY-MM-DD.md`
   - Ask user to confirm or change the save path before writing
3. Report format — see **Report Template** below

## Report Template

```markdown
# Code Review: <branch-name>

**Date:** YYYY-MM-DD
**Reviewer:** AI (code-review skill)
**Mode:** Uncommitted / Committed (diff target: `<target>`) / GitHub PR #N
**Files reviewed:** N

## Summary

<1-3 sentence overall assessment. State the verdict: looks good / has issues to address / needs significant rework.>

### Stats

| Severity | Count |
|----------|-------|
| blocking | N |
| important | N |
| nit | N |
| suggestion | N |
| praise | N |

## Findings

### <file-path>

- **[severity]** <finding title>
  <description — what, why it matters, suggestion>

- **[severity]** <finding title>
  <description>

### <file-path>

- **[severity]** ...

## Good Patterns

- <what was done well and why it's worth keeping>

## Checklist

<paste the filled general + language-specific checklists with [x] for passed items>
```

## Severity Labels

Use one label per finding. Every finding must have a label.

| Label | Meaning | Action required |
|-------|---------|----------------|
| `[blocking]` | Bug, security issue, data corruption risk | Must fix before merge |
| `[important]` | Test gap, unclear naming, moderate perf issue | Should fix; discuss if disagree |
| `[nit]` | Style, minor naming, readability | Nice to have, not blocking |
| `[suggestion]` | Alternative approach worth considering | No action needed |
| `[learning]` | Educational — explains why something matters | No action needed |
| `[praise]` | Good work, reinforcement of good patterns | No action needed |

## Feedback Approach

**Ask questions instead of making statements:**
- "What happens if `items` is an empty array?" instead of "This will fail on empty arrays"
- "How should this behave if the API call fails?" instead of "You need error handling"

**Use collaborative language:**
- "Consider..." / "Have you thought about..." / "Would it make sense to..."
- Not: "You must..." / "This is wrong" / "Why didn't you..."

**Be specific and actionable:**
- Include what the problem is, why it matters, and a concrete suggestion
- Reference the relevant rule or pattern when applicable

**Balance criticism with praise:**
- Call out good patterns with `[praise]`
- Acknowledge thoughtful decisions

## General Code Quality Rules

These apply to any language. Brief checks — detailed patterns live in language references.

- **Single responsibility** — each function/component does one thing
- **Size limits** — functions < 50 lines, files < 300 lines, function parameters < 4
- **Naming** — descriptive names, no single-letter variables (except loop indices), consistent casing
- **Dead code** — no commented-out code, no unused imports or variables
- **Complexity** — no nesting deeper than 3 levels, no long boolean chains (extract to named variables)
- **Error handling** — no swallowed errors, no empty catch blocks, no ignored promise rejections
- **DRY** — no copy-paste blocks longer than 5 lines (extract to shared function)
- **Change scope** — the change does one thing; < 400 changed lines preferred

## General Checklist

- [ ] Each function/component has a single clear responsibility
- [ ] No function exceeds ~50 lines
- [ ] No file exceeds ~300 lines
- [ ] Names are descriptive and consistent
- [ ] No commented-out code or unused imports
- [ ] No nesting deeper than 3 levels
- [ ] All errors are handled (no empty catch, no swallowed rejections)
- [ ] No duplicated logic blocks > 5 lines
- [ ] Change is focused — does one thing

## Language References

Load the matching references based on file extensions in the confirmed list.

| File extension | Reference | Load |
|---------------|-----------|------|
| `.vue` | `references/vue/` | All files in folder |
| `.ts`, `.js` | `references/typescript/` | All files in folder |
| Any (security concern) | `references/security.md` | When security issues spotted |

Each reference folder contains topic files with ❌/✅ patterns and a `checklist.md` for quick final-pass.
