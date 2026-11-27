# Emree UX/AX standards — complexity absorption

Status: **proposed design standard for review**. 2026-11-27. Applies to desktop, first-party modules, third-party presentation, native onboarding, agent-mediated operations and future shared contexts. Complements [UX specification](UX.md), [decision brief](../DECISIONS.md), [security](SECURITY.md), [realtime](REALTIME.md), [MCP](MCP.md) and [MVP gates](../plans/MVP.md). No new milestone or implemented-feature claim. Numerical targets are acceptance hypotheses pending reference-device/user-test calibration.

## 1. Product intent

**People should know what they want to accomplish, not how the system works.** Emree and Dewee absorb technical complexity. Calm, immediate and forgiving interaction is achieved through state consistency, predictable policy, resource-aware execution and excellent implementation, not by removing essential capabilities or hiding material consequences.

- Familiar chat first; advanced workbench and governance reveal themselves when relevant.
- Defaults ready to use; customization remains accessible in settings, command palette and authorized conversation.
- A fast, clear result and safe recovery are more important than more menus, more AI, more motion or more services.
- Do not confuse invisible implementation with invisible authority, cost, data sharing, consent or irreversible effects. Do not trade governance for speed.
- Every feature is judged together on UX (people), AX (agents), and CX (integrators). No feature is complete merely because its endpoint works or a mock looks good.

Research basis: Telegram TDLib asynchronous API, ordered updates, local database and unreliable-network handling; Telegram cloud drafts, chat folders and granular rights. Learn these principles, not Telegram-specific protocol or unverified internal implementation of Telegram Desktop. Nielsen usability heuristics and response-time limits; Apple HIG clarity/familiarity/agency/craft; Microsoft HAX human-AI guidelines; WCAG 2.2 AA as applicable to web/native presentation. Sources at end. No comparative app benchmark has been run.

## 2. Twelve principles

Instant feedback; continuity; default-first; no technical jargon in normal flows; conversation-native actions; self-service within authority; recoverability; transparent authority; calm attention; context-aware assistance; progressive disclosure; truthful states.

Normal paths have one obvious next action. Do not make a beginner choose a model, region, protocol, runtime, tool scope, plugin dependency or delivery transport when an already authorized safe default suffices. A power user can inspect and change permitted options without compromising another user. A model must not become the implementation of a deterministic preference, authorization or synchronization rule.

## 3. Interaction and default contract

For each user-visible decision, classify whether it is a safe implementation detail, personal preference, material user intent, authority change or irreversible external effect.

| Class | Default behavior |
| --- | --- |
| Automatic technical handling | Resolve supported locale/theme/timezone, routing to authorized home cell, pagination/cache/reconnect, image thumbnail, dependency compatibility, version detection and retry of provably safe idempotent reads/writes. Do not ask the user. |
| Personal reversible preferences | Use sensible defaults, save changes after verified action, allow reset/real undo. Agent may adjust typed preferences within explicit user authority, report what changed. |
| Material choice within existing authority | Choose recommendation, show context and impact; ask only if ambiguity materially changes outcome (recipient, budget, data scope, irreversible effect). User can make a permitted choice directly. |
| New authority, expense, sensitive sharing, persistent automation or irreversible consequence | Apply canonical policy; use trusted approval/consent/secure handoff when required. Never infer approval from chat context or a plugin. Existing bounded explicitly approved automation may execute within policy, budget, time and target; no repeated approval of unchanged low-risk scope unless policy requires. |
| Denied / unsupported / unknown | Deny or reconcile, provide safe next step and reason, never broaden scope, silently switch target or replay an uncertain side effect. |

Resolve each preference by a typed deterministic policy: system defaults, organization/workspace configuration, permitted user override, optional bounded contextual recommendation. Operator restrictions, organization constraints, resource ACL, explicit consent and kill switches always win, independent of precedence. Distinguish hard policy from preference; show inherited vs customized and why locked, allow reset to inherited default. Persist version, source, owner, scope and revision; verify current authority on mutation. Adaptive suggestions do not silently change privacy/security/billing or explicit preferences. No inference of a user's instructions from other participants' messages. No arbitrary auto-install, provider spend or background recurrence.

Proposed Settings command path: authorized agent calls the same versioned typed settings operation as UI, with explicit target/principal/revision; returns applied value, source/effective value, changed fields, restart requirement (if any) and safe undo/reset capability where supported. No direct SQL, generic shell, raw token or Computer Use to manipulate settings that have an API. UI reflects canonical change across devices, with conflict handling. Avoid asking for information already present and permitted in the current context.

