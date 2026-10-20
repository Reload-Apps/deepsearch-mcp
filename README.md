# Deep Search MCP Server

Research public information with your Deep Search account. Compare candidate matches,
build sourced reports, research companies and vehicles, and inspect public image
appearances. Source availability varies; keep uncertainty and distinct identities clear.

Deep Search is a **hosted remote MCP server**. There is nothing to install or run: point
your client at `https://deepsearch.app/api/mcp` and authorize.

- **Endpoint:** `https://deepsearch.app/api/mcp`
- **Transport:** Streamable HTTP (stateless)
- **Protocol:** MCP `2025-06-18`
- **Auth:** OAuth 2.1 (dynamic client registration) or an API key as `Authorization: Bearer dsk_…`
- **Docs:** https://deepsearch.app/api/docs#mcp

## Tools

The hosted developer endpoint exposes the following tools (verified 20 October 2026).
Clients should discover the current schemas through `tools/list`.

| Tool | What it does |
| --- | --- |
| `search_people` | Finds distinct candidate matches for a user-supplied identifier. Compare candidates before selecting one. |
| `build_dossier` | Builds a sourced public-information report for a selected subject. |
| `ask_about_person` | Investigates a specific question using public evidence. |
| `search_companies` | Finds company candidates; keep legal entities and jurisdictions separate. |
| `build_company_dossier` | Builds a sourced company report. |
| `reverse_image_search` | Finds available public appearances of a supplied image. |
| `lookup_vin` | Researches vehicle specifications and available records from a VIN. Missing records do not prove a clean history. |
| `create_lookup_job` | Starts a background research operation. |
| `get_lookup_job` | Reads a background operation's status or available result. |
| `get_lookup_job_events` | Reads progress events for a background research operation. |
| `cancel_lookup_job` | Requests cancellation of a background operation. |
| `list_lookup_jobs` | Lists the connected account's accessible research jobs. |

Research can consume the connected account's included usage and create reports or job
state. Do not describe every research tool as having no side effects. Follow each live
schema's required inputs and authorization scopes. Preserve an operation's original
idempotency key when retrying, where that schema supports it.

This public developer package uses the full `/api/mcp` endpoint. The separate Muse,
ChatGPT and Claude directory connector profiles use their own restricted endpoints;
this repository is not their company-and-VIN submission package.

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

Or copy an existing checkout into `~/.cursor/plugins/local/deep-search`. Cursor skips
symlinks whose target is outside its local plugin folder.
Publishers submit this repo at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

To add the hosted MCP server without the plugin, use the client configs below.

### Claude Code / Cowork plugin

This repository is also a Claude Code plugin (`.claude-plugin/plugin.json`).
After directory review it lists as **Deep Search** in the
[Claude plugin directory](https://claude.com/plugins) for Cowork and Claude Code.

1. In Claude Code, open `/plugin` and install **Deep Search**. In Cowork, open
   **Plugins** and add **Deep Search**.
2. Authorize with OAuth when prompted (`/mcp` in Claude Code). No API key is
   required — the hosted server uses OAuth 2.1 with dynamic client registration.

Until the listing is live, load this checkout as a plugin directory:

```bash
claude --plugin-dir /path/to/deepsearch-mcp
```

Publishers submit this repo at
[platform.claude.com/plugins/submit](https://platform.claude.com/plugins/submit).
Validate locally with `claude plugin validate .` (see [SETUP.md](./SETUP.md)).

To add the hosted MCP server without the plugin:

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

Deep Search cannot be billed to an agent directly. If a tool result says the user must sign
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

Deep Search searches **public sources only**. It is built for public-source
research, sales and partner diligence, journalism, and reconnecting with people.

It must **not** be used for surveillance, stalking, or harassment, and must **not** be used
to make employment, credit, housing, or insurance decisions. Deep Search is not a consumer
reporting agency and its output is not a consumer report.

People can remove themselves at https://deepsearch.app/remove-my-info.

## License

MIT — see [LICENSE](./LICENSE). The hosted service itself is proprietary; this repository
covers the Cursor plugin, Claude Code plugin, MCP Registry manifest, documentation, and client configuration.
