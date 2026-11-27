# Dewee integration and upstream work

Emree calls Dewee's supported contracts. This document is a public-facing capability map and handoff proposal, not a disclosure of private deployment details. Re-check current upstream source/issues before implementation; do not copy Dewee source into this Apache-licensed repo without compatible permission. Prior design review inspected Team Room, identity, Browser Relay, runtime event fan-out, cache, media storage, interaction and DevBox. Existing capabilities may be gated by deployment flags and edition.

| Upstream capability | Existing baseline | Required for Emree |
| --- | --- | --- |
| Identity/CCP | Hosted OIDC SSO (userinfo-only access token), scoped customer session/extension auth, workspace provisioning/invites | First-party native client and separate desktop session/credential exchange, native create/join and scope registry; see [onboarding](ONBOARDING.md) |
| Workspace setup | Server-owned Setup Concierge, plan/actions, readiness, Super Agent bootstrap/handoff | Native structured card adapter and safe credential/approval handoff; role-aware resume, no second engine |
| Cross-workspace coordination | Bilateral agent peer grants and cross-runtime relay | Native shared room/DM and resource-owner grants, same-cell first, separate federation phases; see [federation](FEDERATION.md) |
| Team Room | External group/mailbox agent coordination, event log, limits, outbox, peer grants | Native room human/agent membership, messages/threads/DM/read state, typed coordinator adapter, no regressions |
| Agent chat | Structured SSE chat events, sessions, trace | Authorized native room projection, final events, task/approval links |
| Interaction | Human interaction and approval cases | Unified inbox with action binding, replay, takeover |
| Tasks/memory/knowledge/vault | Existing resource APIs | Resource references, ACL-filtered projection and search |
| Runtime cells/realtime | Cell routing, WS and runtime event fanout | Native collab outbox/recovery, subscription authority, separate delivery workload |
| Media | Existing session media and backup mechanisms | Private attachment object storage, scan and retention |
| Connected browser | Device enrollment/grants/control/revocation | Reuse via official capability; native device broker for files/GUI |
| Workstations | Remote execution interface | Future execution provider only; not substitute for safe DevBox terminal |
| DevBox | Lifecycle/scripts/managed runtimes and isolated compute | Scoped access, async operation/worker controller, PTY, structured runtime adapters, sessions, quotas and artifacts |
| MCP protocol | Guarded client/servers, OAuth, structured results and partial modern Events path; full conformance unverified | Modern/legacy capability inventory, MRTR, Apps resource/metadata preservation, Tasks, authorization extensions, secure interaction/task ledger; see [MCP](MCP.md) |
| Plugin registry | Product direction; maturity must be verified | Shared signed catalog and desktop manifest target; no second marketplace |

## Cross-repo work packages

- D-A: first-party SSO, separate native session, version bootstrap/permission scopes, idempotent workspace create/join and Setup Concierge parity; tests for revoked membership, token reuse and cell switch.
- D-B: native collaboration data model, REST, durable outbox and sync/realtime; adapter into Team Room and ACL; shared contracts fixture.
- D-C: agent task/run/interaction/inbox projections and controlled routing; trace coverage and server actions.
- D-D: attachment metadata/authorization/scan lifecycle and storage provider.
- D-E: DevBox execution/session authority, dedicated worker, access, generation fencing, account isolation, PTY and structured adapters.
- D-F: native device grant/command relay, revoke, current policy and local broker contract.
- D-G: approved extension catalog metadata/distribution and private registry integration (later if available).

DevBox upstream epic [#2696](https://github.com/nextlevelbuilder/dewee/issues/2696) and access/accounts/repositories/work/usage/extensions/storage/activity issue references [#3118](https://github.com/nextlevelbuilder/dewee/issues/3118) through [#3125](https://github.com/nextlevelbuilder/dewee/issues/3125) were seen in the reviewed UI capability map, not independently confirmed current. Reconcile with those issues; do not create duplicates or mark them done based on UI placeholders.

- D-H: federation contract runway, then mutual partner relationship, scoped shared room/DM, resource/agent grants, policy and cross-cell relay after approval. Keep tenant ownership and existing agent peer semantics intact.

- D-I: native MCP host/server parity and stable extensions, portable package normalization, app/task/elicitation bridges, versioned vendor adapters; reuse existing identity, Human Interaction, task, audit and registry. See [MCP](MCP.md), [plugin compatibility](PLUGIN-COMPATIBILITY.md).

## Parity matrix

For each operable feature: authoritative Gateway route and policy; documented CLI command with JSON output, error code and help; scoped workspace MCP tool where appropriate (never generic operator/shell proxy); CCP/RGD user/operator views or explicit N/A; Emree user journey; trace/audit evidence for success, refused, cancelled and recovery. Documentation updates across public surfaces after implementation. New privileged surfaces default-off, centralized feature flag registry; tenant opt-in cannot override operator deny. Expose capability discovery and safe not-supported states; no automatic fallback to another tenant, runtime or target.

## Sequencing and ownership

Dewee maintainers own Gateway/worker/database changes and backward compatibility. Emree maintainers own UI/native client/SDK, local broker, fixtures and release. Joint contract review precedes coding. Use mock service for parallel UI development, mark mock explicitly and block production claim until real integration tests pass. No changes to Dewee repository are part of this documentation bootstrap.
