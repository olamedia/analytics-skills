# Design Tokens

Consistency is a property of processing fluency. Tokens enforce it at the system level.

## Checklist

| Practice | Standard |
|----------|----------|
| Never use raw hex in components | Token-based theming |
| Three-layer token architecture: Primitive → Semantic → Component | Token architecture |
| CSS variables for all colors | CSS custom properties |
| Semantic tokens enable theming (light/dark) | Theming system |
| Token-driven theming across all surfaces | System-wide consistency |

---

## Detailed Guidance

### Three-Layer Architecture Explained

**Layer 1: Primitive tokens** — Raw design values, no semantic meaning.
```css
:root {
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-gray-100: #f3f4f6;
  --color-gray-900: #111827;
  --font-sans: 'Inter', system-ui, sans-serif;
  --radius-md: 0.375rem;
}
```

**Layer 2: Semantic tokens** — Purpose-based mappings to primitives.
```css
:root {
  --color-primary: var(--color-blue-600);
  --color-background: var(--color-gray-100);
  --color-text: var(--color-gray-900);
  --color-text-muted: var(--color-gray-500);
}

[data-theme="dark"] {
  --color-background: var(--color-gray-900);
  --color-text: var(--color-gray-100);
}
```

**Layer 3: Component tokens** — Surface-specific usage.
```css
:root {
  --button-bg: var(--color-primary);
  --button-text: white;
  --card-bg: var(--color-background);
  --input-border: var(--color-gray-300);
}
```

Components reference component or semantic tokens only, never primitives directly.

### Why Raw Hex in Components Is Bad

Using hex (or other raw values) in components leads to:
- **Theming breaks** — Hard to switch light/dark or brand themes.
- **Inconsistency** — Similar use cases use different values.
- **Maintenance** — Changes require updates across many files.
- **Accessibility** — Contrast and color fixes must be reapplied everywhere.

Components should use tokens. Tokens are the single source of truth.

### How Semantic Tokens Enable Light/Dark Mode

Semantic tokens abstract "what" the color does (e.g., background, text, primary). Different themes map the same semantic names to different primitives.

```css
:root {
  --color-background: #ffffff;
  --color-text: #1a1a1a;
}

[data-theme="dark"] {
  --color-background: #1a1a1a;
  --color-text: #f5f5f5;
}
```

Switching `data-theme` updates all surfaces. No component-level changes.

Use `prefers-color-scheme` for system-driven themes:
```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-background: #1a1a1a;
    --color-text: #f5f5f5;
  }
}
```

### Token Naming Conventions

- **Primitives** — `--color-{name}-{step}`, `--font-{family}`, `--radius-{size}`.
- **Semantic** — `--color-{purpose}`: `--color-primary`, `--color-background`, `--color-text-muted`.
- **Component** — `--{component}-{property}`: `--button-bg`, `--input-border`.

Use kebab-case and avoid vague names like `--blue` or `--dark`.

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using raw hex in components | Define primitives, then semantic/component tokens; use tokens in components. |
| Skipping the semantic layer | Add a semantic layer between primitives and components for theming. |
| Inconsistent token names | Adopt and document a naming scheme (e.g., primitive/semantic/component). |
| Hardcoding theme values in components | Components should only reference tokens. |
| Missing dark-mode tokens | Define dark values for all semantic color tokens. |
| One-off values instead of tokens | Add new tokens to the scale instead of inventing values inline. |
| Tokens with unclear purpose | Use semantic names (e.g., `--color-text-muted`) instead of visual names (e.g., `--color-gray-500`). |
