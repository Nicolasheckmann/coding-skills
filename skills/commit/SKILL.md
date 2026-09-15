---
name: commit
description: Create meaningful conventional commits for the current branch modifications. Use when the user asks Codex to commit current changes, split changes into atomic commits, or prepare commits after checking status, diffs, lint, and related tests.
---

# Commit

Create meaningful commits for the current branch modifications.

Use plain multiline `-m "..."` commit messages. Do not use heredoc command substitution such as `$(cat <<'EOF'...)`, because it can trigger command substitution permission prompts.

First inspect the current changes:
- run `git status`
- run `git diff --cached`
- run `git diff`

Run `bundle exec rubocop -a` on changed Ruby files when applicable to auto-fix lint issues.

Run `bundle exec rspec` on related spec files when applicable to verify nothing is broken.

Show the user a summary of changes before committing.

Analyze the changes and create well-structured, atomic commits with clear and descriptive commit messages following conventional commit format, such as `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, or `chore:`.

If there are multiple logical changes, split them into separate commits. Stage and commit the changes accordingly.
