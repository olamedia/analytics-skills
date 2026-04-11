# Vue Reactivity Rules

## ref vs reactive

Prefer `ref` for all state. It works with primitives and objects, avoids the destructuring trap, and keeps a consistent `.value` convention.

```vue
<!-- ❌ Mixed ref and reactive without reason -->
<script setup lang="ts">
const count = ref(0)
const state = reactive({ user: null, loading: false })
</script>

<!-- ✅ Consistent ref usage -->
<script setup lang="ts">
const count = ref(0)
const user = ref<User | null>(null)
const loading = ref(false)
</script>
```

`reactive` is acceptable for a group of related fields, but be aware of the destructuring trap.

## Destructuring reactive objects

```vue
<!-- ❌ Destructuring loses reactivity -->
<script setup lang="ts">
const state = reactive({ count: 0, name: 'Vue' })
const { count, name } = state  // count and name are plain values now
</script>

<!-- ✅ Use toRefs to preserve reactivity -->
<script setup lang="ts">
const state = reactive({ count: 0, name: 'Vue' })
const { count, name } = toRefs(state)
</script>
```

## computed must be pure

No side effects inside `computed`. It must only derive state — no mutations, no console.log, no API calls.

```vue
<!-- ❌ Side effects in computed -->
<script setup lang="ts">
const fullName = computed(() => {
  otherRef.value = 'changed'  // mutating another ref!
  return `${first.value} ${last.value}`
})
</script>

<!-- ✅ Pure computed, side effects in watch -->
<script setup lang="ts">
const fullName = computed(() => `${first.value} ${last.value}`)

watch(fullName, (name) => {
  analytics.track('name-changed', name)
})
</script>
```

## shallowRef for large objects

When holding large nested data that only changes by full replacement (e.g. API responses), use `shallowRef` to avoid deep reactivity overhead.

```vue
<!-- ❌ Deep reactivity on large data -->
<script setup lang="ts">
const tableData = ref(hugeNestedObject)  // deep proxy, expensive
</script>

<!-- ✅ shallowRef — only tracks .value replacement -->
<script setup lang="ts">
const tableData = shallowRef(hugeNestedObject)

function refresh(newData: TableData) {
  tableData.value = newData  // triggers update
}
</script>
```

If you need to mutate a nested property and trigger an update, use `triggerRef(tableData)` explicitly.

## Reviewer Questions

- Is `reactive` used where `ref` would be simpler and more consistent?
- Are there any destructured reactive objects that have silently lost reactivity?
- Does any `computed` contain side effects (mutations, logging, API calls)?
- Is `shallowRef` used for large data that only changes by full replacement?
