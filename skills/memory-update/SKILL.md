---
name: memory-update
description: >
  End-of-session memory sync. Reviews what was done, what was learned, what went wrong,
  and what comes next — then writes it to the project memory system. Includes a
  self-reflection phase so future sessions avoid repeating the same mistakes.
  Trigger when: "update memory", "save session", "end of session", "memory update",
  "wrap up session", "save progress", "wrap up", "/memory-update".
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

# Session Memory Update

Sync everything learned in this session to the project's persistent memory system so the next conversation starts with full context, not a blank slate.

---

## CONFIGURATION

Set these paths before use. Replace placeholders with your actual paths.

```
MEMORY_BASE_PATH: ~/.claude/projects/            # memory lives here, under <project-name>/memory/
VAULT_PATH:       /path/to/your/obsidian/vault   # optional — leave blank to skip Obsidian steps
```

Phases 6 and 7 (vault log + daily note) run only if `VAULT_PATH` is set.

---

## AUTOIMPROVEMENT

At session start, read `feedback.log` in this skill directory. Apply any corrections or preferences logged in past sessions.

At session end, append any new corrections to `feedback.log`:
```
[YYYY-MM-DD] CORRECTION: <what was wrong and what the right approach is>
[YYYY-MM-DD] VALIDATED: <approach that worked well>
```

---

## Phase 1 — Locate the Memory System

1. Find the project memory directory at `<MEMORY_BASE_PATH>/<project-name>/memory/`.
2. Read `MEMORY.md` (the index file) to understand what's already stored.
3. Glob `memory/*.md` to see all existing memory files.

If no memory system exists yet, ask the user if they want one created. If yes, create the directory and a starter `MEMORY.md` from the template structure below.

**Starter MEMORY.md structure:**
```markdown
# Memory Index

Pointer-only index. One line per entry. Read the linked file for full content.
Max 200 lines.

## Active Projects
## Feedback & Preferences
## Recent Session Logs
```

---

## Phase 2 — Review the Session

Scan the full conversation and extract:

1. **Tasks completed** — what was built, fixed, or changed this session (specific files, routes, decisions, etc.)
2. **Decisions made** — architectural choices, tool picks, scope changes agreed with the user
3. **Blockers and workarounds** — anything that failed, was retried, or required a different approach
4. **User feedback** — any corrections, preferences, or "don't do that" moments
5. **Next steps** — what's queued up or left unfinished

---

## Phase 3 — Self-Reflection

Answer these questions honestly. Include the answers in the session log.

- **What slowed me down?** (e.g., tried wrong approach first, missed existing code, asked unnecessary questions)
- **What would I do differently next time?** (e.g., read the file before editing, check types first, avoid X pattern)
- **What patterns worked well?** (e.g., parallel tool calls, reading reference code first)

Be specific. Name the actual files, tools, or decisions. Generic reflections like "be more careful" are useless.

---

## Phase 4 — Write Memory Files

For each category of new information, either **update an existing memory file** or **create a new one**.

### Session log (always create)

Create `memory/session_YYYY-MM-DD.md`:

```markdown
---
name: session-YYYY-MM-DD
description: Session log for [date] — [1-line summary of main work]
metadata:
  type: project
---

## Completed
- [bullet list of what was done]

## Decisions
- [any choices made and why]

## Blockers & Workarounds
- [what failed and how it was resolved]

## Self-Reflection
- **Slowed me down:** [specific things]
- **Next time:** [specific improvements]
- **Worked well:** [specific patterns to repeat]

## Next Steps
- [what to do in the next session]
```

If multiple sessions happen on the same day, suffix the filename: `session_YYYY-MM-DD_b.md`.

### Other memory types (update or create as needed)

**Feedback memories** (`feedback_*.md`) — if the user corrected your approach:
```markdown
---
name: feedback-<slug>
description: <one-line summary of the rule>
metadata:
  type: feedback
---

<The rule itself.>

**Why:** <reason the user gave>
**How to apply:** <when this kicks in>
```

**Project memories** (`project_*.md`) — if sprint status, deadlines, or goals changed:
```markdown
---
name: project-<slug>
description: <one-line status of the project>
metadata:
  type: project
---

<Current state of the project.>

**Why:** <motivation or constraint>
**How to apply:** <how this should shape future suggestions>
```

**User memories** (`user_*.md`) — if you learned something new about the user's preferences or role.

**Reference memories** (`reference_*.md`) — if new external resources were discovered.

### Rules for writing
- Check existing files first — update rather than duplicate
- One topic per file, descriptive filename (e.g., `feedback_no_mocks.md`, `project_sprint3_status.md`)
- Always use the frontmatter format with name, description, and metadata.type fields
- Convert relative dates to absolute dates (e.g., "tomorrow" to "2026-06-19")
- Never save secrets, API keys, or passwords to memory

---

## Phase 5 — Update the Index

Add pointers to any new or updated memory files in `MEMORY.md`. Keep entries to one line each:

```markdown
- [Short Title](filename.md) — one-line description of what's in the file
```

**Important:** `MEMORY.md` is an index only. Never write memory content directly into it. Keep it under 200 lines. If it's growing past 150 lines, archive old session log entries.

---

## Phase 6 — Append to Vault Log (optional, requires VAULT_PATH)

Skip this phase if `VAULT_PATH` is not set.

Append a one-line entry to `<VAULT_PATH>/00-dashboard/log.md`:

```
- **YYYY-MM-DD** — [Project]: [1-line summary of session work]
```

If `log.md` doesn't exist, create it:
```markdown
# Vault Log
Append-only chronological record. Never edit past entries.

- **YYYY-MM-DD** — ...
```

---

## Phase 7 — Write Obsidian Daily Note (optional, requires VAULT_PATH)

Skip this phase if `VAULT_PATH` is not set.

Write to (or append to) `<VAULT_PATH>/08-daily/YYYY-MM-DD.md`.

If the file exists, **append** the session summary under the `## Sessions` heading.

If it doesn't exist, create it:

```markdown
---
title: "YYYY-MM-DD"
date: "YYYY-MM-DD"
tags: [daily, <project-name>]
type: daily
status: active
related: ["[[<project-name>]]"]
---

# <Day of Week>, <Month> <Day>, <Year>

## Sessions

### Claude Session: <1-line summary>

**Project:** [[<project-name>]]

**Completed:**
- [bullet list from Phase 2]

**Decisions:**
- [from Phase 2]

**Key Learning:** [most important self-reflection insight]

**Next:** [from Phase 2 next steps]
```

Use `[[backlinks]]` for project names and related daily notes. Keep it concise — full detail lives in the session memory file.

---

## Phase 8 — Confirm to User

Tell the user:
- How many memory files were created or updated
- Whether an Obsidian daily note was written (and where)
- A 3-5 line summary of what was saved
- The recommended first action for the next session

---

## Quality Standards

- Session logs must be specific — mention actual file names, route paths, error messages
- Reflections must be actionable — "check `types/index.ts` before adding new types" not "be more thorough"
- Never save secrets, API keys, or passwords to memory
- Never save information that can be derived from git log or reading the code
- Status updates must reflect current state, not append history
- Memory files are point-in-time snapshots — when facts change, update the file, don't add history

---

## When No Memory System Exists

If the project has no memory directory yet:

1. Tell the user: "This project doesn't have a memory system yet."
2. Offer to create one with: `mkdir -p ~/.claude/projects/<project-name>/memory/`
3. Create a starter `MEMORY.md` index
4. Then run the full session sync as normal

This skill works for any project, not just projects already using this kit.
