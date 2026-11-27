# Emree

**An open-source, agent-native desktop workspace for humans and AI agents, powered by Dewee.**

Emree is designed to be the everyday collaboration surface for teams: chat, tasks, agent runs, approvals, knowledge, remote development environments and controlled Computer Use in one desktop application.

> Status: design / pre-alpha. This repository currently contains the product and engineering specifications, not a working desktop application. API names, performance targets and integrations in the documents are proposals until implemented and tested.

## What we are building

- Human-to-human, human-to-agent and agent-to-agent collaboration in shared rooms and private conversations.
- An actionable inbox for work, decisions and approvals, rather than another stream of unread messages.
- DevBox Workbench: a resizable right sidebar for remote terminal/TUI and structured coding-agent chat, with session-aware handoff and artifacts.
- Attended Computer Use and scoped local files through a separate device broker, with explicit consent and revocation.
- Plugin-first architecture: built-in modules and third-party declarative extensions share versioned contribution contracts.
- macOS, Windows and Linux desktop clients, with Dewee Cloud, dedicated and self-hosted connections.

Emree is a new project, not a fork. Dewee is the authoritative service for identity, authorization, orchestration and shared data. The client does not embed a second agent gateway.

## Start here

| Document | Purpose |
| --- | --- |
| [Decision brief](DECISIONS.md) | Product and architecture choices; decisions still gated on evidence |
| [Product requirements](docs/PRD.md) | Scope, user journeys, feature acceptance criteria |
| [Architecture](docs/ARCHITECTURE.md) | Boundaries, components, data ownership, repository layout |
| [API and data contracts](docs/CONTRACTS.md) | Proposed HTTP, events, schemas, auth and error handling |
| [Realtime and storage](docs/REALTIME.md) | Delivery, offline sync, queues, cache and media |
| [Infrastructure](docs/INFRASTRUCTURE.md) | Deployment profiles, sizing, SLOs, operations |
| [Extension platform](docs/EXTENSIONS.md) | SDK, lifecycle, permissions, distribution and profiles |
| [DevBox Workbench](docs/DEVBOX.md) | Remote TUI, structured agent chat, sessions and compute isolation |
| [Computer Use](docs/COMPUTER-USE.md) | Device broker, grants, platform providers |
| [Security](docs/SECURITY.md) | Threat model, controls and release gates |
| [UX specification](docs/UX.md) | Screen flows, layouts and interaction states |
| [Implementation plan](plans/IMPLEMENTATION.md) | Dependency graph, work packages, phase gates |
| [Dewee integration](docs/DEWEE-INTEGRATION.md) | Upstream capabilities and required changes |
| [Research references](docs/REFERENCES.md) | Public technical sources and provenance |

## License

Apache-2.0. See [LICENSE](LICENSE). This applies to original Emree material in this repository; it does not relicense Dewee or any upstream projects. Contributions must have compatible rights and retain applicable notices.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) and [AGENTS.md](AGENTS.md). Report vulnerabilities privately according to [SECURITY.md](SECURITY.md).
