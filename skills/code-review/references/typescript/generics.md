# TypeScript Generics Rules

## Use generic constraints with `extends`

```typescript
// ❌ No constraint — cannot access properties
function getProperty<T>(obj: T, key: string) {
  return obj[key]  // error: cannot index
}

// ✅ Constrained with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]
}

const user = { name: 'Alice', age: 30 }
getProperty(user, 'name')  // return type: string
getProperty(user, 'foo')   // ❌ TypeScript error
```

## Generic defaults

Provide defaults so callers don't always need to specify the type parameter.

```typescript
// ✅ Default generic parameter
interface ApiResponse<T = unknown> {
  data: T
  status: number
  message: string
}

const raw: ApiResponse = { data: null, status: 200, message: 'OK' }
const typed: ApiResponse<User> = { data: user, status: 200, message: 'OK' }
```

## keyof patterns

```typescript
// ✅ Type-safe property access
function pluck<T, K extends keyof T>(items: T[], key: K): T[K][] {
  return items.map(item => item[key])
}

const names = pluck(users, 'name')  // string[]
const ages = pluck(users, 'age')    // number[]
```

## Avoid over-generics (KISS)

```typescript
// ❌ Over-engineered — generic adds no value
function identity<T>(value: T): T {
  return value
}
const x = identity(5)  // just assign: const x = 5

// ❌ Generic when a concrete type is known
function formatUser<T extends { name: string }>(user: T): string {
  return user.name.toUpperCase()
}

// ✅ Use a concrete type when the generic doesn't add flexibility
function formatUser(user: { name: string }): string {
  return user.name.toUpperCase()
}
```

Use generics when:
- The function operates on multiple types and the caller benefits from type inference
- The return type depends on the input type
- You need to preserve type relationships between parameters

Don't use generics when:
- The type parameter appears only once (no relationship to preserve)
- A concrete type or union covers all use cases

## Common utility type patterns

```typescript
// ✅ Making a type partially updatable
function updateUser(id: string, changes: Partial<User>): Promise<User> {
  return api.patch(`/users/${id}`, changes)
}

// ✅ Extracting a subset for a form
type UserFormData = Pick<User, 'name' | 'email' | 'phone'>

// ✅ Creating a type without internal fields
type CreateUserDto = Omit<User, 'id' | 'createdAt'>

// ✅ Lookup maps
type UserCache = Record<string, User>
```

## Reviewer Questions

- Do generic type parameters have appropriate `extends` constraints?
- Do generic interfaces provide sensible defaults?
- Is any generic used where a concrete type would be simpler (over-engineering)?
- Does the generic type parameter appear in at least two positions (input ↔ output relationship)?
