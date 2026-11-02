# Deep Search for Claude

Find distinct companies, build a sourced company report, and research a vehicle
VIN from Claude. Deep Search uses public evidence and your existing Deep Search
account. Research may consume your included usage; this plugin cannot purchase
credits, subscribe, or top up an account.

This initial version is **0.1.0**. The package uses the same MIT license as Deep
Search's public SDKs. General web research and coding workflows are not
capabilities of this version. Directory submission, approval and publication
are tracked separately from availability of this source package.

## Connect and use

Add Deep Search from Claude's plugin directory when the listing is available.
For private testing, upload the prepared ZIP in Claude under Customize > Plugins
> Add > Upload plugin. Open its Connectors tab, add or connect Deep Search, and
sign in through the Deep Search OAuth page. Team or Enterprise administrators
may need to add the connector before members can connect their own accounts.

For local Claude Code testing, from this plugin folder:

```sh
claude plugin validate .
claude --plugin-dir .
```

Use `/mcp` to connect the `deepsearch` server. The skill is
`/deepsearch:research`. No API key belongs in the plugin files or conversation.

Examples:

- “Find companies named OpenAI and distinguish the legal entities.”
- “Research Microsoft Corporation, domain microsoft.com, with sources.”
- “Research VIN 1HGCM82633A004352 and explain what the evidence cannot establish.”

The connector is `https://deepsearch.app/api/mcp/claude`. It provides only
`search_companies`, `build_company_dossier` and `lookup_vehicle`. People, contact,
image, photo-location, general web research and saved-report tools are unavailable.
Keep the original request key when retrying; a new key can start new research.
Sources may be incomplete or conflicting. Vehicle reports do not identify owners,
track vehicles or prove that missing history is a clean record.

Do not use Deep Search for surveillance, stalking, harassment, or decisions
about employment, credit, housing, or insurance eligibility. Follow the
service's terms and applicable law when using public evidence.

## Privacy and support

Research inputs are sent to Deep Search and its research providers. The service
can retain account, usage, request-recovery and research records as described in
the [privacy policy](https://deepsearch.app/privacy). Do not include secrets in
queries. The package itself contains no credentials, local executable server,
hooks or background jobs.

[Setup documentation](https://deepsearch.app/api/docs/directory-connectors) ·
[Support](https://deepsearch.app/contact) ·
[Terms](https://deepsearch.app/terms)
