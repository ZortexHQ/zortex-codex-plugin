---
name: zortex-bootstrap
description: Connect a user-named application through local Zortex recipes instead of Codex built-in apps; known recipes run deterministic onboarding and unknown apps enter connector fabrication.
---

# Zortex connection recipes

Use this skill only when the user explicitly asks to install Zortex, connect an
application, inspect a Zortex connection, or roll one back. This is a
Windows-only workflow.

For a connection request, use local Zortex rather than `codex_apps.*` merely
because a built-in app is already authenticated. First install or refresh the
current public package:

```powershell
npm install --global @zortex-hq/install@latest --no-audit --no-fund
```

Then inspect the local declarative recipes:

```powershell
zortex autopilot recipes --json
```

Resolve the user's normal application name against `aliases`, then invoke only
the returned source IDs:

```powershell
zortex onboard --agent --host codex --source <resolved-id>... --json
```

An `oauth-browser` recipe identifies a possible native enrollment route; it
does not prove that a browser page opened. After onboarding, retain the receipt
and poll `zortex autopilot connect-status <receipt-id> --json` yourself with
bounded backoff. On `STARTER_READY`, report the exact `itemsRead` count and that
the existing authorization was usable; never say a browser was opened or ask the
user to report completion. On `NEEDS_ATTENTION`, report only its exact native
provider boundary. Never invent an authorization lock or ask permission to
restart Zortex. Never tell the user to run a shell command, paste a credential,
create an API key, or retry Zortex: execute ordinary installation, diagnostics,
and provider setup yourself. A `blocked` recipe reports its exact deferred
reason and stops; it is not connected.

WhatsApp is deferred from this Windows Preview. Do not open its window, request
a chat export, use WhatsApp Web, QR pairing, a local database, or a provider
API.

The receipt check is the readiness source of truth:

```powershell
zortex autopilot connect-status <receipt-id> --json
```

Only a queryable `STARTER_READY` source can provide cited context Q&A. Report
its exact `itemsRead` count; do not backfill history until the user asks.

If no released automatic recipe matches the requested app, normalize the name to a
lowercase kebab source id and run:

```powershell
zortex connect <source> --json
```

This files or joins the existing connector-skill D29 fabrication order. Report
it as being built, never as connected. The MCP bridge is read-only and never
starts, authorizes, or changes connectors.
