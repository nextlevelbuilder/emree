# Clarkcant research and reuse decisions

Reviewed digitopvn/clarkcant at ae71a7591af2a6c8cd4ec814bd73b28da49254c7 (2026-10-09), default branch snapshot, Apache-2.0 repository header. Source/docs review only; no builds, physical devices, real provider accounts or independent conformance runs. README/ledger explicitly distinguish bootstrap, tests and open external gates. Recheck package/file-level licensing, dependencies and public distribution rights before extracting code; prefer original Emree implementation where coupling high. No copy of code in this documentation.

## Valuable components

| Source | Observation | Emree use |
| --- | --- | --- |
| docs/widgets-and-extensions.md | Catalog JSON widgets and approved custom mini-apps, props/state/actions, snapshots versus live instance, pins; document distinguishes proposed and implemented | First-party declarative renderer, form/chart/table/board/action card, explicit history snapshot, live panel |
| packages/widget-catalog/src/registry.ts, validate-props.ts | Registry and validation seams | Bounded schemas, discoverable capabilities, no full catalog in model context |
| packages/contracts/src/widgets.ts, widget-props.ts, widget-state.ts, surface-composition.ts | Widget and composition contracts | Stable IDs, versions, state revision, typed data/action binding, resource provenance |
| packages/conversation-client/src/WidgetFrame.tsx | Opaque-origin iframe allow-scripts, postMessage, shared frame session validates source/nonce/schema/limits, short URL renewal and revision-aware persistence | Extract design/test patterns into MCP Apps host, not a competing bridge; verify Tauri isolation |
| packages/widget-sdk/src/runtime.ts | Typed lifecycle, handshake before sending, endpoint abstraction, capability checks | Host bridge lifecycle and mocked protocol fixtures, state cleanup; use official MCP Apps wire instead of Clark private wire |
| packages/conversation-client/src/ConversationComposerBar.tsx, composer-suggestions.tsx, use-composer-references.ts | Composer with typed slash skills and @ resource references | Plugin commands, mention providers, agent/task/file references with server-side reauthorization |
| apps/runtime/src/application/widget-actions.ts, widget-perform-tool.ts, widget-voice-action.ts | Action and semantic/voice paths exist | Unified authorized action invocation for click/text/voice, verify exact effect and report unknown |
| packages/contracts/src/tasks.ts, effects.ts, grants.ts | Task/effect/authority contracts and state machine | Durable external MCP task linkage and effect reconciliation; no duplicate task authority |
| apps/runtime/src/work-supervisor.ts, voice-session.ts; packages/voice-adapters | Work supervision and voice adapter seams | Observe/stop through one interface; optional future voice controls and speech/click parity |
| packages/mcp-adapters/src/streamable-http.ts | Legacy initialize plus Mcp-Session-Id, JSON/SSE response path | Do not port as modern MCP core; Dewee current security-aware adapter/SDK upgrade preferred |
| apps/desktop/package.json | Thin Electron shell with shared conversation client, typed IPC, no scheduler (declared design) | Learn separation, not change Tauri decision automatically |
| docs/conformance-traceability.md | Test/evidence ledger, external voice/provider gates remain | Keep executable acceptance ledger with PASS/BLOCKED/NOT-IMPLEMENTED and exact test evidence |

## Recommended UI architecture

Two paths, one permission engine: declarative catalog for first-party data-driven UI (fast, accessible, schema-bound), MCP Apps for third-party rich interactive UI (isolated HTML, standard bridge). Core trusted forms/approvals/credentials/device controls are not authored by untrusted apps. Widget state separates UI draft/ephemeral data, server-authoritative result, revision and historical snapshot. Pin/open current authorized instance into side panel; no mutation of message history into live state silently. Action requests resolve through typed Dewee operation, resource and current principal; no raw tool strings or arbitrary event handler from model. Tool activity UI and semantic actions should be portable across native chat, inbox, Workbench and future voice. Use typed event projection; don't emit hidden reasoning or unredacted tool output.

## Reuse priorities

1. P0/P1: compare dependency boundaries, use schema/registry and composable renderers, test fixtures and accessible shared design patterns. Favor extracting narrowly independent TS pieces after license/dependency review; do not import entire runtime.
2. P1/P2: MCP Apps host using official bridge and Clark-style source/nonce/lifecycle, snapshot/draft, state revisions, bounded messages and host-owned action/consent. Add plugin composer mentions and side panel.
3. P2: task/effect unknown outcome/recovery patterns; existing Dewee Interaction and task store remain authority; no second supervisor/database.
4. Later: voice session/adapter and action parity, media player and widget authoring lab if usage justifies. Do not claim all voice vendors or live actions work; verify physical audio and provider account tests separately.

Non-goals: fork Clarkcant desktop/runtime, run its personal agent engine in Emree, copy legacy MCP transport, duplicate Dewee permission system, allow arbitrary JavaScript into trusted renderer, or infer full MCP conformance from widget documentation. A narrow component extraction requires package provenance, compatible license, dependency graph and proof against Emree host; otherwise write original implementation.
