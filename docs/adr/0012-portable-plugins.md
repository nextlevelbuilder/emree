# ADR 0012: Portable Agent Plugins and vendor compatibility

Status: proposed baseline; 2026-11-27. D20. [Compatibility spec](../PLUGIN-COMPATIBILITY.md).

Use Agent Plugins 1.0.0 as canonical distributable format for skills and MCP servers, preserve Emree SDK as namespaced internal UI contribution layer. Import Claude and ChatGPT/Codex legacy formats via versioned adapters with loss/permission report, not blanket execution parity. Standard MCP Apps first, OpenAI extension adapter for sidebar, panels, forms, mentions, file views and related surfaces, capability-negotiated. Preserve provenance, exact version/digest, trusted kernel, explicit scoped installation and fresh auth. Vendor app IDs and credentials nonportable; executable hooks/LSP/native providers disabled until separately reviewed. One Dewee registry, no duplicated marketplace. Tests prove interoperability and safe refusal before release.
