# commit-and-push

A Claude Code skill that automatically generates a git commit message and pushes — no confirmation prompt, no friction.

Inspired by the **Commit and Push** feature in [OpenAI Codex](https://github.com/openai/codex).

## What it does

1. Reads your staged diff
2. Generates a concise `type: message` commit message internally
3. Runs `git commit` + `git push` immediately
4. Reports what was committed on success
5. Only asks you if there's a conflict, hook failure, or push rejection

## Install

Copy `SKILL.md` into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/commit-and-push
curl -o ~/.claude/skills/commit-and-push/SKILL.md \
  https://raw.githubusercontent.com/omrynskyi/commit-and-push/main/SKILL.md
```

## Usage

```
/commit-and-push
```

Stage your files first (`git add`), then run the skill. Claude handles the rest.

## Commit message format

```
<type>: <what changed>
```

Types: `feat`, `fix`, `refactor`, `test`, `chore`, `docs` — max 72 characters.