## 4. Telegram-inspired multiplayer UX

| Surface | Required behavior and edge cases |
| --- | --- |
| Shell/navigation | Chat list as default home, one workspace switcher with recent/active state, keyboard search and named entity picker. Restore permitted last workspace/room/panel, position, selection and draft after switch/restart/reauth. Clear old tenant views before switching; never restore unauthorized history. Narrow window prioritizes conversation. |
| Timeline | Stable virtualized list, server ordering, group consecutive messages, clear human/agent/service actor and on-behalf attribution, relative time with accessible exact time. Preserve scroll anchor on incoming messages or older history; jump-to-unread and new-message counter; autoscroll only if already following end. Media placeholders reserve size. No entire timeline rerender for token deltas. |
| Composer | Always available when allowed, focus remains predictable; Enter/Shift+Enter configurable, Vietnamese IME composition never sends prematurely, paste/drop/multi-file, reply/edit/mention/command, accessible suggestions. Drafts keyed by account/server/workspace/room/thread; private by default; encrypted persistence or disabled by policy; cross-device draft sync requires explicit supported policy, conflict preservation, no draft sent to other room/agent until user submits. |
| Delivery | Immediate local pending bubble; durable ACK = sent, recipient delivery/read only if actual authorized evidence and policy permits. Failed/unknown distinct, safe retry same ID with payload hash, retain unsent draft, no duplicated sends or blind retries. Queued/offline send visibly pending and cancellable before dispatch, reauthorize when online; no offline approvals or privileged actions. |
| Search/organization | Instant local permitted recent results then remote results with status, scoped by workspace and ACL. Recent/favorites/pins/archive/mute/folders progressively introduced, no mandatory taxonomy. Search deleted/revoked content never returns unauthorized snippets; no broad global directory. |
| Concurrent edits | Use resource revision/compare-and-set and visible conflict, preserve user's draft. No silent last-writer wins for material shared state; use explicit merge/reload/retry. Duplicate action/approval is one logical effect. |
| Long work | Task/agent/DevBox operation continues in its authorized server session after UI closes, if supported; status, detach vs cancel vs terminate explicit, reopen exact task/session/artifact. Unknown and partial results visible; verify before declaring complete. |
| Rich content | Native bounded schema widgets for common data/forms, MCP Apps isolated via standard bridge. Inline first, open to side panel/fullscreen as allowed; historical snapshot distinct from live instance; exact owner/permission checked again. Trusted approvals/credential/device controls cannot be imitated by plugins. |
| Multiplayer boundaries | Visible sender and recipient; personal DM/private input and group publication separate, share preview for files/results, no automatic disclosure of private transcript. External workspace badge and owner/participants/policy summary in future federation. Avoid implicit role transfer, agent impersonation or noisy bot-to-bot chatter. |

Performance architecture: local SQLite projection and scoped in-memory state, durable Dewee log + transaction/outbox, replay watermark and idempotency. Render permitted cache first, reconcile asynchronously; explicit offline/stale status. Native Rust host owns network/session/sync/cache, renderer owns bounded projections. Event reducer applies causal/resource revisions and opaque visible cursors; filtered gaps are not automatically lost messages. Progressive fetch visible viewport first, prioritized interactive lane; batch stream deltas at measured cadence, offload expensive parsing/thumbnail/indexing, cancel stale queries on navigation; cap buffers, backpressure slow consumers. Keep media/DevBox/agent streaming independently scheduled. No need to replatform merely to achieve smoothness. Do not persist sensitive history if encryption/policy not proven.

## 5. Calm attention and collaboration

Default notifications: DMs, explicit mentions, a task assigned to the person, blocking questions/approvals and relevant completed work. Ordinary channel activity, intermediate agent deltas/tool logs and presence are silent unless user opts in. Separate unread messages from actions requiring a person; Action Inbox groups by task/request, with deadline/status and one safe next action. Deduplicate across devices and retries, respect focus mode, quiet hours and mute; private notification previews off by default. Critical security/control indicators (active device control, revoke, signed-in security status) cannot be hidden by muting chat. Do not mark messages read just because an event arrived; use visible/read evidence. Do not make global interruption the default: inline state, passive toast with real undo, inbox, then modal only for material policy/consent. Never steal input focus during streaming, new messages, agent action or reconnection. Prefer summaries on explicit request or configured cadence; do not send hidden data to a summarizer.

