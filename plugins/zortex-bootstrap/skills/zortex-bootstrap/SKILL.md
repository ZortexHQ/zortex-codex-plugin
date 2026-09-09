---
name: zortex-bootstrap
description: Install the public Zortex Windows npm package and start bounded source onboarding only when the user explicitly asks to install Zortex or connect named Zortex sources. Do not use for general questions about Zortex.
---

# Zortex bootstrap

Use this skill only for an explicit user request to install Zortex, connect a
named Zortex source, check a Zortex connection, or roll one back. This is a
Windows-only workflow.

## First installation or connection

1. Map only sources the user named to these exact IDs:
   - Gmail -> `gmail`
   - Google Drive or Drive -> `google-drive`
   - Notion -> `notion`
   - PostHog -> `posthog`
   - Outlook or Microsoft Outlook -> `outlook`
   - WhatsApp -> `whatsapp`
2. Do not invent an ID for another app. Say that Chrome history and computer
   files need a bounded selection, GitHub is blocked by its credential rail,
   and WeChat is not part of this Windows path.
3. Confirm that the host is Windows and that `npm` is available. If either is
   unavailable, report the exact blocker and do not try another package manager.
4. If `zortex` is not on `PATH`, install exactly this public package:

   ```powershell
   npm install --global @zortex-hq/install
   ```

   Do not use `npx`, install a package with a similar name, or add a model,
   MCP action tool, browser extension, or connector package.
5. Run the explicit agent path, retaining the returned JSON receipt:

   ```powershell
   zortex onboard --agent --host codex --source <id>... --json
   ```

   Omit all `--source` flags when the user only requested installation. The
   normal path proves inherited Codex/Terminal access, configures Zortex's
   read-only MCP bridge, and automatically starts the bounded first read for
   each selected connector in the background; it does not use a model.
6. Report the receipt ID and state. Do not ask the user to request a Starter
   or choose a sync strategy. Do not claim data is ready until a queryable
   source reports `STARTER_READY`.

## Provider and readiness boundaries

- A `NEEDS_ATTENTION` result is a real source or permission blocker. Run only
  the relevant read-only diagnosis, such as `zortex auth diagnose google`, and
  return its exact next step.
- Never put a credential in a command argument, chat message, log, or skill.
  Do not retrieve OTPs or browser verification codes.
- A completed first read is bounded, not a full-history sync: Gmail reads up to
  10 messages under its 1 MiB body cap, Drive reads up to 10 file metadata
  records, Notion reads up to 10 page metadata records, PostHog reads up to 10
  Insights, and Outlook reads up to 10 messages. WhatsApp is metadata-only
  and does not enable chat-content Q&A.

## Status, questions, and undo

Use the receipt ID for a read-only status check:

```powershell
zortex autopilot connect-status <receipt-id> --json
```

After a queryable source is `STARTER_READY`, use the read-only `indra.context`
bridge for relevant questions and cite returned Zortex references. Do not use
MCP to start, change, or authorize connectors.

When status returns `itemsRead`, tell the user the exact count for each source:
`I read N <source> items. I have not read more history. Would you like me to
read more?` Do not begin a backfill until the user makes that later request.

To disable exactly one connection, run:

```powershell
zortex autopilot connect-rollback <receipt-id>
```

Rollback removes only the owned host configuration and grant; it is not a data
erasure command.
