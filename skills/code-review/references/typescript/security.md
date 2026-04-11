# TypeScript Security Rules

## JSON.parse returns `unknown` — must validate

```typescript
// ❌ Trusting parsed JSON without validation
const data = JSON.parse(rawJson) as UserConfig  // assertion hides invalid data

// ✅ Parse as unknown, then validate
const parsed: unknown = JSON.parse(rawJson)

function isUserConfig(val: unknown): val is UserConfig {
  return (
    typeof val === 'object' &&
    val !== null &&
    'name' in val &&
    typeof (val as Record<string, unknown>).name === 'string'
  )
}

if (!isUserConfig(parsed)) {
  throw new Error('Invalid config format')
}
// parsed is now UserConfig
```

For complex shapes, use a validation library (Zod, Valibot, etc.) instead of manual guards.

## No eval or new Function

```typescript
// ❌ eval — arbitrary code execution
const result = eval(userExpression)

// ❌ new Function — same risk
const fn = new Function('x', userExpression)

// ✅ Parse and handle explicitly
const operators: Record<string, (a: number, b: number) => number> = {
  '+': (a, b) => a + b,
  '-': (a, b) => a - b,
}
const result = operators[op]?.(a, b)
```

## User input in template literals

```typescript
// ❌ Unsanitized user input in URL construction
const url = `/api/users/${userId}/files/${fileName}`
// fileName could be "../../../etc/passwd"

// ✅ Validate and encode
const safeFileName = encodeURIComponent(fileName)
const url = `/api/users/${encodeURIComponent(userId)}/files/${safeFileName}`
```

## Type assertions hiding validation gaps

```typescript
// ❌ Assertion bypasses runtime check — data could be anything
function handleWebhook(body: unknown) {
  const event = body as WebhookEvent  // no validation!
  processEvent(event)  // may crash on unexpected shape
}

// ✅ Validate before use
function handleWebhook(body: unknown) {
  if (!isWebhookEvent(body)) {
    throw new Error('Invalid webhook payload')
  }
  processEvent(body)  // body is now WebhookEvent
}
```

## Reviewer Questions

- Is `JSON.parse` output validated before use (not just asserted)?
- Is `eval` or `new Function` used anywhere?
- Is user input properly encoded when interpolated into URLs or strings?
- Are type assertions (`as X`) used to skip validation on untrusted data?
