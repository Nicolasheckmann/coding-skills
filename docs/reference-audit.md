# Skill reference audit

## Resolved

| Reference | Resolution |
| --- | --- |
| `/plan-v3` in `intent` and `save-intent` | Updated all three references to `/plan`, matching `skills/plan/SKILL.md`. |
| `.opencode/agents/reviewer.md` in `review` | Bundled the supplied contract at `skills/review/references/reviewer.md` and linked it relative to the skill. Copy the complete skill directory during installation. |
| Holivia reviewer identity | Generalized to implementation-plan tasks. |
| OpenCode session in `save-intent` | Generalized to a coding-agent session. |
| `ErrorLogger` in the reviewer contract | Made conditional on the target repository providing it; no shared `ErrorLogger` file is needed. |
| Checklist-only wording in `review` | Included status markers, matching the plan template's `Status: Pending` format. |

## References that already resolve

- `/plan`, `/save-plan`, `/save-intent`, and `/review` each have a corresponding `skills/<name>/SKILL.md`.
- `plan/next/` and `intent/` are output locations in the user's target repository. They do not require directories or placeholder files in this repository.
- Repository rules, implementation-plan paths, and verification files come from the target project or the user.

## Installer follow-up

- Map or omit the imported `agent` and `subtask` frontmatter according to the selected harness. `plan` and `build` agent roles are separate from the skills with those names.
- For a harness that uses a named `reviewer` agent, register the bundled reviewer contract using that harness's agent configuration. Merely copying a Markdown reference does not register an agent or enforce its permissions.
- The reviewer contract retains the supplied OpenCode `mode` and `permission` configuration. Translate supported settings when installing for another harness; do not assume reading the contract enforces those settings.
- Handle `$ARGUMENTS` and slash-command invocation syntax according to the selected harness. The skill names themselves are current.
- No additional source file is currently missing. Harness registration and invocation support remain installer work.
