# ADR 0003: Realtime transport and durable delivery

Status: preferred candidate, spike required; D06,D07.

Separate durable PostgreSQL message commit from realtime transport. Centrifugo + Valkey is candidate for fanout/history/presence; custom Go WS alternative. Current Dewee event fanout is suitable for its existing runtime events, not automatically a durable native room log. PG outbox + bounded worker initially; JetStream only after measured bottleneck. Do not duplicate broker roles. Use current policy epoch, ACL on subscription/dispatch/replay, opaque durable cursor, idempotent sends and effects, bounded buffers. F03/A03 prove revocation, crash/replay and latency. If transport fails, swap adapter and resync from authoritative log; no data migration for transport history.
