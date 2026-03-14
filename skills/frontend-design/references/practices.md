# Frontend Design Practices

Categorized checklists cross-referenced against peer-reviewed research. Use for audit scans and active guidance.

**Research basis:** Processing fluency (Reber & Schwarz 1999), 50ms judgment (Tuch et al. 2012), halo effect (Tractinsky 2000), Stanford Web Credibility (Fogg 2002), NNGroup trust factors (Nielsen 1999), Baymard Institute (150k+ hours testing).

---

## 1. Accessibility

Accessibility signals design quality → triggers halo effect (Tractinsky 2000).

| Practice | Standard |
|---|---|
| Color contrast — body text | >= 4.5:1 (WCAG AA) |
| Color contrast — large text (18px+) | >= 3:1 |
| Visible focus rings on interactive elements | 2-4px outline, `focus-visible` |
| `prefers-reduced-motion` respected | Disable/reduce non-essential animations |
| `aria-label` on icon-only buttons | Always |
| Sequential heading hierarchy | h1 → h2 → h3, no level skip |
| Keyboard nav matches visual order | Tab order = reading order |
| Skip-to-content link | For keyboard users |
| Color never sole indicator | Add icon/text alongside |
| Decorative elements `aria-hidden="true"` | Canvas, WebGL, background effects |

---

## 2. Typography

Processing fluency depends on text being effortless to parse (Reber & Schwarz 1999, Song & Schwarz 2008).

| Practice | Standard |
|---|---|
| Max font families | 2 (display + body) + monospace for data |
| Body line length (desktop) | 55-75 characters |
| Body line length (mobile) | 35-45 characters |
| Min body font size (mobile) | 16px (avoids iOS auto-zoom) |
| Body line-height | 1.5-1.75 |
| Headline letter-spacing | >= -0.04em |
| Consistent type scale | Defined steps (e.g. 12/14/16/18/24/32) |
| Tabular figures for data | Monospace/tabular-nums for numbers |
| Text hierarchy | 3-4 distinct levels via size + weight |
| Fluid type scale | `clamp()` for responsive sizing |

---

## 3. Spacing

Consistent rhythm reduces visual complexity (Tuch et al. 2012).

| Practice | Standard |
|---|---|
| Incremental spacing system | 4px/8px base increments |
| Container padding (responsive) | `clamp(1rem, 5vw, 4rem)` |
| Consistent max-width on desktop | Defined container width |
| Whitespace groups related items | Proximity = relationship (Gestalt) |

---

## 4. Design Tokens

Consistency is a property of processing fluency.

| Practice | Standard |
|---|---|
| Never use raw hex in components | Semantic tokens only |
| Three-layer token architecture | Primitive → Semantic → Component |
| CSS variables for all colors | `var(--color-*)` |
| Semantic tokens enable theming | Light/dark via semantic layer |
| Token-driven theming across all surfaces | No one-off overrides |

---

## 5. Color System

Every color has a defined role; the palette is stable, balanced, predictable (Tuch et al. 2012).

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

> "ONE dominant accent, everything else neutral" is one valid approach, not the only one. Research requires consistency and predictability, not minimalism.

---

## 6. Motion

Fluid animations = smoothness (processing fluency). Janky animation is worse than no animation.

| Practice | Standard |
|---|---|
| Micro-interaction duration | 150-300ms |
| Complex transition max | 400ms |
| Max continuous animations per viewport | 1 |
| CSS-first, escalate to JS when needed | Default approach |
| `prefers-reduced-motion` | Must disable non-essential |
| Easing | ease-out entering, ease-in exiting |
| Exit animations shorter than enter | ~60-70% duration |
| Stagger list items | 30-50ms per item |
| Every animation expresses cause-effect | Not decoration |
| Animations interruptible | User action cancels in-progress |
| Never block UI during animation | Must stay interactive |

---

## 7. Performance

