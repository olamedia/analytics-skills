# General Frontend Security Rules

Universal frontend security concepts. Vue-specific rules (v-html, :is) live in `vue/security.md`. TypeScript-specific rules (JSON.parse, eval) live in `typescript/security.md`.

## XSS Prevention

Never render unsanitized user input as HTML.

```typescript
// ❌ innerHTML with user input
element.innerHTML = userInput  // XSS vector

// ✅ textContent for plain text
element.textContent = userInput  // safe, no HTML parsing

// ✅ DOMPurify for HTML content
import DOMPurify from 'dompurify'
element.innerHTML = DOMPurify.sanitize(userInput)
```

Watch for less obvious vectors:
- URL parameters rendered into the page
- `href` attributes with `javascript:` protocol
- SVG content with embedded scripts
- CSS `url()` values from user input

```typescript
// ❌ User-controlled href — javascript: protocol
<a :href="userUrl">Click</a>

// ✅ Validate protocol
const safeUrl = computed(() => {
  try {
    const url = new URL(props.userUrl)
    return ['http:', 'https:'].includes(url.protocol) ? props.userUrl : '#'
  } catch {
    return '#'
  }
})
```

## Input Validation

Client-side validation is for UX, not security. Never trust it as a security boundary.

```typescript
// ❌ Client-only validation — easily bypassed
function submitForm(data: FormData) {
  if (data.email.includes('@')) {
    api.post('/users', data)  // server must validate too
  }
}
```

Validate shapes and types on data from external sources (APIs, localStorage, URL params):

```typescript
// ❌ Trusting localStorage data
const prefs = JSON.parse(localStorage.getItem('prefs')!) as UserPrefs

// ✅ Validate after parsing
const raw = localStorage.getItem('prefs')
if (raw) {
  const parsed: unknown = JSON.parse(raw)
  if (isValidPrefs(parsed)) {
    applyPrefs(parsed)
  }
}
```

## Sensitive Data

- **No secrets in code** — API keys, tokens, and passwords must not appear in source code or commit history
- **No PII in logs** — never log emails, passwords, phone numbers, or user data
- **No tokens in URLs** — tokens in query strings appear in browser history, server logs, and referrer headers

```typescript
// ❌ Token in URL
fetch(`/api/data?token=${authToken}`)

// ✅ Token in header
fetch('/api/data', {
  headers: { Authorization: `Bearer ${authToken}` }
})
```

- **No secrets in client bundles** — environment variables prefixed with `VITE_` are embedded in the build output and visible to users

```typescript
// ❌ Secret key exposed in client bundle
const apiSecret = import.meta.env.VITE_API_SECRET  // visible to anyone

// ✅ Secret stays on server — client calls a proxy endpoint
const data = await fetch('/api/proxy/external-service')
```

## Dependency Security

- Run `npm audit` regularly
- Keep lock files committed (`package-lock.json` / `pnpm-lock.yaml`)
- Minimize dependencies — each one is an attack surface
- Pin dependency versions or use lock files to prevent supply chain attacks
- Review new dependencies before adding them (check maintainers, download count, last publish date)

## CORS and CSP Awareness

Review CORS and CSP headers during code review when they are modified:

- **CORS:** `origin: '*'` should not be used in production with credentials
- **CSP:** `unsafe-inline` and `unsafe-eval` in script-src weaken protection — justify if present
- **CSP:** Report-only mode should be used before enforcing new policies

These are typically server/infrastructure configs, but frontend code can influence them (e.g. inline scripts, dynamic style injection).

## Security Checklist

- [ ] No unsanitized user input rendered as HTML
- [ ] `href` attributes with user input validate the URL protocol
- [ ] Client-side validation is supplemented by server-side validation
- [ ] External data (APIs, localStorage, URL params) is validated after parsing
- [ ] No secrets, API keys, or tokens in source code
- [ ] No PII in logs or console output
- [ ] Auth tokens sent in headers, not URL query strings
- [ ] No secrets in `VITE_`-prefixed environment variables
- [ ] `npm audit` passes without critical/high vulnerabilities
- [ ] Lock files are committed
- [ ] New dependencies reviewed before adding
- [ ] CORS and CSP headers reviewed when modified
