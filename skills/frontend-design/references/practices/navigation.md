# Navigation

Meaningful nav labels = cognitive fluency (NNGroup). Predictable structure = prototypicality (Tuch et al. 2012).

## Checklist

| Practice | Standard |
|----------|----------|
| Bottom nav max 5 items with labels + icons | ✓ |
| Current location visually highlighted | ✓ |
| Back navigation predictable and consistent | ✓ |
| Key screens deep-linkable (URL-addressable) | ✓ |
| Modals: clear close affordance (X or click-outside) | ✓ |
| Nav placement consistent across all pages | ✓ |
| Breadcrumbs for 3+ level hierarchies | ✓ |
| One primary CTA per screen, secondary actions subordinate | ✓ |
| State preserved on back navigation (scroll, filters, input) | ✓ |

---

## Detailed Guidance

### Why Consistent Nav Placement Matters for Prototypicality

Prototypicality research (Tuch et al. 2012) shows that interfaces that match users’ mental models of “typical” layouts are perceived as more usable and trustworthy. Navigation in a consistent position—e.g., top bar or bottom bar—reduces cognitive load: users know where to look without relearning on each screen. Apple HIG and Material Design both prescribe fixed nav placement (top for primary, bottom for mobile tab bars) so users build muscle memory. Moving nav between pages breaks that expectation and increases errors and frustration.

### One Primary CTA Rule: What Counts as Primary vs Secondary

The primary CTA is the single action that advances the user toward the main goal of the screen. NNGroup’s research on decision-making shows that multiple competing CTAs create choice paralysis and dilute attention. **Primary** = the action you want users to take first (e.g., “Submit,” “Save,” “Continue,” “Add to Cart”). **Secondary** = alternative or less critical actions (e.g., “Cancel,” “Skip,” “Save draft”). Make the primary visually dominant (solid button, higher contrast); secondary actions should use outline or text-only treatment and appear subordinate. One clear primary reduces friction and increases conversion.

### State Preservation: What State to Preserve on Back Nav and How

When users navigate back, they expect to return to the same context. State that should be preserved: **scroll position**, **filters and sort**, **form input** (unless the action was destructive), **expanded/collapsed sections**, **selected tabs**. Implement by: (1) using URL query params for filters/sort so state is encoded in the URL, (2) storing scroll position in sessionStorage or the router’s scroll restoration API, (3) keeping form values in controlled components until explicit submit or cancel. Breaking this expectation—e.g., resetting the list or losing a long form—causes distrust and rework. NNGroup and processing fluency research support that predictable behavior increases perceived usability.

### Deep Linking: Why Every Key Screen Should Be URL-Addressable

URL-addressable screens support sharing, bookmarking, analytics, and accessibility. Users can share a link to a specific item, product, or settings page; crawlers can index content; assistive tech can reference locations. Client-only routing with hash-based or non-URL states makes deep links impossible. Use path-based routes (e.g., `/products/123`, `/settings/notifications`) for key screens so each meaningful view has a unique URL. This aligns with WCAG’s requirement for operable interfaces and supports predictable navigation (2.1.1).

---

## Common Mistakes

- **More than 5 bottom nav items** — Exceeds Miller’s “7 ± 2” chunking limits and clutters mobile; Apple HIG recommends 3–5 tabs.
- **Icon-only nav without labels** — Icons alone are ambiguous; NNGroup recommends labels for clarity and accessibility.
- **No visual indication of current location** — Users lose orientation; active state (e.g., filled icon, underline, bold label) is essential.
- **Inconsistent back behavior** — Sometimes back goes to previous page, sometimes to home; back must always match user expectation (e.g., browser back = previous URL).
- **Modals without clear close** — Missing X or click-outside-to-close frustrates users; every modal needs an obvious way to dismiss.
- **Multiple competing CTAs** — Several equal-weight buttons (e.g., “Save” and “Delete” both primary) create confusion; designate one primary.
- **Resetting state on back** — Losing scroll, filters, or form input on back navigation breaks trust; preserve state where possible.
- **Screens not deep-linkable** — Key views buried behind client-only state cannot be shared or revisited via URL; expose them in the route structure.
