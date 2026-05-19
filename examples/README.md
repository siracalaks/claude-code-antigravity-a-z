# Examples

Runnable extracts from the guide. Copy what you need; the inline guide explains the *why*, these files give you the *what*.

```
examples/
├── skills/         # Drop-in SKILL.md files for ~/.claude/skills/ or .claude/skills/
├── hooks/          # settings.json snippet with all hook examples from Chapter 8
└── workflows/      # GitHub Actions YAML from Chapter 13
```

## Skills

Each folder under `skills/` is a complete skill bundle. Install user-wide:

```bash
cp -r examples/skills/conventional-commit ~/.claude/skills/
```

Or project-wide (committed to your project repo):

```bash
mkdir -p .claude/skills
cp -r examples/skills/conventional-commit .claude/skills/
```

Then in Claude Code: `/skills` to list, or just ask Claude to commit and it will pick up `conventional-commit` from the description.

| Skill | What it does |
|---|---|
| [`conventional-commit`](skills/conventional-commit/SKILL.md) | Writes git commit messages in Conventional Commit format. |
| [`run-tests`](skills/run-tests/SKILL.md) | Runs tests matching a pattern, detects framework, proposes fixes on failure. |
| [`clean-imports`](skills/clean-imports/SKILL.md) | Removes unused imports and sorts the rest. |
| [`pr-description`](skills/pr-description/SKILL.md) | Generates a PR description from the current branch diff. |

## Hooks

[`hooks/settings.example.json`](hooks/settings.example.json) is a reference `~/.claude/settings.json` with the six hook examples from Chapter 8:

1. Auto-format on file edit (Prettier)
2. Block dangerous shell commands (`rm -rf`, fork bombs, ...)
3. Block reads/writes to secret files (`.env`, `.pem`, `.key`)
4. SessionStart context loader (git status + TODO scan)
5. Test-on-commit enforcement (block `git commit` if tests fail)
6. macOS notification on session end

⚠️ **Don't copy the file wholesale** — review each hook against your workflow, then merge selectively into your own `settings.json`.

## Workflows

[`workflows/doc-update.yml`](workflows/doc-update.yml) is the GitHub Action from Chapter 13's self-updating documentation agent. Drop it into `.github/workflows/` of any repo that uses Claude Code, set the `ANTHROPIC_API_KEY` secret, and you'll get a daily PR that proposes doc updates.

> The `/doc-updater` slash command it invokes is defined in Chapter 13 of [`GUIDE.md`](../GUIDE.md#13-self-updating-agent). Install that skill first, then enable the workflow.
