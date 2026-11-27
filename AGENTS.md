# Agent guidance for Emree

Emree is greenfield, Apache-2.0, Dewee-powered, plugin-first and currently design-only. Start with DECISIONS.md, docs/PRD.md, docs/ARCHITECTURE.md, plans/IMPLEMENTATION.md and the relevant subsystem spec. Treat draft contracts and targets as proposals. Check real upstream contracts before implementation; do not duplicate Dewee authority or orchestration, do not copy restricted code.

- Use smallest vertical slice; add seams when a real consumer needs them. No speculative framework or unnecessary services.
- Auth, tenant isolation, grants, consent, audit and stop are non-replaceable core. Never give renderer/plugin raw token, shell, generic proxy or privileged native IPC. User input, agent output and guest data are untrusted.
- Every change includes success, denial, cancellation/retry/recovery and cross-tenant tests where relevant. Server checks current authority at side effect; no prompt-only enforcement. New privileged surfaces default-off.
- Keep durable message truth in Dewee/Postgres; transport/cache is disposable. Do not replay uncertain side effects. Terminal/media/device streams independent of chat.
- Official modules exercise SDK; unload cleans all owned resources. Runtime adapters execute in appropriate guest/worker, not Gateway host.
- Preserve CLI/MCP/CCP/RGD/trace parity on Dewee changes or state N/A; only operate on repositories explicitly authorized. Never change production or credentials without approval.
- Use English public technical documentation; user-facing UI English/Vietnamese. Mock data clearly labeled. No private operational details or secrets in public files.
- Before completion: run relevant tests, report exact evidence and gaps, update requirements/ADR/plan. Never claim unexecuted tests passed. Use separate PR for implementation and avoid committing unrelated work.
