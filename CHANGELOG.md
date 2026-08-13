# Changelog

## 1.0.2 (2026-08-13)

- Apache-2.0: the verbatim `LICENSE` text, a `license` field in `plugin.json`, `.claude-plugin/plugin.json`, and `.claude-plugin/marketplace.json`, and one README line. No behavior change; the endpoint and the tools are the same.

## 1.0.1 (2026-08-10)

- The README states the live server generation plainly: `2026-07-28` only, with the measured client matrix. The two-generation framing existed for the cutover window, which has passed.
- License file and manifest fields removed.

## 1.0.0 (2026-08-10)

Initial release.

- One `streamable-http` MCP server entry for `https://mcp.serverwright.io/mcp`, declared once per manifest format and carrying no credential.
- The portable Agent Plugins 1.0.0 pair (`plugin.json`, `mcp.json`) and the Claude Code pair (`.claude-plugin/`, `.mcp.json`) in one repository.
- The `serverwright` workflow skill: target discovery, capability-aware planning, the approval boundary, and recovery.
