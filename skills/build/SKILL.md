---
name: build
description: Execute the next unfinished task from an implementation plan
---

Execute exactly one task from the implementation plan.

Read the referenced implementation plan file before doing anything else. If the user did not provide a plan file path and there is no obvious current plan file, ask for the path before continuing. Do not infer the task list from conversation alone.

Before selecting or implementing a task, inspect `git status --short`. Require a clean worktree so the current `HEAD` commit is an immutable baseline for task review. If tracked or untracked changes exist, stop without modifying them and ask the user to review and commit the previous task, or otherwise move those changes out of this worktree. Do not build on an ambiguous dirty baseline.

Find the next uncompleted task in the plan:
- preserve the plan order
- skip tasks already marked completed
- choose the first task that is not completed
- if every task is completed, report that the plan is complete and make no changes

Implement only that selected task.

Before editing:
- inspect the relevant code paths named by the task
- confirm the task still matches the current code
- follow existing project patterns
- ask the user if the task is ambiguous, blocked, stale, unsafe, or depends on another unfinished task

While implementing:
- keep changes limited to the selected task
- do not start later tasks
- do not perform unrelated cleanup or refactors
- do not commit or create a pull request unless explicitly requested

After implementing:
- run the verification steps listed for the selected task
- run any additional targeted checks needed for confidence
- if verification fails, fix issues that are within the selected task scope
- if verification cannot pass without expanding scope, stop and explain the blocker

Update the plan file only after the selected task is implemented and verification passes:
- mark only the selected task as completed
- preserve all unrelated plan content
- do not mark later tasks completed
- do not rewrite the plan except for the minimal completion update

Final response:
- completed task title
- summary of changes
- files changed
- verification commands and results
- whether the plan file was updated
- next remaining task, if any
- remind the user to run `/review <plan-path>` before committing the task
