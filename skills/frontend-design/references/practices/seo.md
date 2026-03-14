# SEO for Visual Content

Crawlability of non-text content.

## Checklist

| Practice | Standard |
|----------|----------|
| sr-only text for Canvas/WebGL content | WCAG 1.1.1, Search engines |
| aria-labelledby on visual heroes | WCAG 2.4.6, ARIA |
| Server-rendered semantic HTML alongside client effects | Google SEO, Crawlability |

---

## Why Visual-Only Content Needs Text Alternatives

Crawlers and assistive technologies rely on text. Canvas, WebGL, video, and image-heavy layouts often have no machine-readable content. Without alternatives, search engines can’t index meaning and screen readers can’t convey it. WCAG 1.1.1 (Non-text Content) requires text alternatives for non-text content that conveys information.

---

## sr-only Pattern: What It Is and How to Implement

**sr-only** (screen-reader-only) is CSS that visually hides text while keeping it available to assistive tech and crawlers. Common implementation:

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

Use it for headings, labels, and descriptions that describe visual content. For Canvas/WebGL, wrap the element and add an `sr-only` sibling with a concise description of the content and purpose.

---

## aria-labelledby vs aria-label for Visual Sections

**aria-label** – Use for short, self-contained labels (e.g., “Close dialog”).  
**aria-labelledby** – Use when the label comes from existing heading or text on the page. Points to the element’s `id` and reuses that content.

For hero sections or key visuals, prefer `aria-labelledby` pointing to an `<h1>` or section heading so the heading is both visible and used as the accessible name. Use `aria-label` when no suitable visible text exists.

---

## Server-Side Rendering: Why Client-Only Effects Need SSR Semantic HTML

Client-only rendering (e.g., pure SPA) sends empty or minimal HTML to crawlers. If content appears only after JavaScript, crawlers may not see it, or indexing can be delayed. Stanford and Google emphasize that crawlable HTML is essential for SEO.

**Guidance:** Serve core content in server-rendered semantic HTML (`<main>`, `<article>`, headings, structured data). Use client effects for enhancement (animations, interactivity) on top of that. Ensure critical text, links, and meta are in the initial response.

---

## Common Mistakes

| Mistake | Why it’s bad |
|---------|--------------|
| Canvas/WebGL with no text alternative | Unindexable; fails WCAG 1.1.1 |
| Hero sections with no accessible name | Screen readers and crawlers miss context |
| Critical content only in client-rendered DOM | Crawlers may not index; SEO risk |
| Hiding text with `display: none` or `visibility: hidden` | Often not exposed to assistive tech; use sr-only pattern instead |
| Overloading aria-label with long text | Prefer visible heading + aria-labelledby for clarity |
