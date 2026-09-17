---
name: author-skills
description: Creates and refines reusable agent skills from domain knowledge, working examples, or existing instructions. Use when asked to author a SKILL.md, extract a workflow or philosophy into a skill, or improve skill discovery and effectiveness.
---

# Author Skills

Turn reusable knowledge into concise instructions that help an agent make better decisions. Produce skills that work across models and agent environments; make any necessary environment dependencies explicit.

## Establish the purpose

Use the user's request, supplied sources, existing conversation, and repository conventions to identify:

- The tasks the skill should handle and the requests that should activate it.
- The knowledge, preferences, or procedures the agent would otherwise lack.
- The observable difference between a successful result and a failure.
- Any actual constraints on tools, output, permissions, or runtime.

Ask only for missing information that would materially change the skill. Do not ask the user to repeat supplied context. When updating an existing skill, inspect its instructions and supporting resources before changing them.

For a philosophy or preference skill, translate beliefs into decision criteria: when the principle applies, which tradeoff it favors, and what exceptions matter. Preserve the user's rationale and distinguish defaults from requirements. Do not invent beliefs or turn one example into a universal rule.

## Define evaluations before expanding the instructions

Start with three realistic scenarios covering the main behavior and a meaningful variation or boundary. For each, record the request, necessary input artifacts, expected observable behavior, and what would count as failure. Include a nearby request that should not activate the skill when discovery boundaries matter.

Use supplied examples or observed failures where available. For a narrow update, focus on the affected behavior rather than rebuilding an entire evaluation suite.

When execution is available, establish how an agent performs without the skill, then compare performance with it. Keep the request and artifacts comparable. Otherwise, record proposed evaluations and clearly state that the baseline has not been measured. Do not invent results.

Evaluate outcomes and decisions, not whether the agent repeats particular headings or wording. Write only enough guidance to address the identified gaps.

## Write the smallest useful skill

Assume the agent is capable. Include domain knowledge, non-obvious constraints, useful decision criteria, and procedures that improve execution. Remove generic tutorials, repeated instructions, speculative edge cases, and explanations that do not change behavior.

Match specificity to the task:

- **High freedom:** Describe the outcome and decision criteria when several approaches are valid.
- **Medium freedom:** Supply a preferred pattern, adaptable template, or parameterized script when consistency helps but context varies.
- **Low freedom:** Specify exact steps or validated scripts when deviation would cause a concrete correctness or safety problem.

Give one useful default with a clear condition for an alternative instead of listing every possible approach. Mark templates as required or adaptable. Use concrete input/output examples when they communicate a distinction more effectively than prose.

Preserve the requested scope and existing authorization. A skill should not introduce unrelated work or imply permission for external actions.

## Use portable metadata and structure

Create the skill in the user's chosen location or follow the repository's established skill directory. If neither establishes a destination, clarify where the skill belongs instead of assuming a vendor-specific installation path. Name the directory after the skill.

The minimum artifact is `<skill-name>/SKILL.md`, beginning with YAML frontmatter:

```yaml
---
name: reviewing-migrations
description: Reviews database migrations for compatibility, ordering, and recovery risks. Use when asked to assess a migration or review schema changes before deployment.
---
```

For the portable core:

- Use a descriptive name of at most 64 characters containing lowercase letters, digits, and hyphens. Follow the collection's naming style; avoid vague names such as `helper` or `utils`.
- Write a non-empty description of at most 1,024 characters in third person. State what the skill does and when it applies, using terms likely to appear in real requests.
- Keep markup such as XML tags out of the name and description. Put procedures in the body, not the description.
- Preserve relevant existing metadata when editing. Add host-specific metadata only when the target environment requires or the user requests it.

Start with one self-contained file. Add resources only when they have a concrete use:

```text
skill-name/
  SKILL.md
  references/    # Conditional guidance, schemas, or substantial examples
  scripts/       # Reusable executable helpers
  assets/        # Templates or other files used in generated output
```

Do not create empty directories, unused scaffolding, or duplicate documentation.

## Make detail easy to discover

