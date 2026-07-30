# claude-plugins

Monorepo of Claude Code plugins for georgejinu-labs. Each plugin lives under
`plugins/<name>/` with its own `.claude-plugin/plugin.json` manifest.

Published via the companion marketplace repo:
https://github.com/georgejinu-labs/claude-marketplace

## Plugins

- `plugins/dev-toolkit` — commit hygiene, debugging, dependency audits, doc
  sync, env checks, SQL review, test generation, todo sweeps.

## Adding a new plugin

1. `mkdir -p plugins/<name>/.claude-plugin`
2. Add `plugin.json`, plus `skills/`, `commands/`, `agents/`, `.mcp.json` as needed.
3. Validate locally: `claude plugin validate plugins/<name> --strict`
4. Add a matching entry to `claude-marketplace`'s `marketplace.json` (source
   type `git-subdir`, `path: plugins/<name>`).
5. Push to `main` — the `notify-marketplace` workflow detects the changed
   plugin directory and dispatches an update to the marketplace repo, which
   bumps the pinned `sha` automatically.

## CI setup required

This repo's `notify-marketplace` workflow needs a repo secret
`MARKETPLACE_DISPATCH_TOKEN` — a fine-grained GitHub PAT scoped to the
`claude-marketplace` repo only, with **Contents: Read and write** permission
(needed to hit the `dispatches` API on that repo).
