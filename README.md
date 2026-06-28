# Primitive — Codex plugin marketplace

A [Codex](https://developers.openai.com/codex) plugin marketplace for
[Primitive](https://www.primitive.dev), email infrastructure for AI agents.

## Install

```bash
codex plugin marketplace add primitivedotdev/codex-plugin
codex plugin add primitive@primitive
codex mcp login primitive
```

See [`plugins/primitive/README.md`](plugins/primitive/README.md) for full usage,
the bearer-token alternative, and the MCP-server-only install.

## Layout

```
.agents/plugins/marketplace.json   # marketplace manifest (discovered by `codex plugin marketplace add`)
plugins/primitive/                 # the Primitive plugin
├── .codex-plugin/plugin.json      # plugin manifest
├── .mcp.json                      # bundled hosted MCP server
├── skills/                        # primitive-chat, primitive-inbox
└── assets/icon.png
```

Validated against `codex-cli` 0.142.3 (`codex plugin marketplace add` →
`codex plugin add` → `installed, enabled`).
