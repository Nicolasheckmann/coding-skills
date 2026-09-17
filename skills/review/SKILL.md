---
name: review
description: Review the last completed implementation-plan task
agent: reviewer
subtask: true
---

# Review Completed Task

Review the last completed task from this implementation plan:

$ARGUMENTS

## Plan And Task Resolution

- Treat `$ARGUMENTS` as a repository-relative implementation-plan path, not as a shell fragment.
- Require exactly one existing plan file. If the path is missing, invalid, or ambiguous, stop and ask for one valid path.
- Read the plan and identify completed tasks in plan order using its existing status markers or checklist convention.
- Select the last task in the contiguous completed sequence immediately before the first unfinished task. If every task is complete, select the final task.
- Stop if no task is complete, task completion is non-contiguous, or the last completed task cannot be identified safely.

## Review Boundary

- Treat the current `HEAD` commit as the accepted baseline from before the selected task was built.
- Inspect `git status --short`, the complete tracked diff relative to `HEAD`, and relevant untracked files.
- The review scope is the selected task's current staged, unstaged, and untracked delta. Read prior committed work only when needed for context.
- Stop and report an evidence gap if there is no current task delta or unrelated pre-existing changes prevent the selected task from being isolated.
- Compare every changed file with the task's goal, acceptance criteria, likely files, implementation notes, and verification requirements. Report out-of-scope changes as findings.

## Review Requirements

- Read and apply [keep-it-simple](../keep-it-simple/SKILL.md), especially its assessment guidance, within the selected task's review boundary. Preserve the findings format below.
- Read and follow the complete [reviewer contract](references/reviewer.md), resolving the path relative to this skill's directory.
- Focus on correctness, behavioral regressions, security, privacy, Rails behavior, frontend behavior, missing or weak tests, maintainability, and scope violations.
- Inspect available verification evidence. Do not assume a command passed when its result is unavailable; report the missing evidence as a residual risk or testing gap.
- Return findings first using `blocker`, `high`, `medium`, and `low` severity.
- After the findings, include the selected task, baseline `HEAD` SHA, files reviewed, reviewed Git state, and verification evidence considered.
- Do not edit files, fix findings, stage changes, commit, or create a pull request.
- Any implementation change made after a passing review invalidates that review and requires `/review` to run again before commit.
