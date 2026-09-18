---
name: plan
description: Produces an implementation plan with readable architecture, explicit changes, blast radius, and reviewable tasks from an intent or agreed pre-plan. Use when asked to create an implementation plan or task list, including after architectural discussion with pre-plan.
agent: plan
---

Stay in planning mode. Do not implement, edit files, save the plan, or commit.

Turn the current discussion and any additional input below into a self-contained implementation plan for another coding agent:

$ARGUMENTS

The human must be able to approve the behavior, architecture, and scope of impact before implementation. The builder must understand the approved decisions without the original conversation. Settle meaningful design choices; leave ordinary coding details to the builder.

Read and apply [keep-it-simple](../keep-it-simple/SKILL.md), especially its planning guidance. Carry the resulting scope decisions and deliberate omissions into this plan.

## Use an agreed pre-plan when available

- If the discussion or supplied document contains a pre-plan, use its latest complete draft and explicitly agreed revisions as the architecture input. A request to create the task list, including invoking `/plan`, accepts that draft when it is complete and unambiguous; do not ask for redundant blanket approval.
- Reconcile agreed revisions into one current design. If versions conflict or consequential questions remain unanswered, ask only for the missing resolution before decomposing tasks. Invocation alone does not resolve open decisions.
- Verify the design's consequential references and assumptions against current code. Preserve the agreed behavior, responsibility owners, contracts, scope classifications, deliberate omissions, rationale, and any user-specified execution constraints. Reopen a settled choice only when the user requests it or concrete new evidence challenges it; explain the conflict and resolve it before proceeding.
- A pre-plan may contain concise recommended choices and agreed decisions without a formal blast-radius assessment or execution rules. Resolve any remaining choices needing user input, expand the accepted design into this plan's decision format, and add the impact assessment and execution rules here. Their absence from pre-plan is intentional, not a reason to restart architectural discussion. If that deeper assessment reveals a consequential conflict, resolve the affected choice before task decomposition.
- Carry the architecture into this plan so it stands alone, then add tasks that implement it. Do not silently replace accepted decisions while sizing tasks. Leave routine implementation details to the builder within the agreed boundaries.
- Without a pre-plan, continue directly from the intent or discussion using the workflow below. Pre-plan is optional, and a complete intent does not require a separate architecture session.

## Inspect and clarify first

- Read the applicable repository rules and inspect relevant implementation and coverage.
- Trace callers of shared methods and objects, plus relevant downstream effects such as jobs, events, and external integrations. A short file list does not prove a small blast radius.
- Find existing capabilities to reuse and concrete neighboring implementations to follow. Distinguish reuse from copying an example's structure.
- Use actual file paths and class or method names. Separate inspected facts, proposed additions, and assumptions. Do not invent existing APIs or claim that a flow is unaffected without evidence.
- If an unresolved decision changes behavior, architecture, scope, or task order, ask the minimum necessary questions before writing the plan. Ask at most three at once, give two or three concrete options for each, and recommend one with a short reason. Stop and wait for answers.
- Resolve ordinary implementation choices from existing patterns. State minor assumptions instead of asking unnecessary questions.

## Make the plan easy to read

- Lead with what the user will experience, then explain the structure, then give execution details.
- Use short sentences, direct language, and domain vocabulary. Explain a technical object's role when first naming it.
- State each decision before its rationale. Replace vague advice such as "follow existing patterns" with the actual reference and what to reuse or follow.
- Give each fact one clear home. Tasks should reference whole-feature design decisions instead of repeating them.
- Include short plain-text pseudocode for branching rules, precedence, or meaningful multi-step flows. Skip it for obvious mechanical changes.
- Pseudocode describes behavior, not every helper method or loop. Make it agree with acceptance criteria and resolve any ambiguity it exposes.
- Use concrete examples for tricky boundaries, including timing, permissions, and fallback order when relevant.
- Keep the overview compact enough to review before the tasks, without hiding consequential changes. Omit irrelevant subsections and boilerplate rather than filling every field with "none".

## Architecture must be explicit

Before decomposing the work, establish the whole-feature design:

1. Show the proposed flow and each object's responsibility. Label existing unchanged pieces, modified pieces, and new pieces.
2. Inventory what will be created, modified, renamed, or removed. Include production code, necessary tests, and applicable migrations, configuration, dependencies, jobs, and integrations. Group repetitive companion changes such as locale files with precise paths or patterns.
3. Name the owner of each important business rule and what surrounding layers delegate to it.
4. Name existing capabilities to reuse and examples to follow, with inspected references.
5. Justify every new production object or abstraction: what concrete responsibility needs it, and why extending or reusing existing code would not handle that responsibility clearly. Prefer the simplest sufficient design; do not ban useful abstractions or design for hypothetical requirements.
6. State important interfaces and invariants, such as inputs, outcomes, authorization, state changes, and failure behavior. Include only what is consequential to this feature.
7. Explain indirect effects on shared callers and neighboring flows. Connect each meaningful impact to evidence, a containment decision, and verification. State unresolved uncertainty explicitly.
8. Define which architectural changes require revisiting the plan. Examples include moving responsibility to another layer, changing an agreed public contract, introducing persistence or dependencies, or affecting an additional workflow. Local naming, private helper extraction, and spec organization normally remain builder choices.

