# Motion

Fluid, fitting animations make a website feel responsive and polished — they contribute to the "smoothness" property of processing fluency. A button that transitions its state, a panel that slides into place, a list that fades in — these signal that someone built the interface with care. But the same animations become a liability if they freeze, stutter, or drop frames. A janky animation is worse than no animation: it signals broken software, not premium craft. The rule is not "less motion" — it's "only motion you can run flawlessly."

## Checklist

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

## Duration Guide

| Animation type | Duration | Examples |
|---|---|---|
| Micro-interactions | 150-200ms | Button press feedback, checkbox toggle, tooltip show |
| State transitions | 200-300ms | Tab switch, dropdown open, accordion expand |
| Complex transitions | 300-400ms | Page slide, modal enter, drawer open |
| Stagger per item | 30-50ms | List item entrances, card grid reveals |

**Exit animations should be 60-70% of enter duration.** Users want to dismiss things faster than they want to see them appear. A 300ms enter → 200ms exit feels natural. A 300ms enter → 300ms exit feels sluggish on close.

**Never exceed 400ms** for any single animation. Beyond that, the animation becomes something the user waits for rather than something that enhances flow.

---

## Easing Guide

| Direction | Easing | CSS value | Why |
|---|---|---|---|
| Element entering | ease-out | `cubic-bezier(0, 0, 0.2, 1)` | Fast start, gentle landing — feels like arriving |
| Element exiting | ease-in | `cubic-bezier(0.4, 0, 1, 1)` | Slow start, fast exit — feels like departing |
| Element moving | ease-in-out | `cubic-bezier(0.4, 0, 0.2, 1)` | Smooth acceleration and deceleration |
| Linear (rare) | linear | `linear` | Progress bars, continuous rotation only |

**Never use `ease` (the CSS default)** for deliberate animations — it's a generic curve that doesn't match any natural movement. Always specify the easing explicitly.

**Spring-like motion** (overshoot + settle) feels more natural for UI elements that "land" somewhere. Use for modals, popovers, dragged elements. Avoid for small micro-interactions where overshoot feels twitchy.

---

## CSS vs. JS Decision

| Need | Use | Why |
|---|---|---|
| Hover/focus states | CSS transitions | Zero JS overhead, instant response |
| Simple entrance/exit | CSS keyframes | Lightweight, no library needed |
| `prefers-reduced-motion` | CSS media query | Automatic, no JS runtime cost |
| Layout animations | Framer Motion (React) or FLIP | Animating layout changes needs JS coordination |
| Gesture-based (drag, pan) | Framer Motion or use-gesture | Built-in physics, touch handling |
| Scroll-triggered | Intersection Observer + CSS classes, or GSAP ScrollTrigger | IO for simple reveals, GSAP for complex sequences |
| Complex multi-step timelines | GSAP | Precise control, pausable, reversible |
| SVG path morphing | GSAP MorphSVG | No CSS alternative exists |

**Default to CSS. Escalate to JS only when CSS can't do it.** Every JS animation library adds bundle weight and runtime overhead.

---

## Content Hierarchy and Motion

```
Hero (one flourish) → Content blocks (calm) → Proof (calm) → CTA (highlighted)
```

- **Max 1 hero flourish** — a single animated/visual element (parallax, particle effect, animated illustration, gradient shift). This is where you invest motion budget.
- **Max 1 supporting flourish** — one micro-interaction below the fold (animated counter, glowing CTA, hover effect on a key card).
- **Everything else supports readability** — static or minimal fade-in on scroll.
- **If the page is longer than 1 viewport**, every second section should be mostly static. Continuous animation across the entire page exhausts attention.

---

## Interruptibility

Every animation must be interruptible by user action. If the user taps/clicks during an animation, the animation should:

1. **Cancel immediately** and jump to the target state, OR
2. **Reverse smoothly** if the action undoes the trigger (e.g., closing a dropdown that's still opening)

Never force the user to wait for an animation to complete before they can interact.

```css
/* CSS transitions are naturally interruptible — 
   changing the property mid-transition reverses/redirects */
.dropdown {
  transition: transform 200ms ease-out, opacity 200ms ease-out;
}
```

JS animations (GSAP, Framer Motion) need explicit handling:
- GSAP: `.kill()` or `.reverse()` on the timeline
- Framer Motion: `animate` controls with `stop()` or layout animations auto-interrupt

---

## Jank Detection

An animation running below 60fps (or 30fps on mobile) is janky. Common causes:

| Cause | Fix |
|---|---|
| Animating `width`, `height`, `top`, `left`, `margin`, `padding` | Only animate `transform` and `opacity` |
| Missing `will-change` on animated element | Add `will-change: transform` (remove after animation) |
| Heavy JS running during animation | Move computation to Web Worker or requestIdleCallback |
| Too many animated elements simultaneously | Reduce to 1 continuous animation per viewport |
| Large DOM repaints during animation | Promote animated element to own layer with `transform: translateZ(0)` |
| Image/font loading during animation | Preload critical assets before animation starts |

**Testing:** Open DevTools → Performance tab → record during animation. The frame chart should show consistent ~16ms frames. Spikes above 16ms = jank.

**Rule of thumb:** If you can't guarantee 60fps on a mid-range Android phone, remove the animation and use a CSS transition instead. The animation should never be noticed for being slow — only for being smooth.

---

## `prefers-reduced-motion`

Non-negotiable. Some users experience motion sickness, seizures, or distraction from animations.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

This kills all motion globally. For more nuance, selectively disable decorative motion while keeping functional transitions (opacity fades, state changes):

```css
@media (prefers-reduced-motion: reduce) {
  .hero-animation,
  .parallax,
  .particle-effect {
    animation: none;
  }
  /* Keep functional transitions but make them instant */
  .dropdown, .modal, .tooltip {
    transition-duration: 0.01ms;
  }
}
```

---

## Performance Budgets by Device

| Device tier | Animation approach | Max simultaneous |
|---|---|---|
| Desktop (modern) | Full animations, 60fps target | 1 continuous + micro-interactions |
| Tablet | CSS-only or lightweight JS, 30fps acceptable | 1 continuous, simpler effects |
| Mobile / low-power | CSS transitions only, no continuous animation | Micro-interactions only |

Detect low-power devices and degrade gracefully:

```js
const isLowPower = (
  window.innerWidth < 768 ||
  navigator.hardwareConcurrency <= 4
);
```

---

## Common Mistakes

- Adding animation for decoration rather than cause-effect — "it looks cool" is not a reason. Every animation should answer "what just happened?" or "where did this come from?"
- Exceeding 400ms for any single animation — user starts waiting instead of flowing
- Using `ease` (CSS default) instead of explicit easing — generic curve, doesn't match natural movement
- Animating `width`/`height`/`top`/`left` instead of `transform` — triggers layout recalculation, causes jank
- Forgetting `prefers-reduced-motion` — accessibility violation, can cause physical discomfort
- Multiple continuous animations in the same viewport — exhausts attention, increases visual complexity
- Non-interruptible animations — user clicks during a 400ms transition and nothing happens until it finishes
- Shipping an animation that runs at 60fps on your MacBook but 15fps on a mid-range Android — test on real devices
