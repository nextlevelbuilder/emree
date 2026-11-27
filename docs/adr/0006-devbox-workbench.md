# ADR 0006: DevBox Workbench and session execution

Status: proposed baseline, F04/W01 gates; D09,D10.

Native right sidebar official module; binary PTY/TUI and structured ACP/vendor adapter are different transports under one session authority. Never parse ANSI into authoritative chat or promise session switch. Dedicated worker holds Docker credentials and executes only in isolated gVisor guest; Gateway holds authorization and session ticket. Alternative directly expose SSH/Docker or use script exec: rejected due boundary/stream/lifecycle. Single trust domain per box, exclusive input, generation fencing, bounded streams and no replay of uncertain input. W02/W04 prove real PTY and one structured adapter; if unsupported, disable only that mode. No host fallback; close/detach not stop.
