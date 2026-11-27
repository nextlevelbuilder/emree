# Security and privacy architecture

This is a design, not a certification or completed security audit.

## Trust boundaries

Trusted: Dewee identity and authorization, authoritative storage, verified native host, device broker policy/consent, approved worker controller and signed release pipeline. Untrusted: messages, markdown, files, links, AI outputs, extension packages, guest processes, remote web content, terminal bytes, plugin and device telemetry. Tenant and actor are derived from verified credentials, not text, IDs or model claims. Effective authority intersects requester, agent delegation, resource ACL, current server policy and device/OS grant.

| Threat | Mandatory control |
| --- | --- |
| Cross-tenant reads/writes | Scoped keys/FKs/queries, verified routing, negative tests; deny inaccessible IDs |
| Prompt injection / malicious plugin | Treat content as data; typed allowlisted actions, permission checks, declarative first, no generic proxy |
| Renderer compromise | Local bundled assets, strict CSP, minimal per-window Tauri capabilities; no remote IPC; no token in JS |
| Token theft/replay | PKCE, system browser, secure OS key storage, short-lived audience-bound tickets, rotate/revoke, no plaintext fallback |
| Realtime ACL leak | Authorize subscribe, dispatch and replay, policy epochs and disconnect/refresh on revoke, no unrestricted history |
| Offline cache leak | Per-server/tenant/principal namespace; policy-controlled encryption/disable, purge on revoke/sign-out/switch; cannot recall copied bytes |
| Attachment attacks | Private staging, size/type/checksum, scan, sandbox transforms, scoped delivery, retention/deletion |
| DevBox host escape | Dedicated gVisor workers, no host socket/keys/paths, no fallback, resource quotas, network egress guard, generation fence |
| Credential mixing | One personal or org-service trust domain per box, safe auth flows, no raw shared transcript |
| Terminal escape sequences | xterm renderer isolation; block unapproved clipboard/links/control sequences; no raw output in logs |
| Device abuse | Explicit resource/time grant, local consent, independent emergency stop, exclusive lease, fail closed |
| Supply chain | Pin dependencies, reviewed artifacts, SBOM, signed installers/updater/extension manifests, rollback and revoke |
| Abuse/DoS | Edge and app limits, tenant fairness, bounded buffers/queues, agent chain budgets, independent control lane |

## Native onboarding and federation boundaries

SSO token audience isolation is preserved: first-party native login requires separate scoped session issuance, validated redirect and linked revocation. No browser cookie scraping. See [ONBOARDING.md](ONBOARDING.md). Federation does not grant tenant membership; require verified issuer/workspace/actor, bilateral relationship and resource-owner policy at each share/send/read/execute, deny conflicts and unknown policy, audit both sides. On partition fail closed for new effects; cannot recall delivered copies. See [FEDERATION.md](FEDERATION.md).

## Authorization and observability

Authorize at side effect, including retries, queued jobs and reconnect; never replace policy with UI checks or a system prompt. Current membership AND underlying resource ACL required for artifacts and search snippets. Native grant cannot be self-approved by agent. Use typed confirmation/approval for risky actions; bind exact parameters, revision, target and expiry. Recheck after human decision. Redact secrets, command contents, private outputs and model internals. Trace attempts (allowed/blocked/error/cancel/retry), correlation and reason, not merely success. Add trace/CLI/MCP/RGD/CCP parity as appropriate. Privileged feature flags default off, tenant can only narrow operator allow. Provide a real kill path even when AI or queue is down.

## Release checklist

- Contract fuzz and negative tenant tests, room revoke during replay, stale tickets, offline cache switch, CSRF/redirect, SSRF/path traversal, upload abuse.
- Plugin package corruption, signature failure, permission escalation, dependency cycle, stale callbacks, failed activation rollback; sandbox escapes for later executable tier.
- DevBox gVisor isolation and network checks, host credential absence, process termination, generation race, multi-session credential separation, terminal flood.
- Device app/window scope, lock/permission/lease revocation, duplicate/unknown effects, local stop; OS-specific manual tests.
- Dependency scan and license review, signed build provenance and updater failure/rollback, production readiness and incident runbooks.
- Restore rehearsal, documented RPO/RTO, fault/load evidence. Any critical/high unmitigated security finding blocks release.

## Responsible disclosure

Please use GitHub's private vulnerability reporting if enabled on this repository. If it is unavailable, open a minimal issue requesting a private maintainer contact, without exploit details, private data or credentials. Maintainers should enable private vulnerability reporting before the first executable release. No public disclosure of live exploit details until coordinated remediation. The repository does not currently publish production credentials, deployment topology details or security test results about actual customer systems.
