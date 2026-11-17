# Tolteck MCP server

Connect Tolteck to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own Tolteck account with 13 hosted tools for quotes, invoices, payments, clients and your materials library. Emails to clients and credit notes ask for approval by default.

## What you can ask

> List my unpaid invoices.

> Create a deposit invoice from a signed quote and show me the totals.

> Find a client and list their quotes.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Tolteck tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 tolteck_agent.py serve
```

Or with Docker:

```bash
docker build -t tolteck-mcp .
docker run -i --rm -e COTLET_API_KEY tolteck-mcp
```

Tools: `tolteck_guide`, `tolteck_status`, `tolteck_open`, `tolteck_read`, `tolteck_click`, `tolteck_type`, `tolteck_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 tolteck_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 tolteck_agent.py status    # is Tolteck ready in your Cotlet.dev account
python3 tolteck_agent.py guide     # Tolteck screens and sign-in link
python3 tolteck_agent.py read      # open Tolteck, read the page, close the browser
python3 tolteck_agent.py tools     # list Cotlet.dev tools
python3 tolteck_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Tolteck users

**Which Tolteck account should I connect?**

Sign in at https://tolteck.app in your Cotlet Cloud Browser as the user who writes and sends quotes. Your agent sees what that user can see.

**Which actions ask for approval?**

send_document and create_credit_note ask for approval by default. The other four write tools save clients, quotes, deposit invoices or payment statuses; seven read tools run immediately.

**Is Cotlet affiliated with Tolteck?**

No. Cotlet.dev is independent and not affiliated with Tolteck or Saint-Gobain.

## Learn more

- [Tolteck integration and hosted tools](https://cotlet.dev/integrations/tolteck-api-mcp-server)

---

More: https://cotlet.dev/integrations/tolteck-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=tolteck

Request access: https://cotlet.dev/signup?app=tolteck&utm_source=github&utm_medium=readme&utm_campaign=tolteck

## Workflows on the hosted integration

The live integration page documents these 13 operations:

- `list_documents`
- `get_document`
- `create_quote`
- `create_deposit_invoice`
- `create_credit_note`
- `send_document`
- `list_unpaid_invoices`
- `mark_invoice_paid`
- `list_clients`
- `get_client`
- `create_client`
- `list_materials`
- `get_dashboard`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
