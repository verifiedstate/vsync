# Security — vsync

## What vsync does

vsync is a **read-only observer**. It watches AI session files on your machine (Cursor, Claude Code, Windsurf) and sends session turn summaries to the VerifiedState API. That's it.

## What data vsync collects

- **Session turn text**: User prompts and AI responses, truncated to 10-20KB per turn
- **Tool actions**: Names of tools used (Read, Edit, Bash) and file paths (truncated to 120 chars)
- **Code diffs**: Edit/Write inputs — old_string/new_string pairs, truncated to 2-3KB each
- **Git metadata**: Commit hash, message, branch, files changed, author
- **Session metadata**: Session ID, project name, repo name, model, tool, timestamps

## What vsync does NOT collect

- **Environment variables** — never reads `process.env` beyond `NO_COLOR` and `HOME`
- **API keys / secrets** — scrubbed before any payload leaves your machine
- **.env files** — hard-rejected; never read, period
- **Private keys** — PEM/SSH patterns scrubbed before sending
- **File contents outside session files** — only reads declared paths
- **Anything from disallowed projects** — project allow-list controls scope

## Data flow

```
Your machine                          VerifiedState
+------------------+                 +----------------------+
| AI session files  |--read-only-->  |                      |
| (Cursor, Claude   |                | api.verifiedstate.ai |
|  Code, Windsurf)  |  --scrub-->    |     /ingest          |
|                   |  --send-->     |                      |
+------------------+                 +----------------------+
        |
        v
 ~/.vsync/audit.jsonl  (local log of everything sent)
```

No analytics. No telemetry. No third-party services.

## Network endpoints

| Endpoint | Purpose |
|---|---|
| `api.verifiedstate.ai/ingest` | Send session turn data |
| `api.verifiedstate.ai/whoami` | Verify API key on init |
| `mcp.verifiedstate.ai/mcp` | Working state updates |
| `context.verifiedstate.ai/context` | Hot context refresh |

All configurable via `~/.vsync/config.json`.

## File system access

### Reads (read-only)
- `~/.claude/projects/**/*.jsonl` — Claude Code sessions
- Cursor/Windsurf `state.vscdb` — SQLite database (read-only copy)
- `.git/refs/heads/` — commit polling
- `~/.vsync/config.json` — configuration

### Writes (only to `~/.vsync/`)
- `config.json`, `vsync.pid`, `vsync.log`, `vsync.status.json`
- `queue/*.json` (offline buffer), `resume.json`, `audit.jsonl`

### What vsync NEVER does
- Run shell commands (except `git log` for commit metadata)
- Write to user source files
- Modify git state
- Read `.env` files, credentials, or private keys
- Access files outside declared paths

## Verifying for yourself

```bash
vsync start --dry-run --foreground   # see payload without sending
vsync audit                          # view all API calls ever made
vsync audit verify                   # check hash chain integrity
npm audit signatures                 # verify npm package provenance
```

## Reporting vulnerabilities

Email [security@verifiedstate.ai](mailto:security@verifiedstate.ai). We respond within 48 hours.
