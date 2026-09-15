---
name: intent
description: Guide a rough idea through focused questions into a clear, self-contained product intent
agent: plan
---

Help the user turn a rough idea into an agreed product intent through conversation.

Use the existing discussion and any additional input:

$ARGUMENTS

Stay in planning mode. Do not edit files, save the intent, implement anything, or produce implementation tasks. The user will save the intent separately with `/save-intent`, then start a fresh session, read that file, and invoke `/plan` themselves.

## Guide the conversation

1. Briefly state what you understand about the problem and desired outcome. Use what the user has already explained; do not ask them to repeat it. If no idea is available yet, ask what problem or opportunity they want to explore.
2. Identify the most consequential missing information. Usually ask one or two questions per reply, at most three. Work through the decisions over multiple rounds rather than presenting a questionnaire.
3. Begin with the problem, affected people, and why it matters. Then clarify the desired outcome, main flow, important business rules, constraints, and scope boundaries as needed. Adapt to the idea; do not mechanically ask about every section.
4. Use open questions to discover needs. When a genuine choice emerges, offer two or three concrete options, explain the trade-offs briefly, and recommend one. Allow the user to propose another direction.
5. Distinguish the underlying need from a suggested solution. Explore whether the proposed solution addresses the problem without dismissing decisions the user has already made.
6. Inspect relevant code and repository rules when useful to establish current behavior, existing capabilities, or constraints. Answer repository questions through inspection rather than asking the user. Keep research proportionate; this is product discovery, not architectural planning.
7. Distinguish user-reported observations, verified facts, and hypotheses. Do not turn a suspected cause into an established fact or invent supporting metrics.
8. Resolve important boundary cases with concrete examples. Ask about permissions, failure behavior, timing, or confidentiality when they materially affect this idea, not as a generic checklist.
9. Briefly reflect back consequential decisions as the discussion evolves. Capture rationale when forgetting it could lead a future planner to choose the wrong solution.
10. After asking questions, stop and wait for answers. Do not fill consequential gaps with assumptions merely to finish the document.

## Keep intent separate from architecture

- The intent explains what problem to solve, for whom, why, and what the result must do.
- Leave class design, service boundaries, file inventories, task breakdown, and implementation commands to `/plan`.
- Preserve genuine technical constraints already agreed, such as an existing API contract or integration requirement. Distinguish mandatory constraints from implementation suggestions.
- If feasibility requires deeper technical investigation, record the specific question for planning. Do not promise feasibility without evidence.
- Surface contradictions between requested outcomes and established constraints. Ask the user to resolve product trade-offs rather than silently changing the requirement.

## Write a readable, standalone intent

Once the direction is clear, synthesize the decisions into a concise draft. It must make sense to a new session with no access to this conversation.

- Use plain language, short sentences, and concrete domain terms. Explain necessary technical terms.
- Capture decisions and important rationale, not the conversation transcript.
- Include relevant repository references or evidence only when they help establish current behavior or explain a constraint. Do not dump research notes.
- Give each fact one clear home. Success criteria should demonstrate the outcome rather than repeat the entire behavior section.
- Keep length proportional to the problem. Omit irrelevant sections rather than filling them with boilerplate.
- Do not invent numeric targets. Use observable outcomes when no measured target has been agreed.
- Include minor assumptions explicitly. Consequential product questions that could materially change the desired outcome or scope must be answered before the intent is presented as ready for planning.
- Technical feasibility questions may remain for planning if the product outcome is clear. Label them separately from unresolved product decisions.

Use this structure:

# Intent: [Name]

## Problem
[What happens today, who is affected, and why it matters. Distinguish evidence from hypotheses.]

## Desired outcome
[What should become possible or improve, without prescribing architecture.]

## Expected behavior
[Main flow and consequential business rules. Include examples for ambiguous boundaries.]

## Constraints
[Applicable permissions, confidentiality, compatibility, and other agreed requirements. Include important rationale alongside the requirement.]

## Out of scope
[Related work deliberately excluded.]

## Success criteria
- [Observable condition demonstrating that the problem is solved.]

## Assumptions and questions for planning
[Minor assumptions and specific technical questions, if any. Do not hide unresolved consequential product decisions here.]

## Agreement

Present the draft in the conversation and ask whether the user accepts it or wants changes. Incorporate corrections and present the complete revised intent when its content changes, so `/save-intent` can capture one unambiguous version.

Do not claim approval before the user gives it. A request to `/save-intent` can serve as acceptance of the complete draft. Once accepted, briefly confirm that it is ready for `/save-intent`. Do not save it, start a plan, or automatically hand off to another session.
