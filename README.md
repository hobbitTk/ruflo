# claude-flow — Multi-Agent Orchestration for Claude Code

A framework for coordinating multiple Claude Code CLI instances working together on software engineering tasks via MCP (Model Context Protocol).

## What This Actually Does

- Spawns and coordinates multiple `claude` CLI processes as specialized agents (coder, tester, reviewer, architect, etc.)
- Provides 215+ MCP tools for task management, memory, swarm coordination, and agent lifecycle
- Routes tasks to agents using tabular Q-learning with eligibility traces
- Shares context between agents via file-based memory with optional vector search
- Supports swarm topologies: hierarchical (coordinator + workers), mesh (peer-to-peer)
- Weighted majority voting for multi-agent consensus decisions

## Quick Start

```bash
# Install and initialize
npx claude-flow@latest init --wizard

# Add as MCP server to Claude Code
claude mcp add claude-flow -- npx claude-flow@latest mcp start

# Run diagnostics
npx claude-flow@latest doctor
```

## Architecture

```
User → Claude Code CLI → MCP Server (stdio) → Tool Handlers → Agent Processes
                                                    ↓
                                              File-based Memory
                                              (.claude-flow/)
```

The CLI auto-detects whether it's running interactively or as an MCP server (piped stdin). In MCP mode, it implements JSON-RPC v2 with `initialize`, `tools/list`, `tools/call`, and `ping`.

### Key Packages

| Package | Purpose |
|---------|---------|
| `v3/@claude-flow/cli` | CLI + MCP server (27 tool modules) |
| `v3/@claude-flow/shared` | Types, event bus, plugin system |
| `v3/@claude-flow/guidance` | Constraint-based orchestration gates |
| `v3/@claude-flow/security` | Input validation, path traversal prevention, safe command execution |

### What's Real vs Aspirational

| Feature | Status |
|---------|--------|
| Multi-agent CLI orchestration | Working |
| MCP tool server (215+ tools) | Working |
| Q-learning task routing | Working |
| File-based agent memory | Working |
| CLI with 37+ commands | Working |
| Vector search (HNSW) | Requires optional `@ruvector/*` native deps |
| Neural learning (SONA, EWC++) | Requires optional `@ruvector/*` native deps |
| Plugin marketplace | Not implemented (IPFS registry referenced but no code) |

## CLI Commands

```bash
claude-flow init          # Project setup
claude-flow agent spawn   # Start an agent
claude-flow swarm init    # Initialize swarm coordination
claude-flow memory search # Search agent memory
claude-flow doctor        # System diagnostics
claude-flow status        # Check system health
```

Run `claude-flow --help` for the full command list.

## Development

```bash
# Clone and build
git clone https://github.com/hobbitTk/ruflo.git
cd ruflo/v3/@claude-flow/cli
npm install && npm run build

# Run tests
cd ../../..  # back to repo root
npm test
```

## About This Fork

This is a hardened fork of [ruvnet/ruflo](https://github.com/ruvnet/ruflo) (formerly claude-flow) with:

- Removed obfuscated `preinstall` script that modified `~/.npm` cache ([PR #1292](https://github.com/ruvnet/ruflo/pull/1292))
- Removed 1M+ lines of dead code (v2/, unused wrappers)
- Pinned dependency versions
- Fixed `sanitizePath` bypass vulnerability
- Removed `npx` from safe executor allowlist
- Honest documentation (this README)

## License

MIT
