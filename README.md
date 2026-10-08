# Primitive: Codex plugin marketplace

A [Codex](https://developers.openai.com/codex) plugin marketplace for
[Primitive](https://www.primitive.dev), email infrastructure for AI agents.

## Install

```bash
codex plugin marketplace add primitivedotdev/codex-plugin
codex plugin add primitive@primitive
codex mcp login primitive
```

The plugin gives Codex:

- **Skills**: every skill in
  [`primitivedotdev/skills`](https://github.com/primitivedotdev/skills):
  `primitive-chat`, `primitive-inbox`, `primitive-send`, `primitive-connect`,
  `primitive-network`, `primitive-functions`, `primitive-webhooks` and
  `primitive-domains`. They drive the `primitive` CLI (`npm i -g primitive`).
- **MCP servers**: `primitive` at `https://www.primitive.dev/mcp` (mail, signed
  in over OAuth by `codex mcp login primitive`) and `primitive-docs` at
  `https://www.primitive.dev/mcp/docs` (public docs search, no sign-in).

## Where the plugin lives

This repository is only the marketplace. Its one entry points at
`primitivedotdev/skills`, which is the plugin: `plugin.json` at its root is the
manifest, `skills/` holds the skills and `mcp.json` declares the MCP servers.
A skill added or changed there reaches Codex without a change here.

The entry also carries the plugin's display name, descriptions and brand colour,
so the Codex app has something to show before the plugin is installed. Codex
ignores icon paths on an entry whose plugin lives in another repository, so the
icon appears only after install. To see the icon before install, add the skills
repository as the marketplace instead:
`codex plugin marketplace add primitivedotdev/skills`.

```
.agents/plugins/marketplace.json   # marketplace manifest, read by `codex plugin marketplace add`
```

To pick up new skills after installing:

```bash
codex plugin marketplace upgrade primitive
codex plugin add primitive@primitive
```

## Prefer an API key to OAuth?

The `primitive` server accepts a [Primitive API key](https://www.primitive.dev)
as a bearer token. Set `bearer_token_env_var` on the `primitive` server in your
Codex config and export the key as `PRIMITIVE_API_KEY`.

## MCP server only, no plugin

```bash
codex mcp add primitive --url https://www.primitive.dev/mcp
```

Codex opens the sign-in as part of the add.

## Verify

```bash
codex plugin list                  # primitive@primitive  installed, enabled
codex mcp list                     # primitive and primitive-docs
```

Then in a Codex session, "List my 10 most recent inbound Primitive emails"
should call `listEmails` and return without error.

Validated against `codex-cli` 0.158.0.
