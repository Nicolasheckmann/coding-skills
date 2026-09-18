---
name: creating-great-skills
description: Create, review, and improve OpenCode agent skills. Use when the user asks to create, write, structure, audit, debug, or maintain a SKILL.md, reusable agent skill, or skill library.
---

# Creating Great Skills

Create skills that give an agent the right context at the right time without
replacing its judgment. A good skill is a router: it is easy to discover,
loads only relevant guidance, and points the agent toward deeper context when
needed.

## Decide Whether a Skill Is Warranted

Create or extend a skill when the work is repeated, error-prone,
context-heavy, poorly handled by agents by default, or suitable for autonomous
execution. Prefer improving an existing skill when it already owns the same
trigger and outcome.

Do not create a skill merely to restate knowledge the agent can reliably infer
or retrieve. Each skill adds discovery noise, context cost, and maintenance.

Before authoring, inspect the available tools, existing skills, and evidence
from prior attempts. Ask the user only for decisions or context that cannot be
derived.

## Define the Contract

Be precise about:

- **Trigger:** The user intents, terms, files, or situations that should load
  the skill, including important exclusions.
- **Outcome:** What successful completion looks like from the user's point of
  view.
- **Constraints:** Safety boundaries, required conventions, and actions to
  avoid.
- **Non-derivable context:** Project-specific locations, terminology, tools,
  schemas, and known pitfalls the agent would otherwise have to guess.
- **Verification:** Observable checks that demonstrate the result works.

Stay flexible about:

- The exact sequence of steps when the agent can adapt to the situation.
- Failures that can be diagnosed from their runtime evidence.
- Volatile details such as line numbers, file lists, versions, counts, and
  example output.

Over-specification turns a skill into a brittle workflow. Supply intent and
guardrails, then preserve room for reasoning.

## Design for Progressive Disclosure

Keep the always-visible name and description focused on discovery. Keep
`SKILL.md` focused on routing and the core workflow. Move detailed material
into `references/` and deterministic operations into `scripts/` only when the
extra files materially reduce noise or improve reliability.

A skill should reveal information in layers:

1. **Name and description:** Decide whether to load the skill.
2. **Core instructions:** Explain the outcome, constraints, and route through
   the task.
3. **References and scripts:** Provide specialized detail only when needed.

Do not split a small, cohesive skill into extra files merely to imitate this
structure.

## Write Discoverable Frontmatter

An OpenCode skill lives at either:

```text
.opencode/skill/<skill-name>/SKILL.md
~/.config/opencode/skill/<skill-name>/SKILL.md
```

The folder and `name` must match. Use a lowercase, hyphen-separated name of at
most 64 characters. Name the file exactly `SKILL.md`.

Write the description in the third person. State both what the skill does and
when to use it. Front-load literal terms users are likely to say, including
relevant filenames or tools. Use `Use ONLY when...` when nearby requests must
not trigger the skill.

```markdown
---
name: example-skill
description: Create and review example artifacts. Use when the user asks to draft, validate, or improve an EXAMPLE.md file.
---
```

Avoid vague descriptions such as "Helps with examples." Avoid long feature
inventories that consume context and blur the trigger.

## Organize the Skill

Present guidance from intent to detail:

1. State the skill's purpose and successful outcome.
2. Explain when to create, extend, or decline the requested work.
3. Define the core workflow without scripting every move.
4. Record constraints and context the agent cannot derive.
5. Specify verification and stopping conditions.
6. Route to optional references or scripts where applicable.

Use direct instructions and concrete examples where they resolve ambiguity.
Explain why a surprising constraint exists. Remove generic advice, repeated
rules, decorative prose, and instructions already guaranteed by the host
agent.

## Validate Behavior

Validate the skill as behavior, not only as Markdown:

- Confirm its folder, filename, frontmatter, and name match OpenCode's format.
- Test representative prompts that should trigger it.
- Test adjacent prompts that should not trigger it.
- Run the skill on a realistic task and verify the resulting artifact or
  observable outcome.
- Review the run for missing context, wasted reads, brittle instructions, and
  repeated mistakes.
- Ask what would have helped the agent perform better, then make the smallest
  useful revision.

Do not claim the skill works solely because its prose appears complete.

## Prevent Skill Rot

Separate durable guidance from volatile facts. Point volatile instructions to
an authoritative local source when one exists rather than duplicating it.
Prefer regenerating or rewriting a degraded section from a clear contract over
accumulating exception after exception.

Update the skill when repeated runs expose the same failure, when its source
of truth changes, or when OpenCode's skill format changes. Remove instructions
that are no longer necessary.

## Review Checklist

- Is this recurring or costly enough to deserve a skill?
- Does an existing skill already own this responsibility?
- Will likely user wording discover the skill?
- Are the goal, constraints, non-derivable context, and verification explicit?
- Does the skill preserve agent judgment about implementation details?
- Is detailed context disclosed only when needed?
- Are volatile facts sourced rather than copied where practical?
- Can success be demonstrated on a realistic task?
- Can anything be removed without reducing reliability?
