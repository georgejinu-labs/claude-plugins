# claude-plugins

Monorepo of Claude Code plugins for georgejinu-labs. Each plugin lives under
`plugins/<name>/` with its own `.claude-plugin/plugin.json` manifest.

Published via the companion marketplace repo:
https://github.com/georgejinu-labs/claude-marketplace

## Plugins

- `plugins/dev-toolkit` — commit hygiene, debugging, dependency audits, doc
  sync, env checks, SQL review, test generation, todo sweeps.
- `plugins/knowledge-capture` — `/doc-learning` captures something you just
  learned and publishes it to Confluence via the Atlassian Rovo connector.
  Requires the Atlassian claude.ai connector to be authorized with Confluence
  access (Jira-only scopes aren't enough).

## Adding a new plugin

1. `mkdir -p plugins/<name>/.claude-plugin`
2. Add `plugin.json`, plus `skills/`, `commands/`, `agents/`, `.mcp.json` as needed.
3. Validate locally: `claude plugin validate plugins/<name> --strict`
4. Add a matching entry to `claude-marketplace`'s `marketplace.json` (source
   type `git-subdir`, `path: plugins/<name>`).
5. Push to `main` — the `notify-marketplace` workflow detects the changed
   plugin directory and dispatches an update to the marketplace repo, which
   bumps the pinned `sha` automatically.

## Installing plugins (for consumers)

One-time: register this marketplace with your Claude Code install.

```
claude plugin marketplace add georgejinu-labs/claude-marketplace
```

Then install any plugin from it (marketplace name is
`georgejinu-labs-marketplace`, from its `marketplace.json`):

```
claude plugin install dev-toolkit@georgejinu-labs-marketplace
claude plugin install knowledge-capture@georgejinu-labs-marketplace
```

`-s/--scope` controls where it's installed (`user` (default), `project`, or
`local`) — use `project`/`local` to scope a plugin to one repo instead of
your whole user profile.

## Updating plugins

The marketplace's pinned `sha` per plugin only updates automatically in the
marketplace repo itself (via `notify-marketplace` / `update-plugin-sha`).
Your local Claude Code install won't see a new version until you refresh:

```
claude plugin marketplace update georgejinu-labs-marketplace   # pull latest marketplace.json
claude plugin update dev-toolkit                                # update one plugin to that pinned sha
```

Restart Claude Code afterward — updates apply on next session start.

## Reinstalling plugins

There's no dedicated "reinstall" command; if a plugin's local state gets into
a bad state, uninstall then install it again:

```
claude plugin uninstall dev-toolkit
claude plugin install dev-toolkit@georgejinu-labs-marketplace
```

## Uninstalling plugins

```
claude plugin uninstall dev-toolkit
```

Useful flags:
- `--keep-data` — preserve the plugin's persistent data directory
  (`~/.claude/plugins/data/{id}/`) instead of wiping it.
- `--prune` — also remove any auto-installed dependencies no longer needed
  (pass `-y` to skip the confirmation prompt in non-interactive contexts).
- `-s/--scope` — uninstall from `user` (default), `project`, or `local`
  scope; must match the scope it was installed under.

## CI setup required

This repo's `notify-marketplace` workflow needs a repo secret
`MARKETPLACE_DISPATCH_TOKEN` — a fine-grained GitHub PAT scoped to the
`claude-marketplace` repo only, with **Contents: Read and write** permission
(needed to hit the `dispatches` API on that repo).
