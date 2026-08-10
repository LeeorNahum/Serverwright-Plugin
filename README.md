# Serverwright plugin

One manifest that points an agent at [Serverwright](https://serverwright.io), remote hands for the servers you own.

Serverwright connects agents to your servers through a hosted MCP server: files, search, console, power, and shell, with every call crossing one authorization and audit gate. This repository is the plugin that installs that server. It is an [Agent Plugins 1.0.0](https://agent-plugins.org) plugin, and the same directory also carries Claude Code's own plugin shape, so one repository serves both.

## What is in here

| File | Read by |
| --- | --- |
| `plugin.json` | Agent Plugins 1.0.0 clients (VS Code, Cursor, GitHub Copilot, ChatGPT, Codex, Kiro, Hermes) |
| `mcp.json` | The same clients, for the MCP server declaration |
| `skills/serverwright/SKILL.md` | Agent Plugins clients and Claude Code alike, the one location both formats read identically |
| `.claude-plugin/plugin.json` | Claude Code |
| `.claude-plugin/marketplace.json` | Claude Code, and Codex, which reads this catalog format too |
| `.mcp.json` | Claude Code, for the MCP server declaration |

Both manifests declare one Streamable HTTP server at `https://mcp.serverwright.io/mcp`, and nothing else. There is no `headers` key and no credential of any kind. Agent Plugins 1.0.0 section 7.2.1 is explicit that header values are visible package data, so the correct expression of "authorization is the client's job" is to omit the field entirely.

The bundled skill is workflow guidance for the agent: pick a target from `servers`, plan with that server's capabilities, confirm destructive work, recover from typed errors. Tool contracts live in the tools themselves.

## Signing in

The plugin holds no credential and needs none. Your agent connects to the server, gets an OAuth challenge, and shows you its own sign-in prompt. You authorize Serverwright once in a browser and pick a workspace; the connection is scoped to the workspace you chose, and later sessions just work.

One consequence is worth knowing before you install: Agent Plugins 1.0.0 makes an authorization failure a connection failure rather than a configuration error, so **a plugin installed before you sign in looks like nothing happening**. That is expected. Install it, then complete the sign-in your agent offers.

The tools that arrive are not read-only. Alongside listing servers and reading files, they write, edit, move, delete, upload, run console and shell commands, and send power signals, exactly the access you hold in the Serverwright dashboard, gated per server by what that server's connection supports. Every call is recorded to the workspace audit trail.

## Where it works today

Two server generations matter here, and this README says which is live. **The production server at `mcp.serverwright.io` currently runs the legacy generation**, which speaks the MCP protocol revisions today's coding agents ship (through `2025-11-25`, with the `initialize` handshake). A rewrite that serves protocol revision `2026-07-28` and only that revision is built and pending release; when it deploys, the matrix below flips from the "Live server" column to the "After the pending release" column, with no change to this plugin.

| Client | Revision it sends | Live server (legacy, today) | After the pending release (`2026-07-28` only) |
| --- | --- | --- | --- |
| Claude Code 2.1.226 | `2025-11-25` | **Works** | Signs in, then refused with `-32022` until it ships `2026-07-28` |
| Cursor 3.15.6 | `2025-11-25` | **Works** | Signs in, then refused with `-32022` |
| Codex CLI 0.147.0 | `2025-06-18` | **Works** | Signs in, then refused with `-32022` |
| VS Code 1.132.0 | `2025-11-25` | **Works** (plugin MCP servers start when the Chat view opens) | Cannot connect; `2026-07-28` is absent from its build |
| ChatGPT (web, desktop, mobile) | `2026-07-28` | Not yet: the live server predates both the revision ChatGPT speaks and the sign-in surface it requires | **Works fully** |

Revisions were measured on 2026-08-09 by logging what each client actually puts on the wire. A "refused" entry means the client completes OAuth and is then answered with the unsupported-protocol-version error naming `2026-07-28` as the one revision served. That is the client's own MCP implementation lagging the protocol, not a misconfiguration here; each of those clients connects again, without any change to this plugin, once it ships the current revision.

If you use Claude in the browser or the desktop app, there is no plugin install path there at all. Add Serverwright as a connector instead.

## Install

**ChatGPT and Codex.** Search the plugin directory for Serverwright once it is listed. Until then, Codex can install it straight from this repository:

```bash
codex plugin marketplace add LeeorNahum/Serverwright-Plugin
codex plugin add serverwright@serverwright
```

**Claude Code.** This repository is a marketplace as well as a plugin:

```
/plugin marketplace add LeeorNahum/Serverwright-Plugin
/plugin install serverwright@serverwright
```

**Cursor.** Clone into Cursor's local plugin directory:

```bash
git clone https://github.com/LeeorNahum/Serverwright-Plugin ~/.cursor/plugins/local/serverwright
```

**VS Code and GitHub Copilot.** Clone anywhere, then point VS Code at the directory in your user settings:

```json
"chat.pluginLocations": {
  "/absolute/path/to/Serverwright-Plugin": true
}
```

`Chat: Install Plugin From Source` in the Command Palette installs from a git URL instead. VS Code starts a plugin's MCP servers when you open the Chat view, not at launch.

## Versioning

The endpoint in this repository is production. Staging is exercised with each client's local plugin directory rather than with a second published variant, which is what those directories are for.

The URL is deliberately restated in two files, one per manifest format. A test in the Serverwright repository fetches both of them and asserts they equal the endpoint the deployment itself advertises, so the two cannot silently diverge.

## License

MIT. See [LICENSE](LICENSE).
