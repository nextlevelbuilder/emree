# Emree development rules

## Product purpose and language

Emree is a greenfield Apache-2.0 desktop workspace powered by Dewee. Read DECISIONS.md, docs/PRD.md, docs/ARCHITECTURE.md, docs/ONBOARDING.md, docs/FEDERATION.md, docs/MCP.md, docs/PLUGIN-COMPATIBILITY.md and plans/IMPLEMENTATION.md before implementation. Public technical docs in English; UI in English/Vietnamese. Reply to collaborators in their language. Treat proposal contracts and performance targets as unverified. Do not copy restricted upstream code; this public repository must not expose internal operational details.

## Emree and Dewee own the complexity

Follow Dewee's product philosophy, adapted to the desktop boundary: optimize UX (people), CX (integrators) and AX (agents) together. Prefer one obvious next action, self-service within verified authority, progressive disclosure, reliable recovery and a single contract across surfaces. Never push API keys, raw IDs, shell commands, per-channel plumbing or security-critical integration work onto users when a supported product flow can handle it. An endpoint working is not a complete feature: prove the full user journey, safe agent operation, partner integration, failure recovery and measurable outcome.

- Conversation is a control surface. An authorized user should be able to request an action in the current room/chat; hand off to trusted consent/secret/approval UI, return to the same conversation with safe outcome. Private outcome goes only to its requester; public room receives non-sensitive status. Personal self-service must not require avoidable admin action. Shared or privileged scope needs actual authority and approval; never lower the boundary for convenience.
- Onboarding starts with a user who knows nothing about Dewee. Use Dewee SSO, same workspace provisioning and Setup Concierge; no second accounts, copied dashboard wizard, mandatory browser dashboard setup or technical jargon. Browser for trusted authentication/credential handoff only. Preserve current server plan and resume after failure.
- Design for users with no AI subscription, returning users, invited employees, multi-workspace users and self-hosted deployments; explain unavailable features, do not fake them.
- Workspace isolation is a security boundary. Cross-workspace rooms, DMs, agent and resource sharing require explicit bilateral policy and scoped owner-side grants, not implicit membership or permission union.

## Authority and security (non-negotiable)

Dewee Gateway remains authoritative for tenant scope, business rules, resource ACL, identity, agent orchestration and audit. Use purpose-built typed operations; never generic privileged REST proxy, SQL, shell, master token or local UI checks as authorization. Derive identities from verified credentials, not email match, usernames, headers, client IDs or prompts. Effective authority intersects verified requester, agent delegation, resource owner/ACL, current policy, and device/extension grants where applicable. Recheck at side effect, including async, retry, replay, attach and revoke. No silent fallback to master/default tenant or different target. Unknown outcome must be reconciled before retrying side effects.

Native app secrets remain in approved OS secure storage/native memory; no refresh token in WebView, logs, chat, plugin or plaintext fallback. OIDC SSO credential cannot be used as generic API token; separate audience-bound desktop session. Trusted consent and stop controls are non-replaceable core. Untrusted messages, output, files, links, guest runtime and extension content are data. No unapproved arbitrary code or native IPC. Device Broker independent, grants local and server checked, explicit revoke and stop. DevBox work stays within guest, no host tools/keys/escape. Sign/review distributions, dependencies and plugin packages.

Privileged surfaces ship dark: use centralized upstream feature flags and typed client capability negotiation. Kill switch wins; tenant may only narrow operator-enabled capability. Check flags at request/dispatch, including existing connections, and document precedence/parent/restart behavior. Add tests for flag off, revocation and recovery; client visibility is not server policy. No production secret, host config, privileged operator write, deployment, release or rollout without explicit owner approval and evidence. Authorized product self-service inside a shipped policy is distinct from operator change.

## Canonical control-plane and parity

