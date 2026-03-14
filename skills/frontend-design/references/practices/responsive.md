# Responsive / Mobile-First

Prototypicality is category-specific — users expect mobile sites to work on mobile (NNGroup).

## Checklist

| Practice | Standard |
|----------|----------|
| Design mobile-first, scale up | Mobile-first CSS |
| Systematic breakpoints 375/768/1024/1440 | Common breakpoint set |
| No horizontal scroll on mobile | WCAG / UX |
| Viewport meta: width=device-width, initial-scale=1 | Meta viewport |
| Never disable zoom | WCAG 1.4.4 |
| Touch targets ≥ 44×44px (Apple) / 48×48dp (Material) | Apple HIG / Material Design |
| Min gap between touch targets: 8px | Material Design |
| touch-action: manipulation (reduce 300ms tap delay) | Performance |
| Safe area compliance (notch, gesture bar) | Apple HIG |
| Replace hover with tap on mobile | Touch UX |
| Heavy effect fallback on low-power devices | Performance / Battery |

---

## Detailed Guidance

### Mobile-First CSS Approach: Why Start Small and Add Complexity

Mobile-first means:
1. Base styles target the smallest screen.
2. Layout and layout changes use `min-width` media queries for larger screens.

Benefits:
- **Progressive enhancement** — Small screens get a simpler, faster layout.
- **Fewer overrides** — Add complexity instead of stripping it.
- **Performance** — Mobile gets minimal CSS; larger screens add more.
- **Priorities** — Forces focus on core content and actions first.

```css
/* Base: mobile */
.card {
  padding: 1rem;
  font-size: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .card {
    padding: 1.5rem;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .card {
    padding: 2rem;
    font-size: 1.125rem;
  }
}
```

### Breakpoint Strategy

Common breakpoints:
- **375px** — Small phones (iPhone SE).
- **768px** — Large phones / small tablets, often first layout change.
- **1024px** — Tablets / small laptops.
- **1440px** — Desktop, sometimes max container width.

Use breakpoints for layout and density changes, not for every minor tweak. Prefer fluid values (`clamp`, `min()`, percentages) where possible.

```css
/* Prefer fluid over many breakpoints when possible */
.container {
  padding-inline: clamp(1rem, 5vw, 4rem);
  max-width: min(1200px, 100% - 2rem);
}
```

### Touch Targets: Why 44px Minimum Matters

Apple HIG and Material Design recommend:
- **Apple HIG** — 44×44pt minimum for tappable elements.
- **Material Design** — 48×48dp minimum.

Smaller targets cause mis-taps and frustration. Test with fingertips on real devices; ensure interactive elements meet the minimum and have adequate spacing.

```css
/* Minimum touch target */
.button {
  min-height: 44px;
  min-width: 44px;
  padding: 0.75rem 1.25rem;
}

/* Or use aspect ratio with minimum */
.icon-button {
  min-block-size: 44px;
  min-inline-size: 44px;
}
```

### Safe Area: env(safe-area-inset-*) Usage

Notches and gesture bars can overlap content. Use `env(safe-area-inset-*)` to add padding.

```css
/* Apply safe area insets */
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* Sticky footer with gesture bar */
.footer {
  padding-bottom: max(1rem, env(safe-area-inset-bottom));
}
```

Requires viewport-fit=cover in viewport meta:
```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

### Hover Interactions on Touch Devices

Touch devices usually don’t have hover. Hover-only states (e.g., tooltips, reveals) can:
- Never appear on touch.
- Trigger on tap and stick until another tap (sticky hover).
- Feel wrong when they appear on first tap.

**Approaches:**
- **Use tap as primary** — Show the same info on tap as on hover.
- **Avoid hover-only UIs** — Tooltips and dropdowns should work via tap/click.
- **Detect coarse pointer** — `@media (hover: none)` to disable or change hover behavior.
- **Provide alternative** — If hover reveals info, also surface it via tap or visible controls.

```css
/* Degrade hover on touch */
@media (hover: hover) {
  .card:hover .overlay {
    opacity: 1;
  }
}

@media (hover: none) {
  .card .overlay {
    opacity: 1; /* Always visible, or show on tap */
  }
}
```

### Low-Power Fallbacks: Detect and Degrade

Reduce motion and effects on low-power or reduced-motion setups.

**prefers-reduced-motion** (WCAG 2.3.3):
```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

**Low-power / battery saving** — No reliable media query; use:
- Lighter animations by default.
- Avoid heavy parallax or large canvas animations.
- Provide simpler fallbacks (static images instead of video).

**JavaScript detection** — `navigator.hardwareConcurrency` and `navigator.deviceMemory` can hint at device capability; use cautiously and always provide basic fallbacks.

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Desktop-first with max-width media queries | Use mobile-first with min-width media queries. |
| Horizontal overflow on small screens | Avoid fixed widths > 100vw; use max-width: 100%, overflow-x: hidden only as a last resort. |
| user-scalable=no or maximum-scale=1 | Never disable zoom; omit or use only width and initial-scale. |
| Touch targets under 44px | Use min-height/min-width 44px (or 48px) for all tappable elements. |
| Ignoring safe areas | Use env(safe-area-inset-*) for notch and gesture bar padding. |
| Hover-only interactions | Ensure tooltips, dropdowns, and overlays work on tap/click. |
| Heavy animations everywhere | Respect prefers-reduced-motion; use lighter effects by default. |
| Fixed breakpoints for every tweak | Prefer fluid layout (clamp, min, %) and fewer breakpoints. |
| Forgetting touch-action | Use touch-action: manipulation on interactive elements to reduce tap delay. |
| Tiny gaps between touch targets | Maintain at least 8px between tappable areas. |
