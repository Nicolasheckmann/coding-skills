# Coding skills

Reusable coding-agent instructions. Each skill lives in `skills/<name>/SKILL.md`,
with supporting files inside the same directory.

## Link skills for Codex

Requires Ruby, with no additional gems. From this checkout:

```sh
./scripts/manage-skills --dry-run
./scripts/manage-skills
```

The script links each complete skill directory into `~/.agents/skills/`, making
the skills available to Codex across projects. You can also invoke the script by
its absolute path from any folder; it locates the skills relative to itself.

- Existing links to the same skill are left unchanged.
- Existing files, directories, or different links are reported as conflicts and
  preserved. Other skills are still linked. The script exits with status 1 if
  there are conflicts or errors; inspect and move conflicting entries yourself
  before rerunning it.
- `--dry-run` reports planned links and conflicts without changing files.
- `--destination DIR` overrides the global skills directory.

Edits in this checkout are visible through the links without reinstalling. Keep
the checkout at its current location so links remain valid. Start a new Codex
session after installation to check discovery.

This command installs skill links only. Harness-specific command wrappers,
reviewer-agent registration, and permission configuration remain tracked in the
[reference audit](docs/reference-audit.md).
