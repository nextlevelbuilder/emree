# Milestone evidence ledger

For UX/AX evidence, consult [proposed standards](../docs/UX-AX-STANDARDS.md): record reference device/network, actual interaction timings, novice and agent journey outcomes, permission/recovery behavior and review. Proposed numerical budgets require calibration; do not report PASS based on mock UI or unexecuted tests.

Initial status: NOT TESTED for all gates. Planning/issue creation is not implementation evidence. This ledger is updated by implementing and independent reviewing agents per merged slice; record reproducible artifacts in PRs and links here. Never publish secrets, customer data, private logs or credentials.

| Gate | Issue | Status | Commit/build, environment, fixture versions, commands/results, trace and review |
|---|---|---|---|
| G0 | #2 | NOT TESTED | — |
| G1 | #3 | NOT TESTED | — |
| G2 | #4 | NOT TESTED | — |
| G3 | #5 | NOT TESTED | — |
| G4 | #6 | NOT TESTED | — |
| G5a | #7 | NOT TESTED | — |
| G5b | #8 | NOT TESTED | — |
| G6 | #9 | NOT TESTED (independent) | — |
| G7 | #10 | NOT TESTED | — |

For each gate: exact scenario/user/role/scope (synthetic IDs only), pinned Emree/Dewee SHA, OS/edition/transport and capability versions, build digest, test commands/results, denied/retry/revoke/recovery evidence, authorized trace reference, limitations, independent reviewer and owner decision. Use PASS only if acceptance demonstrated on actual required environment; FAIL when tested and unmet; BLOCKED with exact dependency and next action; NOT TESTED otherwise. Mock-only evidence never passes real integration gate. Security/privilege denial, no acknowledged-message loss within declared fault model, no false advertised compatibility and rollback are required where applicable. Record actual accepted slices per day, review pass, retry and blocked elapsed time, not human hours. No production actions without explicit approval.
