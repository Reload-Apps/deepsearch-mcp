---
name: deep-search
description: Research a person's public online footprint from a name, phone, email, or username. Use when looking up people, building a sourced dossier, or asking a specific question about someone. Public web only; never for employment, credit, housing, or insurance decisions.
---

# Deep Search

Look up real people from a name, phone, email, or username using the hosted Deep Search MCP tools. Public web sources only.

## When to use

- Recruiting checks, sales or partner diligence, journalism, or reconnecting with someone
- The user gives a name, phone, email, or username and wants candidates, a sourced profile, or one fact

Do **not** use this skill for surveillance, stalking, or harassment. Do **not** use it to make employment, credit, housing, or insurance decisions. Deep Search is not a consumer reporting agency and its output is not a consumer report.

## Workflow

1. **Search first.** Call `search_people` with one identifier (name, phone, email, or username). It returns ranked, distinct people with confidence scores — not pages to reconcile yourself.
2. **Pick a person.** If several candidates look plausible, show them and ask the user which one. Do not merge same-name results into one person.
3. **Then dossier or ask.**
   - `build_dossier` — full public footprint: identity, contact, social accounts, work, education, relatives, locations, web mentions. Every claim is linked to a source page.
   - `ask_about_person` — one specific question, cheaper than a full dossier. Use this when the user wants a single fact.

## Auth and billing

The plugin connects to `https://deepsearch.app/api/mcp` (streamable HTTP). Claude Code discovers OAuth 2.1 and dynamic client registration automatically. No API key belongs in this plugin.

- If a tool result says the user must sign in, subscribe, or add credits, **relay that message and its URL**, then retry after they finish.
- A `401` means the token is missing, expired, or invalid — tell the user to re-authorize via `/mcp` (Claude Code) or the plugin connector page (Cowork).
- Optional API keys (`dsk_…`) are created at https://deepsearch.app/api. Do not invent or request that the user paste a key into chat unless they choose that path.

## Output

Prefer sourced claims. When a dossier or answer includes a source URL, keep it. People can opt out at https://deepsearch.app/remove-my-info.
