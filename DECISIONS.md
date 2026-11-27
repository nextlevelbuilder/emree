# Emree decision brief

Version 0.4 | 2026-11-27 | Design baseline, not release approval.

## Executive decision

Build Emree as a public, greenfield desktop client for Dewee. Optimize completed and accepted team work, not message volume or number of agents. Do not fork Buzz, OpenAgents, agentes.chat or a previous desktop application. Study their solutions but implement original code against public contracts. Dewee remains the sole authority for identity, organization, permissions, shared tasks, agent execution and data. Emree adds the user experience, safe device execution and extensibility.

### Tóm tắt quyết định

Emree là desktop app mã nguồn mở của Dewee, nơi con người và AI agents cùng làm việc. Xây mới trên Tauri 2, Rust, React/TypeScript; không nhúng Gateway riêng. Thiết kế plugin-first từ foundation, giữ security kernel không thể bị extension ghi đè. Realtime messaging, durable jobs, media và DevBox terminal là các luồng độc lập. DevBox Workbench mở ở right sidebar, có TUI và Chat UI. Compute, phân quyền và dữ liệu chia sẻ thuộc Dewee. Bắt đầu nhỏ, đo tải thực tế rồi mở rộng theo cell; không xây toàn bộ marketplace hay hạ tầng phân tán ngay từ alpha.

## Decision register

Status: **Baseline** means use for planning; **Spike** means preferred, not locked until acceptance evidence; **Deferred** means not in initial release; **Owner** means an explicit decision is still required.

| ID | Status | Decision | Alternative / rationale / revisit |
| --- | --- | --- | --- |
| D01 | Baseline | Greenfield Apache-2.0 client, public repo | No source copy from noncommercial/copy-left systems without separate clearance. Permissive license with patent grant; owner may review license before external contributions. |
| D02 | Baseline | Dewee authoritative backend; native collaboration is a Dewee module with adapter to existing Team Room coordinator | No duplicate backend, auth, or agent runtime; avoid breaking external group/mailbox semantics. |
| D03 | Spike | Tauri 2, Rust, React/TS, Vite, pnpm, Cargo | Electron fallback if WebView/IPC/accessibility/build benchmarks fail. Wails not preferred for greenfield. |
| D04 | Baseline | Plugin-first with non-replaceable trusted kernel; built-ins consume versioned extension contracts | Do not run arbitrary npm/Rust third-party code in privileged host. First release declarative only; sandboxed UI later. |
| D05 | Baseline | Home-cell routing, PostgreSQL authoritative, client SQLite cache | No cross-cell shared DB; no global chat DB or active-active writes. |
| D06 | Spike | Centrifugo + Valkey for realtime; scoped connection/subscription authorization; REST durable writes, WS event delivery | Self-built Go WS fallback if revocation/recovery gates fail. Keep durable event cursor independent of transport history. |
| D07 | Baseline | Transactional PostgreSQL outbox and bounded workers initially | NATS JetStream later only when measurable lag/contension requires it; no duplicate broker just for fashion. |
| D08 | Baseline | Private R2/S3 attachments, direct authorized upload, separate scanning/transform | Local filesystem/S3 for self-hosted; no binary payload in chat or blind reuse of runtime workspace storage. |
| D09 | Baseline | DevBox Workbench as first-party extension; remote PTY/TUI first, structured Chat second | ACP/SDK adapters per runtime; do not parse ANSI into authoritative chat; no assumed TUI/chat session migration. |
| D10 | Baseline | Separate DevBox execution worker and session gateway before broad shared access | gVisor on supported Linux hosts; no raw Docker fallback or host shell; fence by generation and lease. |
| D11 | Baseline | Attended Computer Use via isolated Rust device broker; per-resource grants and local stop | Browser/file access before GUI input; native providers; no global always-on control or unattended personal desktop in v1. |
| D12 | Spike | Cloudflare edge/Workers/R2 plus existing Dewee infrastructure, Linux K8s or Docker Compose | Confirm actual regions, cost, residency, HA and load; portable interfaces, no Cloudflare requirement for on-prem. |
| D13 | Deferred | WebRTC/SFU calls, arbitrary sandboxed UI plugins, public marketplace, local external runtimes | Establish seams now, deliver only if real usage justifies; signed private catalog first. |
| D14 | Baseline | Secure authorization and trace parity across Gateway, desktop, CCP/RGD, CLI and appropriately scoped MCP | No new generic privileged proxy, no UI-only authorization. |
| D15 | Owner | Product launch scope, staffing, pricing, supported OS versions, enterprise retention/data residency | Plan assumes 5–7 engineers; actual targets require product owner and security review. |

## What makes this worth building

Dewee's durable value is agent coordination, governance and organizational knowledge. Emree makes that accessible in everyday work. A Slack clone alone is not a defensible product. A new terminal alone is also not a defensible product. The combination of human/agent collaboration, shared work context, safe execution and visible evidence is the hypothesis.

Validate with 3–5 design-partner teams; measure weekly accepted human+agent tasks, time-to-first accepted result, inbox resolution, adoption, failed/retried work, support incidents and cost per accepted task. Baseline metrics during alpha before setting growth targets. Do not optimize tokens or messages as proxy for customer value.

## Gates before irreversible implementation

- Confirm licensing and contributor policy; no private operational details, secrets or copied restricted code in this public repo.
- Demonstrate two humans and one agent in a native room using real Dewee authorization; offline/reconnect and revoked-user negative tests.
- Tauri benchmark on target OS; DevBox PTY on real gVisor; verify runtime terms and version compatibility.
- Demonstrate one official and one external declarative extension installing, refusing unauthorized actions and unloading cleanly.
- Prove device grants, exclusive input lease and emergency stop in a test app before enabling any privileged feature.
- Confirm data placement, retention, tenant isolation, recovery and budget limits. Stop release on any cross-tenant leak, host escape, unbounded privilege, unverified updater, or accepted-message loss within specified fault model.

Revisit decisions via an ADR/PR with alternatives, evidence and migration cost. Never treat this brief as a production security assessment.
