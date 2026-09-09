# Zortex Codex Bootstrap Plugin

This public Codex plugin teaches a fresh Codex installation the exact Zortex
package name and the safe Windows onboarding route. It contains instructions
only: no connector, MCP server, user data, token, or provider credential.

For supported source connection requests, it routes Codex to Zortex rather than
to an already-authenticated built-in Gmail or Google Drive app.

## Install once

In Codex CLI, add the public marketplace and install the plugin:

```powershell
codex plugin marketplace add ZortexHQ/zortex-codex-plugin --ref main
codex plugin add zortex-bootstrap@zortexhq
```

Start a new Codex conversation, then say:

> Connect my Gmail and Google Drive through Zortex, not Codex apps.

For every explicit Zortex installation or connection request, the plugin runs
`npm install --global @zortex-hq/install@latest --no-audit --no-fund` before
the receipt-backed agent onboarding command. This intentionally replaces stale
Zortex npm shims rather than trusting that a `zortex` command on `PATH` is
current.
Every named supported source automatically gets a bounded first read; when it
finishes, Codex reports the exact item count and lets the user decide whether
to read more history.

## What it can connect

`gmail`, `google-drive`, `notion`, `posthog`, and `outlook` automatically read
up to 10 bounded records at first connection. `whatsapp` is metadata-only. Chrome history and
computer files need a bounded selection; GitHub is currently blocked and
WeChat is excluded from this Windows path.

When Zortex returns `NEEDS_ATTENTION`, Codex reports the precise provider
blocker rather than treating it as success. It never sends a credential in a
command argument or chat message. Once a queryable source reaches
`STARTER_READY`, Codex can use Zortex's read-only MCP context bridge for cited
answers.

## Remove

```powershell
codex plugin remove zortex-bootstrap
codex plugin marketplace remove zortexhq
```

Removing this bootstrap plugin does not remove Zortex or erase local data.
Use the Zortex connection rollback receipt for a specific connected source.
