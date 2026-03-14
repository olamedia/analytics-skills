# Accessibility

Accessibility signals design quality → triggers halo effect (Tractinsky 2000). WCAG compliance is not optional.

## Checklist

| Practice | Standard |
|----------|----------|
| Color contrast body text ≥ 4.5:1 | WCAG AA |
| Color contrast large text (18px+) ≥ 3:1 | WCAG AA |
| Visible focus rings 2–4px outline, focus-visible | WCAG 2.4.7 |
| prefers-reduced-motion respected | WCAG 2.3.3 |
| aria-label on icon-only buttons | WCAG 4.1.2 |
| Sequential heading hierarchy h1→h2→h3 | WCAG 1.3.1 |
| Keyboard nav matches visual order | WCAG 2.1.1 |
| Skip-to-content link | WCAG 2.4.1 |
| Color never sole indicator | WCAG 1.4.1 |
| Decorative elements aria-hidden="true" | WCAG 1.1.1 |

---

## Detailed Guidance

### How to Test Contrast

**Tools:**
- **WebAIM Contrast Checker** — Paste hex values for foreground/background; shows pass/fail for AA/AAA.
- **Chrome DevTools** — Inspect element → Accessibility pane shows contrast ratio and suggestions.
- **axe DevTools / Lighthouse** — Automated audits catch contrast failures across pages.
- **Color Contrast Analyzer (CCA)** — Desktop app for WCAG compliance checks.

**Automated checks:**
```css
/* Use tools; contrast can't be validated in CSS alone */
/* DevTools: In Elements panel → Accessibility → Contrast */
```

Run Lighthouse with Accessibility audit enabled. Fix all contrast issues before release.

### Focus Management: focus-visible vs focus

**`focus`** — Styles apply whenever the element has focus (including programmatic or mouse-click focus).

**`focus-visible`** — Styles apply only when the browser would show a focus ring (typically keyboard navigation). Modern browsers use heuristics (e.g., Tab vs mouse click) to decide.

**When to use each:**
- Use `:focus-visible` for visible focus rings when you want to hide them on mouse click but show them on keyboard nav.
- Use `:focus` for focus states that must always be visible (e.g., for strict WCAG compliance when user testing shows confusion).
- Best practice: use `:focus-visible` with a visible ring to avoid removing focus indication on keyboard users.

```css
/* Recommended: Show ring on keyboard, subtle on mouse */
button:focus-visible {
  outline: 3px solid var(--color-focus);
  outline-offset: 2px;
}

/* Avoid: Hides focus entirely */
button:focus {
  outline: none;
}
```

### ARIA Patterns: aria-label vs aria-labelledby vs aria-describedby

**aria-label** — Provides a text label when the element has no visible text.
- Use for: icon-only buttons, links represented only by an icon.
- Example: `<button aria-label="Close dialog">×</button>`

**aria-labelledby** — References one or more elements whose text forms the label.
- Use when: the label already exists in the DOM.
- Example: `<div role="dialog" aria-labelledby="dialog-title">` with `<h2 id="dialog-title">Confirm</h2>`

**aria-describedby** — References elements that provide additional description (hints, errors).
- Use when: the label is not enough and you need supplementary text.
- Example: `<input aria-describedby="password-hint" id="pw" />` with `<span id="password-hint">Min 8 characters</span>`

**Rule of thumb:** Prefer `aria-labelledby` when visible text exists. Use `aria-label` only when there is no visible label. Use `aria-describedby` for extra context, not for the primary label.

### Heading Hierarchy: Why Skipping Levels Breaks Screen Reader Navigation

Screen readers let users jump by heading level (h1, h2, h3, etc.). A logical order (h1 → h2 → h3) creates a clear outline.

Skipping levels (e.g., h1 → h4) causes:
- Confusion about document structure.
- Broken heading-navigation in assistive tech.
- Loss of "where am I?" context.

**Correct:**
```html
<h1>Page Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>
  <h2>Another Section</h2>
```

**Incorrect:**
```html
<h1>Page Title</h1>
  <h4>Section</h4>  <!-- skipped h2, h3 -->
```

Use one h1 per page. Keep hierarchy sequential.

### Keyboard Navigation: Tab Order and tabindex

Tab order follows DOM order by default. Elements with `tabindex="0"` are included in the natural order; `tabindex="-1"` is focusable programmatically but skipped in tab order.

**When tabindex is needed:**
- Custom interactive widgets (tabs, accordions, menus): add `tabindex="0"` to focusable items.
- Restore focus after modal close: `tabindex="-1"` on a target element, then `element.focus()`.
- Fix visual order vs DOM order: prefer fixing DOM order over using positive `tabindex` (1, 2, 3…), which creates brittle, hard-to-maintain tab order.

**Rule:** Avoid positive `tabindex`. Use `tabindex="0"` or `tabindex="-1"` only when necessary.

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Removing focus outlines with `outline: none` | Use `:focus-visible` with a visible ring; never remove focus indication. |
| Using color alone for errors or states | Add icons, labels, or patterns; always pair with non-color cues. |
| Icon-only buttons without labels | Add `aria-label` (or visible text) for every interactive icon. |
| Skipping heading levels (h1 → h4) | Use sequential hierarchy: h1, h2, h3. |
| Decorative images announced by screen readers | Add `aria-hidden="true"` to decorative-only elements. |
| Custom widgets not keyboard-focusable | Ensure all interactives are in tab order and operable with Enter/Space/arrows. |
| Forgetting skip-to-content | Add a visible-on-focus skip link as the first focusable element. |
| Ignoring prefers-reduced-motion | Respect `@media (prefers-reduced-motion: reduce)` for animations and transitions. |
