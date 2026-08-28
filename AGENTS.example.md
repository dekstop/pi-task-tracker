# Example AGENTS.md section

## Task tracking

For non-trivial or multi-step coding work, use the `task-tracker` Pi skill:

```text
/skill:task-tracker
```

It maintains `.pi/tasks/{backlog,active,archive}/` plus `.pi/tasks/ready.md`. Task files use stable IDs and meaningful filenames such as `T-014-oauth-callback.md`; the directory is the task's authoritative state, and tasks are moved between directories rather than duplicated. Keep the first 10 lines of backlog tasks concise enough for routine review; active tasks contain detailed current work and are the normal task-context files; archive is cold storage and should not normally be loaded. Use `ready.md` as a lightweight index of backlog tasks ready to be picked up, not as a second source of truth. Use explicit dependencies/blockers, acceptance criteria, and a `Next` action. Work on one primary task at a time, record out-of-scope discoveries as separate tasks, and use Git/repository state as authoritative when resuming after compaction or restart.
