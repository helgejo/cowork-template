---
name: wrk-rules
description: |
  Workspace rules for Claude Cowork. File organization, task folders, conversation memory, filedrop inbox, naming conventions, and session workflow. Load this skill at the start of every session.
---

# Workspace Rules

File organization, memory, and project management for Claude Cowork.

## Core Principles

1. **Never delete — always archive.**
2. **CLAUDE.md = bootstrap.** It tells Claude how to start. Stable rules live here in wrk-rules.
3. **Skills = stable knowledge. Memory = evolving knowledge.** Brand, positioning → skills. Numbers, projects, people → memory.
4. **work/original-source-files/ = read-only originals.** Index in work/INDEX.md.
5. **filedrop/ = user inbox.** The user drops files here for Claude to sort into work/original-source-files/ and update the index.
6. **Task folders group work. Archive when done.** Every non-trivial task gets its own folder with a CONTEXT.md.
7. **Always use the most suitable skills and connectors.** Check available skills and MCP connectors for every task. Don't default to generic approaches when a specialized tool exists.

## Session Start Flow

1. Read CLAUDE.md (bootstrap instructions)
2. Read this skill (wrk-rules)
3. Check `filedrop/` for files the user has dropped → process them
4. **Project detection:**
   - Does the user's request match an existing project? Check `work/INDEX.md`
   - If yes: ask "Want to continue [project name] or start a new task?"
   - If continuing: read the project's `CONTEXT.md` to restore context
   - If new: create a task folder with a fresh `CONTEXT.md`
5. Load any additional skills relevant to the task (brand, company context, data, etc.)

## Conversation Memory (CONTEXT.md)

Every task folder gets a `CONTEXT.md` file that serves as conversation memory. This allows the user to start a new Claude conversation and pick up where they left off.

### What goes in CONTEXT.md

```markdown
# [Task Name]

## Summary
One-paragraph description of what this task is about.

## Status
Current state: in-progress / blocked / ready-for-review / done

## Key Decisions
- [Date]: Decision made and rationale

## Current State
What was accomplished, what's next, any blockers.

## Files in This Folder
- filename.ext — what it is

## Open Questions
Things that still need resolution.
```

### Rules
- Create CONTEXT.md when creating the task folder, even if no files have been generated yet
- Update CONTEXT.md at the end of every conversation that touches this task
- Keep it concise — enough for a fresh Claude session to understand the full context
- When resuming: read CONTEXT.md first, then ask the user what they want to do next
- CONTEXT.md is the primary way to carry context between conversations

## File Naming Convention

Format: `YYYY-MM-DD_[type]-[subject]-[detail].ext`

Rules: ISO date, lowercase after date, hyphens between words, underscore after date, max 50 chars, version as `-v2` not `-final`.

Type codes: `prop` proposal, `rpt` report, `mtg` meeting, `spec` specification, `plan` plan, `pres` presentation, `brief` brief, `tmpl` template, `guide` guide, `draft` draft, `email` email, `model` financial model, `dash` dashboard.

## Task Folders

Every non-trivial task gets its own folder inside the relevant work/ subfolder.

**Format:** `YYYY-MM-DD_[short-task-name]/`

**Example:**
```
work/your-category/
├── _resources/                          # Persistent resources (templates, logs, idea banks)
├── 2026-02-09_project-name/             # ← Task folder
│   ├── CONTEXT.md                       # ← Conversation memory
│   ├── deliverable-one.html
│   ├── deliverable-two.pdf
│   └── planning-notes.md
├── 2026-02-09_another-project/
│   ├── CONTEXT.md
│   └── output-file.txt
└── _archive/                            # ← Completed tasks move here
    └── 2026-01-31_completed-project/
```

### Rules

1. **When to create a task folder:** Any task that produces 2+ files, OR any task the user may want to resume later. Single-file outputs can stay loose, but prefer folders.
2. **Folder naming:** `YYYY-MM-DD_[short-task-name]/` — date is when work started, name is 2-4 words max.
3. **Files inside task folders:** Drop the date prefix from filenames (the folder has the date). Use short descriptive names.
4. **Persistent resources → `_resources/`:** Things that live across tasks (logs, templates, idea banks) go in `_resources/` inside the work category folder.
5. **Auto-archive on task completion:** When done, move the task folder to `_archive/`. Log it in CHANGELOG.
6. **Don't archive _resources/:** Persistent resource folders stay in place.

### Task Start Checklist
1. Create the task folder: `work/[category]/YYYY-MM-DD_[task-name]/`
2. Create `CONTEXT.md` inside it
3. Save all task output files inside it

### Task End Checklist (MANDATORY)
1. Update `CONTEXT.md` with final state
2. Update `work/INDEX.md` (move from active to archive)
3. Move the task folder to `work/[category]/_archive/`
4. Log the archive move in `system/CHANGELOG.md`
5. Move any new persistent resources to `_resources/`

## Project Structure

```
/workspace-root/
├── CLAUDE.md              # Bootstrap file (initialization + where to find things)
├── TASKS.md               # All tasks live here
├── dashboard.html         # Interactive workspace dashboard
├── /company-context/      # Shared company knowledge
│   ├── glossary.md        # Company glossary (terms, acronyms, integrations)
│   └── README.md          # Guide to company knowledge organization
├── /filedrop/             # User drops files here → Claude organizes them
├── /Skills/               # Workspace-only skills (wrk-rules)
│   └── /wrk-rules/        # Workspace rules skill
├── /work/                 # All work content
│   ├── INDEX.md           # Master index of all work files and projects
│   ├── /your-category/    # Work category (create as many as needed)
│   │   ├── _resources/    # Persistent resources + templates
│   │   ├── _archive/      # Completed task folders
│   │   └── task folders...
│   ├── /original-source-files/  # Read-only reference originals
│   └── /another-category/
├── /personal/             # Personal files
│   └── INDEX.md           # Personal file index
└── /system/               # Claude internals
    ├── INDEX.md            # System index (structure, skills inventory, files)
    ├── CHANGELOG.md        # Append-only change log (rotate every 2 weeks)
    ├── /memory/            # Personal/evolving knowledge
    │   ├── /people/        # Contact profiles
    │   ├── /projects/      # Deep project context
    │   └── /context/       # Evolving context (decisions, data)
    ├── /docs/              # Working documents and guides
    └── /archive/           # System-level archive
```

