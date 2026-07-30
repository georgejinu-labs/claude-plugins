---
name: deps-audit
description: Check project dependencies (npm/pip/etc.) for outdated or vulnerable packages and summarize what's safe to bump vs. what needs care.
disable-model-invocation: true
---

# Dependency audit

## Instructions
1. Detect the package manager(s) in use by locating manifest files (package.json, requirements.txt, pyproject.toml, Pipfile, go.mod, Cargo.toml, etc.) — a project may have more than one, e.g. a Python backend plus a Node frontend.
2. For each manifest found, list dependencies and current pinned/installed versions.
3. Check for outdated or vulnerable packages:
   - Node: check for yarn.lock/pnpm-lock.yaml to know the actual package manager, then run its outdated/audit equivalent (`npm outdated`, `npm audit`, `pnpm outdated`, etc.).
   - Python: run `pip list --outdated` inside the project's own virtualenv (not global site-packages), and use an audit tool only if one is already part of the project's toolchain.
4. Summarize results as a table: package, current version, latest version, vulnerability severity if any, and whether the bump is major/minor/patch.
5. Flag major-version bumps separately and call out likely breaking changes from the changelog/release notes if easily found — don't just say "bump it."
6. Do not run installs/upgrades or edit lockfiles/manifests unless the user explicitly asks you to apply a specific upgrade.

## Guidelines
- Never install a new audit tool globally without asking first.
- If a vulnerability has no available fix yet, say so plainly rather than suggesting an unnecessary workaround.
