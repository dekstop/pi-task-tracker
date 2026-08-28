# Pi task-tracker skill

Balanced, low-context task tracking for Pi coding agents.

## Install

Project-local:

```bash
mkdir -p .pi/skills/task-tracker
cp SKILL.md .pi/skills/task-tracker/SKILL.md
```

Global:

```bash
mkdir -p ~/.pi/agent/skills/task-tracker
cp SKILL.md ~/.pi/agent/skills/task-tracker/SKILL.md
```

Invoke explicitly with `/skill:task-tracker`.

See `AGENTS.example.md` for a compact advertisement section.

## Context strategy

Task context is split by lifecycle state under `.pi/tasks/`:
- `backlog/` contains concise task files for planned work. During routine planning, only the first 10 lines should be loaded unless a task is selected for deeper investigation.
- `active/` contains the current implementation context. Active tasks may be loaded in full and should capture useful progress, discoveries, decisions, blockers, and the next action.
- `archive/` is cold storage for completed or inactive work. It should not normally be consulted during planning or routine task review.
- `ready.md` is a lightweight index of backlog tasks that are ready to be picked up. It references task files but is not a second source of truth.
- 
This keeps routine planning cheap while preserving detailed context for the task actually being worked on. When a task changes state, move the existing file rather than copying it.

## Design

Tasks are represented as individual Markdown files with stable IDs and meaningful filenames, for example `T-014-oauth-callback.md`. The directory containing the file defines its lifecycle state, avoiding duplicated task records and keeping state transitions explicit.

Each task retains the established structure and can include additional implementation notes, decisions, dependencies, blockers, acceptance criteria, and a `Next` action as appropriate. The first 10 lines of backlog tasks should provide a useful summary for lightweight review; active tasks can contain the full working context.

`ready.md` provides a deliberately lightweight way to identify work that is ready to start without requiring all backlog tasks to be loaded. It should only reference backlog tasks and should be kept in sync when tasks become ready or move to `active/`.

The archive is intentionally treated as cold storage: historical detail is preserved without making it part of normal planning context. Git and repository state remain authoritative when reconstructing work after compaction or restart.
