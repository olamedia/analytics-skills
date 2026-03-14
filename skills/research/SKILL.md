---
name: research
description: Use when starting analysis on a codebase, before any planning or design work, or when you need to understand an existing project's structure, patterns, and constraints.
---

# Research

Scan a codebase and produce a structured context map that all subsequent analytics skills consume.

## When to Use

- Starting analysis on a project for the first time
- Before running `goal-definition`, `brainstorming`, `prd`, `architecture`, or `breakdown`
- When switching to a different project or codebase
- When the codebase has changed significantly since the last context map

**When NOT to use:** The project already has a recent, accurate `context-map.md` in the artifact folder.

## Input

- A codebase or project path (provided by user or detected from workspace)
- Check `references/context-sources.md` for known project documentation files (`docs/TechStack.md`, `docs/ProjectStructure.md`). If they exist and are fresh, use them to pre-populate tech stack and structure findings — then focus scanning on patterns, conventions, rules, and health notes.

## Output

- `context-map.md` saved to the artifact folder (see `references/formats.md` for template)

## The Process

### Step 1: Determine Artifact Folder

If this is the first skill called in the pipeline, ask the user where to save artifacts:

> "Where should I save the analysis artifacts for this feature? Provide a folder path (e.g. `docs/analytics/user-auth/`), or I'll use `docs/analytics/` as default."

If the user specifies a folder in chat, use that. Create the folder if it doesn't exist.

### Step 2: Scan Project Structure

Use `Glob` to map the directory tree. Identify:

- Source code directories and their organization
- Test directories and testing approach
- Configuration files (package.json, tsconfig, vite.config, etc.)
- Documentation directories
- Build and deployment configuration

### Step 3: Identify Tech Stack

Read key configuration files to determine:

- Languages and versions
- Frameworks and libraries
- Database technology
- Build tools and dev server
- Testing frameworks
- CI/CD setup

### Step 4: Discover Patterns and Conventions

Use `Grep` and `Read` to examine representative files:

- Read 2-3 component/module files to identify structural patterns
- Read 1-2 test files to understand testing approach
- Read API/route files to understand backend patterns
- Look for state management patterns
- Check naming conventions (files, functions, variables, CSS classes)
- Check error handling patterns

### Step 5: Find Rules Files

Search for existing rules and convention files:

- `CLAUDE.md`, `.cursorrules`, `.cursor/rules/*.md`
- `AGENTS.md`, `.windsurfrules`
- `.editorconfig`, `.eslintrc`, `.prettierrc`
- `CONTRIBUTING.md`, style guides

Summarize key contents from any found.

### Step 6: Note Constraints and Health

Identify:

- Technical constraints (browser support, API compatibility, etc.)
- Large or complex files that may need attention
- Missing test coverage areas
- Unclear boundaries between modules
- Any existing features or patterns relevant to upcoming work

### Step 7: Write Context Map

Write `context-map.md` to the artifact folder using the template from `references/formats.md`. Include all findings from Steps 2-6.

Announce the saved path:

> "Context map saved to `[path]/context-map.md`."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I already know this codebase" | Context maps surface patterns you take for granted. Write it down for the skills that follow. |
| "This is a small project, I don't need a map" | Small projects still have conventions and constraints. A short context map is fine. |
| "I'll just read files as I go" | Downstream skills need structured context upfront. A context map prevents repeated scanning. |
| "The README has everything" | READMEs describe intent. Context maps describe reality — what's actually in the code. |

## Red Flags

- Skipping the scan and writing a context map from memory
- Not reading actual source files (only config files)
- Missing the tech stack or key patterns
- Not checking for existing rules files
- Context map that doesn't mention any specific file paths

## Verification

Before handing off, confirm:

- [ ] Artifact folder exists and path is known
- [ ] Tech stack identified with versions
- [ ] Project structure documented with key directories
- [ ] At least 3 patterns/conventions identified from actual code
- [ ] Rules files searched for and documented (even if none found)
- [ ] Constraints and health notes included
- [ ] `context-map.md` saved to artifact folder

## Next

> "Research complete. Next recommended skill: **goal-definition** — define what you want to achieve using this context."
