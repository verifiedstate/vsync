# Security

vsync is designed for local-first operation with explicit, minimal data flowing to VerifiedState's servers. This document explains exactly what happens to your data.

## What stays local

- The `vsync` daemon runs entirely on your machine
- All file watching, JSONL parsing, and turn extraction happens locally
- Generated context files (`CLAUDE.md`, `.cursor/rules/verifiedstate.mdc`, `AGENTS.md`) are written to your repo, never uploaded
- Your code, file contents, and git history are never read or transmitted

## What is sent to VerifiedState servers

When the daemon captures a turn from Claude Code or Cursor, it sends to `api.verifiedstate.ai`:

- The user's prompt text (what you typed to the AI)
- The AI's response text (what the AI said back)
- A timestamp
- The repo name (basename only, not the full path)
- An anonymous session identifier

That data is stored as a cryptographically signed assertion in your VerifiedState namespace, accessible only with your API key.

## What is NOT sent

- File contents from your repo
- Git history or diffs
- Environment variables or `.env` files
- Anything outside the AI tool conversation logs
- File paths beyond the repo root basename

## Authentication

vsync stores your API key in `~/.vsync/config.json` with file permissions set to `600` (read/write for your user only). The key is never logged, never sent to third parties, and never written to your repo.

## MCP server connection

The optional MCP integration with Claude.ai and Claude Desktop uses a bearer token (your API key) sent over HTTPS to `mcp.verifiedstate.ai`. The connection is end-to-end encrypted via TLS. No conversation content is sent through the MCP — the MCP only fetches data you've already stored.

## Removing vsync

Run `npx @verifiedstate/vsync eject` to:

- Stop the local daemon
- Remove generated files from your repo
- Remove the VerifiedState entry from `.mcp.json` (other MCP servers preserved)
- Remove the Cursor MCP entry (other entries preserved)
- Optionally delete `~/.vsync/config.json`
- Optionally request account deletion

Your stored memory remains in your VerifiedState account until you explicitly delete it via the dashboard or via account deletion.

## Reporting vulnerabilities

Security issues should be reported to security@verifiedstate.ai. Please do not file public GitHub issues for security vulnerabilities. We aim to acknowledge reports within 48 hours and patch critical issues within 7 days.

## MCP-specific notes

VerifiedState's MCP server validates all input parameters against schemas before execution. Tool calls are scoped to the authenticated user's namespace — there is no cross-tenant data access path. We monitor the MCP security ecosystem actively and patch vulnerabilities as they're disclosed.

## Open source

The vsync CLI, templates, and installer logic are MIT-licensed and visible in this repository. You can audit exactly what runs on your machine. The hosted VerifiedState substrate (storage backend, signing infrastructure, retrieval workers) is operated by VerifiedState.
