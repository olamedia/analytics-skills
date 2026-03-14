# Analytics Skills

A collection of AI agent skills for project analysis and feature planning — works with any IDE that supports the Agent Skills specification.

**Standalone skills** produce living project documentation. **Pipeline skills** take a raw idea from research through to an interlinked task list.

## The Pipeline

```
research → goal-definition → brainstorming → prd → architecture → breakdown
```

Each skill produces a markdown artifact that the next skill consumes. Human review gates between each phase.

| Step | Skill | Artifact | What It Does |
|------|-------|----------|--------------|
| 0 | research | `context-map.md` | Scan codebase: tech stack, patterns, conventions, constraints |
| 1 | goal-definition | `goal-definition.md` | Reframe idea as goal with success criteria and assumptions |
| 2 | brainstorming | `brainstorming.md` | Explore 2-3 approaches, evaluate trade-offs, pick direction |
| 3 | prd | `prd.md` | User stories, acceptance criteria, functional requirements, non-goals |
| 4 | architecture | `architecture.md` | Components, data flow, dependency graph, tech decisions, risks |
| 5 | breakdown | `tasks.md` | Ordered, sized, interlinked development tasks with verification |

The **analyze** orchestrator chains all 6 steps in one session for straightforward problems.

## Quick Start

### 1. Install skills

```bash
npx skills add olamedia/analytics-skills
```

This detects your IDE and copies the skills into the right location.

### 2. Use a skill

Generate living project documentation:

```
Use the analyze-project skill to document this project.
```

Scan codebase for a feature analysis:

```
Use the research skill to scan this project.
```

Run the full planning pipeline:

```
Use the analyze skill to plan this feature: [describe your idea]
```

## Alternative Installation

### Manual clone

```bash
git clone https://github.com/olamedia/analytics-skills.git ~/.analytics-skills
```

Then symlink or copy to your IDE's skill directory:

| IDE | Target |
|-----|--------|
| Cursor | `.cursor/skills/` |
| Claude Code | `.claude/skills/` or use `claude --plugin-dir ~/.analytics-skills` |
| Codex | `~/.agents/skills/` |
| Windsurf | `.windsurf/skills/` |
| OpenCode | `~/.config/opencode/skills/` |

## Commands

The `commands/` directory contains command files — one per skill. These are short markdown files that instruct the agent to load and follow the corresponding skill.

| Command | Invokes Skill |
|---------|---------------|
| `analyze-project.md` | analyze-project |
| `research.md` | research |
| `goal.md` | goal-definition |
| `brainstorm.md` | brainstorming |
| `prd.md` | prd |
| `architecture.md` | architecture |
| `breakdown.md` | breakdown |
| `all.md` | analyze (orchestrator) |

To use commands, copy them to your IDE's command directory:

**Claude Code:**
```bash
mkdir -p .claude/commands
cp ~/.analytics-skills/commands/*.md .claude/commands/
```

**Cursor:**
```bash
mkdir -p .cursor/rules
cp ~/.analytics-skills/commands/*.md .cursor/rules/
```

Other IDEs: consult your IDE's documentation for command/rule file placement.

## Skills Reference

### Standalone Skills

### analyze-project
Produce or update living project documentation (`docs/TechStack.md`, `docs/ProjectStructure.md`). Use at the start of any project to bootstrap context for AI agent sessions. Tracks staleness via git commit hash and self-updates on re-run. Monorepo-aware — produces per-service docs.

### Pipeline Skills

### research
Scan a codebase and produce a structured context map. Use when starting analysis on a codebase, before any planning or design work, or when you need to understand an existing project's structure, patterns, and constraints.

### goal-definition
Take a raw idea and turn it into a clear goal. Use when you have a raw idea or request and need to define a clear goal with success criteria before exploring solutions.

### brainstorming
Explore approaches and pick a direction. Use when you have a defined goal and need to explore different approaches before committing to a direction.

### prd
Formalize requirements into a Product Requirements Document. Use when you have a chosen direction and need user stories, acceptance criteria, and scope boundaries written down before architecture or implementation.

### architecture
Design the technical approach. Use when you have a PRD and need to design components, data flow, dependencies, and tech decisions before breaking work into tasks.

### breakdown
Slice work into development tasks. Use when you have a PRD and architecture document and need ordered, interlinked development tasks with acceptance criteria and verification steps.

### analyze (orchestrator)
Run the full pipeline in one session. Use when you have a raw idea and want to go from research through to an interlinked task list automatically.

## Shared References

Each skill includes its own copy of reference files in `references/`:

- `formats.md` — artifact templates for the skill's outputs
- `context-sources.md` — registry of known project documentation files that any skill can read for pre-existing context

This ensures every skill is self-contained and works correctly whether installed individually or as a full set.

## License

MIT
