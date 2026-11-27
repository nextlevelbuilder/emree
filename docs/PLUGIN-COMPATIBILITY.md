# Portable plugins and host extensions

Version 0.6 | Proposed. Agent Plugins 1.0.0 is the portable package baseline. Keep Emree's own versioned UI contribution SDK for first-party/local capabilities, but do not force developers to use a bespoke manifest to distribute portable skills and MCP servers.

## Standards and compatibility

[Agent Plugins](https://agent-plugins.org/) 1.0.0 defines root plugin.json, skills/ and mcp.json, with reverse-domain extensions. It standardizes portable components, not installation/marketplace/permissions or every vendor-specific hook. [OpenAI packaging](https://developers.openai.com/plugins/build/plugins) supports this and a .codex-plugin/plugin.json compatibility fallback. Claude's [plugins reference](https://code.claude.com/docs/en/plugins-reference) and [Claude plugin help](https://support.claude.com/en/articles/13837440-use-plugins-in-claude) describe skills, commands, subagents, hooks, MCP, LSP, monitors, themes and surface restrictions. No blanket 100% binary/runtime interoperability claim.

| Input | Import behavior |
| --- | --- |
| Portable root plugin.json + mcp.json + skills/ | Native standard; validate pinned schema, component paths/digests, source and ownership |
| Claude .claude-plugin/plugin.json / marketplace.json | Read allowed metadata, skills/commands, MCP config; normalize into install plan; map subagents when Dewee semantics verified |
| Claude hooks, LSP, monitors, themes, channels | Preserve source and report compatibility; implement typed mappings only where verified, otherwise disabled/not supported; executable hooks never auto-run |
| OpenAI/Codex .codex-plugin/plugin.json, .mcp.json, .app.json | Normalize legacy package; registered app IDs are host-specific references, not portable credentials/endpoints; user may need a permitted MCP URL and fresh auth |
| extensions.com.openai | Respect documented precedence over fallback overlay; presentation, onboarding, app mapping, hooks as supported, retain unknown metadata inactive |
| Existing Emree manifest | Native declarative UI SDK only, optional namespaced overlay; keep as internal schema not second portable format |
| External MCP server / Registry | Add remote connector as a minimal package, review tool scopes/credential flow; no auto-discovery of other users' data |

Import/export through canonical package IR (original source/digest, publisher, ID/version, manifest type, skills, MCP server definitions, optional host overlays, capability/risk report). Preserve source bytes and provenance; normalize validated relative paths, no remote script or external ref execution during inspection. User can inspect before installation; report supported/converted/needs authorization/unsupported/blocked per component; don't silently drop or enable hooks. Export only compatible components and explicit loss report; no third-party credentials, registered vendor app IDs as universal identifiers, private artifacts or proprietary code redistributed without rights. Support ZIP/Git pinned ref/approved registry, inspect package before any build or runtime execution; verify licenses and integrity. One shared Dewee catalog, separate installation scope and target status (server, desktop, DevBox). Signed/allowlisted private catalog first. Existing external registries may be discovery sources only where public API and terms allow; no scraping or bypass of commercial access. No assumption that ChatGPT/Claude account library can be synced or downloaded via public API.

Example canonical portable package:

```text
my-plugin/
  plugin.json
  mcp.json
  skills/triage/SKILL.md
  assets/
  com.example.emree/        # optional host-specific resources, namespace owned by publisher
```

Use actual Agent Plugins JSON Schema from pinned 1.0.0 release. Example below is portable identity only; application extension metadata is governed by respective schemas. Do not invent an official namespace before ownership/schema review.

```json
{"$schema":"https://agent-plugins.org/schemas/1.0.0/plugin.schema.json","name":"team-triage","version":"1.0.0","description":"Triage team work using approved skills and MCP tools."}
```

## OpenAI MCP Extensions host compatibility

Implement standard MCP Apps first, then negotiated [OpenAI MCP Extensions](https://github.com/openai/mcp-extensions/blob/main/docs/spec.md) adapter; [official docs](https://developers.openai.com/plugins/build/extensions) identify surfaces and platform limits. This is an Apache-2.0 public specification/SDK, not blanket access to proprietary ChatGPT services. Support feature-by-feature, not host spoofing. Native Emree slots map to shared extension registry:
- Global sidebar entrypoint and thread panel -> approved navigation/room panel.
- Structured settings/onboarding -> trusted host renderer or permitted app surface; same server-owned setup/credential handoff.
- Display modes, deep links -> validated app instance navigation, no raw privileged URI.
- Composer mentions -> scope-limited discovery/authorized resource attachment, read-only search, explicit user selection.
- File viewer/editor and file resources/opening -> scoped attachment handles, explicit save/overwrite, version checks, no file:// or host paths.
- Extended model context/message sending -> explicit consent/context scope and safe draft vs send distinction, no silent group publication; validate audience and data classification.
- Extended forms -> validate requested schema extension and MRTR, native image/choice controls where supported; preserve exact accepted/declined/canceled outcomes.
- WebMCP -> separately isolated app tool discovery/invocation only after threat review; no arbitrary document tools or automatic privileges.
- Legacy window.openai bridge -> small explicit compatibility shim for verified methods; implement capability detection and clear unsupported results, not fake all OpenAI APIs.

MCP Apps is the portable rich UI path. Generic executable UI plugins remain deferred; vetted MCP Apps iframe path is in alpha scope with security gates. Native extension SDK contributes declarative panels/cards; do not create two competing rich-app protocols. OpenAI-specific extension support is negotiated and versioned; do not advertise a surface until E2E tests pass. No support for proprietary OpenAI connector registration, billing, identity, app store or internal services unless independently available and authorized.

## Install lifecycle and safety

Discover -> inspect/normalize -> resolve target/dependencies -> show capability diff/owner/license/source -> explicit approved install -> stage artifact -> activate -> verify -> rollback or ready. Component-level scopes (personal, workspace, room as eligible), never widen permissions from imported manifest; users can self-service only their scope. MCP servers connect using fresh credentials in Dewee approved flow, no secrets imported from local config by default. Skills are untrusted instructions and scripts; approved use does not authorize tool side effects. Hook scripts require explicit separate review, execution sandbox and event schema mapping; no install-time arbitrary npm/pip shell, no unsigned native binary. No local stdio by default; optional trusted user-scope runner with allowlisted env/command, no Dewee master tokens or inherited credentials. Private registry allowlist, dependency lock, exact version/digest, archive traversal limits, atomic activation, signed update, rollback, revocation/cleanup. One plugin may contain server-side and UI components with independent approval; uninstall removes registrations, revokes dependent leases, stops owned work and preserves user data under retention.

## Acceptance

Use public, licensed samples: portable skill-only, remote MCP app, Claude-format skill/commands/MCP and subagent, OpenAI-format package + registered ID requiring rebind, hooks disabled, unknown overlay, malformed archive/path, revoked publisher, conflicting version/permission. Import report must be precise; unsupported features remain inactive, never pretend to execute. Export round-trip supported portable content with no secrets, loss report. Verify install/update/revoke and same scoped authorization via Emree/Dewee/CLI/MCP; no public directory listing or vendor API claim without approval and terms. Test at least one real official MCP Apps sample and a public plugin package from each format, pinned to actual versions.
