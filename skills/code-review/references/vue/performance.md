# Vue Performance Rules

## v-memo for expensive list items

`v-memo` caches a subtree's rendered output and only re-renders when the specified dependencies change. Use for expensive list rendering.

```vue
<!-- ✅ v-memo — skip re-render unless tracked values change -->
<template>
  <div
    v-for="item in list"
    :key="item.id"
    v-memo="[item.id === selected, item.name, item.status]"
  >
    <ExpensiveComponent :item="item" :selected="item.id === selected" />
  </div>
</template>
```

Only use `v-memo` when you've confirmed a rendering bottleneck. It adds complexity.

## defineAsyncComponent for lazy loading

Split heavy components out of the main bundle.

```vue
<script setup lang="ts">
import { defineAsyncComponent } from 'vue'

const HeavyChart = defineAsyncComponent(() =>
  import('./components/HeavyChart.vue')
)

const AsyncModal = defineAsyncComponent({
  loader: () => import('./components/Modal.vue'),
  loadingComponent: LoadingSpinner,
  errorComponent: ErrorDisplay,
  delay: 200,
  timeout: 3000
})
</script>
```

## KeepAlive for cached dynamic components

Cache component instances when switching between tabs or views to preserve state and avoid re-mounting.

```vue
<template>
  <KeepAlive :max="10">
    <component :is="currentTab" />
  </KeepAlive>
</template>

<script setup lang="ts">
onActivated(() => refreshData())
onDeactivated(() => pauseTimers())
</script>
```

Use `include` / `exclude` props to control which components are cached. Set `:max` to limit cache size.

## Virtual lists for large datasets

Render only visible items. Use `@vueuse/core` `useVirtualList` or a similar library.

```vue
<script setup lang="ts">
import { useVirtualList } from '@vueuse/core'

const { list, containerProps, wrapperProps } = useVirtualList(
  items,
  { itemHeight: 50 }
)
</script>
<template>
  <div v-bind="containerProps" style="height: 400px; overflow: auto">
    <div v-bind="wrapperProps">
      <div v-for="item in list" :key="item.data.id" style="height: 50px">
        {{ item.data.name }}
      </div>
    </div>
  </div>
</template>
```

## shallowRef for large non-reactive data

Already covered in `reactivity.md` — use `shallowRef` for large objects that are replaced wholesale, not deeply mutated.

## Avoid unnecessary reactive conversions

```typescript
// ❌ Wrapping static config in ref — no need for reactivity
const config = ref({ apiUrl: '/api', timeout: 5000 })

// ✅ Plain const for static data
const config = { apiUrl: '/api', timeout: 5000 } as const
```

## Reviewer Questions

- Is `v-memo` used only where a rendering bottleneck has been confirmed?
- Are heavy, rarely-used components loaded with `defineAsyncComponent`?
- Is `KeepAlive` used for tab/view switching to avoid unnecessary re-mounts?
- Are lists with 100+ items using virtual scrolling?
- Is static data wrapped in `ref`/`reactive` unnecessarily?
