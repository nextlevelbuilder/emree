# ADR 0001: Greenfield native desktop

Status: proposed baseline; D01–D03. Date: 2026-11-27.

Context: open-source Emree should be independent of prior desktop code and preserve Dewee authority. Choose Tauri 2/Rust + React/TypeScript over a fork or embedded Gateway. Alternative Electron: consistent Chromium and mature ecosystem, but larger runtime; Wails: Go-friendly but no greenfield reuse requirement. Tauri system WebViews and capability scoping are attractive, not proof of security or low memory.

Decision: local packaged UI, least-privilege native IPC, separate broker, shared protocol DTOs, Apache-2.0 original code. Do not import restricted upstream code. F01 must verify actual supported OS, packaging, accessibility, performance and IPC isolation. If Tauri fails, switch shell implementation while preserving UI/contracts. No app scaffold before choosing minimal vertical slice.
