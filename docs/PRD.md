# Product requirements document

Version 0.7 | Proposed engineering requirements. See [Decision brief](../DECISIONS.md) and [implementation plan](../plans/IMPLEMENTATION.md).

## Problem, audience and success

Teams coordinate in chat, then switch to agent consoles, approval screens, Git tools and execution environments. Work context fragments; people cannot find the right session or understand what changed. Emree gives them a unified collaboration surface over Dewee, not a new system of record for everything.

Personas: employee (communicate and complete tasks without technical setup), manager (decide and review outcomes), engineer (control remote coding agents), workspace administrator (govern access, budgets, retention), extension developer (contribute capabilities without forking). North star: weekly completed human+agent tasks accepted by an authorized reviewer. Measure onboarding success, time to first result, task completion, approvals resolved, recovery success, and cost per accepted task. Avoid storing raw private text in analytics by default.

## Core journeys

1. Install signed app with no prior Dewee account, sign up/sign in via trusted system browser, return to Emree, create or join workspace, resume native Setup Concierge, reach first useful conversation/room. Existing users resume or switch. Disconnect, draft and reconnect without duplicates.
2. Mention a Dewee agent, see who accepted the task, follow structured activity, answer a question or approve a bounded action, see verified result and task link; close the app and later resume observation.
3. From a task card choose an authorized DevBox, wake explicitly, select runtime and session mode, open right sidebar, detach/reconnect, share chosen diff or artifact, cancel/terminate or release control. Raw terminal stays private by default.
4. Request access to a specific local folder, browser origin or application; approve from trusted native UI, see active session and indicator, pause/take over/revoke locally. No action without current user/agent/resource policy.
5. Admin enables an approved extension bundle, reviews permission changes, pins version, disables/uninstalls, inspects audit. Users choose permitted UI modules without gaining privileges.

## Requirements and acceptance criteria

P0 = foundation, P1 = collaboration alpha, P2 = agent/workbench beta, P3 = device and scale beta, P4 = GA. All are planned, not implemented.

