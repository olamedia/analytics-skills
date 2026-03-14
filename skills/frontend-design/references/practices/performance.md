# Performance

53% of users abandon sites that take longer than 3 seconds to load (NNGroup). Speed is a trust signal (Stanford guideline #7: make the site easy to use and genuinely useful). Every 100ms of latency reduces perceived quality.

## Checklist

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

## Cumulative Layout Shift (CLS) Prevention

CLS is when visible content moves unexpectedly during load. It destroys perceived quality — elements jumping around signal broken software.

**Common CLS causes and fixes:**

| Cause | Fix |
|---|---|
| Images without dimensions | Always set `width` and `height` attributes, or use `aspect-ratio` in CSS |
| Web fonts loading late | `font-display: swap` + match fallback font metrics with `size-adjust` |
| Dynamically injected content | Reserve space with `min-height` or skeleton placeholders |
| Ads / embeds loading async | Reserve fixed space with containers of defined dimensions |
| Late-loading CSS | Inline critical CSS in `<head>`, load rest async |

```css
/* Reserve space for images before they load */
img {
  aspect-ratio: attr(width) / attr(height);
  width: 100%;
  height: auto;
}

/* Reserve space for async content */
.async-content-slot {
  min-height: 200px; /* estimate based on content */
}
```

---

## `will-change` — Use and Misuse

`will-change` tells the browser to promote an element to its own GPU layer before the animation starts.

**Correct usage:**
```css
.about-to-animate {
  will-change: transform;
}
.done-animating {
  will-change: auto; /* remove after animation */
}
```

**Common mistakes:**
- `will-change: transform` on every element — creates excessive layers, uses more GPU memory, can actually slow things down
- Leaving `will-change` permanently on elements that aren't animating — wastes resources
- Using `will-change` as a "make it faster" magic fix — it only helps if the element is about to be animated

**Rule:** Apply `will-change` just before an animation starts (e.g., on hover of a parent, or via JS before triggering). Remove it after the animation ends.

---

## Animation-Safe Properties

Only two CSS properties can be animated without triggering layout recalculation:

| Property | What it does | GPU accelerated |
|---|---|---|
| `transform` | Move, scale, rotate, skew | Yes |
| `opacity` | Fade in/out | Yes |

Everything else triggers layout or paint:

| Property | What it triggers | Performance impact |
|---|---|---|
| `width`, `height` | Layout recalculation | High — reflows entire document |
| `top`, `left`, `right`, `bottom` | Layout recalculation | High |
| `margin`, `padding` | Layout recalculation | High |
| `border-width` | Layout recalculation | Medium |
| `background-color` | Paint | Medium |
| `box-shadow` | Paint | Medium |
| `color` | Paint | Low-medium |

**How to replace layout-triggering animations:**

| Instead of | Use |
|---|---|
| `width: 0 → width: 100%` | `transform: scaleX(0) → scaleX(1)` |
| `height: 0 → height: auto` | `transform: scaleY(0) → scaleY(1)` or max-height hack |
| `top: -100px → top: 0` | `transform: translateY(-100px) → translateY(0)` |
| `left: -50% → left: 0` | `transform: translateX(-50%) → translateX(0)` |

---

## Image Optimization

| Format | Use for | Browser support |
|---|---|---|
| **WebP** | Photos, complex images | All modern browsers |
| **AVIF** | Photos (better compression than WebP) | Chrome 85+, Firefox 93+, Safari 16.4+ |
| **SVG** | Icons, logos, simple illustrations | Universal |
| **PNG** | Screenshots, images requiring transparency with sharp edges | Universal |

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" width="800" height="600" loading="lazy">
</picture>
```

**Responsive images** for different viewport sizes:

```html
<img
  srcset="image-400.webp 400w, image-800.webp 800w, image-1200.webp 1200w"
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  src="image-800.webp"
  alt="Description"
  width="800"
  height="600"
  loading="lazy"
>
```

---

## Font Loading Strategy

```html
<!-- Preload ONLY the critical font weight -->
<link rel="preload" href="/fonts/body-regular.woff2" as="font" type="font/woff2" crossorigin>
```

```css
@font-face {
  font-family: 'Body';
  src: url('/fonts/body-regular.woff2') format('woff2');
  font-display: swap;
  font-weight: 400;
}
```

- `font-display: swap` — shows fallback text immediately, swaps when font loads. Prevents invisible text.
- Only preload the body font (regular weight). Other weights load on demand.
- Use `woff2` — best compression, universal modern browser support.
- System fonts (`system-ui`, `-apple-system`) load instantly and require zero network requests.

---

## List Virtualization

For lists with 50+ items, render only the visible items plus a small buffer. The DOM stays small regardless of data size.

When to virtualize:
- Product listings with 50+ items
- Data tables with 100+ rows
- Chat message histories
- Infinite scroll feeds

This is a framework-specific implementation detail — use the virtualization library that matches your stack.

---

## Common Mistakes

- No `width`/`height` on images — causes CLS as images load and push content down
- Preloading all font weights — blocks rendering for fonts that aren't used above the fold
- `will-change: transform` on every element — wastes GPU memory, can make performance worse
- Animating `width`/`height` instead of `transform` — triggers layout recalculation every frame
- No `loading="lazy"` on below-fold images — downloads everything upfront, slows initial load
- Rendering 500+ list items into the DOM — browser struggles to paint; scroll becomes janky
- Using JPEG for everything — WebP/AVIF deliver same quality at 25-50% smaller file size
- Not debouncing scroll/resize handlers — fires 60+ times per second, causes jank if handler is expensive
