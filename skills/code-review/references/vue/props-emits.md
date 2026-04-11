# Vue Props & Emits Rules

## defineProps must have TypeScript types

```vue
<!-- ❌ Array syntax — no type checking -->
<script setup lang="ts">
const props = defineProps(['title', 'count'])
</script>

<!-- ✅ TypeScript type declaration -->
<script setup lang="ts">
interface Props {
  title: string
  count?: number
  items?: string[]
}
const props = withDefaults(defineProps<Props>(), {
  count: 0,
  items: () => []  // factory function for object/array defaults
})
</script>
```

## defineEmits must be typed

```vue
<!-- ❌ Array syntax — no payload type safety -->
<script setup lang="ts">
const emit = defineEmits(['update', 'delete'])
emit('update', someValue)  // payload type unknown
</script>

<!-- ✅ Full type definition -->
<script setup lang="ts">
const emit = defineEmits<{
  update: [id: number, value: string]
  delete: [id: number]
}>()

emit('update', 1, 'new value')  // ✅ type-checked
emit('update', 'wrong')         // ❌ TypeScript error
</script>
```

## Never mutate props directly

```vue
<!-- ❌ Direct props mutation -->
<script setup lang="ts">
const props = defineProps<{ user: User }>()
props.user.name = 'New Name'  // mutating parent state!
</script>

<!-- ✅ Emit an event to let the parent update -->
<script setup lang="ts">
const props = defineProps<{ user: User }>()
const emit = defineEmits<{ update: [name: string] }>()
const updateName = (name: string) => emit('update', name)
</script>
```

## defineModel (3.4+)

Simplifies v-model implementation. Replaces the verbose props + emit + computed pattern.

```vue
<!-- ❌ Verbose v-model pattern -->
<script setup lang="ts">
const props = defineProps<{ modelValue: string }>()
const emit = defineEmits<{ 'update:modelValue': [value: string] }>()
const value = computed({
  get: () => props.modelValue,
  set: (val) => emit('update:modelValue', val)
})
</script>

<!-- ✅ defineModel -->
<script setup lang="ts">
const model = defineModel<string>()
</script>
<template>
  <input v-model="model" />
</template>
```

Named and multiple v-models:

```vue
<script setup lang="ts">
const firstName = defineModel<string>('firstName')
const lastName = defineModel<string>('lastName')
</script>
```

## Reactive Props Destructure (3.5+)

In Vue 3.5+, destructured props stay reactive. Default values replace `withDefaults`.

```vue
<!-- ✅ Vue 3.5+ — destructured props are reactive -->
<script setup lang="ts">
const { count, name = 'default' } = defineProps<{
  count: number
  name?: string
}>()

watch(() => count, (newCount) => {
  console.log('Count changed:', newCount)
})
</script>
```

**Before 3.5**, destructuring props loses reactivity — use `props.fieldName` or `toRefs(props)`.

## withDefaults factory functions

Object and array default values must use factory functions to avoid shared references.

```vue
<!-- ❌ Shared reference — all instances share the same array -->
<script setup lang="ts">
withDefaults(defineProps<{ items: string[] }>(), {
  items: []  // shared between instances!
})
</script>

<!-- ✅ Factory function — each instance gets its own copy -->
<script setup lang="ts">
withDefaults(defineProps<{ items: string[] }>(), {
  items: () => []
})
</script>
```

## Reviewer Questions

- Are `defineProps` using TypeScript generics (not array syntax)?
- Are `defineEmits` fully typed with named payload parameters?
- Is any component mutating props directly instead of emitting?
- Could `defineModel` simplify a manual v-model computed pattern?
- Are object/array defaults using factory functions in `withDefaults`?