Agents in shared rooms: explicit mention/assignment or approved routing policy, one accountable agent/turn or bounded coordinator handoff, provenance and quiet progress. No auto-reply from all agents. Group sees safe progress/final result only where authorized; private questions go to actual requester. Show tool/run metadata and outcomes, not hidden reasoning. User can mute/dismiss/stop without blocking others' conversations. Limit per-agent/room/workspace fanout, turns, tokens, concurrency and cost; show reason and safe recovery if capped. Human chat independent of model outage.

## 6. Permissions and compliance that non-technical users understand

Normal language: “Can view”, “Can contribute”, “Can manage”, “Can use this agent” (illustrative UI presets, not new backend roles). Only map these to actual verified Dewee capabilities on a named resource and principal; show a preview of exact effective scope. Separate read/write/share/execute/configure rights as relevant, never grant all via label or role name. Start with recommended least-privilege preset, invite by name; advanced inspector shows individual rights, source (direct/inherited/denied), policy owner, expiry, and current effective rights. Show who can access the resource and whether external recipients can see it. Do not expose another tenant's hidden identities or resource names in errors.

When opening a sensitive action, show “who / what / where / until when / impact” in a short human-readable card. Preview changes and additional rights requested; ask once per meaningful scope under policy, not every mechanical step. Offer request access only if canonical policy supports it, otherwise explain safe alternative. An approver sees exact actor, action, scope, reason, data/cost/recipient, expiry and expected revision; decision is bound and rechecked at execution. A user cannot approve beyond their own authority. For shared context, bilateral consent and owner grant intersect both organizations' policies. Show compliance restrictions in plain language, retention/residency/export and revocation consequences at share boundary, not on every screen. If data already downloaded cannot be recalled, say so; incompatible policies block sharing, never silently weaken them.

Trusted native/server UI owns consent, secret entry, device control and stop; distinguish external form elicitation from platform approval. Agent can propose access or change a permitted setting but cannot self-approve or widen authority. Revoking stops future authorized effect/streams and purges controllable cache; cannot undo completed external effects. No audit/trace secret content. No forced admin detour for permitted self-service. UI never substitutes for backend policy.

## 7. Agent experience (AX) contract

AX is the ability of an authorized agent to understand available operations, execute accurately, observe outcomes and recover without babysitting.

- Discovery: filtered capability list (actual actor, workspace, device/room/resource, current protocol/edition/feature flags), typed input/output schemas, human-readable purpose, effects, support status, constraints, required scope and idempotency/cancel/reconcile semantics. On-demand schema discovery and pagination; never dump every tool into every prompt. Avoid near-duplicate tool names, avoid undocumented ad-hoc flags.
- Context: server/host resolves verified principal and delegation, authorized current workspace/room/selected message/task/device/resource and permitted relevant context; agent sees stable reference + name, not arbitrary trusted-looking text. Context source/provenance and read boundaries explicit, no hidden cross-workspace prompt injection, no extra secret or entire message history by default. User can inspect/remove attached context.
- Action: typed same underlying operation as UI/CLI/MCP, canonical authority; clearly distinguish inspect/preview, request, execute, cancel/reconcile. Reuse tools and deterministic paths for routine operations, no GUI clicking as replacement for native capability. Requester-specific results and receipts. Re-evaluate authority at dispatch after delay or approval.
- Outcomes: stable machine-readable code and reason, safe display text, correlation/trace ID, retry eligibility, allowed recovery actions and expiry (only fields permitted to requester). State machine distinguishes accepted/pending/running/waiting input/awaiting approval/partial/unknown/failed/cancel requested/cancelled/verified complete. No invented progress, no raw private reasoning. Unknown external effect cannot be retried without reconciliation.
- Continuation: durable forms, approvals and Tasks, no long-lived model loop while awaiting user. Resume exact invocation with bounded state, idempotent decision, correct requester and scope, no re-asking known inputs; preserve cancellation and expiry. Agent receives verified outcome, not just button click. Unsupported operation returns a safe alternative if actually permitted.
- Autonomy: choose available agent/model/capability deterministically where possible; model-based routing bounded by policy and uncertainty. Do not silently swap vendor/recipient/data classification/cost/permissions. No repeated permission prompt for unchanged authorized scope. Ask one question only if material intent missing; safe assumptions disclosed and easily editable. Explicit bounded delegation for recurring work. Respect stop, quotas, time and budget. No arbitrary privilege escalation or irreversible action because a prompt says so.

Candidate typed outcome shape (internal proposal, not existing MCP wire): outcome, operation_ref, resource_ref, safe_message, reason_code, retry_class, permitted_next_actions[], trace_ref, revision, expires_at. Do not leak hidden resource identity or sensitive data. Actual protocol-specific contracts remain authoritative. Conformance tests prove allow/deny, wrong tenant/requester, stale revision, duplicate, cancel, unknown outcome and recovery.