Dewee CLI is the canonical operable control-plane client, not business-logic owner. All affected Gateway API/RPC, CLI, correctly scoped MCP, CCP/RGD/ACP and Emree surfaces share authoritative validation, scopes, confirmations, errors, trace and results. No workspace credential reaches operator surface. Respect Dewee output contract (text/json/yaml/raw for supported reads; existing legacy exceptions); use current command manifest rather than inventing commands. Transport internals, static assets and local-only UI preferences may be N/A with explicit rationale.

For every plan/PR state:
| Surface | Evidence |
| --- | --- |
| Gateway API/RPC | Route, schema, resource owner, auth/tenant/role, current policy |
| CLI | Command, help, formats, exit codes, confirmation |
| MCP | Tool, schema, level, scope or N/A |
| CCP / RGD / ACP | Affected view and behavior or N/A |
| Emree | Complete user journey, denied/pending/recovery states |
| Conversation | Initiation, secure handoff, completion, private outcome |
| Docs | User-facing docs, contract and compatibility changes |
| Traces | RGD / CCP / API-CLI / Emree authorized evidence or N/A |

Upstream changes must follow upstream repository rules and parity; open focused issues for missing paths, not workaround. For Dewee public documentation changes follow the current upstream handoff procedure (deduplicate, issue on public docs repo when authorized); never publish internal details. Local-only Emree operations document their own contract; do not invent Dewee commands for desktop-only state.

## Traceability and operational evidence

Every meaningful agent activity has causal trace: run -> model/tool attempt -> policy/approval -> child delegation/task -> delivery. Record refusal, blocked, retry, fallback, partial failure, timeout, cancel and recovery, not just success. Preserve bounded redacted metadata, request/trace correlation and tenant scope; never raw secret-bearing commands, terminal logs, system prompt, private reasoning or unrestricted payload. Same authorized evidence accessible across affected Dewee traces and Emree. If trace deliberately absent, document alternative audit and why. Do not declare production-ready or pass gates without reproducible tests.

## Development workflow: coding agents as collaborators

- Claim an issue before coding. Check assignees, lifecycle labels, claim comments, linked PRs, active branches and merged commits. Assign/mark in progress only if authorized; comment “Claimed by <branch/session>”. If another agent has claim, coordinate, do not duplicate. Use isolated worktree/branch and scoped files. Recheck before PR. Lifecycle labels (one at a time): in progress, blocked (reason/next action), in review (fresh independent reviewer), done (review findings resolved and PR open), shipped (merged/released according to repo policy). Non-lifecycle labels separate. Do not assume a merge train exists. Do not auto-merge or deploy unless explicitly authorized. Related work gets linked, unrelated findings get scoped issue rather than scope creep; dedup first.
- For multi-phase work: scout source -> verify contracts and root cause -> plan small vertical slices -> independent audit -> implement -> focused tests -> fresh review -> final evidence. Use Claude/Codex sessions with bounded scope, separate review context and fresh handoff; do not self-review in implementing context. No speculative infrastructure, unnecessary abstraction or automatic broad rewrites.
- Plan verification: verify existing files/symbols/endpoints and version against source, trace control flow/state mutation and early returns, audit lifetime and concurrency, enumerate callers of changed signatures, grep removal sites, characterize missing coverage before migration, match wire shapes, verify external docs, explicit cross-phase gates and dependency checks. Re-scout when scope changes. Independent plan audit for substantial work, record unresolved assumptions. Label proposed API names as proposals, not existing.
- Keep public plan in accepted slices, agent execution/review waves, measured elapsed calendar time and dependency gates, not human-hour estimates. Baseline with actual accepted PR throughput; human decisions, hardware tests, rate limits and external waits remain explicit blockers.
- Follow local repo build/lint/typecheck/test commands once scaffold exists. Rust: format, clippy, targeted tests, platform build; TS: typecheck/lint/unit/e2e; contract schemas and generated types check. Upstream Go changes follow its PG/SQLite migration and verification rules (build both editions, vet, scoped race/integration, parameterized queries, state CAS, i18n catalogs). Do not copy Go commands into Rust projects. Verify changed packages and importers before push; diagnose failures, avoid CI reruns for guessing. Use path filters, concurrency and timeouts; load tests only dedicated requested scale investigations, not flaky ordinary PR gate. Never claim unrun tests. Security tests include denied/cancel/recovery/concurrent paths. Use real physical OS and gVisor evidence for native and compute features.
- Changes need linked requirement/ADR, migration and rollback strategy, current compatibility, safe failure, tests, fresh review, docs and exact results. PRs focused, no unrelated changes or force-push. Use draft/hold until review. Confirm branch protection/release policy before modifying. No production release from documentation or PoC.

