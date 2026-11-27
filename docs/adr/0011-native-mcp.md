# ADR 0011: Native MCP host and modern protocol

Status: proposed baseline, verification gates required; 2026-11-27. D19. [MCP spec](../MCP.md).

Implement stable current MCP core (2026-07-28 at review), official Apps/Tasks/auth extensions as first-class product capabilities, MRTR and native elicitation, with explicit negotiated legacy compatibility. Dewee owns MCP connection/security/task/interaction authority; Emree renders native forms/apps/task controls via scoped protocol and isolated iframe. Use existing secure client/server infrastructure and test SDK upgrade; no generic relay or new renderer-held tokens. Clarkcant patterns inform native widget catalog, bridge and durable effects, not legacy transport. Beta Skills/Events and draft files independently gated. No unsupported capability advertisement. P0/P1 proof and release matrix required; fallback to text/secure handoff or unsupported error.
