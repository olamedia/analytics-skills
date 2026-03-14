# Color System

Every color has a defined role; the palette is stable, balanced, predictable (Tuch et al. 2012). Low visual complexity = higher trust. That doesn't mean fewer colors — it means every color has a clear, consistent purpose and the user can unconsciously predict which color means what.

## Checklist

| Practice | Standard |
|---|---|
| Every color has a semantic role | Primary, accent, semantic, neutral, surface |
| Same role = same color everywhere | Predictable usage across all pages |
| Primary color reserved for primary actions | Form submit, main CTA only |
| Active states are NOT a color role | Use border, inset shadow, weight — not primary background |
| Accent not for large background areas | Sparingly, for drawing attention |
| Grayscale preview check | If muddy, reduce the palette |
| Light/dark variants designed together | Test separately |
| Color matches industry expectations | Green = clean, blue = finance, etc. (NNGroup) |

---

## Semantic Color Roles

A design palette has multiple colors. What matters is that each color has a clear, consistent purpose.

| Role | Purpose | Used for | NOT used for |
|---|---|---|---|
| **Primary** | The main brand action color | Form submit, primary buttons, key confirmations | Backgrounds, active states on non-primary elements, decorative fills |
| **CTA / Accent** | Draw attention to key elements | Hero CTA, banners, promotional highlights, badges | Large background areas, body text, borders |
| **Price / Commerce** | Commerce-specific signals | Price tags, discounts, sale indicators, stock status | General UI elements |
| **Semantic** | System state communication | Success (green), error (red), warning (amber), info (blue) | Branding, decoration |
| **Neutral** | Structure and readability | Backgrounds, borders, muted text, dividers, disabled states | Calls to action, status indicators |
| **Surface** | Elevation and layering | Cards, modals, dropdowns, elevated layers, overlays | Text, icons |

### Primary vs. Accent — when you need both

Primary and accent can be the same color if the site has one dominant action color. They become separate when:
- The primary action (form submit) needs to feel calm and reliable (blue)
- The CTA/promotional elements need to feel urgent and attention-grabbing (orange)
- Using the same color for both would make "Buy Now" banners look identical to "Save" buttons

### What counts as a "primary action"

- Form submission (sign up, log in, checkout, save)
- Main CTA on a page (one per screen)
- Key confirmations (confirm deletion, approve, send)

What does NOT count:
- Pagination active page
- Selected tab
- Current sidebar item
- Toggle in "on" state
- Active filter chip

These are **selection states**, not actions. They should look selected, not actionable.

---

## Active States

Active/selected states are not a color role. In non-flat design, "active" can mean pressed-in (shadow inset, slight darkening, depth change) — not a primary-colored background.

**Problem:** Using primary color as background for non-primary elements (e.g., active pagination page, selected tab, current sidebar item) creates false visual weight and distracts from actual primary actions. The eye is drawn to every selected tab instead of the submit button.

**How to express active/selected states:**

| Technique | When to use | CSS example |
|---|---|---|
| Border / underline | Tabs, nav items | `border-bottom: 2px solid var(--color-primary)` |
| Font weight change | Tabs, list items | `font-weight: 600` (from 400) |
| Inset shadow | Buttons, toggles | `box-shadow: inset 0 2px 4px rgba(0,0,0,0.15)` |
| Slight background tint | List items, table rows | `background: var(--color-primary-50)` (very light tint, not full primary) |
| Indicator dot/bar | Nav items, stepper | Small accent element next to the item |

**Reserve full primary color background** for the things the user should actually do next.

---

## Palette Construction

Methods for building a balanced color palette:

| Method | How it works | Best for |
|---|---|---|
| **Complementary** | Two colors opposite on the wheel | High contrast, strong CTA differentiation from primary |
| **Analogous** | 2-3 adjacent colors on the wheel | Harmonious, low-friction, editorial feel |
| **Split-complementary** | Base + two colors adjacent to its complement | Balanced variety without the tension of pure complementary |
| **Triadic** | Three equally spaced on the wheel | Vibrant but needs careful restraint — easy to over-saturate |
| **Monochromatic + accent** | One hue family + one contrasting accent | Safest for trust-focused sites, hardest to mess up |

> "ONE dominant accent, everything else neutral" is one valid approach (monochromatic + accent), not the only one. Real design systems (Bootstrap, Material, Ant Design) use multiple purposeful colors. The research requirement is consistency and predictability, not minimalism.

---

## Industry Color Expectations

NNGroup found that color should match service type. Users have unconscious associations:

| Industry | Expected colors | Risky choices |
|---|---|---|
| Finance / Banking | Blue, dark blue, navy, green | Neon, bright orange, red |
| Healthcare / Medical | Blue, green, white | Dark themes, neon, high-saturation |
| Cleaning / Hygiene | Green, light blue, white | Dark, brown, orange |
| Legal / Law | Dark blue, burgundy, charcoal | Bright colors, playful palettes |
| Tech / SaaS | Blue, purple, teal (wider range accepted) | Brown, beige (feels dated) |
| Food / Restaurant | Warm colors (red, orange, yellow), earth tones | Cold blue, clinical white |
| Creative / Agency | Wider range — novelty expected here | Generic corporate blue |
| E-commerce | Depends on product category — match the category norms | — |

These are not rules — they're expectations. Breaking them increases cognitive friction (processing disfluency) which reduces trust, unless the audience specifically expects novelty (creative/agency sites).

---

## Testing

### Grayscale preview
Convert the palette to grayscale. If elements that should be visually distinct look the same, the palette relies too much on hue and not enough on lightness contrast. Dial it back.

### Contrast check
Every text-on-background combination must pass WCAG AA:
- Body text: >= 4.5:1
- Large text (18px+ or 14px bold): >= 3:1
- Interactive element boundaries: >= 3:1 against adjacent colors

### Predictability test
Open 5 different pages/states of the site. For each color, ask: "does this color mean the same thing everywhere?" If the same blue is a link on one page and a background on another, the system is inconsistent.

---

## Common Mistakes

- Using primary color for active pagination, selected tabs, or sidebar active items — creates false visual weight
- Having the same color serve both "primary action" and "information" roles — confuses action with status
- Choosing colors based on personal preference rather than industry expectations — looks "off" to users without them knowing why
- Adding too many accent colors without defined roles — increases visual complexity, reduces trust
- Not testing light and dark modes separately — colors that work on white may fail on dark backgrounds
