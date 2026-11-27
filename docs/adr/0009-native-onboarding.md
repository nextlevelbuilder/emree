# ADR 0009: Native onboarding through Dewee SSO

Status: proposed baseline; 2026-11-27. D16. See [onboarding spec](../ONBOARDING.md).

Emree must support a first-time user without a prior Dewee account/workspace. Reuse hosted Dewee OIDC identity, provisioning, invite acceptance, Setup Concierge, structured cards and quiet Super Agent handoff. Current SSO access tokens are userinfo-only; first-party native client and separate desktop credential issuance are required. Prefer system-browser PKCE with validated native redirect, no embedded login, no browser-cookie scraping or second account/setup engine. Existing workspace, invite, new workspace and self-host paths resume authoritative state. Default Router only when eligible, with explicit costs/consent. Native session/redirect and provisioning contracts gated by upstream security review, real staging signup/recovery and revoked-token negative tests. Fallback is explicit safe first-party recovery, not pretending setup succeeded.
