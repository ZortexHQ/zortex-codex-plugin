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

An `oauth-browser` recipe opens the provider page and waits only for the person
to complete account login, MFA, and consent. A `stored-credential` recipe names
its exact missing credential path. A `local-observation` recipe uses the
inherited Windows user context. Do not pass credentials, verification codes,
paths, URLs, or user content in this workflow.

After connection, retain the receipt and check it before claiming readiness:

```powershell
zortex autopilot connect-status <receipt-id> --json
```

Only a queryable `STARTER_READY` source can provide cited context Q&A. Report
its exact `itemsRead` count; do not backfill history until the user asks. A
metadata-only source such as WhatsApp never makes chat content available for
Q&A.

If no installed recipe matches the requested app, normalize the name to a
lowercase kebab source id and run:

```powershell
zortex connect <source> --json
```

This files or joins the existing connector-skill D29 fabrication order. Report
it as being built, never as connected. The MCP bridge is read-only and never
starts, authorizes, or changes connectors.
