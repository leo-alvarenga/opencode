---
description: Import a previous session summary to pick up where you left off
agent: plan
---

Restore context from a previous session's summary so you can continue where you left off.

## Step 1 — Check the summary directory

Check if `session-summaries/` exists in the workspace root.

- If it does not exist, tell the user: *"No session-summaries/ directory found — you haven't run /summarize yet in this workspace."* and stop.
- If it exists but is empty, tell the user and stop.

## Step 2 — List available summaries

List all `session-summary-*.md` files in `session-summaries/` sorted newest first.

For each file, read its YAML frontmatter and present:

```
  #  Timestamp              Status    Goals
 ─── ─────────────────────── ───────── ──────────
  1  2026-07-13 14:30:00    paused    2/3 done
  2  2026-07-12 09:15:00    completed 4/4 done
```

Ask the user to pick one by number. Wait for their response.

## Step 3 — Read the chosen summary

Parse the YAML frontmatter and section headings. Extract:

- `title`, `timestamp`, `model`, `status`
- `goals_completed`, `goals_remaining`, `total_files_changed`
- **Goals** table — especially in-progress and not-started items
- **Todowrite State** — all checklist items with their status
- **Files Changed** — list of modified files
- **Key Decisions**
- **Weak Points & Risks**
- **Unanswered Questions**
- **Next Steps**

## Step 4 — Restore context

### A. Recreate todowrite

For every item in **Todowrite State** (completed, in-progress, and pending), create a todowrite entry with matching status. Use the todowrite tool — not the edit file tool.

If todowrite is not available to you, tell the user: *"I cannot call todowrite from this agent. Would you like me to switch to a different agent to restore the task list?"*

### B. Present the restored session

Summarise what the user was working on:

```
=== Session Restored ===
Previous session:  <timestamp> — <status>
Goals:             <X> completed, <Y> remaining of <Z> total
Files changed:     <N>
Status:            <paused/completed/failed>

Key decisions:
  - <summary>

Weak points to watch:
  - <summary>

Open questions:
  - <summary>

Next steps:
  1. <summary>
  ...
```

Then ask: *"Your previous session has been restored. What would you like to work on next?"*
