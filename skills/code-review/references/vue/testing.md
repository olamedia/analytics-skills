# Vue Testing Rules

## Vitest as recommended test runner

Use Vitest for unit and component tests. It integrates natively with Vite and supports Vue SFCs.

## Mount vs shallowMount

```typescript
// ✅ mount — renders child components, tests real behavior
const wrapper = mount(MyComponent, {
  props: { title: 'Hello' }
})

// ✅ shallowMount — stubs child components, isolates the unit
const wrapper = shallowMount(MyComponent, {
  props: { title: 'Hello' }
})
```

Use `mount` by default (tests behavior as the user sees it). Use `shallowMount` when child components have heavy setup or are tested separately.

## Black-box testing — test behavior, not internals

```typescript
// ❌ Testing implementation details
it('sets loading ref to true', () => {
  const wrapper = mount(MyComponent)
  expect(wrapper.vm.loading).toBe(true)  // testing internal state
})

// ✅ Testing observable behavior
it('shows loading indicator while fetching', () => {
  const wrapper = mount(MyComponent)
  expect(wrapper.find('.loading-spinner').exists()).toBe(true)
})
```

## Async testing — flushPromises and nextTick

```typescript
import { flushPromises } from '@vue/test-utils'

// ✅ Wait for async operations
it('renders user after fetch', async () => {
  const wrapper = mount(UserProfile, { props: { userId: '1' } })

  await flushPromises()  // wait for API call to resolve

  expect(wrapper.text()).toContain('Alice')
})

// ✅ Wait for DOM update after state change
it('updates text on click', async () => {
  const wrapper = mount(Counter)

  await wrapper.find('button').trigger('click')
  await nextTick()

  expect(wrapper.text()).toContain('1')
})
```

## Pinia store mocking — createTestingPinia

```typescript
import { createTestingPinia } from '@pinia/testing'

// ✅ Mock stores with createTestingPinia
it('shows user name from store', () => {
  const wrapper = mount(UserBadge, {
    global: {
      plugins: [
        createTestingPinia({
          createSpy: vi.fn,
          initialState: {
            user: { name: 'Alice', email: 'alice@example.com' }
          }
        })
      ]
    }
  })

  expect(wrapper.text()).toContain('Alice')
})
```

## Composable testing — wrapper component pattern

Composables that use lifecycle hooks or `inject` need a host component.

```typescript
// ✅ Test composable via wrapper component
import { mount } from '@vue/test-utils'

function mountComposable<T>(composable: () => T): T {
  let result!: T
  mount({
    setup() {
      result = composable()
      return () => null
    }
  })
  return result
}

it('returns count ref starting at 0', () => {
  const { count } = mountComposable(() => useCounter())
  expect(count.value).toBe(0)
})
```

## No snapshot-only tests

```typescript
// ❌ Snapshot with no behavioral assertion — passes even when broken
it('renders correctly', () => {
  const wrapper = mount(MyComponent)
  expect(wrapper.html()).toMatchSnapshot()
})

// ✅ Behavioral assertions (snapshot as supplement, not sole check)
it('renders title and items', () => {
  const wrapper = mount(MyComponent, {
    props: { title: 'Tasks', items: ['a', 'b'] }
  })
  expect(wrapper.find('h1').text()).toBe('Tasks')
  expect(wrapper.findAll('li')).toHaveLength(2)
})
```

## Reviewer Questions

- Are tests testing observable behavior (not internal state like `wrapper.vm.xxx`)?
- Are async operations properly awaited with `flushPromises` or `nextTick`?
- Are Pinia stores mocked with `createTestingPinia` (not manual mocks)?
- Are composables tested via a wrapper component when they use lifecycle hooks?
- Are there snapshot-only tests with no behavioral assertions?
