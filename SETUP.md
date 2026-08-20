# Deep Search setup

This plugin connects Claude to the hosted Deep Search MCP server. It ships no API keys, tokens, or headers. Authentication is OAuth 2.1 with dynamic client registration, discovered from the server.

- **Endpoint:** `https://deepsearch.app/api/mcp`
- **Transport:** Streamable HTTP (`type: http` in `.mcp.json`)
- **Account / credits:** https://deepsearch.app
- **API keys (optional):** https://deepsearch.app/api — send as `Authorization: Bearer dsk_…` only if you add the server yourself; the plugin does not embed a key
- **Privacy:** https://deepsearch.app/privacy
- **Terms:** https://deepsearch.app/terms
- **Docs:** https://deepsearch.app/api/docs

## Claude Code

1. Install **Deep Search** from the plugin directory, or load this repo with `claude --plugin-dir .`.
2. Run `/mcp`, select the `deepsearch` server, and authenticate. Claude Code opens Deep Search's OAuth sign-in in your browser.
3. After sign-in, ask Claude to look someone up (name, phone, email, or username).

If a tool result says you must sign in, subscribe, or add credits, open the URL it returns, then retry.

## Cowork

1. Install **Deep Search** from Plugins.
2. When Cowork prompts you to connect the MCP server, complete Deep Search's OAuth sign-in.
3. Manage the connector from the installed plugin's connector page.

## Without the plugin

```bash
claude mcp add --transport http deepsearch https://deepsearch.app/api/mcp
```

Then authorize with `/mcp` the same way.
