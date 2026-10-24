---
name: deep-search
description: Sourced public research with a Deep Search account. Use for people (name, phone, email, or username), companies, public image appearances, VIN lookups, and background research jobs. Public web only; never for surveillance or harassment, and never for employment, credit, housing, or insurance decisions.
---

# Deep Search

Research public information with the hosted Deep Search MCP tools. Public web sources only. Discover the current schemas with `tools/list` and follow each tool's required inputs and scopes. Source availability varies; keep uncertainty and distinct identities clear.

## When to use

- Journalism, reconnecting with someone, sales and partner diligence, or other public-source research that is not for hiring, credit, housing, or insurance decisions
- A name, phone, email, or username, when the user wants candidates, a sourced profile, or one fact
- A company name or identifier, kept as a distinct legal entity and jurisdiction
- Public appearances of an image the user supplied
- Vehicle specifications and available records from a VIN
- A long lookup the user wants started, polled, listed, or cancelled in the background

Do **not** use this skill for surveillance, stalking, or harassment. Do **not** use it to make employment, credit, housing, or insurance decisions. Deep Search is not a consumer reporting agency and its output is not a consumer report.

## Tools

| Tool | What it does |
| --- | --- |
| `search_people` | Ranked, distinct people for one identifier |
| `build_dossier` | Sourced public footprint for one selected person |
| `ask_about_person` | One specific question about a person |
| `search_companies` | Company candidates; keep legal entities and jurisdictions separate |
| `build_company_dossier` | Sourced company report for one selected company |
| `reverse_image_search` | Public appearances of a supplied image |
| `lookup_vin` | Vehicle specifications and available records from a VIN |
| `create_lookup_job` | Starts a background research operation |
| `get_lookup_job` | Status or completed result of a background operation |
| `get_lookup_job_events` | Progress events for a background operation |
| `cancel_lookup_job` | Requests cancellation of a background operation |
| `list_lookup_jobs` | Lists the connected account's accessible research jobs |

Research can consume the connected account's included usage and can create reports or job state. Cached results consume none. A valid empty result can still consume usage on search tools.

## People

1. **Search first.** Call `search_people` with one identifier (`query`, and `type` of `name`, `phone`, `email`, or `username`). It returns ranked, distinct people with confidence scores. Username searches may include `platforms`.
2. **Pick a person.** If several candidates look plausible, show them and ask which one. Do not merge same-name results into one person.
3. **Then dossier or ask.**
   - `build_dossier` — identity, contact, social accounts, work, education, relatives, locations, and web mentions. Pass the name plus the headline or username from the chosen candidate so the right individual is profiled. Every claim is linked to a source page.
   - `ask_about_person` — one specific question, cheaper than a full dossier. Pass a prior dossier summary as `context` when you already have one.

An empty result is an absence of public evidence, not proof that a claim is false.

## Companies

1. Call `search_companies` with the company name or identifier.
2. Compare legal name, jurisdiction, domain, and registry seeds before choosing. Companies that share a name and sit in different jurisdictions are different entities. Do not combine them.
3. Pass the selected company to `build_company_dossier`, keeping `id`, `jurisdiction`, `domain`, `seedDomain`, `seedRegistryId`, and `seedRegistrySource` when search returned them.
4. State uncertainty, stale public information, and jurisdiction differences. `refresh` rebuilds on purpose and spends usage again.

## Images

Call `reverse_image_search` with the public `imageUrl` the user supplied. Report the available public appearances and keep each source page. This finds appearances of the image. It does not perform facial recognition, and a matching page does not establish who is depicted. Do not use it to identify a private person or to locate someone.

## Vehicles

Validate the VIN before calling `lookup_vin`. A VIN is 17 characters and excludes the letters I, O, and Q. Reject an invalid VIN before research starts. Report specifications, available public records, and sources separately. Missing records do not prove a clean history. The report does not replace a professional inspection and does not guarantee title, ownership, safety, or condition. Keep the same VIN on every follow-up. `refresh` forces a paid rebuild.

## Background jobs and idempotency

Use `create_lookup_job` for a lookup that may run long. The `operation` is one of `search`, `dossier`, `chat`, `entity_search`, `entity_dossier`, `reverse_image`, or `vin`, with that operation's `input`. Creating the job adds no surcharge. The underlying operation's scope and billing still apply.

- Generate one `idempotency_key` per logical operation. Reuse that same key and the same inputs when retrying, reconnecting, or checking status. Every live research schema that accepts `idempotency_key` says to reuse it for the same inputs.
- An intentional refresh is a new operation and needs a new key.
- `get_lookup_job` reads status or the completed result. It is free and returns only jobs the connected account owns.
- `get_lookup_job_events` reads progress after a numeric event id (`after`). Continue from the returned cursor.
- `cancel_lookup_job` requests cancellation of a queued or running job. Work already delivered can remain charged. Cancellation itself adds no charge.
- `list_lookup_jobs` pages through the connected account's accessible jobs. It is free.

## Auth and billing

The plugin connects to `https://deepsearch.app/api/mcp` (streamable HTTP). Cursor, Grok Bot, and Claude Code discover OAuth 2.1 and dynamic client registration from the server. No API key belongs in this plugin.

- If a tool result says the user must sign in, subscribe, or add credits, **relay that message and its URL**, then retry after they finish. Deep Search cannot be billed to an agent.
- A `401` means the token is missing, expired, or invalid. Ask the user to re-authorize (the Cursor or Grok Bot plugin connector, `/mcp` in Claude Code, or the Cowork connector page).
- A `403` means the token is missing a scope the tool requires.
- Optional API keys (`dsk_…`) are created at https://deepsearch.app/api. Do not invent a key or ask the user to paste one into chat unless they choose that path.

## Output

Prefer sourced claims. Keep source URLs that come back with a dossier, answer, company report, image appearance, or vehicle record. People can opt out at https://deepsearch.app/remove-my-info.
