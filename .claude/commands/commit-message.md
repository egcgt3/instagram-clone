---
description: Create a commit message by analyzing git diffs
allowed-tools: Bash(git status:*), Bash(git diff --staged), Bash(git commit:*)
---

Analyse the staged git changes in this repository and craft a commit message following the rules below. Then present it to me for review — do NOT run `git commit` until I explicitly confirm.

## Steps

1. Run `git diff --staged` to see the full diff of staged changes.
2. Run `git status --short` to see which files are involved.
3. Run `git log --oneline -5` to match the existing commit style in this repo.
4. Determine the **type** of change and pick the matching emoji + prefix from the table below.
5. Write the commit message and show it to me as a code block.
6. Ask me: "Does this look good? Reply **yes** to commit, or give me feedback to revise."
7. Only run `git commit -m "..."` after I confirm with yes (or a similar affirmative).

## Emoji + Type Guide

| Emoji | Type | Use when... |
|-------|------|-------------|
| ✨ | `feat` | A new feature or capability is added |
| 🐛 | `fix` | A bug or broken behaviour is corrected |
| ♻️ | `refactor` | Code is restructured without changing behaviour |
| 💄 | `style` | UI/CSS changes — visual only, no logic |
| 📝 | `docs` | Documentation, comments, or README changes |
| 🧪 | `test` | Tests added or updated |
| 🔧 | `chore` | Build config, tooling, dependencies, CI |
| 🚀 | `perf` | A performance improvement |
| 🔒 | `security` | A security vulnerability is fixed |
| 🗑️ | `remove` | Dead code, files, or dependencies deleted |
| 🏗️ | `init` | Project scaffolding or initial setup |

## Commit Message Format

```
<emoji> <type>(<optional scope>): <short imperative summary>

<blank line>
<body — explain WHY this changed, not just what. What problem does it solve?
What would break without this change? Keep lines under 72 chars.>
```

### Rules

- Subject line: **≤72 characters**, imperative mood ("add", not "added"), no period at the end.
- Body is required when the change is non-trivial — skip it only for tiny one-liners like dependency bumps.
- Focus the body on **why**, not what (the diff already shows what).
- If multiple logical changes are staged, mention each briefly in the body.
- Do **not** add the `Co-Authored-By` trailer — I will handle attribution separately.

## Example Output

```
✨ feat(auth): add login page layout with Instagram branding

Scaffolds the login screen so users have a recognisable entry point.
The layout mirrors Instagram's split design — branding image on the
left, form on the right — giving the clone a realistic look from day
one.
```
