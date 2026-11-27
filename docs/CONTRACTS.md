# Proposed API, data and event contracts

These names are draft contracts for joint review with Dewee; they are not claims about existing routes. Server enforces verified principal, tenant, membership, resource ACL and operation-specific policy at side effect. Never accept tenant/user identity from an untrusted request field. Use existing Dewee error/envelope conventions where possible.

## Version negotiation and auth

Discover supported capabilities and protocol range through an authenticated workspace bootstrap. Native public OAuth client uses authorization code + PKCE S256, state, external browser, exact registered redirect (loopback or claimed HTTPS as appropriate), rotation/reuse detection, revocation and expiry. Scoped access tokens remain native; refresh material in OS secure store only; no master keys or raw tokens in WebView. Treat self-hosted origins as explicit connections, TLS validated. Desktop server switching cancels requests and subscriptions, clears previous projection.

Proposed bootstrap fields: workspace_id, runtime_tenant_id (server-derived mapping), home_cell, api_version, realtime_endpoint, feature/capability flags, policy_version, user profile and permitted action descriptors. No infrastructure credential. Realtime and DevBox attach tickets separately audience-, resource-, connection- and expiry-bound; never persist/log ticket in URL. Validate negotiated version; refuse unsupported features gracefully.

## REST surface (candidate v1)

| Method/path under /v1/collab | Semantics |
| --- | --- |
| GET /rooms | Cursor list visible rooms and unread projection |
| POST /rooms | Create with name, visibility and authorized invites |
| GET/PATCH /rooms/{id} | Read/edit with version, policy and ACL |
| GET/POST/DELETE /rooms/{id}/members | Membership transitions, version checks |
| POST /dms | Idempotent canonical participant set (tenant scope) |
| GET /rooms/{id}/messages?before=... | Cursor history, stable ordering |
| POST /rooms/{id}/messages | Durable send; client_message_id idempotency |
| PATCH/DELETE /messages/{id} | Versioned edit/tombstone; retention rules |
| PUT /messages/{id}/reactions/{emoji} | Idempotent set desired reaction |
| PUT /rooms/{id}/read-state | Monotonic read cursor, bounded to visible seq |
| GET /sync?cursor=... | Authorized durable event replay; reset if expired |
| POST /realtime/ticket | Scoped short-lived realtime connect ticket |
| GET /inbox | Actionable tasks, mentions, approvals and unread |
| GET /search?q=... | ACL-checked lexical results |
| POST /attachments | Allocate private staging object and upload grant |
| POST /attachments/{id}/complete | Verify upload, queue scan idempotently |
| GET /attachments/{id}/download | Recheck ACL then issue short-lived URL or proxy |
| POST /resource-links | Link existing permitted task/run/knowledge/artifact |
| POST /actions/{actionId}/invoke | Typed allowed operation, policy/approval; not generic REST proxy |

Use separate control-plane routes for DevBox/device sessions (see specs). HTTP 401 unauthenticated, 403 denied, 404 for inaccessible resources as appropriate, 409 stale/idempotency conflict, 413 size limit, 429 rate limit with Retry-After, 503 unavailable. Error includes stable code, localized safe message, request_id, retry classification. Unknown outcome requires read/reconcile rather than automatic repeat of a side effect.

## Data model (PostgreSQL, migrations in Dewee)

| Entity | Key / invariant |
| --- | --- |
| collab_rooms | tenant_id + room_id; visibility, version, current event seq |
| collab_members | tenant_id + room_id + principal kind/id; state, role, version |
| collab_messages | tenant_id + room_id + message_id; author type/id, thread root, body, version, message seq, deletion marker |
| collab_events | tenant_id + room_id + event_seq; event_id, type, resource version, visibility; committed in same tx as change |
| collab_read_state | tenant_id + room_id + user_id; last_read_seq nondecreasing |
| collab_reactions | tenant_id + message_id + principal + emoji unique |
| collab_outbox | event_id/job_id unique; attempt, next_attempt, lease, status |
| collab_room_binding | native room + existing coordinator destination, explicit policy |
| collab_resource_links | typed ref to source authority, no copied secret values |
| attachments | tenant/resource scope, object key, size/checksum/type, scan state, retention |
| device_installations/grants/commands | separate broker domain; see device spec |
| devbox_sessions/session_events | execution domain; generation and control lease; see DevBox spec |

Foreign keys include tenant scope where feasible; enforce invariants and negative tests in both PG and supported SQLite server edition when applicable. Use UTC server times, opaque IDs, optimistic versions. Room event sequence allocated under transactional row lock; do not use client timestamp as order. Durable event stream can contain sequence gaps invisible to a given user: sync cursors are server-issued opaque and advance across filtered records without exposing hidden content. Cross-room ordering not promised. Index (tenant,room,event_seq), (tenant,room,message_seq), outbox next_attempt; unique (tenant,room,actor,client_message_id) plus payload hash to reject conflicting retries. Partition only when measured.

## Event envelope (draft)

```json
{"schema":"emree.event.v1","event_id":"evt_example","type":"message.created","room_id":"room_example","seq":"42","resource_version":1,"occurred_at":"2026-11-27T00:00:00Z","trace_id":"trace_example","payload":{"message_id":"msg_example","author":{"kind":"human","id":"user_example"},"text":"Hello"}}
```

Sequence is a decimal string to avoid JS integer precision. Tenant route is bound by connection, not trusted from payload. Separate delivery scopes for room public events, addressed/private events and operator evidence; emit only recipient-authorized projections. Durable: message created/edited/deleted, membership, task and approval transitions, run final. Ephemeral: typing, presence, run deltas. Device command and terminal streams are separate protocols; never broadcast raw tool payloads or secret prompts. Unknown safe event types ignored with telemetry; unknown security/command types rejected. Preserve event identity across retries and transport changes. Replay authorization is current, not historical.

## Delivery correctness

Transaction commits message + event + outbox before ACK. Outbox at-least-once, consumer inbox dedup and idempotent effects. ACK means durable under declared DB durability, not read by recipient. Client keeps draft/pending/confirmed/failed state; retry same ID if outcome unknown; reconcile with authoritative receipt. Subscribe/replay handoff uses server high-water boundary and buffers live events; on gap get delta or snapshot; never discard a gap silently. Details in [REALTIME.md](REALTIME.md).

## Compatibility

Canonical JSON Schema/OpenAPI and fixtures live in contracts/ after joint review. Generate Rust and TS types in CI; round-trip golden fixtures. Additive changes within major version; unknown optional fields tolerated, invalid scopes/commands denied. Versioned extension and realtime contracts independent of app version. Dewee CI verifies Gateway/CCP/RGD/CLI/MCP coverage where applicable, and Emree CI verifies compatibility against supported Dewee versions. Privileged surfaces ship off by default.
