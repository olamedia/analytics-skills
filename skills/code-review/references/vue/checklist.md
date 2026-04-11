# Vue Review Checklist

Quick-reference checklist aggregating all Vue review points. Use for a final pass.

## Reactivity
- [ ] `ref` is used consistently (or `reactive` with `toRefs` for destructuring)
- [ ] No destructured `reactive` objects without `toRefs`
- [ ] `computed` contains no side effects (no mutations, no API calls)
- [ ] `shallowRef` used for large objects that change by full replacement
- [ ] No unnecessary reactive wrappers on static data

## Props & Emits
- [ ] `defineProps` uses TypeScript generics (not array syntax)
- [ ] `defineEmits` is fully typed with named payload parameters
- [ ] No direct props mutation — changes go through emit
- [ ] Object/array defaults use factory functions in `withDefaults`
- [ ] `defineModel` used where it simplifies v-model (3.4+)
- [ ] Reactive Props Destructure used correctly (3.5+)

## Watchers
- [ ] Async watchers have cleanup functions (abort stale requests)
- [ ] `deep: true` not used on large objects (prefer specific property getters)
- [ ] Watchers are not used where `computed` would suffice
- [ ] `{ immediate: true }` used when the initial value matters
- [ ] `onWatcherCleanup` used for complex cleanup (3.5+)

## Composables
- [ ] Composable names start with `use`
- [ ] Return refs, not `.value`
- [ ] Exposed state uses `readonly` where mutation should be controlled
- [ ] Props passed to composables preserve reactivity (ref/getter, not raw value)
- [ ] Event listeners and timers cleaned up on unmount
- [ ] `effectScope` used for complex composables with many effects

## Templates
- [ ] `v-for` keys are unique and stable (not array indices on mutable lists)
- [ ] `v-if` and `v-for` never on the same element
- [ ] Complex inline event handlers extracted to methods
- [ ] `useTemplateRef` considered for template refs (3.5+)
- [ ] `useId` used for form element IDs (3.5+)

## Performance
- [ ] `v-memo` used only for confirmed rendering bottlenecks
- [ ] Heavy components loaded with `defineAsyncComponent`
- [ ] `KeepAlive` used for tab/view switching where state preservation matters
- [ ] Lists with 100+ items use virtual scrolling
- [ ] Static data not wrapped in `ref`/`reactive`

## Testing
- [ ] Tests check observable behavior (not internal state like `wrapper.vm.xxx`)
- [ ] Async operations awaited with `flushPromises` or `nextTick`
- [ ] Pinia stores mocked with `createTestingPinia`
- [ ] Composables tested via wrapper component when they use lifecycle hooks
- [ ] No snapshot-only tests without behavioral assertions

## Security
- [ ] `v-html` only used with sanitized content (DOMPurify)
- [ ] Dynamic `:is` bindings validated against an allowlist
- [ ] No user input evaluated in template expressions
