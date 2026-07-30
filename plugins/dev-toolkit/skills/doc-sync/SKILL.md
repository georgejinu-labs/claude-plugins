---
name: doc-sync
description: Check whether README/CLAUDE.md/docs have drifted out of sync with recent code changes, and flag exactly what's stale.
disable-model-invocation: true
---

# Doc sync check

## Working tree
!`git status --short`

## Recent commits
!`git log --oneline -15`

## Instructions
1. Look at what changed recently (staged/unstaged diff, and the last several commits if the tree is clean) — new files, renamed modules, changed CLI commands/flags/scripts, changed config keys, removed features.
2. Read the project's README.md, CLAUDE.md, and any docs/ directory for claims that reference what changed: file paths, commands, setup steps, architecture descriptions, env vars.
3. For each doc claim that no longer matches the code, report the doc file + line, what it currently says, and what's actually true now.
4. Don't rewrite the docs yourself unless asked — report findings first so the user can confirm scope, then apply edits if they say go ahead.
5. Ignore purely stylistic drift (minor wording) — focus on claims that would mislead or break for someone following the docs.

## Guidelines
- If there's no README/CLAUDE.md in the project, say so rather than inventing one.
