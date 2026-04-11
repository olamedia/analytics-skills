# TypeScript Type Safety Rules

## No `any` — use `unknown` with type guards

```typescript
// ❌ any defeats the type system
function processData(data: any) {
  return data.value  // no type checking, crashes at runtime
}

// ✅ Proper types
interface DataPayload { value: string }
function processData(data: DataPayload) {
  return data.value
}

// ✅ unknown + type guard for truly unknown input
function processUnknown(data: unknown) {
  if (typeof data === 'object' && data !== null && 'value' in data) {
    return (data as { value: string }).value
  }
  throw new Error('Invalid data shape')
}
```

## Prefer type narrowing over type assertions

```typescript
// ❌ Unsafe assertion — wrong at runtime if value is a string
function getLength(value: string | string[]) {
  return (value as string[]).length
}

// ✅ Type narrowing — safe at runtime
function getLength(value: string | string[]): number {
  if (Array.isArray(value)) {
    return value.length
  }
  return value.length
}

// ✅ Discriminated union narrowing
interface Success { ok: true; data: User }
interface Failure { ok: false; error: string }
type Result = Success | Failure

function handle(result: Result) {
  if (result.ok) {
    console.log(result.data.name)  // TS knows it's Success
  } else {
    console.error(result.error)    // TS knows it's Failure
  }
}
```

## Type-only imports

```typescript
// ❌ Regular import for types — included in JS output, may cause side effects
import { User, UserService } from './user'

// ✅ Separate type imports — erased at compile time
import type { User } from './user'
import { UserService } from './user'

// ✅ Inline type import
import { UserService, type User } from './user'
```

## Discriminated unions over type assertions

```typescript
// ❌ Type assertion hiding invalid state
type ApiResult = { data?: User; error?: string }
function handle(result: ApiResult) {
  const user = result.data as User  // might be undefined!
}

// ✅ Discriminated union — impossible to access wrong field
type ApiResult =
  | { success: true; data: User }
  | { success: false; error: string }

function handle(result: ApiResult) {
  if (result.success) {
    console.log(result.data.name)  // data guaranteed
  }
}
```

## Utility types — use them, don't reinvent

```typescript
// Common utility types
type PartialUser = Partial<User>        // all fields optional
type RequiredUser = Required<User>      // all fields required
type ReadonlyUser = Readonly<User>      // all fields readonly
type NameOnly = Pick<User, 'name'>      // subset of fields
type WithoutId = Omit<User, 'id'>       // exclude fields
type UserMap = Record<string, User>     // dictionary
```

## `as const` for literal types

```typescript
// ❌ Type too wide — method is string
const config = { endpoint: '/api', method: 'GET' }

// ✅ as const — method is 'GET'
const config = { endpoint: '/api', method: 'GET' } as const

function request(method: 'GET' | 'POST', url: string) { /* ... */ }
request(config.method, config.endpoint)  // works because method is 'GET', not string
```

## `satisfies` operator

Type-check a value without widening its type.

```typescript
// ❌ Type annotation widens the type
const colors: Record<string, string> = { primary: '#000', secondary: '#fff' }
colors.primary  // string — lost the literal key information

// ✅ satisfies — validates shape but keeps literal types
const colors = {
  primary: '#000',
  secondary: '#fff'
} satisfies Record<string, string>

colors.primary  // '#000' — literal type preserved
colors.missing  // ❌ TypeScript error — key doesn't exist
```

## String literal unions over enums

```typescript
// ❌ Enum — generates runtime code, harder to tree-shake
enum Status { Active = 'active', Inactive = 'inactive' }

// ✅ String literal union — zero runtime cost
type Status = 'active' | 'inactive'
```

## Explicit function return types for public APIs

```typescript
// ❌ Inferred return type — may accidentally change
export function getUser(id: string) {
  return db.query(`SELECT * FROM users WHERE id = $1`, [id])
}

// ✅ Explicit return type — contract is stable
export function getUser(id: string): Promise<User | null> {
  return db.query(`SELECT * FROM users WHERE id = $1`, [id])
}
```

For internal helper functions, inferred types are acceptable.

## Reviewer Questions

- Is `any` used anywhere? Could it be replaced with a proper type or `unknown`?
- Are there type assertions (`as X`) that could be replaced with narrowing?
- Are types imported with `import type`?
- Are union types using discriminants for safe narrowing?
- Could `satisfies` or `as const` improve type safety without losing inference?
- Are enums used where string literal unions would work?