## 8. Craft and accessibility

Maintain shared semantic tokens and primitives; native platform keyboard shortcuts, menus, text selection, clipboard and focus behavior. System/light/dark theme, user zoom, density options only after default is excellent; avoid fake titlebars or unusual scrolling. Use consistent typography, hierarchy, whitespace and motion, never decorate with excessive AI badges or metrics. Render AI as a participant, not the entire app. Meaningful animation (roughly 120–220ms for simple transitions, measured and interruptible), honor reduced-motion preference; do not delay navigation/network or essential status for animation. Hover-only controls have keyboard equivalent. Clear destructive confirmation with exact resource and action; real undo where supported, not fake reversal. Skeleton only when required; no global spinner blocking unrelated surfaces, no empty-state dead ends or raw error codes as primary copy. Screen reader names, predictable focus, no color-only status, WCAG 2.2 AA applicable criteria, zoom and native assistive technology. Test Vietnamese/English, IME, text expansion, keyboard-only, 390/768/1024/1440 logical widths, light/dark/system and reference platform WebViews. Use accessibility checks plus human testing, not automated score alone.

## 9. Proposed measurable acceptance budgets

Measure p95 unless specified. Report reference hardware, OS/WebView, version, screen refresh, data size, network RTT/loss, cold/warm/cached condition and background workload. These are Emree targets, not measured Telegram statistics. Existing PRD SLOs preserved; adjust targets only with documented evidence. Dedicated benchmark runs, not flaky latency assertions in normal PR CI.

| Measure | Initial proposed target |
| --- | --- |
| Key input to visible character, ordinary typing | <=50ms p95 on reference machine under concurrent streaming |
| Local click/shortcut to first visible feedback | <=100ms p95, no network wait |
| Open already authorized cached chat / local search first result | <=150ms p95; no caching of denied data; network refresh separate |
| Warm launch to usable shell | <=2s p95; cold <=4s as PRD |
| Send to local pending bubble | <=100ms p95 |
| Durable send ACK / recipient room delivery | <=300ms / <=500ms p95 in declared same-region test workload (existing PRD) |
| Reconnect and resume | <=5s p95 for 500 permitted events after usable network restored, authenticated session valid; no lost ACK/duplicate logical effects in suite |
| Long operation feedback | visible acknowledgement <=100ms local; remote pending/progress as actual evidence; after 1s show context/working, after 10s allow detach/background or cancel where supported, not fake percentage |
| Scroll/composer under agent/media load | target >=95% frames meet refresh-budget in scripted 60Hz trace (16.7ms); document dropped/long frames and actual device, no focus theft |
| Total idle RSS including WebView/sidecar | <=350MB p95 reference workload (existing PRD); measure CPU/soak/growth separately |
| Emergency stop | <=250ms p95 local prevention of further input in device test; cannot undo already performed actions (existing PRD) |
| Safety correctness | zero unauthorized delivery/effect, no duplicate accepted effect, no false success in required negative/fault suites |

These targets are conditional on cached authorization policy, supported platform and specified workload; network failures do not magically satisfy remote latency. Measure stages separately (event/feedback, network, durable commit, delivery, paint). Web INP <=200ms at p75 is external guidance, not a desktop benchmark. Use browser profiler and native timing where supported, actual frame/keypress measurement and trace correlation. Example load profiles: 50/150/300ms RTT, intermittent disconnect/reorder/duplicate, 500 replay events, 10k-message synthetic room, busy agent stream and large-file/PTY workload when available. 1k/10k CCU cell tests remain separate scale gates; don't make them prerequisite for first internal preview. Do not collect raw messages, screen content, keystrokes, credentials or terminal transcript in product telemetry.

## 10. No-manual test and AX evaluation

Before a pilot, recruit 5–8 consenting nontechnical participants for formative tests, plus power-user/accessibility cases, using synthetic accounts. Avoid giving workflow instructions or coaching. Tasks: first-run invite/create, open chat and send/reply, find earlier conversation, recover draft/offline send, ask agent to do bounded task, understand requester/action/permission and approve or refuse, change notification preference directly or by agent, recover blocked action, verify result. For DevBox/device/plugin/federation use milestone-specific scenarios only when implemented. Provisional goal: at least 80% complete each supported critical journey without assistance; report actual numerator/denominator and observations, not statistical population claim. Measure time to first useful task, forced decisions (only material ones), repeated inputs, wrong-target errors, help requests, abandonment, subjective confidence and accessibility barriers. Mandatory auth/legal/security steps are not “avoidable friction.” Existing onboarding timing hypotheses preserved; record user/provider/provision waits separately. Fix repeated novice confusion before adding advanced options.

