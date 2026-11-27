# ADR 0007: Attended Computer Use

Status: proposed baseline; D11.

Separate least-privilege device broker with explicit per-device/app/window/file grants, independent trusted consent and local emergency stop. Gateway and broker both check current authority. Alternative universal native shell/control plugin rejected. Browser/files first, native macOS/Windows after hardware tests, Linux capability-dependent. Providers replaceable only by trusted review; OS permission distinct from resource grant. C01–C06 prove revocation, lease fencing, held-input cleanup and local stop; disable feature per OS if tests fail. No silent provider fallback or replay of uncertain side effects.
