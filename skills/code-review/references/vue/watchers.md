# Vue Watchers Rules

## watch vs watchEffect

```vue
<script setup lang="ts">
// ✅ watch — explicit dependency, lazy by default, access to old value
watch(
  () => props.userId,
  async (userId, oldUserId) => {
    user.value = await fetchUser(userId)
  }
)

// ✅ watchEffect — auto-tracks dependencies, runs immediately
watchEffect(async () => {
  user.value = await fetchUser(props.userId)
})
</script>
```

**When to use which:**
- Need the old value? → `watch`
- Need lazy execution (don't run on mount)? → `watch`
- Many dependencies and you want auto-tracking? → `watchEffect`

## Cleanup functions — prevent stale requests

```vue
<!-- ❌ No cleanup — stale response can overwrite fresh one -->
<script setup lang="ts">
watch(searchQuery, async (query) => {
  const res = await fetch(`/api/search?q=${query}`)
  results.value = await res.json()
})
</script>

<!-- ✅ Abort previous request on re-trigger -->
<script setup lang="ts">
watch(searchQuery, async (query, _, onCleanup) => {
  const controller = new AbortController()
  onCleanup(() => controller.abort())

  try {
    const res = await fetch(`/api/search?q=${query}`, {
      signal: controller.signal
    })
    results.value = await res.json()
  } catch (e) {
    if ((e as Error).name !== 'AbortError') throw e
  }
})
</script>
```

## onWatcherCleanup (3.5+)

More flexible cleanup — can be called anywhere inside the callback, not just as the third argument.

```vue
<script setup lang="ts">
import { onWatcherCleanup } from 'vue'

watch(source, async (value) => {
  const controller = new AbortController()
  onWatcherCleanup(() => controller.abort())

  if (needsExtraResource(value)) {
    const resource = createResource()
    onWatcherCleanup(() => resource.dispose())
  }

  await fetchData(value, controller.signal)
})
</script>
```

## Watch options

```vue
<script setup lang="ts">
// immediate — run on mount
watch(userId, (id) => fetchUser(id), { immediate: true })

// deep — track nested changes (expensive, use sparingly)
watch(state, (s) => save(s), { deep: true })

// deep with numeric level (3.5+) — limit traversal depth
watch(state, (s) => save(s), { deep: 2 })

// flush: 'post' — run after DOM update
watch(content, () => scrollToBottom(), { flush: 'post' })

// once (3.4+) — fire once then auto-stop
watch(source, (val) => initOnce(val), { once: true })
</script>
```

**Caution with `deep: true`:** On large objects this is expensive. Prefer watching specific properties via a getter: `watch(() => state.count, ...)`.

## Watching multiple sources

```vue
<script setup lang="ts">
watch(
  [firstName, lastName],
  ([newFirst, newLast], [oldFirst, oldLast]) => {
    console.log(`${oldFirst} ${oldLast} → ${newFirst} ${newLast}`)
  }
)

// Or with getters for reactive object properties
watch(
  () => [state.count, state.name],
  ([count, name]) => { /* ... */ }
)
</script>
```

## Prefer computed over watchers for derived state

```vue
<!-- ❌ Watch to derive state — unnecessary watcher -->
<script setup lang="ts">
const fullName = ref('')
watch([firstName, lastName], ([f, l]) => {
  fullName.value = `${f} ${l}`
})
</script>

<!-- ✅ Use computed -->
<script setup lang="ts">
const fullName = computed(() => `${firstName.value} ${lastName.value}`)
</script>
```

## Reviewer Questions

- Does any async watcher lack a cleanup function (risk of stale responses)?
- Is `deep: true` used on large objects where watching a specific property would suffice?
- Could any watcher be replaced with a simpler `computed`?
- Are there watchers that should use `{ immediate: true }` but currently miss the initial value?