## Filedrop Inbox

**Purpose:** The user drops files here for Claude to sort and organize. Claude processes them every session start.

### Session Start — Filedrop Check (MANDATORY)

1. List contents of `filedrop/`
2. If empty → continue normally
3. If files found → for each file:
   a. Determine the correct `work/original-source-files/` subfolder
   b. Move file from `filedrop/` to the correct subfolder
   c. Add entry to `work/INDEX.md` source files section
   d. Log the move in `system/CHANGELOG.md`
4. Confirm to user: "Processed X files from filedrop/ → [list destinations]"

### Routing Rules

| File type / content | Destination |
|---------------------|-------------|
| Budgets, financial documents, data models | work/original-source-files/financials/ |
| Proposals, decks, playbooks | work/original-source-files/resources/ |
| Customer data, testimonials, case studies | work/original-source-files/customers/ |
| Logos, brand assets, guidelines | work/original-source-files/brand/ |
| Product documentation, specs | work/original-source-files/product/ |
| Contracts, policies, compliance | work/original-source-files/legal/ |
| Unclear | Ask the user before moving |

### Rules
- Never leave files in filedrop/ after processing
- If a file already exists in the destination, ask user whether to replace or version it
- Extract key facts for the index entry

## Sources Library

**Location:** `work/original-source-files/` — read-only reference originals.

**Rules:**
- Never modify originals — treat as read-only
- When a source file is updated, archive the old version and update work/INDEX.md
- Key facts in the index should be enough for quick reference without opening the file

## Memory System

```
CLAUDE.md               ← Bootstrap (initialization, where to find things, rules)
company-context/
  glossary.md            ← Company-wide glossary (loaded on demand)
work/INDEX.md            ← Master file index (loaded on demand)
system/memory/
  people/                ← Contact profiles
  projects/              ← Deep project context (decisions, patterns)
  context/               ← Evolving context (financials, decisions)
```

### What Goes Where

| Info | Location | Changes |
|------|----------|---------|
| Initialization instructions | CLAUDE.md | Rarely |
| Where to find things | CLAUDE.md | When structure changes |
| All terms/acronyms | company-context/glossary.md | As needed |
| File inventory | work/INDEX.md | When files added |
| Task lists | TASKS.md | Every session |
| Conversation memory | CONTEXT.md in task folders | Every conversation |
| Full contact profiles | system/memory/people/ | As needed |
| Deep project context | system/memory/projects/ | Per project |
| Brand, positioning, company knowledge | company-context/ | When product changes |

### Lookup Flow

Request → CLAUDE.md → company-context/glossary.md → work/INDEX.md → system/memory/ → ask user → learn it.

### No Duplication Across Layers

| Layer | Contains | Changes |
|-------|----------|---------|
| **Preferences** | Behavior (tone, format, workflow) | Rarely |
| **Skills** | Stable identity (brand, product, knowledge) + workspace rules | When product changes |
| **CLAUDE.md** | Bootstrap instructions + where to find things | When structure changes |
| **Memory** | Evolving context (numbers, projects, people) | Every session |
| **CONTEXT.md** | Per-task conversation continuity | Every conversation |

If it's in a skill, don't put it in memory. If it's in CLAUDE.md, don't duplicate in the skill.

## Shared Company Knowledge

**Location:** `company-context/` at workspace root.

**Purpose:** Company knowledge and guidelines shared with the team.

**Rules:**
- Single source of truth for company knowledge — edit here, not elsewhere
- All skills live in the top-level `Skills/` folder (wrk-rules and any other shared workspace skills)
- Do not put workspace-specific files in company-context/
- Team members can share and update this folder across AI tools

## CHANGELOG.md

Located at `system/CHANGELOG.md`. Append-only, reverse chronological.

Sections: Added, Changed, Archived, Notes. Include rationale, not just what changed.

### Rotation Policy
Keep the last 2 weeks of entries in `system/CHANGELOG.md`. Archive older entries to `system/archive/changelog-history.md` (append at bottom). This keeps the active changelog readable.

## Workflow

**Session Start:** Read CLAUDE.md → Load wrk-rules → Check filedrop/ → Project detection → Load relevant skills

**During:** Simple tasks: execute → verify → log. Complex: plan → confirm → execute → verify → log. Update memory when new info emerges.

**Task Completion:** Update CONTEXT.md → Update work/INDEX.md → Archive task folder → Log in CHANGELOG

**Session End:** Update CONTEXT.md → Update CHANGELOG → Verify file structure

## Permissions

**Without asking:** Read files, create files (following convention), archive completed task folders, update CLAUDE.md/CHANGELOG/INDEX.md/CONTEXT.md, create standard folders, process filedrop/.

**Ask first:** Delete files, modify locked/approved files, external actions, modify skill source files.

## Archive Policy

Never delete. Move completed task folders to `work/[category]/_archive/`. Move system-level items to `system/archive/`. Log in CHANGELOG. Update work/INDEX.md.
