# Agents Working in This Repo

This project uses [VerifiedState](https://verifiedstate.ai) for shared memory across AI tools. Multiple AI agents (Claude Code, Cursor, Windsurf, etc.) can work in this repo and share project context.

## How to pick up where the last agent left off

1. Read `.claude/hot-context.md` (or `.cursor/hot-context.md` — same content)
2. Check the "Recent Activity" and "Open Questions" sections
3. Query VerifiedState memory if you need deeper history:
   - `memory_query` for past decisions
   - `working_state_get` for current state
   - `session_load` for previous sessions

## How to hand off to the next agent

Just work normally. vsync captures every turn automatically. The next agent (whether Claude Code, Cursor, Claude.ai web, or any MCP client) will see your work in their hot-context file or via memory queries.

## Behavioral guidelines

The current behavioral guidelines for this project are in `.claude/hot-context.md` and `.cursor/hot-context.md`. They auto-update when changed via the VerifiedState dashboard.

## MCP server

This repo is configured with the VerifiedState MCP server at `mcp.verifiedstate.ai`. See `.mcp.json` for the connection config.

Available tools include:

- `memory_query` — search past decisions and assertions
- `working_state_get` — current goal, open loops, next action
- `working_state_update` — modify current state
- `session_load` — load context from a previous session
- (12 additional tools for assertion management, retrieval, and verification)

## Uninstall

To remove VerifiedState from this repo: `npx @verifiedstate/vsync eject`
