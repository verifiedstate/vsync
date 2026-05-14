<p align="center">
  <img src="https://verifiedstate.ai/favicon.svg" width="48" height="48" alt="VerifiedState">
</p>

<h1 align="center">vsync</h1>

<p align="center">
  <strong>The audit trail for AI coding agents.</strong><br>
  Know what your AI decided, why it decided it, and whether it's safe to continue.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@verifiedstate/sync"><img src="https://img.shields.io/npm/v/@verifiedstate/sync.svg?style=flat&color=22d3ee&label=npm" alt="npm version"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat" alt="MIT License"></a>
  <a href="https://github.com/verifiedstate/vsync"><img src="https://img.shields.io/github/stars/verifiedstate/vsync?style=social" alt="GitHub Stars"></a>
  <a href="https://verifiedstate.ai"><img src="https://img.shields.io/badge/website-verifiedstate.ai-4ade80?style=flat" alt="Website"></a>
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> &middot;
  <a href="#-what-it-captures">What It Captures</a> &middot;
  <a href="#-commands">Commands</a> &middot;
  <a href="#-trust--security">Trust & Security</a> &middot;
  <a href="https://verifiedstate.ai/pricing">Pricing</a> &middot;
  <a href="https://verifiedstate.ai/docs">Docs</a>
</p>

---

## The problem

You're shipping code written by AI agents. But you can't answer basic questions:

- *What did the AI decide and why?*
- *Did two agents just edit the same file with conflicting assumptions?*
- *Is it safe to deploy what the AI wrote?*

There's no `git blame` for AI decisions. Code review catches syntax — it doesn't catch *"the AI chose batched updates instead of a migration because it misunderstood the prompt."*

## The solution

vsync watches every AI coding session and produces a **cryptographically signed trace**: what was asked, what the AI decided, what changed, and why.

```
Agent works  -->  vsync captures  -->  Decisions extracted  -->  Signed trace created
```

One line: **we make AI coding agents auditable.**

---

## Quick Start

```bash
npx @verifiedstate/sync init
```

That's it. One command, 30 seconds. It will:

1. Open your browser for one-click sign-in
2. Detect your AI tools (Claude Code, Cursor, Windsurf)
3. Write config files (`CLAUDE.md`, `.cursor/rules/`, `AGENTS.md`)
4. Start the background daemon

```bash
vsync start     # start the daemon
vsync status    # check what's running
vsync stop      # stop the daemon
```

---

## Supported Tools

| Tool | How it captures | Status |
|---|---|---|
| **Claude Code** | Watches `~/.claude/projects/**/*.jsonl` | Stable |
| **Cursor** | Reads `state.vscdb` via sql.js | Stable |
| **Windsurf** | Reads `state.vscdb` via sql.js | Stable |
| **Any MCP client** | Via MCP server at `mcp.verifiedstate.ai` | Stable |
| **Git** | Polls for new commits, captures metadata | Stable |

---

## What It Captures

| Data | Example |
|---|---|
| Session turns | User prompts + AI responses |
| Decisions | "Use batched updates to avoid long locks" |
| Code changes | File diffs with +/- line counts |
| Tool actions | Read, Edit, Bash commands (truncated) |
| Git metadata | Commit hash, message, branch, author |
| Agent identity | Tool, model, session ID |

### What it does NOT capture

| Never collected |
|---|
| Full source code files |
| Environment variables or `.env` files |
| API keys, secrets, tokens, credentials |
| Private keys (PEM/SSH patterns scrubbed) |
| Files outside AI session directories |
| Network traffic or browsing history |

---

## Commands

| Command | Description |
|---|---|
| `vsync init` | Interactive setup: sign in, detect tools, write configs |
| `vsync start` | Launch background daemon |
| `vsync stop` | Stop daemon |
| `vsync status` | Show daemon health + watcher status |
| `vsync logs` | Tail recent daemon activity |
| `vsync recap` | Summarize recent memory activity |
| `vsync agents` | List agents that have written to memory |
| `vsync why` | Ask why a decision was made |
| `vsync last` | Show last N ingested items |
| `vsync trail` | Audit trail for an entity |
| `vsync diff` | What changed between two points in time |
| `vsync audit` | View local audit log (every API call logged) |
| `vsync audit verify` | Verify audit log hash chain integrity |
| `vsync allow <path>` | Add project to allow-list |
| `vsync deny <path>` | Remove project from allow-list |
| `vsync start --dry-run` | Run full pipeline, print payload, skip network |

