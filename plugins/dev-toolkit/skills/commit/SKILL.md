---
name: commit
description: Review staged changes, scan for secrets and PII, then commit staged files with a conventional commit message.
disable-model-invocation: true
---

# Session commit

## Staged changes
!`git diff --staged --stat`

## Working tree
!`git status --short`

## Instructions
1. If nothing is staged, stop and list unstaged/untracked files. Never stage files yourself.
2. Summarize staged changes in 2-4 bullets.
3. Run `git diff --staged` and scan for: hardcoded keys/tokens/passwords, real-looking PII/PHI, .env or credential files, debug prints. If found: STOP, list findings with file:line, do not commit.
4. If clean, write a conventional commit message (type(scope): summary) and run `git commit` with it. Commit only staged files - never use -a or extra paths. Show the resulting hash.
5. Flag any README.md or CLAUDE.md sections made stale by these changes.
6. End with one line: "next session, start with: ..."

## Guidelines
- Never run `git add` or `git push`.
- Never use `git commit --no-verify`.
- If the user says "commit anyway" after a scan finding, proceed and note the override in the commit body.