---
description: Export compressed session summary for model handoff
agent: build
---

Synthesize the current session into a structured markdown summary and write it to a file.

## What to gather

From the current session context (conversation history, todowrite, file changes, tool calls):

1. **Goals** — What was the user trying to accomplish? List each objective.
2. **Progress** — Per goal: done, in progress, or not started.
3. **Todowrite state** — Dump all todos with their status (completed/in-progress/pending).
4. **Files changed** — List every file created or modified this session with a one-line description.
5. **Commands run** — Notable commands and their outcomes.
6. **Key decisions** — Architecture choices, library selections, design tradeoffs.
7. **Lint/test results** — Any verification outcomes and failures.

## What to flag

- **Weak points & risks**: unvalidated assumptions, missing error handling, untested paths, known failures, security concerns, partial implementations, workarounds.
- **Unanswered questions**: questions the model asked that weren't answered, ambiguous requirements, deferred decisions, next steps discussed but not started.

## Output

Write to `session-summaries/session-summary-<YYYY-MM-DD-HHMMSS>.md` relative to the workspace root. Create the directory if it doesn't exist.

Use this structure:

```markdown
---
title: Session Summary
timestamp: 2026-07-13T14:30:00Z
model: <model-id>
status: paused
goals_completed: 2
goals_remaining: 1
total_files_changed: 4
---

# Session Summary

## Goals

| Goal | Status |
|------|--------|
| ...  | ✅ / 🔄 / ⏳ |

## Todowrite State

- [x] Completed task
- [ ] In-progress task
- [ ] Pending task

## Files Changed

| File | Change |
|------|--------|
| `path/to/file` | description |

## Key Decisions

- Decision with rationale

## Weak Points & Risks

- Risk or concern

## Unanswered Questions

- Open question

## Next Steps

1. Next step
```

## Rules

- Include everything a follow-up model needs to continue without re-reading the raw session.
- Be honest about unknowns. Flag uncertainty rather than guessing.
- `status` field: `completed` if all goals done, `paused` if partially done, `failed` if blocked.
- Create the `session-summaries/` directory if it doesn't exist.
