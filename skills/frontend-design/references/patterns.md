# Code Patterns

Reusable, tech-stack-agnostic patterns for common frontend needs. Read this when you need a specific implementation — not loaded by default.

---

## 1. Reduced Motion (CSS)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 2. Reduced Motion (JS Hook)

```js
function usePrefersReducedMotion() {
  const mq = matchMedia('(prefers-reduced-motion: reduce)');
  let prefersReduced = mq.matches;
  mq.addEventListener('change', e => { prefersReduced = e.matches; });
  return () => prefersReduced;
}
```

---

## 3. Staggered Entrance (CSS only)

```css
@keyframes fade-up {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.stagger-container > * {
  opacity: 0;
  animation: fade-up 0.6s ease-out forwards;
}
.stagger-container > *:nth-child(1) { animation-delay: 0.1s; }
.stagger-container > *:nth-child(2) { animation-delay: 0.2s; }
.stagger-container > *:nth-child(3) { animation-delay: 0.3s; }
.stagger-container > *:nth-child(4) { animation-delay: 0.4s; }
.stagger-container > *:nth-child(5) { animation-delay: 0.5s; }
```

---

## 4. Skeleton Loader (Shimmer)

```css
@keyframes shimmer {
  100% { transform: translateX(100%); }
}

.skeleton {
  position: relative;
  overflow: hidden;
  background: var(--color-surface, #e5e7eb);
  border-radius: 8px;
}

.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  transform: translateX(-100%);
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
  animation: shimmer 2s infinite;
}
```

---

## 5. Glassmorphism (with fallback)

```css
.glass {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
}

@supports not (backdrop-filter: blur(1px)) {
  .glass {
    background: rgba(30, 30, 30, 0.95);
  }
}
```

> `backdrop-filter` not supported in Firefox < 103. Always provide a fallback.

---

## 6. Animation Library Decision Guide

| Need | Use | Why |
|---|---|---|
| Hover/focus states | CSS | Zero JS, instant |
| Simple entrance | CSS keyframes | Lightweight |
| Layout animations | Framer Motion | `layout` prop |
| Gesture-based | Framer Motion | Built-in drag/pan |
| Scroll-triggered | GSAP ScrollTrigger | Most powerful |
| Text splitting | GSAP SplitText | Industry standard |
| Complex timelines | GSAP | Precise control |
| SVG morphing | GSAP MorphSVG | No alternative |

Default to simpler. Escalate complexity only when needed.

---

## 7. Design Token Three-Layer Pattern

```css
/* Layer 1: Primitive (raw values) */
--color-blue-600: #2563EB;

/* Layer 2: Semantic (purpose aliases) */
--color-primary: var(--color-blue-600);

/* Layer 3: Component (component-specific) */
--button-bg: var(--color-primary);
```

---

## 8. Low-Power Device Detection

```js
function isLowPowerDevice() {
  return (
    window.innerWidth < 768 ||
    navigator.hardwareConcurrency <= 4 ||
    /Android|iPhone|iPad/.test(navigator.userAgent)
  );
}
```

Use to disable heavy animations, WebGL, or particle effects on constrained devices.

---

## 9. WebGL Accessibility

```html
<!-- Decorative: hidden from assistive tech -->
<div aria-hidden="true" class="pointer-events-none">
  <canvas><!-- background effect --></canvas>
</div>

<!-- Meaningful: labeled for screen readers -->
<div role="img" aria-label="3D visualization of network connections">
  <canvas><!-- data visualization --></canvas>
</div>
```

---

## 10. SEO for Visual-Only Heroes

```html
<section aria-labelledby="hero-title">
  <h1 id="hero-title" class="sr-only">Page Title for Crawlers</h1>
  <p class="sr-only">SEO description of the page content</p>
  <div aria-hidden="true">
    <!-- visual hero (canvas, video, animation) -->
  </div>
  <div class="relative z-10">
    <span class="text-display">Visible Title</span>
  </div>
</section>
```

---

## 11. Focus Indicators

```css
.interactive:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

---

## 12. Scroll-Driven Animations (CSS only)

```css
@supports (animation-timeline: scroll()) {
  .parallax-bg {
    animation: parallax linear;
    animation-timeline: scroll();
  }
  @keyframes parallax {
    from { transform: translateY(0); }
    to { transform: translateY(-30%); }
  }
}
```

> Chrome 115+ only. Use as progressive enhancement — must work without it.

---

## 13. Content Hierarchy Layout Rule

```
Hero (one flourish) → Content blocks (calm) → Proof (calm) → CTA (highlighted)
```

- Max 1 hero flourish (animation, shader, or bold visual)
- Max 1 supporting flourish (micro-interaction, animated stat)
- Everything else supports readability
- If page > 1 scroll-length, every second section should be mostly static

---

## 14. Fluid Type Scale

```css
--text-hero: clamp(2.5rem, 8vw, 7rem);
--text-h1:   clamp(2rem, 5vw, 4rem);
--text-h2:   clamp(1.5rem, 3vw, 2.5rem);
--text-body: clamp(1rem, 2vw, 1.125rem);
```

---

## 15. Spacing Scale

```css
--space-1:  0.25rem;  /* 4px */
--space-2:  0.5rem;   /* 8px */
--space-3:  0.75rem;  /* 12px */
--space-4:  1rem;     /* 16px */
--space-6:  1.5rem;   /* 24px */
--space-8:  2rem;     /* 32px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
--space-24: 6rem;     /* 96px */
--space-32: 8rem;     /* 128px */
```