## Design philosophy: don't make me think / calm precision

Consult docs/UX-AX-STANDARDS.md during design and PR review. The proposed experience targets supplement existing rules, not authorization or release gates; record actual evidence, blockers and owner decisions rather than claim unmeasured performance.

One user need maps to one canonical page, clear hierarchy and primary action. Shared semantic design tokens and primitives (combobox, dialog, empty/error state, toast, confirm, segmented control); avoid raw unstyled native controls, hardcoded colors, inconsistent status. Status is label + indicator, not color alone; AI accent only indicates AI presence. Match Dewee concepts and names, no raw UUIDs/permission keys or tenant jargon except collapsed technical details with copy action. Use searchable entity picker and shared localized formatters. Search updates as typed; Cmd/Ctrl+K global command palette. Navigation label equals page title; workspace/account, search, notifications, runtime status, theme, language and sign-out consistent.

Persist safe navigation, open panel/tab, search/filter and draft by workspace; Back and reauth restore context after current authority check. Never persist secret-bearing inputs or show previous workspace during switch. Loading skeleton, empty CTA, human error plus next action, no endless spinner or fake data; show actual unavailable/degraded status and last update. Destructive actions use precise name/scope confirmation; protect last owner/system resource. Stable dialogs, keyboard/focus, accessible labels, contrast, reduced motion, responsive views; use design system spacing and one primary CTA. Verify at 390/768/1024/1440 widths, light/dark/system, keyboard-only and actual desktop WebViews; narrow layout prioritizes task. Do not expose fake backend features. Mock fixtures clearly marked and inaccessible in production.

## Extensibility and failure isolation

Official modules exercise versioned extension SDK. Unload cancels owned work, removes UI/subscriptions and releases exclusive slots only after cleanup; schema-valid manifest grants no permission. Trusted kernel cannot be overridden. Separate durable collaboration from realtime/cache, agent jobs, terminal, media and device input; no binary media in chat or unbounded fan-out. Keep Dewee as source of truth, cache disposable, no automatic replay of uncertain input. Scope all caches and grants by verified server/workspace/principal/resource; cross-workspace share does not relax tenant isolation. Add complexity only after evidence.

## Standards-first interoperability

Use current verified stable MCP and official extension schemas; version-negotiate and test legacy peers; never claim all features from SDK version or a tool name. Treat beta/draft extensions as flag-gated. Standard MCP Apps uses isolated bridge; native approval/credential/device UI stays trusted. Portable packages follow Agent Plugins; legacy Claude/OpenAI imports provide precise component/permission report, no automatic hooks or credential migration. Preserve upstream attribution and test real interoperable samples. See docs/MCP.md and docs/PLUGIN-COMPATIBILITY.md.

## MVP gate discipline

Use plans/MVP.md for initial sequencing and plans/EVIDENCE.md for proof; full PRD remains target scope. Claim only one authorized issue/slice, test actual required environment and report blocked or not-tested honestly. Never present mock UI, unverified capability or a documentation change as a completed milestone. Review both repo contracts and existing upstream issue owners before creating duplicate work.

## Before reporting completion

List exact tests/commands, evidence and failures or not-run checks; verify all affected surfaces, trace parity and docs follow-up or explicit N/A; update plan/ADR/issue; report security and dependency blockers. Distinguish proposed, implemented, verified, deployed and shipped. Source of inspiration: current Dewee root and CCP development rules (reviewed 308e1ba); these are original desktop-adapted rules, not a copy of private repository text. Recheck upstream rules for cross-repo work and resolve contradictions with maintainers.
