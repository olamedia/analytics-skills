# Audit Report Format

Template for audit mode output. Adapt sections to the scope of the audit — skip sections that don't apply.

---

## Template

```markdown
# Frontend Design Audit

## Summary
[One paragraph: what was audited (files, pages, components), overall assessment (healthy / needs work / critical issues), key theme of findings.]

---

## Problems

### Critical
[Issues that actively harm trust, accessibility, or usability. Must fix.]

| # | Element | Issue | Practice |
|---|---------|-------|----------|
| 1 | How to find it (see below) | What's wrong | Category § item from practices.md |

### Warning
[Issues that reduce quality but don't break functionality.]

| # | Element | Issue | Practice |
|---|---------|-------|----------|
| 1 | How to find it | What's wrong | Category § item |

### Note
[Minor issues or inconsistencies worth addressing.]

| # | Element | Issue | Practice |
|---|---------|-------|----------|
| 1 | How to find it | What's wrong | Category § item |

---

## Suggestions
[Improvement opportunities based on processing fluency principles and trust research. Not checklist violations — these are "could be better" items.]

| # | Area | Suggestion | Rationale |
|---|------|------------|-----------|
| 1 | e.g. Typography | What to improve | Why — reference research principle |

---

## Quick Wins
[Subset of problems and suggestions that are easy to fix — low effort, clear improvement.]

- [ ] [Quick win 1 — reference problem or suggestion #]
- [ ] [Quick win 2]
- [ ] [Quick win 3]

---

## Score (optional)
[Category-level summary. Use when auditing a full page or application.]

| Category | Status |
|----------|--------|
| Accessibility | pass / warn / fail |
| Typography | pass / warn / fail |
| Spacing | pass / warn / fail |
| Design Tokens | pass / warn / fail |
| Color System | pass / warn / fail |
| Motion | pass / warn / fail |
| Performance | pass / warn / fail |
| Loading/Error/Empty States | pass / warn / fail |
| Responsive / Mobile | pass / warn / fail |
| Navigation | pass / warn / fail |
| Forms | pass / warn / fail |
| Icons | pass / warn / fail |
| SEO | pass / warn / fail |
| Trust Signals | pass / warn / fail |
```

---

## Identifying Elements

Describe elements so the user can find them. Adapt to the audit source:

| Source | How to identify |
|---|---|
| Source code | `file:line` or component name |
| HTML file | CSS selector: `.hero-banner h2`, or tag + text: `<button> "Sign Up"` |
| Live site / URL | Position + visible text: "the 'Subscribe' button in the footer", or CSS selector if inspectable |
| Screenshot | Position + description: "the gray text block in the top-right of the hero section" |

Combine approaches when helpful: "the `<input>` with placeholder 'Enter email' in the newsletter form (`.newsletter-form input`)"

---

## Severity Guide

| Severity | Meaning | Examples |
|----------|---------|---------|
| **Critical** | Harms trust, accessibility, or usability | Contrast < 3:1, no focus indicators, broken layout on mobile, hidden costs |
| **Warning** | Reduces quality, noticeable to attentive users | Inconsistent spacing, missing skeleton loaders, no error recovery path |
| **Note** | Minor, worth fixing when convenient | Slightly off type scale, missing `aria-label` on one icon button |

---

## Usage Notes

- **Many problems found:** Focus the report on problems. Keep suggestions brief.
- **Few problems found:** Expand the suggestions section — this is where the value shifts.
- **User asked for problems only:** Skip suggestions section.
- **User asked for suggestions only:** Skip problems section.
- **Quick wins:** Always include — they give the user immediate actionable items regardless of scope.