Agent fixture matrix: pinned model/runtime and tools, representative permitted workflows, denied and ambiguous counterparts, explicit correct target, stale state, scope widening attempt, revoked grant, failure, duplicate, cancel and unknown result. Record accepted end-to-end task success (initial goal >=90% for well-defined permitted fixture scenarios, not general model reliability), avoidable clarifications, invalid tool calls, extra round trips, duplicate prompts, recovery without help, cost/latency per accepted result and policy refusals. 100% correct authority/recipient and zero denied side effects in negative suite are hard release gates, not an average score. Test across model versions, use real external interactions when required, report sample size and limitations. A low-risk action may need no prompt; risky cases must still get the required real consent. Do not optimize away necessary questions.

## 11. PR review and milestone gates

For every change, answer:
1. What human task and agent task does this complete? Is the obvious next action available and named in everyday language?
2. Can Emree/Dewee handle any mechanical choice, repeated entry, routing, retry or setup automatically within existing authority?
3. What can be customized, reset or requested via typed capability? Is policy separate from preference?
4. Does local feedback remain responsive while network, agents, media and terminal are busy? Preserve focus, draft, anchor and context?
5. Are states truthful, recoverable and scoped, including denial/offline/unknown/revoke/concurrency?
6. Can the person understand effective permission/impact and private vs shared scope? No unnecessary prompts, no bypass.
7. Is AX discoverable, typed, idempotent where applicable, traceable, bounded and recoverable through the same authoritative operation?
8. Do accessibility, locale/theme, keyboard, privacy, fixtures and independent review pass? Show exact evidence or BLOCKED/NOT TESTED.

Map to existing gates, not a new epic:
- G0: tokens/primitives, reference platform, input/focus/scroll benchmark fixture, safe defaults and test ledger.
- G1: fast chat, identity-safe restore, draft/stream/cancel/error, clear human vs agent identity; real user and traces.
- G2: no-manual onboarding, no repeated setup, entitlement/role recovery and trusted secure handoffs.
- G3: two-user realtime, scroll anchor, unread/DM/revoke, offline/replay and concurrent edits.
- G4: agent task/approval, context/authority, action inbox, bounded agent attention and acceptance evidence.
- G5a/G5b: native form/apps/tasks, truthful live/snapshot, import compatibility, permissions and cancel/resume/unload.
- G6: separately gated Workbench responsiveness, exclusive controller, detach/reconnect, high-output isolation.
- G7: actual pilot feedback, usability/accessibility/performance/effective authority, signed build and recovery. Media gate remains independent, don't widen scope.
- Later: device stop/consent, external workspace sharing and full supported-platform/interop benchmarks.

Evidence in [ledger](../plans/EVIDENCE.md) includes measured scenario, platform/version, before/after, reviewer and actual result; no mock equals production readiness. Standards proposed here are for owner review; no untested number is a service guarantee.

## Research references

- [Telegram TDLib](https://core.telegram.org/tdlib/) and [Getting started](https://core.telegram.org/tdlib/getting-started): async API, local storage and ordered updates.
- [Telegram updates](https://core.telegram.org/api/updates), [drafts](https://core.telegram.org/api/drafts), [rights](https://core.telegram.org/api/rights), [FAQ](https://telegram.org/faq): recovery, draft sync, granular rights and chat organization.
- [Nielsen Norman Group response times](https://www.nngroup.com/articles/response-times-3-important-limits/) and [usability heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/).
- [Apple Human Interface Guidelines: design principles](https://developer.apple.com/design/human-interface-guidelines/design-principles) and [feedback](https://developer.apple.com/design/human-interface-guidelines/feedback).
- [Microsoft HAX guidelines](https://www.microsoft.com/en-us/haxtoolkit/ai-guidelines/) and [research summary](https://www.microsoft.com/en-us/research/blog/guidelines-for-human-ai-interaction-design/).
- [WCAG 2.2 understanding](https://www.w3.org/WAI/WCAG22/Understanding/) and [INP](https://web.dev/articles/inp).
- [Slack notification preferences](https://slack.com/help/articles/201355156-Configure-your-Slack-notifications): attention policy and scoped notifications.

Source review: Emree main 18788a9, public product/protocol/research documentation reviewed 2026-11-27. No source code from Telegram or other apps copied. This is proposed product criteria, not deployed implementation or empirical benchmark.
