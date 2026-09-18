---
name: pre-plan
description: Refines architecture through conversation before implementation tasks are defined. Use when the user wants to explore a proposed flow, inspect a change inventory, or discuss design choices before creating a task list. Separates recommended choices from decisions needing user input and hands the agreed design to plan. Does not replace product discovery, code explanation, or implementation.
---

# Pre-plan

Develop an architecture the user understands and can refine before committing to a task list. Success is a standalone design with clear responsibilities, justified changes, understood effects, and no unresolved consequential choices.

Use the supplied intent and conversation; a saved intent is not required. If the desired behavior is still unclear, resolve the product question that blocks design rather than inventing requirements.

Keep the work in the conversation. Do not edit or save files, implement, commit, or produce tasks, ordering, or estimates. Leave the detailed blast-radius assessment and execution rules to plan.

## Ground the proposal

Inspect repository rules, the relevant implementation and coverage, and callers or downstream flows that could be affected. Use inspected paths and object names; distinguish current behavior, proposed additions, assumptions, and evidence gaps. Investigate questions the code can answer instead of asking the user.

Use [keep-it-simple](../keep-it-simple/SKILL.md) when assessing scope and design complexity. Prefer existing capabilities, justify new responsibilities and abstractions, and keep optional work distinguishable from what the outcome requires.

For the proposed flow and change inventory formats, consult those two sections of [plan](../plan/SKILL.md). Use them as a reference only. The lighter design discussion below replaces plan's detailed decision format during pre-planning; do not import its blast-radius table, execution rules, task generation, or save-plan handoff.

Offer an evidence-backed first draft as soon as it gives the user something useful to assess. Do not require every choice to be settled first. If a missing answer would fundamentally change the proposal, ask that question before drafting.

## Make the conversation useful

Focus each round on the open choice that most affects the design. Explain what is being decided and its practical consequences in plain language. Ask one focused question at a time when possible, and wait for answers that determine behavior, architecture, or scope. Resolve ordinary implementation details from existing patterns.

Let the user steer: they may compare options, challenge an abstraction, explore an example, or change a constraint. Adapt the depth and order of discussion to what helps them decide. There is no required sequence of topics or minimum number of rounds.

Maintain a clear distinction between a proposal and an accepted decision. Exploring an alternative does not select it; accepting one choice does not approve the whole draft. Preserve settled choices unless the user reopens them or new evidence creates a concrete conflict. Explain that conflict instead of silently redesigning.

Treat a changed decision as a change to the whole design. Reconcile its effects on the flow, inventory, contracts, omissions, and other choices. Show the affected sections with a short explanation of what changed; present the full draft when requested or ready for handoff.

For example, choosing checkout-only eligibility instead of changing a rule shared with renewal must change more than the decision paragraph. The owner, inventory, and flow must reflect the narrower scope, and the decision must state that renewal behavior stays unchanged.

## Make decisions easy to understand

Separate consequential decisions by whether user input is needed, not by how technically complex they are:

- **Recommended choices:** The requirements and inspected code support a clear default. Use a short bullet stating the choice, why it fits, and any meaningful consequence. These are visible for reference and open to challenge; do not request approval for each one or manufacture alternatives. A recommendation is still a proposal until accepted with the design.
- **Choices to discuss:** Viable alternatives have materially different consequences, a product preference is missing, or evidence is insufficient to choose responsibly. Explain the question, compare the meaningful options, recommend one when justified, and ask the user to choose. Confidence alone does not resolve a missing requirement or authorize optional scope.

For an open choice, use a plain-language question as the title, such as "Generate the export now or in the background?" Briefly explain what changes for the user or system. When comparison helps, use this adaptable table:

| Option | What changes | Benefit / consequence |
| --- | --- | --- |
| Generate now | The request returns the file | Immediate download; the request waits for generation |
| Generate in the background | A job prepares the file for later retrieval | Supports longer work; requires job handling and a way to retrieve the result |