---

## Trust & Security

vsync is designed so you don't have to trust us — you can **verify**.

### Observe-only

vsync is a read-only observer. It cannot:
- Run shell commands (except `git log` for commit metadata)
- Write to your source files
- Modify git state (no commits, no pushes)
- Access files outside declared paths
- Send data anywhere except your configured endpoint

### Verify for yourself

```bash
# 1. See the exact payload before anything is sent
vsync start --dry-run --foreground

# 2. View every API call vsync has ever made
vsync audit

# 3. Verify no audit entries were tampered with
vsync audit verify

# 4. Check npm package provenance
npm audit signatures
```

### Secrets filtering

Payloads are scrubbed before sending:

- `.env` files are **hard-rejected** — never read, period
- API keys (`sk-*`, `ghp_*`, `AKIA*`), bearer tokens, PEM headers are regex-filtered
- Redacted items logged locally: `[redacted: pattern_name]`
- Files matching `.gitignore` are skipped

### Project allow-list

vsync only watches projects you explicitly allow:

```bash
vsync allow ~/my-project          # allow this project
vsync allow                       # show current allow-list
vsync deny ~/my-project           # remove from allow-list
```

Full security details: **[SECURITY.md](./SECURITY.md)**

---

## What you see

The [VerifiedState dashboard](https://verifiedstate.ai/dashboard) shows:

- **AI Work Brief** — what your agents did today, in plain English
- **Review Queue** — items that need human review, ranked by severity
- **Trace Modal** — full 2-panel view: *why* (prompt, decisions, blockers) + *what changed* (code diffs with line numbers)
- **Hot Files** — files with the most AI edits, flagged for review
- **Collision Detection** — alerts when 2+ agents edit the same file
- **Signed Receipts** — cryptographic proof of what happened and when

---

## Pricing

| | Free | Pro | Team | Enterprise |
|---|---|---|---|---|
| **Price** | $0/mo | $29/mo | $59/seat/mo | Custom |
| Users | 1 | 1 | Unlimited | Unlimited |
| Agents | 1 | Unlimited | Unlimited | Unlimited |
| Trace history | 30 days | Unlimited | Unlimited | Custom |
| Signed receipts | | Yes | Yes | Yes |
| Shared team memory | | | Yes | Yes |
| Collision detection | | | Yes | Yes |
| SSO / compliance | | | | Yes |

[View full pricing &rarr;](https://verifiedstate.ai/pricing)

---

## How it works

```
Your machine                              VerifiedState
+-----------------+                      +------------------------+
| AI session files |--read-only-->       |                        |
| (Cursor, Claude  |                     | api.verifiedstate.ai   |
|  Code, Windsurf) |  --scrub-->         |     /ingest            |
|                  |  --send-->          |         |              |
+-----------------+                      |    extract (LLM)       |
        |                                |         |              |
        v                                |    verify (Ed25519)    |
 ~/.vsync/audit.jsonl                    |         |              |
 (local log of everything sent)          |    signed receipt      |
                                         +------------------------+
```

1. Daemon watches AI session files (read-only)
2. Payloads pass through secrets filter
3. Every API call logged to local audit file *before* sending
4. VerifiedState extracts decisions via LLM
5. Ed25519 signed receipt created for each assertion
6. Queryable via dashboard, CLI, or MCP tools

---

## For AI agents

To connect any MCP-compatible agent, add to your MCP config:

```json
{
  "mcpServers": {
    "verifiedstate": {
      "url": "https://mcp.verifiedstate.ai/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

23 tools available: `memory_ingest`, `memory_query`, `session_save`, `session_load`, and more.

---

## Contributing

Issues and feature requests: [github.com/verifiedstate/vsync/issues](https://github.com/verifiedstate/vsync/issues)

Security vulnerabilities: [security@verifiedstate.ai](mailto:security@verifiedstate.ai)

## License

MIT &mdash; [VerifiedState](https://verifiedstate.ai)
