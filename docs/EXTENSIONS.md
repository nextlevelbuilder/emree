# Extension platform and SDK

## Goals

Learn from DeepSeek Harness's Cordis services, effects, profiles/bundles and conversation registries without importing its in-process trust model. Emree is extensible by construction, not a privileged host with arbitrary package execution. Built-in features use versioned registration contracts; authorization, credentials, consent, update verification, local kill switch and audit are a non-replaceable trusted kernel.

## Trust tiers

| Tier | Execution | Examples |
| --- | --- | --- |
| Core | Reviewed signed app/native broker | Auth, policy, storage, emergency stop |
| Official built-in | Bundled, reviewed, signed; typed interfaces | Rooms, Inbox, Workbench, Devices |
| Declarative extension | Host renders validated data, no arbitrary JS or native calls | CRM panel, task card, command, theme |
| MCP Apps (alpha after gate) | Isolated standard HTML bridge; no privileged native IPC | External interactive tool UI |
| Arbitrary sandboxed UI (later) | Separate isolated origin/process, restricted bridge, no native IPC, network deny by default | Rich integrations |
| Trusted native provider | Operator-installed signed component, separate process, explicit high-risk review | OS Computer Use adapter, approved execution provider |

Never expose arbitrary native commands, SQL, filesystem, shell, token values, or broad HTTP proxy to a plugin. Treat marketplace package content and remote output as untrusted. Agent can draft a plugin but cannot install it or widen permissions without authorized policy/consent.

## Extension points (v1 proposals)

| Contract | Contribution |
| --- | --- |
| navigation.view@1 | Navigation entries |
| room.panel@1 | Right sidebar and room context panel |
| workbench.panel@1 | Execution workbench and session tabs |
| conversation.card@1 | Typed message/activity cards with safe actions |
| command.palette@1 | Discoverable actions |
| settings.section@1 | Scoped settings views |
| events.subscribe@1 | Filtered typed event subscriptions |
| action.invoke@1 | Invoke registered, authorized action |
| artifact.preview@1 | Safe preview renderers |
| terminal.renderer@1 | Official terminal presentation only at first |
| device.provider@1 | Trusted device provider registration |
| runtime.adapter@1 | Worker-side coding agent adapter |
| preview.provider@1 | Authorized preview transport |
| view.sandboxed@1 | Future isolated custom UI, not alpha |

Start with navigation, panel, card, command, settings, event subscription and action invocation. Implement Workbench/terminal and device/runtime provider seams with their owning features, not empty registries. Publish SDK version separately from desktop releases. Unique namespaced IDs, explicit input/output schemas, semver compatibility, resource context injected by host, never trust extension-supplied tenant or principal. Unknown contribution types are rejected, unavailable action is visible as disabled.

## Portable distribution and vendor interoperability

Agent Plugins 1.0.0 is the canonical portable package, not the Emree declarative manifest below. Support Claude and ChatGPT/Codex formats through explicit adapters. The native manifest is only a host-specific contribution overlay. Standard MCP Apps are the rich interactive path and are in alpha scope after sandbox gate; arbitrary third-party UI remains deferred. See [MCP](MCP.md) and [compatibility](PLUGIN-COMPATIBILITY.md).

## Native contribution manifest

A declarative manifest describes id, version, publisher, kind, compatibility, dependencies, requested permissions, contribution entries and asset digests. See [example](../examples/extensions/crm/manifest.json) and [draft schema](../contracts/extension-manifest.schema.json). This schema is a minimal alpha proposal; installation additionally needs signature/digest verification, dependency resolution, full package validation and policy. No eval, executable script or remote code in declarative packages; templates are a small data-only vocabulary with bounded nesting, sanitization and host-controlled actions. No external URL fetched without explicit allowed capability.

## Lifecycle

discovered -> verified -> staged -> installed -> resolving -> activating -> active -> draining -> disposed/disabled. Failure transitions retain diagnostics and restore previous healthy version. Register returns idempotent disposer; track resources by plugin ID + generation. Unload prevents new calls immediately, revokes leases, cancels requests, timers and subscriptions, removes UI, waits bounded cleanup, terminates isolated processes if needed. Do not activate dependents before required dependencies; reject cycles and incompatible versions; release exclusive provider slot only after cleanup. Persistent data remains scoped and export/delete follows retention policy. Crash recovery reads installed vs active state and falls back to known-good, never equates selected with running.

SDK proposal: activate(ctx), ctx.register(slot, descriptor), ctx.subscribe(topic, filter, handler), ctx.invoke(action, typedInput), ctx.effect(setup)->disposer, ctx.onDispose. Only built-ins use code callbacks initially; declarative plugins contribute JSON that the host translates to these operations. Each registration is owned, automatically disposed even when activation partially fails; test duplicate registration, dependency loss, update interruption and stale callbacks.

## Permission model

Effective authority = authenticated user/agent delegation intersection workspace policy intersection resource ACL intersection requested/granted plugin permission intersection device/OS grant. Permission names are hints to the policy engine, not rights in themselves. Separate read/modify/share/execute; grants scoped per server/tenant/installation/version. Updated package requesting new permissions remains inactive until approved. Revocation invalidates live actions/subscriptions and local grants; recheck at side effects. Sensitive approvals use core native/server UI; extension cannot draw a fake approval which confers rights. All actions and refusal traces carry extension ID/version and request/actor ID; safe logs only.

## Profiles and distribution

Profiles are ordered compositions (Essentials, Operations, Engineering, Computer Use) with explicit feature compatibility. System defaults -> approved bundles -> workspace preferences -> user preferences, but policy restrictions and kill switches always win. Selecting a profile cannot grant permissions, install executable code, or override security policy. Show installed vs enabled vs active vs failed vs incompatible distinctly.

Use the Dewee plugin marketplace/registry as shared catalog when its contract is available. Bundle may contain server-side skills, MCP, workflows, agent templates and desktop extension manifests, with separate per-target install/permission checks. Start with a signed allowlisted catalog and local development packages using isolated fixture workspace; no public marketplace dependency in alpha. Private registry cannot fall back to public without explicit admin action. Verify exact version, publisher, artifact digest and signature before unpack; prevent archive traversal/symlink, limits and unsafe scripts; stage then atomically switch active version; rollback from retained trusted artifact; revocation list and kill switch. Built-in updates follow app release. No hot reload of privileged providers/durable infrastructure by default.

## Developer and agent experience

Proposed emree-ext CLI: init, dev (fixture-only), validate, test, pack, inspect, publish (authorized registry). Template/SDK docs, typed mocks, compatibility fixtures, examples, diagnostic codes, accessibility and geometry tests. Package validation and dry-run permissions available without install. Review contributions for third-party rights, SBOM, provenance and security. First alpha proof: built-in inbox/room panel plus external declarative CRM example can install, use scoped read capability, refuse unauthorized action and cleanly unload. No arbitrary third-party code necessary.
