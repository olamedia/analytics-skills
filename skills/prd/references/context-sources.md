# Context Sources

Known project documentation files that any skill can read for pre-existing context. Check for these before scanning the codebase from scratch — if they exist and are up to date, use them.

Not all files will exist in every project. Missing files are fine — just skip them.

## Project Documentation

| File | Description | Produced by |
|------|-------------|-------------|
| `docs/TechStack.md` | Tech stack with major versions — languages, frameworks, databases, tools | `analyze-project` |
| `docs/ProjectStructure.md` | Operational project map — directory layout, services, dev/prod/lint/test tooling | `analyze-project` |
| `docs/DatabaseStructure.md` | Database tables, fields, migrations, purpose of each table | `analyze-database` |

## Per-service Documentation (monorepos)

In monorepos, each service may have its own docs:

| File | Description | Produced by |
|------|-------------|-------------|
| `[service-path]/docs/TechStack.md` | Service-specific tech stack | `analyze-project` |
| `[service-path]/docs/ProjectStructure.md` | Service-specific structure and tooling | `analyze-project` |
| `[service-path]/docs/DatabaseStructure.md` | Service-specific database structure | `analyze-database` |

## How to Use

1. At the start of any skill or task, check if these files exist
2. If a file exists, read it — the contents bootstrap your context
3. Check the `commit` field in YAML frontmatter against current `HEAD` to assess freshness
4. If files are missing or stale, proceed with normal scanning — but note that running `analyze-project` or `analyze-database` first would save time on future runs
