---
name: commit-and-push
description: Use when the user wants to commit and push changes to git. Generates a concise commit message from staged diff, runs git commands, retries with alternate message on failure.
model: haiku
---

# Commit and Push

Stage, commit, and push changes with a Claude-generated message. Retry on failure.

## Steps

**1. Check staged changes**

```bash
git diff --staged --stat
git diff --staged
```

If nothing staged, run `git status` and ask user which files to stage.

**2. Generate commit message (token-efficient)**

Using ONLY the diff output, write a single-line commit message:
- Format: `<type>: <what changed>` (e.g. `fix: handle null session in replay`)
- Types: `feat`, `fix`, `refactor`, `test`, `chore`, `docs`
- Max 72 characters
- No body unless the change is non-obvious
- Do NOT repeat file names — describe the intent

**3. Run git commands immediately — no confirmation**

```bash
git commit -m "<message>"
git push
```

Announce what you committed after success: `Done. Committed: \`<message>\` → pushed to origin/<branch>`

**4. On failure — retry logic**

| Failure | Action |
|---------|--------|
| `git commit` fails (hook, conflict) | Show error, generate alternate message or fix, ask user to confirm retry |
| `git push` fails (rejected, no upstream) | Show error, suggest `git push --set-upstream origin <branch>` or `git pull --rebase` then push, confirm with user before running |
| Any other error | Surface raw error to user, do not retry automatically |

**Never** use `--no-verify` or `--force` unless the user explicitly asks.

## Example Flow

```
You: /commit-and-push

Claude: [runs git commit + git push silently]
        Done. Committed: `feat: add token budget validation in session handler` → pushed to origin/main
```

## Token Budget

Keep the diff-to-message prompt internal — do not narrate it. No confirmation, just execute and report the result.
