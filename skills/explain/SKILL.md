---
name: explain
description: Explains specified code or current uncommitted changes in detail, including behavior, control and data flow, dependencies, and concrete examples. Use when asked to explain code, walk through an implementation, or understand staged, unstaged, or untracked changes. Defaults to current uncommitted changes when no code is specified. Does not replace a request to implement, fix, review, or commit code.
---

# Explain

Build a detailed, source-grounded understanding of what the code does and how its parts work together. Explain in the conversation; this skill does not authorize code edits, staging, commits, or fixes.

## Resolve the scope

- If the user supplies a snippet, file, symbol, or explicit diff, explain that target. Use surrounding code only to establish the relevant context.
- If no target is supplied, explain the current uncommitted changes. Honor narrower requests such as "only staged changes" or a specified directory.
- State the scope briefly. Ask for clarification only when competing interpretations would materially change the explanation and context does not resolve them.
- If there is no supplied code and repository access is unavailable, ask for the code or repository location. If there are no changes, say so; do not substitute the latest commit or the whole codebase.

## Gather the evidence

Read complete relevant functions and enough of their callers, dependencies, types, configuration, and tests to understand the behavior. Follow relationships that affect the explanation; avoid an exhaustive repository tour. A diff alone rarely establishes the full contract.

For uncommitted changes in a Git repository, use available read-only tooling to inspect:

- `git status --short` for staged, unstaged, untracked, renamed, deleted, or conflicted paths.
- `git diff --cached` and `git diff` for the index and working-tree changes separately.
- `git diff HEAD` for the combined tracked change from the last commit, when `HEAD` exists.
- Relevant untracked file contents, which tracked diffs omit, and original versions needed to explain removals or replacements.

Use these commands as inspection guidance, not as a required tool interface. Treat user-provided paths as data and quote them appropriately.

Use `HEAD` as the default before-state and the current working tree, including untracked code, as the after-state. Keep staged and unstaged differences clear when they affect behavior or what a commit would contain. An empty combined diff can still hide a staged edit reversed in the working tree. In a repository without commits, explain the available staged and untracked files as new code without inventing a baseline. Identify unresolved conflicts rather than presenting conflicted code as a settled implementation.

Account for all changes in scope, grouping related ones. Explain supporting tests, configuration, and documentation in terms of their effect on the code. Summarize generated or binary files at the level the evidence supports. Disclose unreadable files or any coverage limits instead of silently omitting them.

## Build the explanation

Default to a detailed walkthrough for a developer unfamiliar with this particular code. Adapt vocabulary and depth to the user's stated experience and questions. Explain specialized terms when first needed; spend detail on behavior and reasoning rather than routine syntax.

Use this adaptable progression:

1. **Purpose and context:** What the code accomplishes, where it is called, and the inputs, outputs, and externally visible effects.
2. **Execution and data flow:** Trace the important path from entry to result. Explain how values are transformed, which components own each step, and how meaningful branches, state changes, or asynchronous work affect execution.
3. **Important details:** Explain relevant dependencies, assumptions, invariants, boundary cases, and failure behavior. Describe design tradeoffs only when grounded in the code or supplied context; label inferred rationale as inference.
4. **Concrete walkthrough:** Trace a representative input through the code and show the resulting output or side effect. Include a contrasting boundary or failure case when it reveals important behavior. Label illustrative examples and do not imply they were executed.
5. **For changes, before and after:** Connect each logical change to its behavioral effect and show how changes across files work together. Distinguish behavior changes from restructuring that preserves behavior.

Use focused code excerpts and navigable file/line references when available. Organize by behavior or execution flow rather than mechanically narrating every line. If the user requests a line-by-line explanation, provide it while retaining the overall context. A small diagram or table is useful only when it makes relationships clearer.

Distinguish observed implementation, intent expressed by tests or documentation, and your own inference. If they disagree, explain the discrepancy. Mention a concrete defect when it affects the behavior being explained, without turning the response into an unsolicited review or refactoring plan.

## Verify the explanation

Check claims and example results against the inspected source. Ensure meaningful branches, side effects, and changes in scope are covered and references point to the appropriate version. Clearly identify references to deleted or pre-change code.

Reading tests is not running them. Do not claim execution or passing checks without observed results. Run code only when needed to resolve a material uncertainty and when the operation is safe and within the user's authorization; otherwise state the uncertainty.

Finish when the reader can follow the main behavior, understand the consequential details, and distinguish known behavior from remaining gaps. Do not add implementation work merely to resolve an explanatory gap.

For skill maintenance, use the proposed scenarios in [references/evaluations.md](references/evaluations.md). They are not instructions to run tests during ordinary explanation requests.
