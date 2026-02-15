# System Index

This is the top-level system index. It provides an overview of system structure, skills inventory, and system-level files. This file is stable and always loaded.

## System Structure

The `system/` folder is Claude's internal workspace. It contains:

- **INDEX.md** — This file. System overview and structure.
- **CHANGELOG.md** — Append-only change log (reverse chronological). Rotate every 2 weeks.
- **memory/** — Evolving knowledge that changes session-to-session
  - **people/** — Contact profiles and team information
  - **projects/** — Deep project context, decisions, patterns
  - **context/** — Evolving context (financials, strategic data)
- **docs/** — Working documents, guides, and system documentation
- **archive/** — System-level archive of old changelogs and deprecated items

## Skills Inventory

All workspace skills are located in the top-level `Skills/` folder (NOT in system/).

### Available Skills

- **Skills/wrk-rules/** — Workspace rules, conventions, and session workflow
  - File organization, task folders, memory system, filedrop processing
  - Load this at the start of every session

## Important Files

| File | Location | Purpose |
|------|----------|---------|
| Bootstrap & initialization | `CLAUDE.md` | Where to find things, session start flow |
| Workspace rules | `Skills/wrk-rules/SKILL.md` | File conventions, memory system, workflow |
| Task list | `TASKS.md` | All active and completed tasks |
| Change log | `system/CHANGELOG.md` | Append-only log of all changes |
| Company knowledge | `company-context/` | Shared company knowledge and glossary |
| Work projects | `work/INDEX.md` | Master index of all work files and projects |
| Personal files | `personal/INDEX.md` | Personal file index |
| Dashboard | `dashboard.html` | Interactive workspace dashboard |

## Session Start Flow

1. Read `CLAUDE.md` (bootstrap instructions)
2. Load `Skills/wrk-rules/` skill
3. Check `filedrop/` for files to process
4. Check `work/INDEX.md` for existing projects
5. Load any additional skills relevant to the task

## No System-Level Skills

This template does **not** include Globus AI Context or other advanced system integrations. All skills are in the top-level `Skills/` folder for transparency and portability.

## Updating This Index

Update this file when:
- System folder structure changes
- New skills are added (note them in the inventory above)
- Important files are added or moved
- Documentation structure changes

Keep it concise — detailed documentation goes in CLAUDE.md or skill files.
