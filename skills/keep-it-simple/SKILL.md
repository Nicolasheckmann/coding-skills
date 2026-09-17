---
name: keep-it-simple
description: Guides software planning, implementation, and branch assessment toward the simplest sufficient solution. Use when proposing or generating code changes, classifying work against an agreed intent, or reviewing changes for overengineering, scope creep, and unnecessary indirection.
---

# Keep It Simple

Every addition should earn its place. Deliver the agreed intent with the least complexity needed to satisfy it. Choosing not to write code is a useful outcome.

Apply these principles throughout the requested work. Use the planning, implementation, or assessment guidance below as appropriate; do not turn every invocation into a separate audit or change the surrounding workflow's permissions, task boundaries, and review rules.

## Separate necessity from implementation complexity

Start from the agreed intent, acceptance criteria, approved plan, and relevant existing contracts. Reuse available context rather than asking the user to repeat it. If intent is missing, identify the evidence gap; do not infer requirements from the implementation being assessed.

For each meaningful addition, modification, or deletion, ask two questions:

1. **Is the behavior necessary?** Which present requirement does this serve? What required outcome would be incomplete or incorrect if this change were omitted?
2. **Is this implementation complexity necessary?** Can the same outcome be delivered clearly with existing behavior, fewer concepts, less indirection, a smaller change, or an in-scope removal?

Classify meaningful changes, grouping companion edits rather than accounting for every line:

- **Core:** Needed to deliver the agreed outcome, including concrete supporting work necessary for correctness and existing contracts. Explain the dependency; calling something infrastructure or a best practice does not establish necessity.
- **Non-core:** Optional behavior, speculative future support, or adjacent improvements that the agreed outcome does not depend on. State the present benefit, if any, and why it can be omitted.

Keep uncertainty explicit instead of forcing an unsupported classification. Core/non-core status and approval are separate: an optional improvement may be explicitly approved, while an unapproved addition must not become authorized merely because the agent considers it core.

A core feature can still be overengineered. Connecting an abstraction to a required feature does not establish that the abstraction itself is necessary. Equally, deleting required behavior is a regression, not a simplification.

## Prefer the simplest sufficient design

- Prefer no change when existing behavior already satisfies the request; verify that it does before claiming completion.
- Prefer a direct change using existing capabilities over introducing a new layer. Reuse should make responsibilities clearer, not force unrelated concepts through one abstraction.
- Justify new abstractions, configuration, dependencies, compatibility paths, and extension points with concrete present needs. Plausible future requirements are insufficient on their own.
- Keep useful abstractions when they clarify a real responsibility, enforce an invariant, or coordinate behavior that must evolve together. Similar-looking code alone is not a reason to generalize.
- Judge simplicity by how easily someone can understand behavior and make the next concrete change. Line count, file count, clever compression, and removing all abstraction are poor substitutes for that judgment.
- Preserve required behavior, authorization, data integrity, compatibility with actual consumers, and meaningful verification. Simplicity does not justify hiding errors or deleting necessary safeguards.
- Remove obsolete code within the authorized scope when evidence shows it is no longer needed. Do not expand the task into unrelated cleanup to make the system look simpler.

Leave non-core work out by default unless already authorized. Briefly surface consequential tradeoffs so the user can choose scope and order; do not interrupt routine core work to ask about every imaginable addition.

## During planning

Analyze scope and complexity before breaking the design into implementation tasks.

Extend the plan's existing change inventory with core/non-core classification and the requirement or evidence supporting each meaningful change. Include additions and deletions of behavior, code, tests, configuration, and documentation when relevant. Use a compact table or equivalent prose rather than creating a duplicate inventory.

Separate what is needed from how it is implemented. For consequential complexity, identify a simpler sufficient option and explain why it is accepted or insufficient. Do not manufacture alternatives for routine choices.

Keep optional proposals distinguishable from committed work. Order core work by actual dependencies; include non-core work in the task list only when authorized, without making core delivery depend on it unnecessarily.

Briefly record **Deliberately omitted** possibilities when they explain a meaningful scope or design decision: what was considered, why it is unnecessary now, and concrete evidence that would warrant reconsideration if known. Omit trivial possibilities and do not turn this record into an automatic backlog or promise to implement later.

Carry the accepted scope, consequential design decisions, and relevant omissions into the self-contained plan so a builder without this conversation can follow them.

## During implementation

Use the approved plan's classifications and omissions while implementing the selected task. Keep routine simplicity checks internal; explain consequential decisions without producing a report for each edit.

Before adding a new concept, inspect the existing path and consider the smallest sufficient change. Do not add a side feature or abstraction merely because implementation revealed a possible future use.

Respect the approved architecture. If a materially simpler design would change an agreed responsibility, contract, dependency, or scope, propose a plan revision before departing from it. Resolve ordinary local choices directly within the approved design.

Verify the required behavior and relevant existing contracts. In the normal completion summary, mention material simplifications or deliberate omissions when useful; no additional ceremony is required for routine changes.

## When assessing changes

Assessment is read-only unless the user separately requests fixes. Review the selected scope; do not replace a task review's boundary with a whole-branch audit.

For a standalone branch assessment, establish the base from the user's request or available repository evidence. Inspect committed branch changes against the appropriate merge base, plus staged, unstaged, and relevant untracked changes. State the baseline, what was included, and any exclusions or ambiguity. Do not silently assume that comparing the worktree with `HEAD` covers the branch.

Read relevant callers and contracts before judging an abstraction, fallback, or deletion. Compare actual changes with the intent and plan, including their deliberate omissions. Keep approved choices distinct from unapproved scope growth; approval does not prevent recommending a simpler alternative, but changing that choice requires revisiting the plan.

Map meaningful changes to core/non-core status and their supporting requirement, reusing an existing inventory when available. Report material findings with:

- The changed location and evidence.
- Whether the issue is unnecessary scope, unnecessary implementation complexity, or lost required behavior.
- The concrete cost or consequence, and the smallest sufficient correction.

Prioritize required-behavior regressions and decisions that materially reduce scope or complexity. Distinguish evidence-backed findings from uncertain suggestions. Preserve the surrounding review format when one exists; avoid duplicate summaries, speculative warnings, stylistic nits, and unrelated existing debt. No findings is a valid result.

## Authoring evaluations

When testing or revising this skill, use the [evaluation scenarios](references/evaluations.md). They are not additional steps for ordinary coding work.
