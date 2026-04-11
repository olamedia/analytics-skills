# Vue Composables Rules

## Naming convention

Composables must start with `use` prefix. This signals reactive/lifecycle dependency.

```typescript
// ❌ No use prefix — looks like a plain utility
export function fetchUser(id: Ref<string>) { /* ... */ }

// ✅ use prefix — clearly a composable
export function useUser(id: MaybeRefOrGetter<string>) { /* ... */ }
```

## Return refs, not .value

```typescript
// ❌ Returning unwrapped .value — loses reactivity
export function useCounter() {
  const count = ref(0)
  return { count: count.value }  // plain number, not reactive
}

// ✅ Return the ref itself
export function useCounter(initial = 0) {
  const count = ref(initial)
  const increment = () => count.value++
  const decrement = () => count.value--
  return { count: readonly(count), increment, decrement }
}
```

## Use readonly for exposed state

Prevent consumers from mutating internal state directly. Expose mutation methods instead.

```typescript
// ❌ Mutable return — consumers can break internal invariants
export function useAuth() {
  const user = ref<User | null>(null)
  return { user }  // anyone can set user.value = anything
}

// ✅ Readonly return with explicit actions
export function useAuth() {
  const user = ref<User | null>(null)
  async function login(credentials: Credentials) { /* ... */ }
  function logout() { user.value = null }
  return { user: readonly(user), login, logout }
}
```

## Props reactivity in composables

When a composable accepts props, keep reactivity alive. Don't pass `props.value` — pass a ref or getter.

```vue
<!-- ❌ Props value passed — loses reactivity -->
<script setup lang="ts">
const props = defineProps<{ userId: string }>()
const { user } = useUser(props.userId)  // snapshot, not reactive
</script>

<!-- ✅ Pass a ref or getter -->
<script setup lang="ts">
const props = defineProps<{ userId: string }>()
const { user } = useUser(toRef(props, 'userId'))
// or with computed
const { user } = useUser(computed(() => props.userId))
</script>

<!-- ✅ Vue 3.5+ — destructured props as getter -->
<script setup lang="ts">
const { userId } = defineProps<{ userId: string }>()
const { user } = useUser(() => userId)
</script>
```

## Async composable pattern

```typescript
// ✅ Standard async composable — sync return, async execution
export function useFetch<T>(url: MaybeRefOrGetter<string>) {
  const data = ref<T | null>(null)
  const error = ref<Error | null>(null)
  const loading = ref(false)

  const execute = async () => {
    loading.value = true
    error.value = null
    try {
      const response = await fetch(toValue(url))
      if (!response.ok) throw new Error(`HTTP ${response.status}`)
      data.value = await response.json()
    } catch (e) {
      error.value = e as Error
    } finally {
      loading.value = false
    }
  }

  watchEffect(() => {
    toValue(url)
    execute()
  })

  return { data: readonly(data), error: readonly(error), loading: readonly(loading), refetch: execute }
}
```

## Lifecycle cleanup

Composables that add event listeners, timers, or subscriptions must clean up on unmount.

```typescript
// ❌ No cleanup — memory leak
export function useWindowSize() {
  const width = ref(window.innerWidth)
  window.addEventListener('resize', () => { width.value = window.innerWidth })
  return { width }
}

// ✅ Cleanup on unmount
export function useWindowSize() {
  const width = ref(window.innerWidth)
  const handler = () => { width.value = window.innerWidth }

  onMounted(() => window.addEventListener('resize', handler))
  onUnmounted(() => window.removeEventListener('resize', handler))

  return { width: readonly(width) }
}
```

For complex composables with many effects, use `effectScope`:

```typescript
export function useFeature() {
  const scope = effectScope()

  scope.run(() => {
    const state = ref(0)
    watch(state, () => { /* ... */ })
    watchEffect(() => { /* ... */ })
  })

  onUnmounted(() => scope.stop())

  return { /* ... */ }
}
```

## Reviewer Questions

- Does the composable name start with `use`?
- Does it return refs (not `.value`)?
- Is exposed state wrapped in `readonly` where mutation should be controlled?
- When accepting props, is reactivity preserved (ref/getter, not raw value)?
- Are all event listeners, timers, and subscriptions cleaned up on unmount?
