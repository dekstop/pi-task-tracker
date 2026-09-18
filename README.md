# Pi task-tracker skill

A simple task tracking skill for Pi coding agents, using structured markdown files for tasks and directories for state: `backlog` → `active` → `archived`.

## Install

Global:

```bash
mkdir -p ~/.pi/agent/skills
git clone git@github.com:dekstop/pi-task-tracker.git ~/.pi/agent/skills
```

Project-local (without git files):

```bash
mkdir -p .pi/skills/task-tracker
cp path/to/SKILL.md .pi/skills/task-tracker/
```

Advertise the skill in `AGENTS.md` and ask your agent to create tickets for you. See `AGENTS.example.md` for a compact advertisement section.

Alternatively, invoke explicitly with `/skill:task-tracker`.

## Task lifecycle

Task files are stores in subdirectories under `.pi/tasks/` according to lifecycle state:
- `backlog/` contains concise task files for planned work. During routine planning, only the first 10 lines should be loaded unless a task is selected for deeper investigation.
- `active/` contains the current implementation context. Active tasks may be loaded in full and should capture useful progress, discoveries, decisions, blockers, and the next action.
- `archive/` is cold storage for completed or inactive work. It should not normally be consulted during planning or routine task review.
- `ready.md` is a lightweight index of backlog tasks that are ready to be picked up. It references task files but is not a second source of truth.

This keeps routine planning cheap while preserving detailed context for the task actually being worked on. When a task changes state, move the existing file rather than copying it.

## Task format

Tasks are represented as individual Markdown files with stable IDs and meaningful filenames, for example `T-014-oauth-callback.md`. The directory containing the file defines its lifecycle state, avoiding duplicated task records and keeping state transitions explicit.

Each task retains the established structure and can include additional implementation notes, decisions, dependencies, blockers, acceptance criteria, and a `Next` action as appropriate. The first 10 lines of backlog tasks should provide a useful summary for lightweight review; active tasks can contain the full working context.

`ready.md` provides a deliberately lightweight way to identify work that is ready to start without requiring all backlog tasks to be loaded. It should only reference backlog tasks and should be kept in sync when tasks become ready or move to `active/`.

The archive is intentionally treated as cold storage: historical detail is preserved without making it part of normal planning context. Git and repository state remain authoritative when reconstructing work after compaction or restart.

## Implementation

See [SKILL.md](SKILL.md) for a more detailed description.
