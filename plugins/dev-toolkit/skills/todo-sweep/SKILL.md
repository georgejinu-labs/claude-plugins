---
name: todo-sweep
description: Scan the codebase for TODO/FIXME/HACK/XXX comments and summarize them by file, age, and apparent priority.
disable-model-invocation: true
---

# TODO sweep

## Instructions
1. Search the codebase for TODO, FIXME, HACK, XXX, and similar markers (case-insensitive), excluding vendored/dependency directories (node_modules, .venv, vendor, dist, build).
2. For each hit, capture file:line, the comment text, and — if git is available — the commit/date it was introduced (`git log -1 --format=%ad -- <file>` or `git blame` near that line) to gauge age.
3. Group results by rough priority signal: explicit "FIXME"/"HACK" outrank plain "TODO"; anything mentioning security, auth, secrets, or data loss gets called out first regardless of marker type.
4. Present as a short table (file:line, age, note) grouped by directory/module, not a flat unsorted dump.
5. Don't resolve or delete any of them yourself — this is a triage report, not a cleanup task, unless the user asks you to act on a specific one afterward.

## Guidelines
- Skip generated/vendored code paths.
- If there are dozens of hits, summarize counts per directory first, then show the oldest/highest-priority ones in full rather than dumping everything.
