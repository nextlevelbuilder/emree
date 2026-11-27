# UX specification and screen wireframes

Design for daily collaboration, not an admin dashboard. Default shell: workspace rail, navigation/rooms, central conversation, optional resizable right sidebar. Users can collapse navigation, resize panels, maximize/pop out Workbench, return via deep link. Keyboard navigation, focus restoration, accessible names, reduced motion, font zoom, English/Vietnamese, system/light/dark. Preserve drafts and sidebar state per workspace; never display previous-tenant content during switching. Native security consent and emergency controls outside plugin UI. Treat visual examples as mock data.

## Screen inventory

| Screen | Key content | Empty/error/recovery states |
| --- | --- | --- |
| Connect & onboarding | Welcome, Dewee SSO signup/sign-in, invite or create/choose workspace, native Setup Concierge and first useful task; self-host secondary | Invalid TLS/origin, expired login, entitlement/provisioning failure, missing provider, unsupported version, role-aware resume |
| Inbox | Mentions, tasks, questions, approvals, grouped status | Nothing needs attention, pending/expired/refused, retry |
| Room | Message timeline, threads, composer, agents, task links | No messages, offline/draft, failed send, removed membership |
| DM | Human or agent participant, private history | Invite unavailable, blocked resource |
| Task board | Authorized linked task, assignee, status, outcome | No task, stale conflict, missing permission |
| Knowledge | Links/authorized search and previews | Resource access denied, removed, index lag |
| Run explorer | Live semantic events, final result, trace link, cancel | Waiting approval, provider error, unknown/recover |
| Devices | Enroll, grants, activity, pause/stop/revoke | Offline, unsupported OS, expiry, permission denied |
| Settings | Server, accounts, notifications, privacy, updates, diagnostics | Secure store unavailable, update rejected |
| Extensions | Catalog, permissions, profile, install/enable/disable/rollback | Incompatible, missing dependency, failure, revoked |
| DevBox Workbench | Box/session selector, TUI/Chat/changes/files, lifecycle | Sleeping, no backend, wake progress, no access, reconnect, unsupported mode |

## First-run and future partner UX

See [onboarding](ONBOARDING.md) for the exact novice, returning and invited paths. No technical terms, manual gateway key, separate Dewee dashboard wizard or unnecessary provider question. Secure browser handoffs return to same native screen; show canonical setup progress, one next action, recovery without repeated data. For future [shared contexts](FEDERATION.md), show verified external badge, owner, participating organizations, share scope and policy preview; invite/DM consent, resource ACL and revoke states. Never expose unimplemented capabilities.

## Native interactive capability surfaces

Render MCP Apps inline or in right panel/fullscreen when negotiated, native forms with safe accept/decline/cancel, and durable MCP task cards in inbox/activity. Preserve text fallback and distinguish live state vs historical snapshot. Plugin manager imports portable/vendor packages with component-level compatibility, permission changes, unsupported features and fresh connection flow. Map sidebar apps, settings, mentions and file viewers to existing navigation/composer/preview slots; no second UI system. See [MCP](MCP.md), [plugin compatibility](PLUGIN-COMPATIBILITY.md), [Clarkcant research](CLARKCANT-RESEARCH.md).

## Wireframe A: team room + execution panel

```text
+--------------------------------------------------------------------------+
| emree    workspace v     Search / command palette        Inbox   Workbench |
+-----------+--------------------------------------+-----------------------+
| Rooms     | # engineering                        | DevBox Workbench  [^]x|
| # product | Minh: Fix reconnect in the gateway.  | api-dev v | Ready     |
| # eng     | Engineering Agent: Task assigned.    | session-12 v   + tab  |
| DMs       | [Task: fix reconnect] [Open session]  | [Chat][TUI][Changes]  |
| Agents    |                                      | Claude / Codex / DSH  |
| Tasks     | Thread: QA review                    | tool: tests running   |
| Knowledge | [Share diff] [Review requested]      |                       |
| Extensions|                                      | [Prompt / terminal]   |
|           | [Message or assign task...] [Send]   | [Detach] [Cancel]     |
+-----------+--------------------------------------+-----------------------+
```

For TUI use resizable terminal with keyboard focus and status line; mode availability from runtime capabilities. Explicitly distinguish detach, cancel current run, terminate session, and sleep box. Show input controller and observer status. Do not reveal other users' secret-bearing transcripts. Right sidebar can pin session even when main room changes; task linking deep link binds server/workspace/box/session and reauthorizes.

## Wireframe B: actionable inbox

```text
Inbox             [All] [Approvals] [Questions] [Tasks]
  Approval required   Deploy release to staging
  Requested by Engineering Agent · scope: staging · expires 10:30
  [View exact changes] [Approve] [Decline]
  Task blocked         Needs access to repository
  [Open workbench] [Request access]
  Completed            Review report ready
  [Inspect result] [Accept]
```

Approval button opens trusted detail with exact action, actor and scope. Stale approval rejected server-side. Avoid approvals by opaque generic text. OS notification previews private content off by default.

## Wireframe C: device and extension governance

```text
Device: My laptop       Connected | Active grant: none
  Agent: Research Agent  Resource: selected application
  [ ] Observe     [ ] Control   Duration: 15 min
  [Review native consent]   [Emergency stop]
Extensions   Profile: Engineering
  Workbench  Official   Active      [Permissions] [Disable]
  CRM panel  Declarative Available  [Review & install]
  Unknown provider: Incompatible    [Details]
```

Do not show grant successful until broker and Gateway acknowledge. Revocation confirmation shows actual state; local stop works even offline. Extension permission review shows changes from previous version, dependencies and publisher. Profiles do not increase rights.

## UI component and interaction tests

Story fixtures for 11 screens (loading, empty, populated, denied, offline, error, conflict). Virtualized timeline scroll anchor and unread jump; composer markdown/IME, attachments, keyboard shortcuts; responsive panels at desktop minimum and narrow widths. Accessibility tests, real WebView snapshots, no misleading mock data outside dev mode. Benchmark cold startup and long busy-room soak; memory bounds and process cleanup. User-test 3–5 design partners; prioritize completed tasks and discoverability, not admin feature count.
