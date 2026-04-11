# TypeScript Testing Rules

## Typed mocks — avoid `as any` in tests

```typescript
// ❌ as any to bypass types — hides real type errors
const mockService = {
  getUser: vi.fn()
} as any as UserService

// ✅ Properly typed mock
const mockService: Pick<UserService, 'getUser'> = {
  getUser: vi.fn<[string], Promise<User>>()
}

// ✅ Or use vi.mocked for existing functions
import { getUser } from './api'
vi.mock('./api')
const mockedGetUser = vi.mocked(getUser)
```

## @ts-expect-error over @ts-ignore

```typescript
// ❌ @ts-ignore — silently swallows all errors, even future ones
// @ts-ignore
const result = brokenFunction()

// ✅ @ts-expect-error — fails if the error is fixed (catches stale suppressions)
// @ts-expect-error testing invalid input handling
const result = brokenFunction()
```

`@ts-expect-error` is self-documenting: it says "this line should have an error." If the error disappears (e.g. after a fix), TypeScript will flag the now-unnecessary suppression.

## Testing type correctness with expectTypeOf

```typescript
import { expectTypeOf } from 'vitest'

it('returns a User', () => {
  const result = getUser('1')
  expectTypeOf(result).toEqualTypeOf<Promise<User>>()
})

it('accepts readonly arrays', () => {
  expectTypeOf(processItems).parameter(0).toMatchTypeOf<readonly Item[]>()
})
```

## Proper generic typing for test utilities

```typescript
// ❌ Generic test helper that loses type info
function createMock(overrides: any) {
  return { ...defaultUser, ...overrides }
}

// ✅ Typed factory
function createUser(overrides: Partial<User> = {}): User {
  return {
    id: '1',
    name: 'Test User',
    email: 'test@example.com',
    ...overrides
  }
}
```

## Type assertions in test assertions

```typescript
// ❌ Asserting without type safety
expect(result).toEqual({ name: 'Alice' })  // no check that result matches User shape

// ✅ Type-safe assertion
const user: User = result
expect(user.name).toBe('Alice')
expect(user.email).toBe('alice@example.com')
```

## Reviewer Questions

- Are test mocks typed properly (not using `as any`)?
- Is `@ts-ignore` used anywhere? Should it be `@ts-expect-error` with an explanation?
- Are test factories returning properly typed objects?
- Could `expectTypeOf` validate type contracts in key utility functions?
