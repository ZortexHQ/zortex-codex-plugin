# Zortex Codex Bootstrap Plugin

This public Codex plugin teaches a fresh Codex installation the exact Zortex
package name and the safe Windows onboarding route. It contains instructions
only: no connector, MCP server, user data, token, or provider credential.

## Install once

In Codex CLI, add the public marketplace and install the plugin:

```powershell
codex plugin marketplace add ZortexHQ/zortex-codex-plugin --ref main
codex plugin add zortex-bootstrap@zortexhq
```

Start a new Codex conversation, then say:

> Install Zortex and connect my Gmail and Google Drive. Use the Starter path,
> not a full-history sync.

The plugin runs `npm install --global @zortex-hq/install` only for an explicit
Zortex installation or connection request, then runs the receipt-backed agent
onboarding command. If Zortex is already installed, it skips npm installation.

## What it can connect

`gmail`, `google-drive`, `notion`, `posthog`, and `outlook` support bounded,
queryable Starter reads. `whatsapp` is metadata-only. Chrome history and
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
