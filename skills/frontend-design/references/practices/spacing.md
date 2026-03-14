# Spacing

Consistent rhythm reduces visual complexity (Tuch et al. 2012).

## Checklist

| Practice | Standard |
|----------|----------|
| Incremental spacing system: 4px/8px base | Design system |
| Container padding responsive: clamp(1rem, 5vw, 4rem) | Fluid layout |
| Consistent max-width on desktop | 1200px–1440px typical |
| Whitespace groups related items (Gestalt proximity) | Gestalt principles |

---

## Detailed Guidance

### The 4px/8px Grid and Why These Increments

Using 4px and 8px as base units gives:
- **Divisibility** — Both divide evenly into common viewport sizes (320, 375, 768, 1024, 1440).
- **Consistency** — One rhythm across layout, typography, and components.
- **Scale** — Easy to build a coherent scale: 4, 8, 12, 16, 24, 32, 48, 64, 96.

Designers and developers can align elements without arbitrary values.

### Spacing Scale Values

A typical scale in `rem` (assuming 1rem = 16px):

| Token | Value | Use case |
|-------|-------|----------|
| 0 | 0 | No gap |
| 0.25 | 4px | Tight inline gaps |
| 0.5 | 8px | Icon–text, small padding |
| 1 | 16px | Default component padding |
| 1.5 | 24px | Section gaps, card padding |
| 2 | 32px | Between blocks |
| 3 | 48px | Section spacing |
| 4 | 64px | Major sections |
| 6 | 96px | Page sections |
| 8 | 128px | Hero/landing gaps |

```css
:root {
  --space-0: 0;
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 1rem;
  --space-4: 1.5rem;
  --space-5: 2rem;
  --space-6: 3rem;
  --space-8: 4rem;
  --space-10: 6rem;
  --space-12: 8rem;
}
```

### When to Use Which Spacing Value

- **Component internal padding** — `0.5rem`–`1rem` for buttons, inputs, chips.
- **Section gaps** — `1.5rem`–`2rem` between stacked sections.
- **Page margins** — `clamp(1rem, 5vw, 4rem)` for fluid edge padding.
- **Dense UI (tables, lists)** — `0.25rem`–`0.5rem` between rows.
- **Hero/landing sections** — `4rem`–`6rem` vertical spacing.
- **Inline elements** — `0.25rem`–`0.5rem` between icon and text.

### Gestalt Proximity: How Spacing Communicates Relationships

Proximity indicates grouping (Gestalt principle). Closer elements read as related; farther elements as separate.

- **Related** — Tight spacing (e.g., label + input).
- **Separate sections** — Larger gaps (e.g., between form groups).
- **Lists** — Consistent gaps within items, larger gaps between groups.

Use spacing to reinforce structure instead of adding extra borders or boxes.

### Max-Width for Content Containers

Typical values:
- **Content/readable** — 65–75 characters (~600–720px).
- **Wide content** — 1200px for dashboards, grids.
- **Full-width layouts** — 1440px–1920px for hero or marketing.

```css
.container {
  max-width: 1200px;
  margin-inline: auto;
  padding-inline: clamp(1rem, 5vw, 4rem);
}
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Random values (13px, 27px) | Use only values from your spacing scale. |
| Inconsistent padding across breakpoints | Use `clamp()` or scale tokens for responsive padding. |
| No max-width on large screens | Add max-width and center with `margin-inline: auto`. |
| Ignoring proximity for grouping | Reduce gaps within groups, increase gaps between groups. |
| Overcrowding on mobile | Increase padding on small screens; don’t shrink everything. |
| Mixing rem, px, em inconsistently | Pick one unit for spacing (rem recommended) and stick to it. |
| No spacing scale in design system | Define tokens (e.g., --space-1…--space-12) and use them everywhere. |