Follow with a short recommendation grounded in inspected constraints and one focused question. Do not assume either option is appropriate without checking the feature's needs. Mention concrete costs or boundaries, not vague claims such as "more scalable". Include references where they support the choice; avoid turning every decision into a formal record with repeated fields for ownership, evidence, rationale, and revisit conditions.

Once the user chooses, retain the outcome and its important consequence in the reference list, clearly marked as agreed. Drop the comparison unless its rationale remains useful. Promote a recommended choice into discussion if the user challenges it or new evidence reveals a material tradeoff. If there are no open choices, omit that section instead of inventing questions.

## Review choices interactively

Show the proposed flow, change inventory, and recommended choices before walking through open decisions, unless a blocking question prevents a useful overview.

- Use the host's available interactive question tool when its usage rules permit; otherwise ask the same focused question in ordinary chat. Do not build a separate interface or assume a particular tool exists.
- Present one open decision at a time. Explain its context and consequences in the conversation, then offer concise, concrete options in the question card. Put the recommended option first and label it when there is a justified preference. Avoid repeating a long comparison inside the card.
- Allow a free-text response for questions, examples, alternative approaches, or changed constraints. Use the host's built-in free-text facility when available; otherwise accept those responses in chat. A request to explore is discussion, not a selection.
- Keep recommended choices in the overview unless the user challenges one. Do not turn every reference decision into a mandatory question card.
- Wait for the user's answer before settling the decision or advancing to a dependent choice. An asynchronous tool returning, a preselected option, silence, or an empty response is not acceptance. If the user defers a choice, leave it visibly open and discuss independent choices where useful.
- After a selection, record it as agreed, update affected design sections, and continue to the next open choice. Let the user revisit a decision by its short descriptive name at any time; reconcile dependent choices if the answer changes.

End the review with the consolidated design and any unresolved choices. Deferred consequential decisions still need resolution before the draft is ready for task planning.

## Keep one current design

Use the title `Pre-plan: [Feature name]` so the draft can pass directly to plan. Organize it as follows:

- **Expected result:** Behavior, scope limits, and minor assumptions.
- **Proposed flow:** Entry points, responsibilities, and existing, modified, or new pieces using plan's flow format.
- **Change inventory:** What changes and why, using plan's inventory columns.
- **Design choices:** Recommended choices (including clearly marked agreed decisions) for reference, then choices to discuss when user input is needed.
- **Deliberately omitted:** Consequential possibilities left out and why they are unnecessary for this intent. Omit when there is no meaningful omission.

The inventory includes necessary tests and companion changes, with core/non-core classification and optional-work approval status. Make important responsibilities and contracts understandable through the flow and choices. Surface effects on users, shared callers, or neighboring behavior where they matter to a decision; deferring the formal blast-radius assessment must not conceal a consequence that would change the user's choice.

Keep the draft proportional to the feature. Use concrete examples or short behavioral pseudocode where they resolve ambiguity. Omit empty sections and rejected alternatives that no longer explain a decision. Leave local naming, private helper extraction, and test organization to the builder.

## Finish at the architecture boundary

Before presenting the complete draft, check that its sections agree and that proposed additions and claimed effects have adequate evidence. State unavailable evidence explicitly. An interim draft may contain open questions; a draft ready for tasks must not hide decisions that would materially change behavior, architecture, or scope. If further progress needs user input or unavailable context, name the specific blocker and stop.

Once ready, present the complete design with its rationale and invite refinement or `/plan`. Acceptance alone does not start task planning. An explicit request to create tasks, including `/plan`, accepts the latest complete, unambiguous draft and hands off to plan; it does not answer open questions. Plan carries this architecture forward, rechecks consequential assumptions, and adds the detailed blast-radius assessment, execution rules, and tasks.

For a fresh session, the user must supply the complete draft. Do not assume access to the earlier conversation or save it automatically.
