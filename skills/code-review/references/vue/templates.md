# Vue Template Rules

## v-for keys must be unique and stable

```vue
<!-- ❌ Index as key — breaks state when list is reordered/filtered -->
<template>
  <li v-for="(item, index) in items" :key="index">
    {{ item.name }}
  </li>
</template>

<!-- ✅ Unique, stable identifier -->
<template>
  <li v-for="item in items" :key="item.id">
    {{ item.name }}
  </li>
</template>
```

When there is no unique ID, prefer a composite key over index:

```vue
<template>
  <li v-for="(item, index) in items" :key="`${item.name}-${item.type}-${index}`">
    {{ item.name }}
  </li>
</template>
```

## Never use v-if and v-for on the same element

In Vue 3, `v-if` has higher priority than `v-for`, so `v-if` cannot access the `v-for` variable.

```vue
<!-- ❌ v-if and v-for on same element -->
<template>
  <li v-for="user in users" v-if="user.active" :key="user.id">
    {{ user.name }}
  </li>
</template>

<!-- ✅ Computed filter -->
<script setup lang="ts">
const activeUsers = computed(() => users.value.filter(u => u.active))
</script>
<template>
  <li v-for="user in activeUsers" :key="user.id">
    {{ user.name }}
  </li>
</template>

<!-- ✅ Or template wrapper -->
<template>
  <template v-for="user in users" :key="user.id">
    <li v-if="user.active">{{ user.name }}</li>
  </template>
</template>
```

## Event handlers — methods over inline logic

```vue
<!-- ❌ Complex inline logic -->
<template>
  <button @click="items = items.filter(i => i.id !== item.id); count--">
    Delete
  </button>
</template>

<!-- ✅ Extract to method -->
<script setup lang="ts">
const deleteItem = (id: number) => {
  items.value = items.value.filter(i => i.id !== id)
  count.value--
}
</script>
<template>
  <button @click="deleteItem(item.id)">Delete</button>
</template>
```

Simple one-liners like `@click="visible = false"` are fine inline.

## useTemplateRef (3.5+)

Provides clearer template ref binding than the implicit ref-name convention.

```vue
<!-- Traditional — ref variable name must match template ref string -->
<script setup lang="ts">
const inputRef = ref<HTMLInputElement | null>(null)
</script>
<template>
  <input ref="inputRef" />
</template>

<!-- ✅ useTemplateRef — explicit, supports dynamic refs -->
<script setup lang="ts">
import { useTemplateRef } from 'vue'

const input = useTemplateRef<HTMLInputElement>('my-input')
onMounted(() => input.value?.focus())
</script>
<template>
  <input ref="my-input" />
</template>
```

## useId (3.5+)

Generates SSR-safe unique IDs for form elements and accessibility attributes.

```vue
<!-- ❌ Random IDs — inconsistent between server and client -->
<script setup lang="ts">
const id = `input-${Math.random()}`
</script>

<!-- ✅ useId — SSR-safe -->
<script setup lang="ts">
import { useId } from 'vue'
const id = useId()
</script>
<template>
  <label :for="id">Email</label>
  <input :id="id" type="email" />
</template>
```

## Reviewer Questions

- Are `v-for` keys unique and stable (not array indices on mutable lists)?
- Is `v-if` used on the same element as `v-for`?
- Are complex inline event handlers extracted to methods?
- Could `useTemplateRef` (3.5+) replace ambiguous ref naming?
- Are form elements using `useId` (3.5+) for accessible labeling?
