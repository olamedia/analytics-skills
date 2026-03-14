# Icons

Visual consistency across the product signals design quality.

## Checklist

| Practice | Standard |
|----------|----------|
| SVG vector icons (never emojis for structural UI) | WCAG, Material Design |
| One icon set across product | Apple HIG, Material Design |
| Consistent stroke width within same layer | Visual consistency |
| Icon contrast meets WCAG (4.5:1 small, 3:1 large) | WCAG 1.4.3 |
| Official brand assets with correct proportions | Brand guidelines |

---

## Why SVG Over Icon Fonts or Emojis

**SVG** scales without blur, supports multi-color and CSS control, and has good accessibility via `aria-hidden` and adjacent text. Icon fonts require fallbacks, can cause FOUT, and make per-icon styling harder. Emojis vary by OS, break consistency, and aren’t intended as structural UI; WCAG cautions against relying on pictograms alone for meaning.

**Guidance:** Use SVG for all structural icons. Reserve emojis for informal content (comments, reactions), not navigation or controls.

---

## Icon Sizing Conventions

**16px** – Inline with body text, dense UIs, table/row actions.  
**20px** – Buttons, list items, secondary nav.  
**24px** – Primary actions, main nav, prominent controls.

Material Design and Apple HIG define touch targets (min 44–48px). Keep icons centered within targets; don’t shrink tap area to icon size. Use a 24px base and scale down for compact layouts.

---

## Stroke Width Consistency

Mixed stroke widths look uneven and suggest different icon sources or quality levels. Use one stroke width per “layer” (e.g., 1.5px or 2px for outline icons). If mixing outline and filled icons, keep stroke width consistent within each style. Inconsistent weights signal lack of design system rigor.

---

## Common Mistakes

| Mistake | Why it’s bad |
|---------|--------------|
| Mixing icon sets (e.g., Feather + Heroicons in same product) | Inconsistent weight, style, and meaning |
| Using emojis as nav or controls | Platform-dependent; not structural UI |
| Inconsistent sizes (16px next to 24px at same hierarchy) | Looks random; weakens hierarchy |
| Icons below contrast (e.g., gray on gray) | Fails WCAG; hard to see |
| Icon fonts instead of SVG | Scaling issues; accessibility limitations |
| Varying stroke width in same set | Looks unpolished and inconsistent |
