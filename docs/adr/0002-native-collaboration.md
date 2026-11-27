# ADR 0002: Native collaboration and home cells

Status: proposed baseline; D02,D05,D14.

Keep authoritative native rooms/messages/ACL in Dewee with PostgreSQL transaction + event + outbox. Existing channel coordination remains; explicit adapter binds a native room to its coordinator. Alternative: fork a chat server or directly reuse channel room schema; rejected due duplicate identities/authorization or incompatible human membership semantics. Home cell owns writes; no global active-active DB. Need migration, current ACL at dispatch/replay, scoped FKs and cursor tests. A02/F02 contract review decides schema; rollback via expand/contract migration, flag off and preserving old channel behavior.
