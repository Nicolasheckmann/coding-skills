---
name: save-intent
description: Save the latest agreed product intent under intent/ for a fresh planning session
agent: build
---

Save the latest complete product intent from this conversation as a Markdown file under `intent/` in the repository root.

Additional user instructions:

$ARGUMENTS

## Select the agreed content

- Read the current conversation and identify the latest complete intent, including explicitly agreed revisions.
- Treat this save request as acceptance of that draft. Do not add a redundant approval step when the content and destination are unambiguous.
- If no complete intent is available, multiple versions are ambiguous, or an agreed revision has not been resolved into clear content, ask for clarification before writing.
- If consequential product questions remain that could materially change the outcome or scope, ask for their resolution before saving. Explicit minor assumptions and technical questions reserved for planning may remain.

## Save faithfully

- Use a concise kebab-case filename based on the intent name: `intent/<feature-name>.md`.
- Inspect the destination before writing. Create the `intent/` directory if needed.
- If a file already exists at the chosen path, inspect it. If it already contains the agreed content, report that without rewriting it. Otherwise, ask whether to update that file or choose another name unless the user has explicitly requested that specific update.
- Preserve the agreed content and important rationale. Do not invent requirements, resolve open questions, expand scope, or add architecture and implementation tasks while saving.
- Save only the intent document, not the surrounding conversation or approval messages. Ensure it stands alone without references such as "as discussed above". If replacing such a reference requires interpreting an unresolved decision, ask rather than guess.
- Do not change other files or commit.

## Verify and report

- Read the saved file and confirm it matches the agreed intent, including explicit assumptions and planning questions.
- Report the exact saved path.
- Briefly remind the user of their chosen handoff: start a fresh coding-agent session, ask it to read the saved file, then invoke `/plan` themselves.
- Stop. Do not generate a plan or start implementation.
