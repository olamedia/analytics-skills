---
name: frontend-design
description: >-
  Guide frontend development toward websites that feel premium and trustworthy,
  grounded in cognitive science research (processing fluency, prototypicality).
  Two modes: active guidance during build, and audit (problems + suggestions).
  Use when building UI, reviewing frontend code, or when the user mentions
  premium, trust, design quality, audit, review, or frontend polish.
---

# Frontend Design

Make websites feel premium and trustworthy — based on peer-reviewed research, not aesthetic opinions.

## Core Principles

The brain uses processing fluency as a shortcut: "easy to process = probably true/safe/good" (Reber & Schwarz 1999). Users form stable judgments within 50ms (Tuch et al. 2012). Two factors drive that judgment:

1. **Low visual complexity** — fewer competing elements, clear hierarchy
2. **High prototypicality** — looks like what the user expects for this category of site

This triggers a halo chain (Tractinsky 2000):

**Readable + Clean + Predictable → fluent → "beautiful" → perceived usable → perceived trustworthy**

Four measurable properties of "premium feel":

| Property | Meaning |
|---|---|
| **Readability** | Text and structure effortless to parse |
| **Smoothness** | Nothing breaks the user's flow; animations are flawless or absent |
| **Predictability** | Site behaves like the user expects for its category |
| **Cleanliness** | No competing elements, clear visual hierarchy |

Trust is also built by factors beyond visual design (Stanford, NNGroup, Baymard): real organization proof, easy contact, upfront pricing, zero errors, external reviews, no hidden costs. Visual "premium-ness" overdone reduces trust by looking like a scam trying too hard.

**General rule:** All details, even the smallest, should be cared for. Visual clarity is one part of it.

## Mode Selection

When this skill is invoked, determine the operating mode:

- If the user is **building or writing frontend code** → Active Guidance
- If the user is **reviewing, auditing, or checking existing code** → Audit
- If **ambiguous** → ask:

> "Should I apply design guidance while we build, or audit existing code for problems and suggestions?"

## Active Guidance Mode

Apply research-backed design practices while generating frontend code. The goal is correct output, not commentary.

### Process

1. Read [practices.md](references/practices.md) for the category index, then read the relevant category guides for the current task
2. While generating code, silently apply the relevant practices:
   - Accessibility: contrast, focus states, ARIA, heading hierarchy
   - Typography: line length, font size, hierarchy, fluid scaling
   - Spacing: consistent scale, container padding
   - Color: semantic roles, predictable usage, primary reserved for primary actions
   - Motion: CSS-first, 150-300ms micro, 400ms max complex, `prefers-reduced-motion`
   - Performance: lazy loading, code splitting, animate only transform/opacity
   - States: skeleton loaders, error messages with cause + fix, empty states with action
   - Responsive: mobile-first, touch targets >= 44px, no horizontal scroll
   - Forms: visible labels, validate on blur, progressive disclosure
3. If you need a specific implementation pattern → read [patterns.md](references/patterns.md)
4. If the user's request conflicts with a research-backed practice → mention the conflict briefly, follow the user's instruction
5. If the user's request matches a known opinionated claim → check [warnings.md](references/warnings.md), note it's opinion not research, follow the user's instruction
6. Do not lecture. Produce correct code.

### What NOT to do in active guidance

- Do not explain every design decision unless asked
- Do not refuse user requests based on the checklist — note conflicts, then comply
- Do not prescribe specific fonts, colors, or hex values — those are project-specific
- Do not assume dark or light mode — match the project's existing theme

## Audit Mode

Scan existing frontend code for problems, then provide improvement suggestions. The fewer problems found, the more focus shifts to suggestions.

### Process

1. Read [practices.md](references/practices.md) for the category index, then read the relevant category guides
2. Read [report-format.md](references/report-format.md) for the output template
3. **Phase 1 — Checklist Pass:** Scan the code against the checklist in each relevant category guide
   - For each violation: note file location, what's wrong, which practice it violates
   - Classify severity: critical (harms trust/accessibility/usability), warning (reduces quality), note (minor)
4. **Phase 2 — Suggestions Pass:** Apply the core principles above to find improvement opportunities
   - Look for areas where readability, smoothness, predictability, or cleanliness could improve
   - Check trust signals (§14 in practices.md) — are organizational proof, contact info, upfront pricing present?
   - If few problems were found in Phase 1, expand this section
5. Check [warnings.md](references/warnings.md) — flag any opinionated patterns found in the code
6. Produce the report following the template from report-format.md
7. If the user asked only for problems or only for suggestions, respect that

## Quick Reference

The most critical items across all categories. Full checklists in [practices.md](references/practices.md).

### Accessibility (WCAG)
- Contrast: >= 4.5:1 body, >= 3:1 large text
- Focus: visible `focus-visible` rings on all interactive elements
- Motion: `prefers-reduced-motion` disables non-essential animation
- Headings: sequential h1 → h2 → h3, no level skip
- Icons: `aria-label` on every icon-only button

### Typography (Processing Fluency)
- Line length: 55-75 chars desktop, 35-45 mobile
- Min body size: 16px mobile
- Line height: 1.5-1.75
- Max 2 font families + monospace for data
- Fluid scale via `clamp()`

### Color (Predictability)
- Every color has a semantic role (primary, accent, semantic, neutral, surface)
- Primary reserved for primary actions only
- Active states expressed via border/shadow/weight — NOT primary color background
- Color matches industry expectations

### Motion (Smoothness)
- 150-300ms micro, 400ms max complex
- CSS-first, JS only when needed
- Only animate `transform` and `opacity`
- Janky > remove it. Only ship what runs flawlessly.

### Performance (Trust)
- Lazy load below-fold, code split by route
- `font-display: swap`, preload critical fonts only
- Reserve space for async content (prevent CLS)
- JS budget: < 500KB desktop, < 150KB mobile

### States (Care for Details)
- Skeleton loaders for > 300ms waits
- Error messages: cause + how to fix
- Empty states: helpful message + action

### Trust Signals (Research)
- Real org proof: address, team bios, photos
- Easy contact: phone, email, address prominent
- Upfront pricing: no hidden costs, no login walls
- External validation: third-party reviews, press
- Zero errors: typos and broken links destroy credibility
- Max 5 trust badges — more creates suspicion

## Reference Files

| File | When to read |
|---|---|
| [practices.md](references/practices.md) | Both modes — index of 14 categories, each linking to a detailed guide with checklist + guidance + common mistakes |
| [report-format.md](references/report-format.md) | Audit mode — output template with severity tiers |
| [patterns.md](references/patterns.md) | When you need a specific code implementation |
| [warnings.md](references/warnings.md) | When checking if design advice is opinion vs research |

## Red Flags

Stop and reconsider if you find yourself:

- Prescribing specific fonts or colors as universally "premium"
- Recommending dark mode as inherently more trustworthy
- Breaking prototypicality (category-standard layouts) for "originality"
- Adding animations that haven't been tested for smoothness
- Using primary color for active states on non-primary elements
- Ignoring trust signals (§14) while perfecting visual details
- Lecturing the user about design theory instead of producing code

## Verification

After completing work in either mode:

- [ ] No opinionated claims presented as fact
- [ ] Accessibility basics covered (contrast, focus, ARIA, motion)
- [ ] Typography readable (line length, size, hierarchy)
- [ ] Color usage predictable (semantic roles, primary reserved for actions)
- [ ] Motion is flawless or absent — nothing janky
- [ ] Performance considered (lazy loading, code splitting, CLS prevention)
- [ ] Every state handled (loading, error, empty)
- [ ] Trust signals addressed or noted as missing
