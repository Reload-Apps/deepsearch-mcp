---
name: setup
description: Connect Deep Search MCP. Use when installing the plugin, authorizing OAuth, signing in, adding credits, or when a Deep Search tool is unavailable or returns 401.
---

# Set up Deep Search

The plugin already declares the remote server in `.mcp.json`. Do not ask the user for an API key unless they choose that path themselves.

## Claude Code

1. Confirm the `deepsearch` MCP server is listed in `/mcp`.
2. If it needs authentication, tell the user to select `deepsearch` in `/mcp` and complete the browser OAuth flow (OAuth 2.1 + dynamic client registration).
3. After they finish, retry the tool. Do not invent a token or put `dsk_…` in config.

## Cowork

Tell the user to complete Deep Search's OAuth sign-in from the plugin connector prompt or the plugin's connector page.

## If a tool says sign in, subscribe, or add credits

Relay the tool's message **and its URL** verbatim. Deep Search cannot be billed to an agent. After the user finishes at that URL, retry.

## Optional API key

Users who prefer a key can create one at https://deepsearch.app/api and add the server themselves with `Authorization: Bearer dsk_…`. This plugin does not store or request that key.

More detail: [SETUP.md](../../SETUP.md).
