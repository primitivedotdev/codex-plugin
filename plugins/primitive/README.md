# Primitive — Codex plugin

Email infrastructure for AI agents, packaged as a [Codex](https://developers.openai.com/codex)
plugin. Bundles the hosted Primitive MCP server plus two skills so Codex can
send, receive, search, and reply to email from inside a coding session.

- **MCP server** — `https://www.primitive.dev/mcp` (streamable HTTP, OAuth or
  Primitive API key bearer token)
- **Skills** — `primitive-chat` (send and await a reply), `primitive-inbox`
  (receive and read mail)

## What's in the package

```
primitive/                       # plugin root
├── .codex-plugin/plugin.json    # plugin manifest (name, version, interface, component pointers)
├── .mcp.json                    # bundled MCP server definition (mcpServers → type:http)
├── skills/
│   ├── primitive-chat/SKILL.md
│   └── primitive-inbox/SKILL.md
└── assets/icon.png
```

The plugin is published from a marketplace root whose manifest lives at
`.agents/plugins/marketplace.json` and points at this `primitive/` plugin
directory.

## Install (plugin)

Add the marketplace, then install the plugin:

```bash
codex plugin marketplace add primitivedotdev/codex-plugin
codex plugin add primitive@primitive
```

On first use, authenticate the bundled MCP server over OAuth:

```bash
codex mcp login primitive
```

Prefer a static API key instead of OAuth? The bundled server accepts a
[Primitive API key](https://www.primitive.dev) as a bearer token — set
`bearer_token_env_var` on the `primitive` server in your config and export the
key as `PRIMITIVE_API_KEY`.

## Install (MCP server only, no plugin)

To skip the plugin and just wire up the tools:

```bash
codex mcp add primitive --url https://www.primitive.dev/mcp
codex mcp login primitive          # or set a bearer token
```

## Verify

```bash
codex plugin list                  # primitive@primitive → installed, enabled
```

Then in a Codex session: "List my 10 most recent inbound Primitive emails"
should call `listEmails` and return without error.

## Notes

- Sending tools (`sendEmail`, `replyToEmail`) have external, irreversible side
  effects — the skills instruct Codex to confirm before sending.
- The same hosted MCP server backs Primitive's ChatGPT app and Claude connector,
  so behavior is identical across surfaces.
