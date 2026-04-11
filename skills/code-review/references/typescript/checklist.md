# TypeScript Review Checklist

Quick-reference checklist aggregating all TypeScript review points. Use for a final pass.

## Type Safety
- [ ] No `any` — use `unknown` with type guards or proper types
- [ ] No unsafe type assertions (`as X`) — prefer narrowing
- [ ] Types imported with `import type` (or inline `type` keyword)
- [ ] Discriminated unions used for variant types (not optional fields)
- [ ] Utility types used where appropriate (Partial, Pick, Omit, Record)
- [ ] `as const` used for constant literal values
- [ ] `satisfies` used to validate shape without widening type
- [ ] String literal unions preferred over enums
- [ ] Public API functions have explicit return types

## Generics
- [ ] Generic parameters have `extends` constraints where needed
- [ ] Generic interfaces provide sensible default type parameters
- [ ] No over-engineering — generic is justified (appears in 2+ positions)
- [ ] Built-in utility types used instead of custom reimplementations

## Async
- [ ] All async functions have error handling (try/catch or caller-level)
- [ ] No floating promises (every promise awaited, caught, or voided)
- [ ] `Promise.allSettled` used when partial results are acceptable
- [ ] Race conditions handled with AbortController
- [ ] No `async` callback in `forEach` (use `for...of` or `Promise.all`)

## Immutability
- [ ] Function parameters not mutated — new objects/arrays created
- [ ] `readonly` modifier on parameters that should not be changed
- [ ] `as const` used for constant data structures
- [ ] Sort/reverse/splice not used on original arrays (spread first)

## Testing
- [ ] Mocks properly typed (no `as any` in test setup)
- [ ] `@ts-expect-error` used instead of `@ts-ignore`
- [ ] Test factories return correctly typed objects
- [ ] `expectTypeOf` used for key type contracts

## Security
- [ ] `JSON.parse` output validated before use (not just asserted)
- [ ] No `eval` or `new Function`
- [ ] User input encoded when interpolated into URLs/strings
- [ ] Type assertions not used to skip validation on untrusted data
