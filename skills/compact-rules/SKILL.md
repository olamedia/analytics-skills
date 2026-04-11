---
name: compact-rules
description: >-
  Meta-skill for writing compact skill definitions. Reduces skill file size without
  losing instructions. Use when creating or refactoring SKILL.md files, or when
  another skill asks for compact/terse/shorter rule definitions.
---

# Compact Rules

How to make skill definitions shorter without cutting instructions.

## What to cut

- **Doubling down** — saying the same thing twice in different words
- **Extensive explanations** — if the definition is clear, it doesn't need a paragraph explaining why
- **Standalone examples** — examples separated from definitions they illustrate; fold them into the definition instead

## What to keep

- Every instruction — no rule gets dropped for brevity
- Every distinct constraint or boundary

## How

Definitions must be self-explanatory. If a definition needs a separate example section to be understood, rewrite the definition to include the example inline.

Not:
```
## Rule
Drop filler words.

## Examples
Not: "I just really think..."
Yes: "I think..."
```

Yes:
```
## Rule
Drop filler (just/really/basically) — "I think..." not "I just really think..."
```
