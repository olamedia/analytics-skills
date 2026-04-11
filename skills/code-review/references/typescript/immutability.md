# TypeScript Immutability Rules

## readonly arrays and objects

```typescript
// ❌ Mutating a function parameter
function processUsers(users: User[]) {
  users.sort((a, b) => a.name.localeCompare(b.name))  // mutates original!
  return users
}

// ✅ readonly parameter — compiler prevents mutation
function processUsers(users: readonly User[]): User[] {
  return [...users].sort((a, b) => a.name.localeCompare(b.name))
}
```

## No parameter mutation

Functions should not modify their arguments. Create new objects/arrays instead.

```typescript
// ❌ Mutating the input object
function setDefaults(config: AppConfig) {
  config.timeout = config.timeout ?? 5000  // modifies caller's object
  return config
}

// ✅ Spread to create new object
function setDefaults(config: Readonly<AppConfig>): AppConfig {
  return { ...config, timeout: config.timeout ?? 5000 }
}
```

## Readonly<T> for function parameters

```typescript
// ✅ Signal that the function won't modify the input
function renderUser(user: Readonly<User>): string {
  return `${user.name} <${user.email}>`
}
```

## DeepReadonly pattern

For nested objects where `Readonly` only protects the top level:

```typescript
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object ? DeepReadonly<T[K]> : T[K]
}

function processConfig(config: DeepReadonly<AppConfig>) {
  // config.nested.field = 'x'  // ❌ TypeScript error at any depth
}
```

## as const for constant data

```typescript
// ✅ Fully immutable literal type
const ROUTES = ['home', 'about', 'contact'] as const
// type: readonly ['home', 'about', 'contact']

const CONFIG = {
  api: { url: '/api', timeout: 5000 },
  features: { darkMode: true }
} as const
// all properties deeply readonly with literal types
```

## Reviewer Questions

- Are function parameters that shouldn't be mutated marked `readonly`?
- Are arrays/objects being mutated in-place instead of creating new copies?
- Would `DeepReadonly` or `as const` prevent accidental deep mutations?
- Are sort/reverse/splice used on arrays without spreading first?
