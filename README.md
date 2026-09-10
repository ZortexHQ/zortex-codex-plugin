# Zortex Codex Bootstrap Plugin

This public Codex plugin teaches a fresh Codex installation the exact Zortex
package name and the safe Windows onboarding route. It contains instructions
only: no connector, MCP server, user data, token, or provider credential.

Privacy policy: [PRIVACY.md](PRIVACY.md).

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
`npm install --global @zortexhq/install@latest --no-audit --no-fund` before
the receipt-backed agent onboarding command. This intentionally replaces stale
Zortex npm shims rather than trusting that a `zortex` command on `PATH` is
current. Onboarding authorizes every released source without reading messages or
files. Content is adaptively paged only after the user asks a question.

## What it can connect

`gmail`, `google-drive`, and `outlook` are the released automatic Windows
sources. Notion and PostHog enter connector fabrication; WhatsApp and WeChat
are excluded from this Windows release.

When Zortex returns `NEEDS_ATTENTION`, Codex reports the precise provider
blocker rather than treating it as success. It never sends a credential in a
command argument or chat message. Once a queryable source reaches
connected authorization, Codex chooses a question-sized provider search batch,
increases it only when evidence is insufficient, and uses `--all` only for an
explicit complete request. It never scans unrelated account data.

## Remove

```powershell
codex plugin remove zortex-bootstrap
codex plugin marketplace remove zortexhq
```

Removing this bootstrap plugin does not remove Zortex or erase local data.
Use the Zortex connection rollback receipt for a specific connected source.
