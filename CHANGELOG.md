# Changelog

## 1.0.1 (2026-08-10)

- The README states the live server generation plainly: `2026-07-28` only, with the measured client matrix. The two-generation framing existed for the cutover window, which has passed.
- License file and manifest fields removed.

## 1.0.0 (2026-08-10)

Initial release.

- One `streamable-http` MCP server entry for `https://mcp.serverwright.io/mcp`, declared once per manifest format and carrying no credential.
- The portable Agent Plugins 1.0.0 pair (`plugin.json`, `mcp.json`) and the Claude Code pair (`.claude-plugin/`, `.mcp.json`) in one repository.
- The `serverwright` workflow skill: target discovery, capability-aware planning, the approval boundary, and recovery.
