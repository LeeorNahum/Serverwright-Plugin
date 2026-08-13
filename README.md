# Serverwright plugin

[![GitHub Release](https://img.shields.io/github/v/release/LeeorNahum/Serverwright-Plugin?sort=semver)](https://github.com/LeeorNahum/Serverwright-Plugin/releases/latest)

Points an agent at [Serverwright](https://serverwright.io), remote hands for the servers you
own: one MCP server at `https://mcp.serverwright.io/mcp`, declared in both the
[Agent Plugins 1.0.0](https://agent-plugins.org) format (`plugin.json`, `mcp.json`) and
Claude Code's (`.claude-plugin/`, `.mcp.json`). `skills/serverwright/` holds the workflow
skill; tool contracts live in the tools themselves.

Sign-in is OAuth through your agent's own prompt, scoped to the workspace you pick. A plugin
installed before you sign in shows nothing until the sign-in completes, because the format
treats an unauthorized server as a failed connection.

The tools are not read-only. Alongside listing servers and reading files, they write, edit,
move, delete, upload, run console and shell commands, and send power signals: the access you
hold in the dashboard, gated per server by what its connection supports, with every call
recorded to the workspace audit trail.

## Where it works

The server speaks MCP revision `2026-07-28` only. Measured 2026-08-09:

| Client | Connects |
| --- | --- |
| ChatGPT | **Yes** |
| Claude Code 2.1.226 | Not yet, sends `2025-11-25` |
| Cursor 3.15.6 | Not yet, sends `2025-11-25` |
| VS Code 1.132.0 | Not yet |
| Codex CLI 0.147.0 | Not yet, sends `2025-06-18` |

A "not yet" client signs in and is then answered `-32022` naming `2026-07-28`. That is the
client's MCP implementation lagging the protocol; each connects, without any plugin change,
once it ships the current revision.

## Install

```bash
# Codex
codex plugin marketplace add LeeorNahum/Serverwright-Plugin
codex plugin add serverwright@serverwright

# Claude Code
/plugin marketplace add LeeorNahum/Serverwright-Plugin
/plugin install serverwright@serverwright

# Cursor
git clone https://github.com/LeeorNahum/Serverwright-Plugin ~/.cursor/plugins/local/serverwright
```

VS Code: clone anywhere, then add the directory to `chat.pluginLocations` in user settings.

A test in the Serverwright repository asserts both manifests equal the endpoint the
deployment advertises, so they cannot silently diverge.

Licensed under the [Apache License 2.0](LICENSE).