| ID | Priority | Requirement | Acceptance |
| --- | --- | --- | --- |
| R01 | P0 | Native login and workspace connection | System-browser PKCE; exact server validation; no long-lived token in renderer; sign-out purges scoped cache and closes connections. |
| R02 | P0 | Cross-platform shell | Packaged macOS, Windows, Linux builds load local signed assets; keyboard navigation, theme and accessibility smoke tests. |
| R03 | P0 | Extension kernel | Official module plus example declarative extension use same registry; duplicate IDs refused; disable cancels subscriptions and actions. |
| R04 | P1 | Workspace switching | Cell routing server-owned; stale response and previous-tenant state never appear after switching. |
| R05 | P1 | Rooms and membership | Public-to-workspace and private rooms, invite/leave, human and agent membership; revoked member cannot read new events/history. |
| R06 | P1 | DMs | Human-human and human-agent; exact participants, no accidental room publication. |
| R07 | P1 | Messaging | Send/edit/delete with server sequence, idempotent client ID and optimistic UI; crash/retry yields one logical message. |
| R08 | P1 | Threads and reactions | Reply, react/unreact and unread cursor with ACL; event replay reconciles edits/deletes. |
| R09 | P1 | Presence and notifications | Presence expires, mentions and actionable notifications deduped; quiet hours/preferences; private content hidden from OS notification by default. |
| R10 | P1 | Sync and offline | Cursor gap triggers bounded replay/snapshot; drafts survive restart; queued sends explicitly shown; offline data policy and purge. |
| R11 | P1 | Search | ACL-filtered results and snippets; deletion/ACL change prevents results even if index stale; no unrestricted semantic indexing. |
| R12 | P1 | Attachments | Direct upload, pending/scanning/ready states; no download before clean/authorized; quota and orphan cleanup. |
| R13 | P1 | Extension management | View package, publisher, version, permissions and compatible targets; install/disable/rollback; failed activation keeps previous healthy version. |
| R14 | P2 | Agent roster/routing | Explicit mentions and policy-driven routing; no agent answers every message by default; human chat independent of AI outage. |
| R15 | P2 | Tasks and handoff | Link existing Dewee task, assign authorized human/agent, show status/results; do not duplicate task source of truth. |
| R16 | P2 | Approvals and inbox | Show exact action, scope, expiry; current authority rechecked at execution, one logical decision; private outcomes not broadcast. |
| R17 | P2 | Run explorer | Structured tool/activity events, final response, stop/resume when supported; redacted trace links; no private reasoning or secrets exposed. |
| R18 | P2 | Knowledge and decisions | Link approved resources; server checks room membership AND resource ACL; no copied vault secrets. |
| R19 | P2 | DevBox lifecycle | Capability discovery; list/wake/sleep/stop under policy; async operation progress; missing backend explained without fallback. |
| R20 | P2 | Right-sidebar Workbench | Resize/collapse/maximize, session tabs, context link to task/room; close sidebar detaches, does not silently stop job. |
| R21 | P2 | Remote TUI/shell | PTY, resize, Ctrl-C, bounded output and reconnect; no host exec; exclusive input; raw transcript not published. |
| R22 | P2 | Structured coding chat | ACP or vendor adapter, deltas, tools, cancel, permission requests; unsupported resume/mode switch disabled; no ANSI scraping. |
| R23 | P2 | Workbench artifacts | Authorized diff/file/preview links, explicit share of result back to room; no auto-sharing credentials or logs. |
| R24 | P3 | Device enrollment | Per-install key and identity, enroll grants nothing; list/revoke devices; fail closed when offline for control. |
| R25 | P3 | Browser/file capability | Scope origin/folder/actions, expiry and current agent; prevent traversal/symlink escapes; no silent target fallback. |
| R26 | P3 | Attended desktop observe/control | Selected app/window, OS permissions, visible indicator, independent read/control scope, exclusive lease and emergency stop; OS capability matrix. |
| R27 | P3 | Compute governance | Dedicated DevBox worker, aggregate resource admission, disk quotas, generation fencing, lifecycle audit, idle sleep only if no active sessions/runs. |
| R28 | P4 | Enterprise deployment | SaaS and self-hosted/dedicated config parity; signed update, offline/local cache settings, retention, admin controls, recovery exercises. |
| R29 | P4 | Plugin distribution | Signed allowlisted registry and private catalog, version pin, review on scope widening, revoke/disable, no mandatory public marketplace. |
| R30 | P4 | Accessibility and i18n | English/Vietnamese, keyboard-only flows, screen reader labels, reduced motion, light/dark/system, zoom and responsive layout. |
| R31 | P4 | Operability | Contract, load, fault, security and real OS tests with evidence, trace parity, supportable error codes and diagnostics. |
| R32 | Later | External runtimes, voice/video, arbitrary custom UI outside MCP Apps | Only ship after explicit demand, interoperability/security tests; no parity claim for unsupported capability. |
| R33 | P1 | First-time native onboarding | First-party Dewee SSO returns to Emree; separately audience-bound desktop session, no dashboard detour, duplicate accounts or generic SSO bearer. |
| R34 | P1 | Workspace create/join | Explicit entitlement/region/confirmation, verified invite, idempotent provisioning, resume and switch; no redundant workspace. |
| R35 | P1 | Guided Dewee setup in Emree | Reuse server-owned Concierge/structured cards, provider verification, Super Agent handoff; owner/member/managed role split; secret-free recovery. |
| R36 | P1 | Friendly recovery and first value | New, existing, invited, multiple workspace, offline/expired, no provider, interrupted and self-host capability scenarios; no repeated completed steps; permitted human chat independent of AI readiness only under canonical policy. |
| R37 | P1 | Federation runway | Issuer-qualified principal, resource ownership, room authority and policy epoch in contracts; external collaboration off by default. |
| R38 | Later | Shared rooms and cross-workspace DMs | Bilateral approval, exact participants, owner ordering, explicit invitation/history policy/block/revoke; no full tenant membership. |
| R39 | Later | Shared resources and agents | Explicit scoped owner grant, remote execution in owner workspace, provenance/ACL/budgets, no credential or transitive sharing. |
| R40 | Later | Cross-workspace governance | Both policies intersect, retention/residency/DLP/export/legal hold, auditable consent and conflict denial; disclose recipient copies. |
| R41 | Later | Cross-cell / independent deployment collaboration | Authenticated relay, authoritative commit ACK, dedup/recovery/partition fail-closed, key rotation, independent security tests. |
| R42 | P1 | Native MCP core | Modern stateless discovery and invocation, tools/resources/prompts, schema validation, auth and legacy negotiation; supported peers tested, no silent fallback or unsupported advertised capability. |
| R43 | P1 | Native MCP forms/MRTR | Schema-driven form, accept/decline/cancel, secure URL handoff, durable resume and requester privacy; no forged consent. |
| R44 | P1 | Native MCP Apps | Standard sandboxed bridge, scoped UI resource, inline and panel, permitted actions, text fallback, revocation and teardown; real official sample and IPC escape tests. |
| R45 | P2 | Native MCP Tasks | Durable external handle linked to Dewee tasks, poll/update/cancel per spec, pending inputs, resume after disconnect and verified result; no duplicate effect or false cancel. |
| R46 | P1 | Portable plugin import/export | Agent Plugins 1.0.0 and Claude/ChatGPT/Codex adapters, provenance, component-level compatibility and scope report, fresh auth, no automatic hooks or proprietary credentials. |
| R47 | P2 | OpenAI extension compatibility | Sidebar/panel/settings/onboarding, composer mentions, display/deep links, file and form extensions, message/context bridge with safe draft and permission semantics; unsupported features explicit. |
| R48 | P4 | MCP auth and interoperability matrix | Stable applicable official authorization extensions, pinned versioned contract/real integration tests across clients and surfaces; capability registry, security gates. |
| R49 | Experimental | Beta/draft MCP extension support | Skills/Events per pinned revision, file transfer experiment behind flags; do not claim draft stable or advertise unimplemented delivery modes. |

