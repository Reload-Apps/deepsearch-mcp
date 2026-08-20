# DeepSearch MCP Server

Research a person's public online footprint from a name, phone number, email address, or
username — ranked candidate people with confidence scores, then a dossier with a linked
source for every detail. Public sources only.

DeepSearch is a **hosted remote MCP server**. There is nothing to install or run: point
your client at `https://deepsearch.app/api/mcp` and authorize.

- **Endpoint:** `https://deepsearch.app/api/mcp`
- **Transport:** Streamable HTTP (stateless)
- **Protocol:** MCP `2025-06-18`
- **Auth:** OAuth 2.1 (dynamic client registration) or an API key as `Authorization: Bearer dsk_…`
- **Docs:** https://deepsearch.app/api/docs#mcp

## Tools

| Tool | What it does |
| --- | --- |
| `search_people` | Resolves one identifier — name, phone, email, or username — to a ranked list of real, distinct people, each with a confidence score. Separates same-name individuals into candidates you can choose between, rather than returning pages to reconcile yourself. |
| `build_dossier` | Assembles one person's entire public footprint into a single sourced profile: identity, contact details, social accounts unified across platforms, work history, education, relatives, locations, and web mentions — every claim linked to the page it came from. |
| `ask_about_person` | Answers one specific question about a person, grounded in their public footprint, and suggests follow-ups. Cheaper and more direct than a full dossier when you want a single fact. |

All three tools are read-only (`readOnlyHint: true`, `destructiveHint: false`) and touch the
open web (`openWorldHint: true`). None of them mutate anything.

Scopes map one-to-one onto the tools: `search`, `dossier`, `chat`.

## Install

### Cursor / Grok Bot plugin

This repository is a single-plugin Cursor package. After marketplace review it
lists as **Deep Search** (not the kebab-case id) with the official logo.

1. In Cursor, open **Customize** and install **Deep Search** from the
   [Marketplace](https://cursor.com/marketplace). In Grok Bot, open **Plugins**
   and add **Deep Search**.
2. Authorize with OAuth when prompted. No API key or plugin variable is
   required — the hosted server uses OAuth 2.1.

Until the listing is live, load the plugin locally and reload Cursor
(**Developer: Reload Window**):

```bash
git clone https://github.com/Reload-Apps/deepsearch-mcp.git ~/.cursor/plugins/local/deep-search
```

Or copy / symlink an existing checkout into `~/.cursor/plugins/local/deep-search`.
Publishers submit this repo at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

To add the hosted MCP server without the plugin, use the client configs below.

### Claude Code

```bash
claude mcp add --transport http deepsearch https://deepsearch.app/api/mcp
```

### Claude Desktop / any client reading `claude_desktop_config.json`

```json
{
  "mcpServers": {
    "deepsearch": {
      "type": "http",
      "url": "https://deepsearch.app/api/mcp"
    }
  }
}
```

### Cursor — `.cursor/mcp.json`

```json
{
  "mcpServers": {
    "deepsearch": {
      "url": "https://deepsearch.app/api/mcp"
    }
  }
}
```

### VS Code — `.vscode/mcp.json`

```json
{
  "servers": {
    "deepsearch": {
      "type": "http",
      "url": "https://deepsearch.app/api/mcp"
    }
  }
}
```

## Authentication

The server advertises the standard OAuth flow, so most clients need no configuration —
they discover it from the `WWW-Authenticate` challenge and
[`/.well-known/oauth-protected-resource`](https://deepsearch.app/.well-known/oauth-protected-resource),
then run dynamic client registration.

To use an API key instead, create one at https://deepsearch.app/api and send it as
`Authorization: Bearer dsk_…`.

A `401` means the token is missing, expired, or invalid — refresh or re-authorize, then
retry. A `403` means the token is missing a scope the tool requires.

DeepSearch cannot be billed to an agent directly. If a tool result says the user must sign
in, subscribe, or add credits, relay that message and its URL to the user, then retry.

## Discovery documents

| Path | Contents |
| --- | --- |
| [`/.well-known/mcp.json`](https://deepsearch.app/.well-known/mcp.json) | Server manifest with the full tool list |
| [`/.well-known/mcp/server-card.json`](https://deepsearch.app/.well-known/mcp/server-card.json) | Server card |
| [`/.well-known/oauth-protected-resource`](https://deepsearch.app/.well-known/oauth-protected-resource) | Resource metadata, scopes, transport |
| [`/.well-known/oauth-authorization-server`](https://deepsearch.app/.well-known/oauth-authorization-server) | Authorization server metadata |

[`server.json`](./server.json) in this repo is the
[official MCP Registry](https://registry.modelcontextprotocol.io) manifest for
`app.deepsearch/deepsearch`.

## SDKs

Prefer plain HTTP? The Developer API has dependency-free clients:

```bash
npm i @reloadapp/deepsearch-client   # TypeScript client
npm i -g @reloadapp/deepsearch-cli   # CLI
```

## Acceptable use

DeepSearch searches **public sources only**. It is built for recruiting checks, sales and
partner diligence, journalism, and reconnecting with people.

It must **not** be used for surveillance, stalking, or harassment, and must **not** be used
to make employment, credit, housing, or insurance decisions. DeepSearch is not a consumer
reporting agency and its output is not a consumer report.

People can remove themselves at https://deepsearch.app/remove-my-info.

## License

MIT — see [LICENSE](./LICENSE). The hosted service itself is proprietary; this repository
covers the Cursor plugin, MCP Registry manifest, documentation, and client configuration.
