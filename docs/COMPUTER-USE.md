# Computer Use and device execution

## Principle

Dewee decides whether an action is authorized. A local Device Broker verifies again and executes only scoped actions through OS-native providers. Enrolling a machine grants no access. Native Computer Use is independently opt-in and initially attended; no unattended personal desktop control or general local shell.

## Native architecture

Tauri host has typed IPC to separately supervised Rust broker; broker runs as user with minimum privileges, no service-wide admin. Device generates installation identity/key in OS secure storage; enroll with proof of possession and authenticated principal, outbound TLS connection; rotate and revoke. Avoid listening on a public port; local IPC authenticated and limited to app identity, per-session nonce. Keep long-lived credentials out of WebView, logs and extensions. Device broker must maintain local kill switch and visible indicator outside untrusted renderer. On disconnect/locked desktop, deny new commands, release held input and expire control lease. On broker crash, recovery checks OS state, no automatic continuation of uncertain effects.

| Provider | Platform / initial capability |
| --- | --- |
| Browser | Existing Dewee connected-browser relay; exact origin/tab scope, no silent managed fallback |
| Files | User-chosen folder/file handle, read vs write, safe realpath/symlink/TOCTOU protections |
| macOS | ScreenCaptureKit, Accessibility/CoreGraphics; test permissions/signing, scoped app/window |
| Windows | Windows Graphics Capture, UI Automation/SendInput; respect desktop lock/UIPI/elevation |
| Linux | Wayland portal/PipeWire with compositor-dependent capability; X11 only after separate review |
| DSH/Cua Driver | Optional trusted adapter candidate, no arbitrary code bridge or bypass of grants |

Providers register supported capability, version and platform constraints; one exclusive input lease per device/session. DSH provider registry is reference for exclusive registration and cleanup, not an authorization boundary. Avoid automatic fallback to another provider/target when scope differs.

## Grant and command contracts

Device installation identity, tenant, principal, agent/allowed session, capability, exact resource/window/origin/path, max duration, policy version, grant ID and nonce. Enrollment independent from grant. Suggested capabilities browser.read/control, files.read/write, desktop.observe/control. Observation and control separately granted; capture only selected surface and re-check target focus/identity before action; deny protected/secure surfaces and permission dialogs, never automate consent. Native system permissions are separate from app grant; OS permission is not user consent for every action.

Command envelope: command ID, device/grant/agent/session IDs, policy epoch, lease generation, target handle, action enum + validated args, deadline, trace ID. Gateway checks actor delegation, resource ACL, policy, grant and quota; broker checks same binding/freshness, local consent, lease and OS capability; enforce deadlines and bounded output. No remote arbitrary JavaScript/native/shell command. Ack/result separate; duplicate command ID handled without repeating side effect; if result lost after dispatch, mark outcome_unknown and re-observe. Cancel/stop prevents further input, does not undo past effects. Revocation invalidates live lease, closes session or pauses; local stop works without network. Deny on policy unavailable; no stale cached positive authority.

## UX

Device panel shows enrollment, current connection, active grants, agent, allowed app/window, countdown, activity and status. Native consent describes requester, specific resource, capabilities, duration and consequences; read only default; explicit grant for control. Human can pause, takeover (releases all held keys/buttons), resume via fresh authority, revoke and emergency stop. Show safe error states offline, permission denied, unsupported OS, protected desktop, expired grant and unknown action outcome. No background activation hidden in plugin. Audit safe metadata/hash/outcome; screenshots/accessibility trees are sensitive, encrypted bounded ephemeral data; no recording by default. Live monitoring (optional later) WebRTC viewing does not confer input control.

## Testing and release gate

Real Windows/macOS test machines and Linux supported compositor matrix; no claimed universal parity. Test app focus change, modal, lock screen, permission revocation, lost socket, replay/late commands, held keys, multi-agent concurrency, path traversal, stale window handles and malicious screen prompt injection. Benchmark local emergency stop p95 <=250 ms to prevent further dispatched input; no guarantee about already delivered OS events. Screenshot/credential leakage negative tests and explicit retention controls. Rollout behind default-off flags; fail closed if broker/OS checks fail.
