# Research and references

The following public sources informed the proposed design. Source inspection is not an execution/security benchmark. Exact versions, licensing and APIs must be checked again before importing dependencies or implementation. Do not copy restricted code or logos; retain notices for permitted reuse.

## Comparable projects

- [Buzz App](https://github.com/block/buzz-app) (reviewed commit cc0d3b01): React/Tauri, channel timeline, activity grouping and composer; learn from UI, avoid adopting its Nostr/relay identity and backend.
- [OpenAgents](https://github.com/openagents-org/openagents) (746b78f5): launcher, local runtime adapters, daemon lifecycle, response outbox. Reuse concepts, not a second workspace platform.
- [agentes.chat](https://github.com/agentes-chat/agentes-chat) (fd7f9cf3): pairing, native controls, approvals, runner and performance tests; AGPL/commercial terms require license review, no code copied.
- [Mattermost](https://github.com/mattermost/mattermost) and [desktop](https://github.com/mattermost/desktop): unread, notifications, desktop operations and enterprise UX.
- [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness/tree/4b399c39a6d1479cd84ca591b4d7c88e24d38a96): plugin lifecycle, typed services, reversible effects, profiles, bundles, conversation node registry, computer-use provider registration and ACP. MIT repository license; inspect package-level notices. Its host plugins may execute in-process; Emree deliberately does not expose that trust level to ordinary extensions.

## Protocols and stack

- [Tauri v2 capabilities](https://v2.tauri.app/security/capabilities/), [sidecars](https://v2.tauri.app/develop/sidecar/), [updater](https://v2.tauri.app/plugin/updater/)
- [Electron security](https://www.electronjs.org/docs/latest/tutorial/security/) (fallback spike)
- [RFC 8252 OAuth for native apps](https://www.rfc-editor.org/rfc/rfc8252.html), [RFC 7636 PKCE](https://www.rfc-editor.org/rfc/rfc7636.html)
- [Agent Client Protocol](https://agentclientprotocol.com/), [xterm.js security](https://xtermjs.org/docs/guides/security/), [flow control](https://xtermjs.org/docs/guides/flowcontrol/), [tmux](https://github.com/tmux/tmux), [gVisor](https://gvisor.dev/)
- [Slack real-time architecture](https://slack.engineering/real-time-messaging/), [Centrifugo](https://centrifugal.dev/docs/getting-started/introduction), [history/recovery](https://centrifugal.dev/docs/server/history_and_recovery), [permissions](https://centrifugal.dev/docs/server/channel_permissions)
- [NATS JetStream](https://docs.nats.io/nats-concepts/jetstream), [PostgreSQL partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html), [Valkey](https://valkey.io/), [SQLite](https://www.sqlite.org/)
- [R2 presigned URLs](https://developers.cloudflare.com/r2/api/s3/presigned-urls/), [R2 pricing](https://developers.cloudflare.com/r2/pricing/), [Durable Objects limits](https://developers.cloudflare.com/durable-objects/platform/limits/), [Cloudflare Realtime](https://developers.cloudflare.com/realtime/), [LiveKit](https://github.com/livekit/livekit)
- [ScreenCaptureKit](https://developer.apple.com/documentation/ScreenCaptureKit), [Windows Graphics Capture](https://learn.microsoft.com/en-us/windows/uwp/audio-video-camera/screen-capture), [RemoteDesktop portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html)

## Evidence status

Upstream Dewee reviewed in November 2026; [integration map](DEWEE-INTEGRATION.md) records high-level observations and dependencies. No production deployment, interoperability benchmark, physical-device tests or security audit was performed by this documentation bootstrap. Numbers are targets or explicit workload assumptions. Public documentation intentionally excludes customer data, credentials, internal operational endpoints and host-level security details.