53% abandon sites > 3s load (NNGroup). Speed is a trust signal (Stanford #7).

| Practice | Standard |
|---|---|
| Lazy load below-fold images/media | `loading="lazy"` |
| Code split by route/feature | Dynamic imports |
| `will-change` for animated elements | GPU acceleration |
| Only animate `transform` and `opacity` | Never width/height/top/left |
| Reserve space for async content | Prevent CLS |
| Image optimization | WebP/AVIF, responsive `srcset` |
| `font-display: swap` | Avoid invisible text |
| Preload critical fonts only | Don't preload all |
| Debounce/throttle high-frequency events | scroll, resize, input |
| Virtualize long lists | 50+ items |
| Per-frame work | < 16ms for 60fps |

**Performance budgets:**

| Device | JS Budget | Animation Target |
|---|---|---|
| Desktop | < 500KB | 60fps |
| Tablet | < 300KB | 30fps or CSS-only |
| Mobile | < 150KB | CSS-only |

---

## 8. Loading, Error, and Empty States

Handling every state signals care → halo effect (Tractinsky 2000).

| Practice | Standard |
|---|---|
| Skeleton loaders for > 300ms waits | Not spinners |
| Error messages: cause + fix | Not "Invalid input" |
| Empty states: message + action | Guide the user |
| Error boundaries for WebGL/3D | Graceful fallback |
| Auto-dismiss toasts | 3-5 seconds |
| Confirm before destructive actions | Dialog or undo |
| Auto-focus first invalid field on error | After submit |
| Long forms auto-save drafts | Prevent data loss |
| Timeout: clear feedback + retry | Not silent failure |

---

## 9. Responsive / Mobile-First

Prototypicality is category-specific — users expect mobile sites to work on mobile (NNGroup).

| Practice | Standard |
|---|---|
| Design mobile-first, scale up | CSS approach |
| Systematic breakpoints | 375/768/1024/1440 or project-specific |
| No horizontal scroll on mobile | Ever |
| Viewport meta | `width=device-width, initial-scale=1` |
| Never disable zoom | Accessibility |
| Touch targets | >= 44x44px (Apple) / 48x48dp (Material) |
| Min gap between touch targets | 8px |
| `touch-action: manipulation` | Reduce 300ms tap delay |
| Safe area compliance | Notch, gesture bar |
| Replace hover with tap on mobile | No hover-only interactions |
| Heavy effect fallback on low-power | CSS gradient instead of WebGL |

---

## 10. Navigation

Meaningful nav labels = cognitive fluency (NNGroup). Predictable structure = prototypicality.

| Practice | Standard |
|---|---|
| Bottom nav max 5 items | Labels + icons |
| Current location highlighted | Visual indicator |
| Back navigation predictable | Consistent behavior |
| Key screens deep-linkable | URL-addressable |
| Modals: clear close affordance | X button or click-outside |
| Nav placement consistent | Same position all pages |
| Breadcrumbs for 3+ level hierarchies | On web |
| One primary CTA per screen | Secondary actions subordinate |
| State preserved on back nav | Scroll, filters, input |

---

## 11. Forms

Upfront disclosure is an NNGroup trust factor. Form friction = processing disfluency = distrust.

| Practice | Standard |
|---|---|
| Visible label per input | Not placeholder-only |
| Error message below related field | Inline, not toast |
| Required fields marked | Asterisk or text |
| Validate on blur | Not on keystroke |
| Semantic input types | email, tel, number for mobile keyboard |
| Password show/hide toggle | Always |
| Autofill/autocomplete attributes | Standard values |
| Progressive disclosure | Don't overwhelm upfront |
| Multi-step: progress indicator | Show position in flow |
| Success feedback after actions | Visible confirmation |
| Confirm before dismissing unsaved | Dialog or warning |

---

## 12. Icons

| Practice | Standard |
|---|---|
| SVG vector icons | Never emojis for structural UI |
| One icon set across product | Consistent style |
| Consistent stroke width | Same layer, same weight |
| Icon contrast meets WCAG | 4.5:1 small, 3:1 large |
| Official brand assets | Correct proportions |

---

## 13. SEO for Visual Content

| Practice | Standard |
|---|---|
| `sr-only` text for Canvas/WebGL | Screen reader accessible |
| `aria-labelledby` on visual heroes | Section labeling |
| Server-rendered semantic HTML | Alongside client effects |

---

## 14. Trust Signals

The strongest predictors of website trust according to peer-reviewed research.

### Stanford Web Credibility (Fogg 2002, 4,500+ participants)

| Practice |
|---|
| Verifiable accuracy — citations, links to sources |
| Real organization — physical address, office photos |
| Visible expertise — team credentials, affiliations |
| Real people — employee bios with photos |
| Easy contact — phone, address, email prominent |
| Zero errors — typos and broken links destroy credibility |
| Regular updates — visible "last reviewed" dates |
| Restraint with promotion — no excessive pop-ups/ads |

### NNGroup Trust Factors (Nielsen 1999, cross-cultural)

| Practice |
|---|
| Upfront disclosure — prices, fees, policies visible immediately |
| No login walls — provide value before asking for info |
| Real process content — show the work, not just results |
| Content for all customer types — don't gatekeep |
| External reviews — link to third-party review sites |
| Connection to the web — social, press, inbound links |

### Baymard Institute (150,000+ hours user testing)

| Practice |
|---|
| No hidden costs — 48% abandon over unexpected fees |
| No forced account creation — 24% abandon |
| Clear delivery times — 22% abandon over unclear timing |
| Max 5 trust badges — 6+ creates suspicion |
| Security badges near payment form only |
| Specific certifications — generic "Secure Checkout" has no credibility |

### Prototypicality (Tuch et al. 2012, 1,530 participants)

The single strongest trust predictor: **look like what users expect a site in your category to look like.** Study competitors. Match established patterns. Add brand personality within conventions — don't break them.
