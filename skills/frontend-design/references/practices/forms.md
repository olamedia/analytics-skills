# Forms

Upfront disclosure is an NNGroup trust factor. Form friction = processing disfluency = distrust.

## Checklist

| Practice | Standard |
|----------|----------|
| Visible label per input (not placeholder-only) | WCAG 1.3.1, NNGroup |
| Error message below related field (inline, not toast) | NNGroup, WCAG 3.3.1 |
| Required fields marked | WCAG 3.3.2 |
| Validate on blur, not on keystroke | NNGroup |
| Semantic input types (email, tel, number) | WCAG, Apple HIG |
| Password show/hide toggle | NNGroup, WCAG |
| Autofill/autocomplete attributes | NNGroup, WCAG 1.3.5 |
| Progressive disclosure (don't overwhelm upfront) | NNGroup |
| Multi-step: progress indicator | NNGroup |
| Success feedback after actions | NNGroup |
| Confirm before dismissing unsaved changes | NNGroup |

---

## Placeholder-Only Labels: Why They Fail

**Disappear on focus:** When the user focuses the field, the placeholder vanishes. Labels must persist so users know what the field is for after typing starts.

**Accessibility:** Placeholders often fail contrast requirements (WCAG 1.4.3). Gray-on-white text rarely meets 4.5:1. Screen readers may not expose placeholders as labels; `placeholder` is not a substitute for `<label>`.

**Guidance:** Use a visible label (above or beside the input) for every field. Placeholders may supplement with hints (e.g., “e.g., john@example.com”) but must not be the sole source of meaning.

---

## Validation Timing: Why Blur Beats Keystroke

Validating on every keystroke interrupts users before they finish. Mid-word validation (e.g., “Invalid email” while typing “john@”) causes frustration and perceived hostility. Processing fluency research links repeated interruptions to distrust and abandonment.

**Guidance:** Validate on blur (when the user leaves the field) or on submit. Optionally show inline success after valid blur. Avoid live validation until the user has had a chance to complete input.

---

## Progressive Disclosure: Breaking Long Forms Into Steps

Long forms increase cognitive load and abandonment. NNGroup recommends progressive disclosure: reveal complexity as needed.

**Guidance:**
- Group related fields into logical sections (e.g., “Contact,” “Shipping,” “Payment”).
- Use multi-step wizards for 7+ fields; show 3–5 per step.
- Provide a visible progress indicator (steps or percentage).
- Allow moving back without losing data.
- Consider accordions or collapsible sections for optional blocks.

---

## Autocomplete Attributes: Which Values to Use

`autocomplete` improves accuracy, speed, and accessibility. Use the correct tokens for each field.

**Common values:**
- `name`, `given-name`, `family-name`
- `email`
- `tel`, `tel-country-code`, `tel-national`
- `street-address`, `address-line1`, `address-line2`, `address-level1`, `address-level2`, `postal-code`, `country`
- `cc-number`, `cc-exp`, `cc-csc`
- `username`, `current-password`, `new-password`

**Guidance:** Add `autocomplete` to every appropriate input. WCAG 1.3.5 (Identify Input Purpose) recommends this for input fields that collect personal data.

---

## Semantic Input Types: Mobile Keyboard Benefits

`type="email"` shows an email keyboard (@, .com). `type="tel"` shows a numeric keypad. `type="number"` shows a number pad. `type="url"` shows a URL-friendly layout.

**Guidance:** Use semantic types for all form inputs. This reduces typos, speeds input, and improves accessibility. Match the type to the data (e.g., `type="tel"` for phone, not `type="text"`).

---

## Common Mistakes

| Mistake | Why it’s bad |
|---------|--------------|
| Placeholder as only label | Fails accessibility; disappears on focus; often fails contrast |
| Errors in toasts or at top | User must map error to field; breaks flow |
| Required fields not marked | WCAG violation; users surprised on submit |
| Validation on keystroke | Interrupts before user finishes; feels hostile |
| `type="text"` for email/tel/number | Wrong keyboard on mobile; more errors |
| No password visibility toggle | Users can’t verify input; friction |
| Missing autocomplete | Slower input; missed WCAG 1.3.5 |
| Single long form | Overwhelming; higher abandonment |
| Multi-step without progress | Users don’t know how far along they are |
| No success feedback | User unsure if submit worked |
| Dismissing with unsaved changes, no confirm | Data loss; frustration |
