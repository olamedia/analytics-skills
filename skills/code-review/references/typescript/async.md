# TypeScript Async Rules

## Proper error handling in async functions

```typescript
// ❌ No error handling — unhandled rejection on network failure
async function fetchUser(id: string) {
  const response = await fetch(`/api/users/${id}`)
  return response.json()
}

// ✅ Handle errors with typed catch
async function fetchUser(id: string): Promise<User> {
  try {
    const response = await fetch(`/api/users/${id}`)
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }
    return await response.json()
  } catch (error) {
    if (error instanceof Error) {
      throw new Error(`Failed to fetch user: ${error.message}`)
    }
    throw error
  }
}
```

## No floating promises

Every promise must be awaited, caught, or explicitly voided.

```typescript
// ❌ Floating promise — error silently lost
async function save() { /* ... */ }
save()  // no await, no catch

// ✅ Await
await save()

// ✅ Or catch
save().catch(console.error)

// ✅ Or explicitly void (when fire-and-forget is intentional)
void save()
```

Enable `@typescript-eslint/no-floating-promises` to catch these automatically.

## Promise.all vs Promise.allSettled

```typescript
// ❌ Promise.all — one failure rejects everything
async function fetchAll(ids: string[]) {
  return await Promise.all(ids.map(fetchUser))  // all or nothing
}

// ✅ Promise.allSettled — get all results regardless of failures
async function fetchAll(ids: string[]) {
  const results = await Promise.allSettled(ids.map(fetchUser))

  const users: User[] = []
  const errors: Error[] = []

  for (const result of results) {
    if (result.status === 'fulfilled') {
      users.push(result.value)
    } else {
      errors.push(result.reason)
    }
  }

  return { users, errors }
}
```

Use `Promise.all` when all results are required (fail fast is desired). Use `Promise.allSettled` when partial results are acceptable.

## Race conditions — AbortController

```typescript
// ❌ Stale response overwrites fresh one
let latestQuery = ''
async function search(query: string) {
  latestQuery = query
  const res = await fetch(`/api/search?q=${query}`)
  const data = await res.json()
  if (query === latestQuery) results.value = data  // still racy
}

// ✅ AbortController cancels previous request
let controller: AbortController | null = null

async function search(query: string) {
  controller?.abort()
  controller = new AbortController()

  try {
    const res = await fetch(`/api/search?q=${query}`, {
      signal: controller.signal
    })
    results.value = await res.json()
  } catch (e) {
    if ((e as Error).name !== 'AbortError') throw e
  }
}
```

## No async forEach — use for...of or Promise.all

```typescript
// ❌ async callback in forEach — fires all at once, no await
items.forEach(async (item) => {
  await processItem(item)  // forEach ignores the returned promise
})

// ✅ Sequential processing
for (const item of items) {
  await processItem(item)
}

// ✅ Parallel processing
await Promise.all(items.map(processItem))
```

## Reviewer Questions

- Are all async functions handling errors (try/catch or caller-level catch)?
- Are there floating promises (no await, no catch, no void)?
- Is `Promise.all` used where `Promise.allSettled` would be more appropriate?
- Are sequential API calls protected against race conditions (AbortController)?
- Is `forEach` used with async callbacks?
