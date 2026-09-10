# WO-CODEX-ZORTEX-BOOTSTRAP-001

## User outcome

After a one-time installation from this public Codex marketplace, a Windows
Codex user can explicitly ask to install Zortex and connect named sources.
Codex knows the exact npm package, installs it when absent, starts the existing
agent onboarding path, and reports a receipt-backed result.

## System shape

| Component | Reused or added | Boundary |
| --- | --- | --- |
| `@zortexhq/install` | Reused | Owns Windows executable, permission probe, connector workers, and read-only MCP configuration. |
| `zortex-bootstrap` skill | Added | Maps explicit user requests to the public package and existing CLI; it contains no connector or credential implementation. |
| Codex marketplace | Added | Distributes the skill only; it does not become a Zortex data marketplace. |

## Acceptance criteria

1. `.agents/plugins/marketplace.json` names a public `zortexhq` marketplace and
   exposes `zortex-bootstrap` with an explicit install policy.
2. The plugin manifest is schema-valid and has one skill, no MCP server, app,
   hook, credential, or connector payload.
3. The skill runs the exact npm `latest` installation and onboarding only for
   explicit Zortex installation/connection requests; it maps only declared source IDs and
   starts Zortex's normal background full sync without asking users to choose a
   Starter or sync strategy.
   A matching supported-source connection routes through Zortex rather than an
   already-authenticated `codex_apps.*` provider app.
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

## Execution receipt (2026-09-09)

1. Public repository `ZortexHQ/zortex-codex-plugin` was created with commit
   `003b41e` on `main`; no private Zortex repository was exposed.
2. `validate_plugin.py plugins/zortex-bootstrap` passed, and the marketplace
   JSON parsed successfully.
3. A real local Codex CLI added `ZortexHQ/zortex-codex-plugin --ref main` as
   marketplace `zortexhq`, then installed and enabled
   `zortex-bootstrap@zortexhq` version `0.1.0` from its Git snapshot.
4. The installed cache contains the published bootstrap skill verbatim. A new
   Codex conversation is the consumer boundary for implicit skill activation;
   no user data or source credential was used for this distribution proof.
5. Version `0.1.1+codex.20260909031006` was published from commit `660df87`
   and reinstalled through the upgraded `zortexhq` marketplace. It removes the
   user-facing Starter instruction, lets named connections run the first read
   automatically, and tells Codex to report `itemsRead` before asking whether
   the user wants more history.
6. Version `0.1.3+codex.20260909034046` was published from commit `d8964f3`,
   then reinstalled from `zortexhq`; its supported-source routing instruction
   explicitly refuses to substitute authenticated `codex_apps.*` provider apps.
