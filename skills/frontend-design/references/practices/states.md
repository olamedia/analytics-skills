# Loading, Error, and Empty States

Handling every state signals "someone cared about details" which triggers the halo effect (Tractinsky 2000). A component that only handles the happy path feels unfinished. A component that gracefully handles loading, errors, and empty data feels premium.

## Checklist

| Practice | Standard |
|---|---|
| Skeleton loaders for > 300ms waits | Not spinners |
| Error messages: cause + fix | Not "Invalid input" |
| Empty states: message + action | Guide the user |
| Error boundaries for WebGL/3D | Graceful fallback |
| Auto-dismiss toasts | 3-5 seconds |
| Confirm before destructive actions | Dialog or undo |
| Auto-focus first invalid field on error | After submit |
| Long forms auto-save drafts | Prevent data loss |
| Timeout: clear feedback + retry | Not silent failure |

---

## Loading States: Skeleton vs. Spinner vs. Progress Bar

| Pattern | When to use | Duration range |
|---|---|---|
| **No indicator** | Response is instant (< 100ms) | 0-100ms |
| **Subtle delay** (disable button, change cursor) | Brief wait | 100-300ms |
| **Skeleton loader** | Content loading, layout is predictable | 300ms - 5s |
| **Spinner** | Action in progress, layout is unknown | 300ms - 5s |
| **Progress bar** | Long operation where progress is measurable | 5s+ |
| **Background indicator** (favicon, tab title) | Very long operations | 30s+ |

### Skeleton loaders

Skeletons are rectangular placeholders that match the shape and position of the content that will load. They signal "content is coming" and prevent layout shift.

**Rules:**
- Match the shape of the real content — use rectangles for text lines, circles for avatars, aspect-ratio boxes for images
- Use a shimmer animation (left-to-right gradient sweep) to signal activity
- Show the skeleton for at least 300ms even if data arrives faster — prevents a flash
- Remove the skeleton and render content in place without layout shift

**When NOT to use skeletons:**
- You don't know what the content layout will be (use a spinner)
- The content is a single small element (use a subtle opacity transition)
- The wait is under 300ms (show nothing)

### Spinners

Use spinners when:
- The layout of the result is unpredictable
- The operation is an action (submitting, processing) not a content load
- The spinner is small and localized (inside a button, next to a field)

**Avoid full-page spinners** — they block all interaction and signal slow software. If the page is loading, use a skeleton for the page layout instead.

---

## Error States

Every error message needs two things: **what went wrong** and **how to fix it**.

### Error message anatomy

| Component | Required | Example |
|---|---|---|
| **What happened** | Yes | "Your card was declined" |
| **Why** (if known) | If available | "The expiration date doesn't match" |
| **What to do** | Yes | "Check your card details and try again" |
| **Recovery action** | If applicable | [Try Again] button, link to support |

### Bad vs. good error messages

| Bad | Good |
|---|---|
| "Error" | "We couldn't save your changes" |
| "Invalid input" | "Email must include @ and a domain (e.g. name@example.com)" |
| "Something went wrong" | "The server didn't respond. Check your connection and try again." |
| "403" | "You don't have permission to view this page. Sign in or contact support." |
| "Validation failed" | "The password needs at least 8 characters with one number" |

### Error placement

- **Form field errors:** Inline, directly below the field. Not in a toast, not at the top of the form.
- **Form submission errors:** At the top of the form AND auto-focus the first invalid field.
- **Page-level errors:** Replace the content area with the error + recovery action. Don't show a broken page.
- **Background operation errors:** Toast notification with retry action. Auto-dismiss in 5 seconds unless the error requires user action.

### Error boundaries

For components that might crash (WebGL, 3D, complex visualizations, third-party widgets):

- Wrap in an error boundary that shows a graceful fallback
- The fallback should explain what was supposed to be here and offer a retry
- Never let a component crash take down the entire page

---

## Empty States

An empty state is the first thing a new user sees in any list, feed, or dashboard. It's a first-impression moment.

### Empty state anatomy

| Component | Required | Example |
|---|---|---|
| **What this area is for** | Yes | "Your saved items will appear here" |
| **Why it's empty** | If not obvious | "You haven't saved any items yet" |
| **What to do next** | Yes | [Browse Products] button |
| **Visual** | Optional | Simple illustration or icon (not required, but helps) |

### Empty state types

| Type | What to show | Example |
|---|---|---|
| **First use** | Welcome + first action | "Start by creating your first project" + [Create Project] |
| **No results** (search/filter) | Feedback + suggestion | "No results for 'xyz'. Try different keywords or [clear filters]" |
| **Cleared by user** | Confirmation + next step | "All done! Your inbox is empty." |
| **Error state** | What went wrong + retry | "Couldn't load your data. [Retry]" |

### Common mistakes with empty states

- Showing nothing — blank white space with no explanation
- Showing a technical message — "No records found in database"
- No action — telling the user it's empty but not giving them a way to fill it
- Sad/negative imagery — empty shopping cart with "Nothing here :(" feels punishing

---

## Confirmation Before Destructive Actions

Any action that loses data or is hard to undo needs a confirmation step.

**When to confirm:**
- Delete (anything — messages, files, accounts)
- Discard unsaved changes (navigating away from a form)
- Overwrite existing data (import that replaces current data)
- Send irreversible communication (email blast, published post)

**Confirmation dialog anatomy:**
- Title: what will happen ("Delete this project?")
- Body: consequences ("All files and settings will be permanently removed.")
- Primary action: the destructive action, labeled specifically ("Delete Project" not "OK")
- Secondary action: cancel, visually subordinate
- Destructive action button should use warning/danger color, not primary color

**Alternative: Undo instead of confirm**
For less critical actions (archive, mark as read, move), skip the confirmation dialog and show an undo toast instead. Faster for the user, still recoverable.

---

## Toast / Notification Behavior

| Type | Auto-dismiss | Duration | User action needed |
|---|---|---|---|
| Success confirmation | Yes | 3 seconds | No |
| Info notification | Yes | 4 seconds | No |
| Warning | Yes (but longer) | 5 seconds | Optional |
| Error requiring action | No — persists | Until dismissed | Yes |

**Stacking:** If multiple toasts appear, stack them vertically. Max 3 visible at once — older ones auto-dismiss to make room.

**Placement:** Bottom-right or top-right on desktop. Bottom-center on mobile (above the navigation bar, not behind it).

---

## Auto-Save for Long Forms

For forms that take more than 2 minutes to fill (multi-step flows, long applications, content creation):

- Save draft to local storage or server at regular intervals (every 30 seconds or on blur of each field)
- Show a subtle "Draft saved" indicator (not a toast — too disruptive for continuous work)
- When the user returns, offer to restore the draft: "You have an unsaved draft from [date]. [Continue] or [Discard]"
- Clear the draft after successful submission

---

## Common Mistakes

- Using a full-page spinner for content loading — blocks all interaction, use skeletons instead
- Error messages that say "Error" or "Something went wrong" with no recovery path — useless to the user
- Empty states with no action — the user sees blank space and doesn't know what to do
- Missing confirmation on delete — user loses data, trust is destroyed instantly
- Toasts that require action but auto-dismiss — user didn't see it, error goes unnoticed
- Not auto-focusing the first invalid field after form submit error — user has to hunt for the problem
- Showing loading states for instant operations (< 100ms) — flicker of skeleton/spinner is worse than showing nothing