See [MCP host](MCP.md), [plugin compatibility](PLUGIN-COMPATIBILITY.md) and [Clarkcant research](CLARKCANT-RESEARCH.md). R42–R48 are release requirements; R49 is independently staged.

See [native onboarding](ONBOARDING.md) and [federation](FEDERATION.md). R38–R41 require separate pilot/release decisions, not first GA blockers.

## Initial delivery gates

The [MVP roadmap](../plans/MVP.md) supersedes the original all-at-once alpha ordering. Existing-workspace real chat is the first useful preview, followed by native signup/create/join, two-human room, agent work, and verified MCP Form/App/Task plus portable plugin. Private DevBox TUI is independently gated; Computer Use, full vendor/enterprise conformance and federation follow separate releases. All R01–R49 remain product commitments, not claims of initial MVP completeness.

## Scope and non-goals

Alpha: first-time native Dewee SSO/signup, create/join/setup, rooms/DMs, threads, realtime sync, basic attachments, extension registry, native MCP core/forms/Apps and portable plugin import, real Dewee integration and federation contract runway. Beta adds agent work and remote DevBox TUI then structured chat; Computer Use is independently flagged. GA requires enterprise reliability and secure distribution. Do not rebuild Slack calls, Notion docs, CRM, IDE, agent runtime, marketplace backend, or cross-region active-active replication. Do not promise all native Computer Use actions on every OS. Cross-workspace live collaboration remains separately gated.

## Proposed acceptance targets

p95 durable message ACK <=300 ms and room delivery <=500 ms in the same test region; 1,000 concurrent clients/cell alpha, 10,000 beta subject to benchmark; no loss of acknowledged messages in defined recovery tests; zero unauthorized delivery/side effect in negative suites; local emergency stop p95 <=250 ms for prevention of further input (cannot undo committed actions). Startup p95 <=4 s and total idle memory p95 <=350 MB measured on reference machines. GA availability target 99.9%, define service boundary and RPO/RTO in infrastructure spec. No target is measured yet.
