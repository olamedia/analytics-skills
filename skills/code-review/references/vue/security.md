# Vue Security Rules

## v-html requires sanitization

`v-html` renders raw HTML. If the content comes from user input or an API, it must be sanitized.

```vue
<!-- ❌ Unsanitized user input — XSS vulnerability -->
<template>
  <div v-html="userComment" />
</template>

<!-- ✅ Sanitize with DOMPurify -->
<script setup lang="ts">
import DOMPurify from 'dompurify'

const safeHtml = computed(() => DOMPurify.sanitize(userComment.value))
</script>
<template>
  <div v-html="safeHtml" />
</template>
```

If the content is fully controlled (e.g. markdown rendered at build time), `v-html` is safe — but add a comment explaining why sanitization is not needed.

## No user input in dynamic component :is

```vue
<!-- ❌ User-controlled component name — can render arbitrary components -->
<template>
  <component :is="userInput" />
</template>

<!-- ✅ Allowlist of known components -->
<script setup lang="ts">
const componentMap: Record<string, Component> = {
  alert: AlertBox,
  info: InfoBox,
}
const safeComponent = computed(() => componentMap[props.type] ?? FallbackBox)
</script>
<template>
  <component :is="safeComponent" />
</template>
```

## No user input in template expressions

Vue template expressions are sandboxed but can still leak data or cause unexpected behavior.

```vue
<!-- ❌ User input evaluated in template expression -->
<template>
  <div>{{ eval(userInput) }}</div>
</template>

<!-- ✅ Always bind data, never evaluate dynamic expressions -->
<template>
  <div>{{ sanitizedText }}</div>
</template>
```

## Reviewer Questions

- Is `v-html` used with unsanitized content?
- Are dynamic `:is` bindings validated against an allowlist?
- Is user input ever evaluated inside template expressions?
