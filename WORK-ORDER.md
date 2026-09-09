# WO-CODEX-ZORTEX-BOOTSTRAP-001

## User outcome

After a one-time installation from this public Codex marketplace, a Windows
Codex user can explicitly ask to install Zortex and connect named sources.
Codex knows the exact npm package, installs it when absent, starts the existing
agent onboarding path, and reports a receipt-backed result.

## System shape

| Component | Reused or added | Boundary |
| --- | --- | --- |
| `@zortex-hq/install` | Reused | Owns Windows executable, permission probe, connector workers, and read-only MCP configuration. |
| `zortex-bootstrap` skill | Added | Maps explicit user requests to the public package and existing CLI; it contains no connector or credential implementation. |
| Codex marketplace | Added | Distributes the skill only; it does not become a Zortex data marketplace. |

## Acceptance criteria

1. `.agents/plugins/marketplace.json` names a public `zortexhq` marketplace and
   exposes `zortex-bootstrap` with an explicit install policy.
2. The plugin manifest is schema-valid and has one skill, no MCP server, app,
   hook, credential, or connector payload.
3. The skill runs npm installation and onboarding only for explicit Zortex
   installation/connection requests; it maps only declared source IDs.
4. The skill reports typed readiness and provider blockers, never credentials
   or false source readiness.
5. The public Git repository is pushed, then a local Codex client can add its
   marketplace and install the plugin by the commands in `README.md`.

## Undo

`codex plugin remove zortex-bootstrap` removes the distributed skill and
`codex plugin marketplace remove zortexhq` removes its marketplace source.
Zortex connection rollback remains receipt-scoped; none of these commands
erase append-only local data.

## Out of scope

This is not a Zortex cloud marketplace, a public pack registry, a connector,
or a provider OAuth flow. It is a distribution shim for the existing Windows
npm package.
