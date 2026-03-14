# Typography

Processing fluency depends on text being effortless to parse (Reber & Schwarz 1999). Song & Schwarz (2008) found that hard-to-read fonts make people rate the task itself as harder — reading difficulty gets misattributed to the thing being described. Typography errors destroy trust (Stanford); font choice has no proven trust effect.

## Checklist

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

## Line Length Enforcement

The most impactful typography rule. Lines that are too long exhaust the eye tracking back to the start; lines too short break reading rhythm.

```css
.prose {
  max-width: 65ch; /* ~55-75 characters depending on font */
}

.prose-mobile {
  max-width: 45ch;
}
```

`ch` is the width of the `0` character in the current font. `65ch` is a reliable proxy for the 55-75 character range across most body fonts.

For containers where `ch` units don't work (e.g., layout grids), approximate:
- 16px font at 65ch ≈ 600-700px container
- 18px font at 65ch ≈ 680-780px container

---

## Font Size and iOS Auto-Zoom

Safari on iOS auto-zooms any `<input>` with a font-size below 16px. This causes a jarring viewport shift that breaks the user's flow.

**Rule:** Never set body or input font-size below 16px on mobile.

```css
/* Safe: 16px is the floor */
:root {
  --text-body: clamp(1rem, 2vw, 1.125rem); /* 16px minimum */
}

input, select, textarea {
  font-size: 1rem; /* inherits 16px minimum */
}
```

**Do not use** `maximum-scale=1` or `user-scalable=no` to prevent zoom — this is an accessibility violation.

---

## Fluid Type Scale

Use `clamp()` to scale typography smoothly between viewport sizes instead of jumping at breakpoints.

```css
--text-hero: clamp(2.5rem, 8vw, 7rem);   /* 40px → 112px */
--text-h1:   clamp(2rem, 5vw, 4rem);      /* 32px → 64px */
--text-h2:   clamp(1.5rem, 3vw, 2.5rem);  /* 24px → 40px */
--text-h3:   clamp(1.25rem, 2.5vw, 1.75rem); /* 20px → 28px */
--text-body: clamp(1rem, 2vw, 1.125rem);  /* 16px → 18px */
--text-sm:   clamp(0.875rem, 1.5vw, 0.9375rem); /* 14px → 15px */
```

**How `clamp()` works:** `clamp(minimum, preferred, maximum)`
- Below the minimum viewport: uses the minimum
- Above the maximum viewport: uses the maximum
- In between: scales linearly with the viewport

**When to use each scale step:**

| Token | Use for |
|---|---|
| `--text-hero` | Landing page hero headlines. One per page maximum. |
| `--text-h1` | Page titles, section heroes |
| `--text-h2` | Section headings |
| `--text-h3` | Sub-section headings, card titles |
| `--text-body` | All body text, form labels, descriptions |
| `--text-sm` | Captions, helper text, metadata, timestamps |

---

## Line Height

| Text type | Line height | Why |
|---|---|---|
| Body text | 1.5-1.75 | Readability — loose enough for comfortable scanning |
| Headlines | 1.1-1.3 | Tight — large text needs less line spacing to feel cohesive |
| UI labels / buttons | 1.0-1.2 | Compact — single-line elements, vertical centering matters |
| Code blocks | 1.4-1.6 | Moderate — needs scanability without wasting space |

---

## Letter Spacing

- Headlines: can go as tight as -0.04em. Tighter than this kills readability at speed.
- Body text: leave at default (0). Adjusting body letter-spacing almost always reduces readability.
- ALL CAPS text: add +0.05em to +0.1em. Uppercase letters need extra tracking to remain legible.
- Small text (captions): consider +0.02em for legibility at small sizes.

---

## Font Selection Criteria

No study shows font *choice* affects trust — only font *readability* (Song & Schwarz 2008). Choose fonts based on:

1. **Readability at body size** — test at 16px, on mobile, in paragraphs (not just the preview card)
2. **Character set coverage** — supports the languages your audience uses
3. **Loading performance** — every custom font adds load time. System fonts load instantly (zero FOIT).
4. **Weight range** — needs at least regular (400) and bold (700) for hierarchy. Semi-bold (600) is useful for UI.
5. **Tabular/monospace figures** — if the site shows numbers (prices, stats, tables), the font needs `font-variant-numeric: tabular-nums` or a monospace alternative for data. Without this, numbers cause layout shift as they change.
6. **Match audience expectations** — a playful rounded font on a law firm site creates processing disfluency.

### Font loading

```css
@font-face {
  font-family: 'BodyFont';
  src: url('/fonts/body.woff2') format('woff2');
  font-display: swap; /* Show fallback immediately, swap when loaded */
  font-weight: 400;
}
```

- `font-display: swap` prevents invisible text during load
- Preload only the critical weight (usually regular 400): `<link rel="preload" href="/fonts/body.woff2" as="font" type="font/woff2" crossorigin>`
- Do not preload all weights — it blocks rendering

---

## Text Hierarchy

Build hierarchy with 3-4 distinct levels using size AND weight, not color alone:

| Level | Purpose | Typical treatment |
|---|---|---|
| 1 - Primary | Page title, hero headline | Largest size, heaviest weight |
| 2 - Secondary | Section headings | Medium-large size, bold or semi-bold |
| 3 - Tertiary | Sub-sections, card titles | Slightly larger than body, semi-bold |
| 4 - Body | Content, descriptions, labels | Base size, regular weight |

**Squint test:** Blur your eyes or scale the page to 25%. You should still see the hierarchy. If everything looks the same, the levels aren't distinct enough.

---

## Common Mistakes

- Setting mobile body text below 16px — triggers iOS auto-zoom on inputs, breaks user flow
- Using `px` units for font sizes — doesn't respect user browser settings. Use `rem`.
- No `max-width` on text containers — lines stretch to full viewport on desktop, exhausting to read
- Using 3+ custom fonts — each adds ~50-100KB and a FOIT risk. Two is the practical maximum.
- Headline letter-spacing tighter than -0.04em — looks stylish in mockups, fails readability at scanning speed
- Missing tabular figures for data — numbers shift as they change (price counters, stats), causing layout jank
- No visible hierarchy difference between h2 and h3 — user can't scan the page structure
