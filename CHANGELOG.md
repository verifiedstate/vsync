# Changelog

## v2.9.0 — Trust Infrastructure (2026-05-07)

### Added
- **Local audit log** — every API call logged to `~/.vsync/audit.jsonl` before sending
- **Audit log hash chain** — `vsync audit verify` checks tamper-evidence
- **Secrets filtering** — `.env` files hard-rejected, API keys/tokens/PEM headers scrubbed
- **Project allow-list** — `vsync allow` / `vsync deny` to control watched projects
- **`--dry-run` mode** — `vsync start --dry-run` prints payload, skips network
- **SECURITY.md** — comprehensive data flow documentation
- **CI safety audit** — grep-based check for dangerous APIs

## v2.8.0 — Windsurf Support (2026-04-28)

### Added
- Windsurf session watcher (state.vscdb via sql.js)
- `vsync recap` — summarize recent memory activity
- `vsync agents` — list connected agents

## v2.6.0 — Git Capture (2026-04-15)

### Added
- Git commit watcher — polls for new commits, captures metadata
- `vsync trail` — audit trail for any entity
- `vsync diff` — what changed between two time points
- Daemon heartbeat with sync-degraded marker

## v2.0.0 — Daemon Mode (2026-03-20)

### Added
- Background daemon with PID management
- Cursor state.vscdb watcher via sql.js WASM
- Offline buffer — queues locally, flushes on reconnect
- `vsync init` / `start` / `stop` / `status` / `logs`

## v1.0.0 — Initial Release (2026-02-15)

### Added
- Claude Code session watcher
- Basic turn extraction and ingestion