For consequential choices, briefly explain why a simpler or more localized option was insufficient. Do not manufacture alternatives for routine decisions.

## Task sizing and execution

- Keep tasks ordered and dependency-aware. Each introduces one explainable behavior or structural change and leaves the application coherent with relevant checks passing.
- Aim for roughly 50–150 changed lines of handwritten code and tests across 1–4 handwritten files. These are review estimates, not quotas. Split earlier for complex logic; explain necessary exceptions and separate generated or repetitive changes from the estimate.
- Keep behavior and necessary tests together. Existing coverage suffices when it already proves the contract. Do not add tests merely because a file changes.
- Keep required companion changes, such as translations across supported locales, together.
- Do not create temporary scaffolding, artificial abstractions, or broken intermediate states to satisfy task size.
- Before presenting, split tasks that combine independently meaningful rules or prerequisite refactoring with separable new behavior. Ensure reviewers do not need future tasks to judge the current diff.
- Preserve the user's execution workflow: implement exactly one task, verify it, summarize the diff and review focus, then stop for human review and a user-managed commit. Do not authorize automatic commits or progression.
- Require the builder to revisit the plan before a consequential architectural departure. If scope grows substantially, stop at a coherent boundary and propose a revised breakdown. If no coherent boundary is possible, report the blocker rather than marking incomplete work complete.
- Mark only the selected task completed, and only after implementation and required verification pass. Preserve unrelated plan content.

## Output structure

Use the following structure, adapting detail to the actual feature. The architecture overview is the primary human review surface; the task list makes that approved design executable.

# Implementation Plan: [Feature name]

## Expected result
[Plain-language behavior and outcome. State important scope limits and minor assumptions here.]

## Architecture overview

### Proposed flow
```text
[Actual entry point and responsibility]
  → [Existing object, responsibility, and change label]
  → [Next object, responsibility, and change label]
```
[Use a short map appropriate to the feature; do not force a linear flow onto unrelated paths.]

### Change inventory
| Action | File / object | What changes and why | Core / non-core and supporting requirement |
| --- | --- | --- | --- |
| Modify / Create / Rename / Remove | `actual/path` — object name | Concrete purpose | Classification, evidence, and approval status for optional work |

[Include necessary test and companion changes. Mark uncertain paths as proposed rather than pretending they are established.]

### Deliberately omitted
[Consequential possibilities left out and why they are unnecessary for the current intent. Omit this section when there is no meaningful decision to record.]

### Design decisions
For each consequential decision:
- **Decision:** [Responsibility, owner, and important contract.]
- **Evidence and reuse:** [Inspected file/method; what is reused or followed.]
- **Why this design:** [Why it is sufficient; justify any new object or abstraction.]
- **Constraint / revisit if:** [Task-specific architectural boundary and when to escalate.]

### Blast radius
| Shared change or boundary | Affected callers / flows and evidence | Containment and verification |
| --- | --- | --- |
| [Concrete change] | [Direct and indirect effects, with references] | [Design decision and relevant check] |

[Explain meaningful uncertainty. For a genuinely isolated change, use a brief evidence-backed explanation instead of an empty table.]

## Execution rules
- Read this plan and the applicable repository rules before implementation. Confirm the selected task still matches the current code.
- Follow this plan's core/non-core decisions and deliberate omissions. Use the simplest sufficient implementation within its approved architecture.
- Implement and verify exactly one unfinished task in order, then stop for human diff review and a user-managed commit.
- Follow the architecture decisions. Revisit the plan before consequential departures or substantial scope growth; report blockers rather than silently expanding the change.
- Mark only that task completed after its implementation and required verification pass. Keep unrelated plan content intact.

## Task list

### Task 1: [Plain-language title]
**Status:** Pending

**What changes:** [Two or three plain-language sentences describing this task's scope and outcome.]

**Depends on:** [Earlier tasks or concrete prerequisites.]

**Files:**
- Modify `...`: [purpose]
- Create `...`: [purpose, only if applicable]

**How it works:**
```text
[Short behavioral pseudocode when useful; omit this section for mechanical changes.]
```

**Implementation constraints:**
- [Reference applicable design decisions and exact code to reuse. Add only task-specific details needed by a builder without conversation context.]

**Acceptance criteria:**
- [ ] [Observable outcome or invariant, including important boundaries.]

**Verification:**
- `actual command` — [what it proves; identify existing coverage or required additions]
- [Relevant manual or browser check only when needed.]

**Review boundary:** [Estimated changed lines/files, specific review focus, and coherent stopping point. Explain any size exception.]

### Task 2: ...

## Remaining risks or assumptions
[Only unresolved items not already explained above. Omit if unnecessary. Do not defer decisions that would change architecture or scope.]

## Final planning check

Before responding, check silently:
- Can the human understand the expected behavior without decoding technical jargon?
- Can they see the overall architecture and every proposed production addition before reading tasks?
- Is each new object justified, and is each important responsibility assigned?
- Does the blast-radius assessment include shared callers and indirect effects, not just edited files?
- Do the change inventory, design decisions, pseudocode, and tasks agree?
- Can a builder execute each task and verify its outcome without guessing at consequential decisions?
- Are tasks small and coherent without adding artificial complexity?

Present the plan in the conversation only. End by asking whether to adjust the architecture or accept the plan for `/save-plan`. Do not start implementation.
