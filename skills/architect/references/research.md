# Research

Scan a codebase and produce (or update) a structured context map that all phases of the architect pipeline consume.

In the architect pipeline, research is **not a phase** — it's a tool invoked on demand whenever context is missing.

## When to Invoke

1. **After user confirms initial idea is complete** — mandatory first research
2. **When new requirements mention things not in context-map** (types, APIs, components, DTOs) — research before proceeding
3. **When design references unknown code** — research before documenting it
4. **Never assume** — if something is not in `context-map.md`, discover it first

**When NOT to invoke:** Everything referenced is already in `context-map.md` and nothing new was introduced.

## Input

- A codebase or project path (provided by user or detected from workspace)
- The current `context-map.md` if it already exists (to know what's already documented)
- Check `references/context-sources.md` for known project documentation files (`docs/TechStack.md`, `docs/ProjectStructure.md`). If they exist and are fresh, use them to pre-populate tech stack and structure findings — then focus scanning on patterns, conventions, rules, and health notes.

## Output

- `context-map.md` saved to (or updated in) the artifact folder (see `references/formats.md` for template)

## Context Map Scope

The architect pipeline's context map is **feature-focused** — it only contains context relevant to the feature being planned, not a full project scan. It should contain:

- Existing APIs and their signatures (relevant to the feature)
- Relevant DTOs and their fields
- Existing components that will be reused or modified
- Patterns and conventions to follow
- Things that are missing (untyped fields, missing endpoints, etc.)
- Prior art (similar existing features)

On **first research** (after idea is confirmed), the scope is broader — scan for everything the idea touches. On **subsequent research** (triggered by new requirements or references), the scope is narrow — only scan for the new things mentioned.

## The Process

### Step 1: Determine What to Scan

**First research (after idea confirmed):**
Scan for everything mentioned or implied by the idea:
- APIs, DTOs, types mentioned in the idea
- Existing implementations of similar features
- Helpers, utilities, and conventions that relate to the feature
- Components that will be reused or modified
- Store modules involved

**Subsequent research (triggered by new context needs):**
Scan only for the specific things that are missing:
- A new DTO mentioned by the user
- An API the user referenced
- A component that needs to be integrated
- A type or pattern not yet in context-map

### Step 2: Scan Project Structure

Use `Glob` to find relevant files. Identify:

- Source code directories and their organization
- Test directories and testing approach
- Configuration files (package.json, tsconfig, vite.config, etc.)
- Documentation directories
- Build and deployment configuration

On subsequent research, skip this step if structure is already documented.

### Step 3: Identify Tech Stack

Read key configuration files to determine:

- Languages and versions
- Frameworks and libraries
- Database technology
- Build tools and dev server
- Testing frameworks
- CI/CD setup

On subsequent research, skip this step if tech stack is already documented.

### Step 4: Discover Patterns and Conventions

Use `Grep` and `Read` to examine representative files:

- Read 2-3 component/module files to identify structural patterns
- Read 1-2 test files to understand testing approach
- Read API/route files to understand backend patterns
- Look for state management patterns
- Check naming conventions (files, functions, variables, CSS classes)
- Check error handling patterns

On subsequent research, focus only on patterns relevant to the new context needed.

### Step 5: Find Rules Files

Search for existing rules and convention files:

- `CLAUDE.md`, `.cursorrules`, `.cursor/rules/*.md`
- `AGENTS.md`, `.windsurfrules`
- `.editorconfig`, `.eslintrc`, `.prettierrc`
- `CONTRIBUTING.md`, style guides

Summarize key contents from any found.

On subsequent research, skip this step if rules are already documented.

### Step 6: Note Constraints and Health

Identify:

- Technical constraints (browser support, API compatibility, etc.)
- Large or complex files that may need attention
- Missing test coverage areas
- Unclear boundaries between modules
- Any existing features or patterns relevant to upcoming work

### Step 7: Write or Update Context Map

**First research:** Write `context-map.md` to the artifact folder using the template from `references/formats.md`. Include all findings from Steps 2-6.

**Subsequent research:** Read existing `context-map.md`, add new findings to the appropriate sections, and save. Do NOT overwrite existing content — append to it.

The context map is a **living document**. It grows throughout the pipeline.

Announce what was found:

> "Research complete. Added to context-map: [brief summary of new findings]."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I already know this codebase" | Context maps surface patterns you take for granted. Write it down for the phases that follow. |
| "This is a small project, I don't need a map" | Small projects still have conventions and constraints. A short context map is fine. |
| "I'll just read files as I go" | Downstream phases need structured context upfront. A context map prevents repeated scanning. |
| "The README has everything" | READMEs describe intent. Context maps describe reality — what's actually in the code. |
| "I can assume this API works like the others" | Never assume. Every assumption about unknown code is a potential bug in the design. Research it. |

## Red Flags

- Skipping the scan and writing a context map from memory
- Not reading actual source files (only config files)
- Missing the tech stack or key patterns
- Not checking for existing rules files
- Context map that doesn't mention any specific file paths
- Overwriting existing context-map content on subsequent research (append, don't replace)
- Assuming an API signature without reading the source
- Not updating context-map after discovering new information

## Verification

After each research run, confirm:

- [ ] Artifact folder exists and path is known
- [ ] All things referenced by the idea/requirement are documented
- [ ] APIs documented with actual signatures (not assumed)
- [ ] DTOs documented with actual fields (not assumed)
- [ ] At least 3 patterns/conventions identified from actual code (first research)
- [ ] Rules files searched for and documented, even if none found (first research)
- [ ] Constraints and health notes included (first research)
- [ ] `context-map.md` saved to artifact folder
- [ ] Existing content preserved on updates (subsequent research)
