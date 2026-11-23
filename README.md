# Tolteck API integration and MCP server: connect Tolteck to ChatGPT, Claude and any AI agent

Tolteck API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in Tolteck account: quotes (devis), invoices, deposits and progress invoices, clients, item library and supplier prices, on the same screens your login opens. No developer and no Tolteck API key needed. Cotlet is independent and is not affiliated with Tolteck.

**Does Tolteck have an API or an MCP server?** Tolteck has no public API and no Zapier or Make app (checked 23 Nov 2026). Its integrations are Point.P and Cédéo supplier prices, a Solu+ import and Pennylane for receiving e-invoices, and its export is an Excel file of quotes, invoices and clients. Tolteck has no MCP server and no ChatGPT or Claude app.

## What tradespeople and small builders ask their AI to do in Tolteck

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Quote from site notes | "Write a quote for the Martin bathroom from my site notes, using my library items and prices. Show me before you save." | When a job comes in |
| Unpaid invoices | "List invoices unpaid after 30, 60 and 90 days and draft a reminder for each client. Send nothing yet." | Weekly |
| Excel for your accountant | "Export this month's quotes and invoices to Excel for my accountant." | Monthly |
| Margins per job | "Show cost price, sale price and margin for each job invoiced this month." | Monthly |
| Deposit and progress invoices | "Make the 30% deposit invoice for the accepted Dubois quote. Show me before you save." | When a quote is accepted |
| Supplier prices | "Update my library with this Point.P price list and show me which items changed price." | When prices change |
| Quotes waiting for an answer | "List quotes sent over 2 weeks ago with no answer and draft a follow-up for each." | Weekly |
| Invoices in a spreadsheet | "Put each invoice with client, amount and payment status in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Import clients | "Import the clients from this spreadsheet into Tolteck. Show me before you save." | Once |
| Leaving Tolteck | "Export all my quotes, invoices, clients and library before I switch software." | Once |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own Tolteck API, built with Cotlet

Tolteck gives you no API and no Zapier app. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, Pennylane, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `unpaid-invoices` | invoices unpaid past the days you name, with client and amount |
| `open-quotes` | quotes with no answer, with client, total and date sent |
| `invoices-for-period` | invoices for a period with client, amount and status |
| `job-margins` | cost price, sale price and margin per job |
| `library-items` | library items with unit price and supplier |
| `draft-quote` | saves a draft quote from the lines you send; nothing is sent (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/unpaid-invoices \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=tolteck

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to Tolteck once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

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

## Questions from Tolteck tradespeople
**Does Tolteck have an API or a Zapier app?**

No. Tolteck has no public API and no Zapier or Make app (checked 23 Nov 2026). With Cotlet you build your own endpoints in your signed-in Tolteck account and call them from Google Sheets, Zapier, Make or n8n.

**Can ChatGPT or Claude write a Tolteck quote from my site notes?**

Yes. Your AI turns your notes into quote lines from your library and saves a draft. You check it before it goes to the client.

**Can AI chase unpaid Tolteck invoices?**

Yes. It lists invoices unpaid after 30, 60 and 90 days and drafts a reminder for each client. Nothing goes out until you say yes.

**How do I send Tolteck invoices to my accountant?**

Your AI exports the month's quotes and invoices to Excel, ready for your accountant or for Pennylane.

**Can AI show my margin per job in Tolteck?**

Yes. It shows cost price, sale price and margin for each invoiced job.

**How do I export everything from Tolteck before I switch?**

Your AI exports quotes, invoices, clients and your library from the screens your login opens.

## Find it on

- [Tolteck MCP server on Cotlet.dev](https://cotlet.dev/integrations/tolteck-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=tolteck&utm_content=find)
- [Tolteck API with Cotlet](https://cotlet.dev/integrations/api/tolteck?utm_source=github&utm_medium=readme&utm_campaign=tolteck&utm_content=find)
- [Does Tolteck have an API or MCP server?](https://cotlet.dev/api-mcp-status/tolteck?utm_source=github&utm_medium=readme&utm_campaign=tolteck&utm_content=find)
- [PyPI package tolteck-mcp](https://pypi.org/project/tolteck-mcp/)

## More trades and construction apps on Cotlet

- [Obat MCP](https://github.com/cotlet-connect/obat-mcp)
- [Plancraft MCP](https://github.com/cotlet-connect/plancraft-mcp)
- [Tradify MCP](https://github.com/cotlet-connect/tradify-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [Buildxact MCP](https://github.com/cotlet-connect/buildxact-mcp)
