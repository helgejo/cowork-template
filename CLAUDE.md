# Workspace

## Initialization

1. Read this file
2. Load the **wrk-rules** skill (`Skills/wrk-rules/SKILL.md`) — all workspace conventions live there
3. Check `filedrop/` for files the user has dropped for organizing
4. Check if user's request matches an existing project → read `work/INDEX.md`
   - **Existing project:** Ask if they want to continue or start a new task
   - **Continue:** Read the project's `CONTEXT.md` to restore conversation context
   - **New task:** Create a task folder with a fresh `CONTEXT.md`
5. Always use the most suitable **skills** and **connectors/plugins** for the task at hand

## Where to Find Things

| What | Where |
|------|-------|
| Workspace rules & conventions | `Skills/wrk-rules/SKILL.md` |
| System index & structure | `system/INDEX.md` |
| Company knowledge (brand, product, guides) | `company-context/` |
| Company glossary | `company-context/glossary.md` |
| Work projects & file index | `work/INDEX.md` |
| Personal files | `personal/INDEX.md` |
| Source files (read-only originals) | `work/original-source-files/` |
| Tasks | `TASKS.md` |
| Dashboard | `dashboard.html` |
| Change log | `system/CHANGELOG.md` |
| Project deep context | `system/memory/projects/` |
| System documentation | `system/docs/` |

## Filedrop

`filedrop/` — The user drops files here for Claude to sort and organize. On session start, move files to the correct `work/original-source-files/` subfolder, update `work/INDEX.md`, and log in changelog.

## Rules

- Always load wrk-rules skill before starting work
- Keep each project's `CONTEXT.md` updated with a conversation summary after every session
- Never delete — always archive
- `work/original-source-files/` is READ ONLY
- Company knowledge edits go in `company-context/` (single source of truth)
- Follow task-folder convention: `work/[category]/YYYY-MM-DD_[task-name]/`
- All tasks live in `TASKS.md`, not here