Design for progressive disclosure: metadata supports selection, the entrypoint supplies essential instructions, and supporting files provide detail only when needed. Do not assume every host implements loading in the same way.

Keep the body of `SKILL.md` comfortably below 500 lines; that is a ceiling to work beneath, not a target. Move substantial conditional guidance into references before it obscures the main instructions.

- Link supporting references directly from `SKILL.md` and explain when to read each. Avoid chains of references that hide essential instructions.
- Organize resources by task, domain, or operating mode. Read only the parts relevant to the request.
- Give each fact one authoritative home instead of duplicating it across files.
- Use descriptive filenames and relative paths with forward slashes.
- Add a contents list near the top of references longer than about 100 lines so partial reads reveal their scope.

Keep terminology consistent. Avoid calendar-based branches and unsupported claims about what is currently available. Include version constraints or legacy guidance only when the workflow needs them, and keep legacy details separate from the main path.

## Make workflows verifiable

For complex tasks, provide a clear sequence with decision points and observable completion criteria. Use a checklist when it prevents consequential omissions; do not force one onto simple work.

Build feedback into quality-critical work: produce an output, validate it, correct specific failures, and validate again. Define when to stop and report a blocker if the same failure persists or requires unavailable information, capabilities, or authorization.

For batch changes, destructive operations, or complex mapping rules, use a reviewable intermediate artifact where it helps: analyze inputs, record proposed changes, validate them, apply them within authorization, then verify the result. Validation passing does not itself grant permission to execute.

When appearance is part of correctness, include rendering and visual inspection if the environment supports them. State any verification limitation when it does not.

## Add executable helpers only when useful

Use scripts for repeated logic or operations where deterministic execution materially improves reliability. For skills without executable helpers, omit this machinery.

- Document required runtimes and packages, inputs, outputs, and how to run the helper relative to the skill directory.
- Check dependencies before relying on them. Do not assume network access, package installation, a shell, or a particular operating system is available.
- Distinguish instructions to execute a script from instructions to read its implementation as reference.
- Handle expected errors explicitly. Provide actionable diagnostics and a failure status when the requested result cannot be produced; do not silently replace missing required input with fabricated defaults.
- Document the reasons for non-obvious constants and configuration choices.
- Keep retries bounded and safe for the operation; do not repeat external mutations blindly.
- Run new or changed scripts on representative inputs and relevant failure cases.

For connected tools, describe the required capability and identify the exact tool exposed by the target environment. Use its fully qualified identifier when supported; do not prescribe one universal tool-name syntax. If the capability is unavailable, explain what is missing or use a suitable available alternative within scope.

## Validate, observe, and refine

Before delivery, check:

- The frontmatter parses, the name matches the directory, and the description distinguishes intended requests from unrelated ones.
- Instructions retain the user's choices and contain no unfinished placeholders or contradictory rules.
- Referenced files exist, links resolve, and conditional guidance is reachable from the entrypoint.
- The skill assumes no particular model family, vendor, installation path, tool syntax, or unsupported capability unless the task explicitly requires it.
- Included scripts and critical outputs have been verified with appropriate checks.

Use an available structural validator when appropriate, but do not confuse format validity with behavioral effectiveness.

When behavioral testing is available and authorized, test in a fresh agent session with the skill, a realistic request, and only the necessary input artifacts. Keep the expected answer and authoring discussion out of that session. Use an isolated workspace for test outputs where appropriate.

Evaluate across the models and environments the user intends to support, if accessible. Check whether faster models receive enough guidance and stronger models avoid needless procedural overhead. Report which combinations were actually tested; portable wording alone does not establish cross-model compatibility.

Observe whether the skill activates appropriately, whether the agent follows relevant references, and whether it produces the intended result. Repeatedly missed guidance may need clearer routing or prominence; unused resources may be unnecessary. Refine narrowly from observed failures and rerun the affected scenarios.

Deliver the completed skill, identify its location, summarize consequential design choices, and distinguish structural checks from behavioral tests that were run or remain proposed.

## Source

Adapted from the user-supplied [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices). Model-specific examples and runtime assumptions have been generalized; the linked article is provenance, not a required runtime dependency.
