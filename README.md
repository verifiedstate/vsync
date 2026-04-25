# Claude Code and Cursor, synced.

**Stop your AI from forgetting between sessions and tools.** Shared, auto-updating memory across Claude Code, Cursor, and any MCP client.

```bash
npx @verifiedstate/vsync init
```

*One command. 30 seconds. Free forever.*

[![npm version](https://img.shields.io/npm/v/@verifiedstate/vsync.svg)](https://www.npmjs.com/package/@verifiedstate/vsync)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/verifiedstate/vsync?style=social)](https://github.com/verifiedstate/vsync)

---

[demo.gif — 20 seconds, split-screen: Claude Code → Cursor sync]

---

## The problem

You scaffold a backend with Claude Code. You switch to Cursor for the frontend.

Cursor has no idea what Claude just decided. You copy-paste context, manually update `.cursorrules`, and watch your AI rebuild things you already built.

Every developer using AI coding tools hits this daily.

## What `vsync init` does

In one command:

- Detects your repo and which AI tools you use (Claude Code, Cursor, Windsurf)
- Opens your browser for a one-click sign-in (no API keys to copy)
- Writes `CLAUDE.md`, `.cursor/rules/verifiedstate.mdc`, `AGENTS.md`, `.mcp.json`
- Starts a local daemon that captures every AI coding turn
- Generates a live hot-context file that updates as you work

After it finishes, every AI tool in your repo shares the same project memory. A decision in Claude Code shows up in Cursor's context within seconds. Open a new session — your AI already knows what you were doing.

## What it creates

vsync writes four files. You can see exactly what they look like in [`/examples`](./examples) before you run anything:

- **`CLAUDE.md`** — current project state for Claude Code
- **`.cursor/rules/verifiedstate.mdc`** — persistent Cursor context
- **`AGENTS.md`** — portable agent instructions (cross-tool standard)
- **`.mcp.json`** — VerifiedState MCP server config

Don't want it anymore? `npx @verifiedstate/vsync eject` removes everything cleanly.

## How it works

A local daemon captures every Claude Code and Cursor turn as signed assertions in your VerifiedState memory. A hot-context worker regenerates the markdown files above on every state change. AI tools query the same backend via MCP, so a decision made in one tool is immediately available in another.

The memory substrate is cryptographically signed — every assertion is Ed25519-signed and hash-chained — so you have a verifiable record of what your AI did. That matters when the AI hallucinates or when you need to debug why it made a decision.

## Compared to alternatives

|                              | Karpathy pack | Mem0 / Letta | Anthropic Memory | **vsync** |
|------------------------------|:-------------:|:------------:|:----------------:|:---------:|
| One-command install          |       ✓       |              |                  |    ✓      |
| Auto-updates with project    |               |              |                  |    ✓      |
| Cross-tool sync              |               |              |                  |    ✓      |
| Cryptographic provenance     |               |              |                  |    ✓      |
| Free forever                 |       ✓       |   limited    |    Claude only   |    ✓      |

The Karpathy pack made Claude behave better. vsync makes it remember — across tools.

Works alongside Anthropic's official MCP memory server. We're the cross-tool sync layer they don't provide.

## Pricing

**Free forever** for individual developers — unlimited assertions, cross-tool sync, hot-context generation.

**Pro (waitlist)** — team workspaces, audit log exports, signed receipt verification dashboard. [Join waitlist](https://verifiedstate.ai)

## Security

vsync runs locally and only sends signed assertions to your VerifiedState account. Generated files are clearly marked and easy to remove. See [`SECURITY.md`](./SECURITY.md) for details on what data leaves your machine and how it's protected.

## Links

- Website: [verifiedstate.ai](https://verifiedstate.ai)
- MCP server: [mcp.verifiedstate.ai](https://mcp.verifiedstate.ai)
- Dashboard: [verifiedstate.ai/dashboard](https://verifiedstate.ai/dashboard)
- npm: [@verifiedstate/vsync](https://www.npmjs.com/package/@verifiedstate/vsync)
- Documentation: [verifiedstate.ai/docs](https://verifiedstate.ai/docs)

## License

MIT for this repo (CLI wrapper, templates, examples).

The hosted VerifiedState memory substrate is operated by VerifiedState. Free tier is fully featured for individual developers.

---

[![Star History Chart](https://api.star-history.com/svg?repos=verifiedstate/vsync&type=Date)](https://star-history.com/#verifiedstate/vsync&Date)
