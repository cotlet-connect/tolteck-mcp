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

## What you can ask your AI to do in Tolteck

These are real jobs Tolteck users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in Tolteck account.

### Unpaid invoices and quotes

#### Can ChatGPT or Claude list my unpaid Tolteck invoices, oldest first, and draft a reminder email for each?

Yes. Tolteck has no automatic reminders: you open each invoice from the Factures en attente de paiement link and send it by mail. With Cotlet, your agent lists the unpaid invoices oldest first and drafts a reminder for each. You send them from Tolteck yourself.

Ask your agent: "List my unpaid Tolteck invoices, oldest first, with client and amount, and draft a reminder email for each. Flag any over 60 days for a call."

As an API endpoint: Yes. Save the unpaid list as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier to get invoices older than 30 days by email.

#### Can AI check which of my sent Tolteck quotes got no answer in 10 days and write the follow-up?

Yes. With Cotlet, your agent lists the quotes sent more than 10 days ago with no answer in your Tolteck account and drafts a follow-up for each client. You send each follow-up from Tolteck yourself.

Ask your agent: "List my Tolteck quotes sent more than 10 days ago with no answer and draft a short follow-up for each client."

As an API endpoint: Yes. Save the open-quote list as an endpoint in Cotlet and call it every two days from n8n, Make or Zapier.

#### Can AI tell me which clients have an accepted quote and no invoice yet in Tolteck?

Yes. With Cotlet, your agent reads the quotes in your Tolteck account, finds the accepted ones with no invoice and lists client, quote number and amount for each job still to invoice.

Ask your agent: "List the Tolteck clients with an accepted quote and no invoice yet, with quote number and amount."

As an API endpoint: Yes. Save the list as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

#### Can I link my business bank account to Tolteck so paid invoices are ticked automatically?

Partly. Tolteck's help centre shows no bank sync: you set the status Payée by hand. With Cotlet, your agent compares your unpaid invoices with the payments in your online bank, signed in in the same Cloud Browser, and lists the matches. Nothing changes until you say yes.

Ask your agent: "Compare my unpaid Tolteck invoices with this month's incoming payments in my online bank and list the matches. Do not change anything yet."

### Quotes and invoices

#### Can an AI agent turn my notes from a site visit into a draft quote in Tolteck, using my own materials library?

Yes. With Cotlet, your agent reads your notes, picks the matching works and materials from your Tolteck library and saves a draft quote, listing any line with no matching price. Nothing is sent to the client until you say yes.

Ask your agent: "Turn my notes from the Lefèvre bathroom job into a draft quote from my Tolteck library. List lines with no matching price. Do not send it."

#### Can an AI agent prepare the deposit invoice and then the progress invoices from an accepted Tolteck quote?

Yes. Tolteck makes these from the quote's Actions menu: Faire une facture d'acompte, then intermediate or situation invoices, and the final one deducts the deposit. With Cotlet, your agent opens the accepted quote and prepares each one for you to check. Nothing is sent until you say yes.

Ask your agent: "Open the accepted Tolteck quote for Mme Girard and prepare the 30 percent deposit invoice. Show me the amounts before you save."

#### Can AI tell me my margin per job from my Tolteck quotes, since I enter the cost price anyway?

Yes. Tolteck keeps a dry cost and a sale price per article and can show margin in the library. With Cotlet, your agent reads those on each quote and works out cost, sale price and margin per job, so you do not add them up by hand.

Ask your agent: "For each Tolteck quote accepted this quarter, show cost price, sale price and margin in euros and percent."

As an API endpoint: Yes. Save the margin table as an endpoint in Cotlet and call it from n8n, Make or a script at the end of each month.

#### Can AI split my Tolteck revenue between materials and labour for my URSSAF declaration?

Yes. Tolteck's dashboard already has a materials versus labour section for the URSSAF declaration. With Cotlet, your agent reads it for your period, checks it against your invoices and gives you the two totals. You file the declaration yourself.

Ask your agent: "Read my Tolteck dashboard for last quarter and give me total revenue split between materials and labour."

#### How do I add the legal mention for reduced VAT (5.5% or 10%) and the CEE wording to every quote at once in Tolteck?

Partly. Tolteck has help articles for the reduced VAT mention and the CEE wording, and you add them to each document. With Cotlet, your agent opens each draft quote you name and adds the right mention. Nothing changes until you say yes.

Ask your agent: "Open my draft Tolteck quotes for the Oak Street renovation and add the reduced VAT legal mention to each one. Show me the list first."

### Exports and accountant

#### How do I export all my Tolteck quotes and invoices of the quarter to Excel for my accountant, with margins?

Yes. Tolteck's Export Excel under Devis & factures lets you pick document types, statuses, margins and cost price, archived sites and a period. With Cotlet, your agent runs it for the quarter and gives you the file, with the client list if you ask.

Ask your agent: "Export this quarter's Tolteck quotes and invoices to Excel with margins and cost price, and send me the file for my accountant."

As an API endpoint: Yes. Save the export as an endpoint in Cotlet and call it on the first of each quarter from n8n, Make or Zapier.

#### How do I get my Tolteck quotes into Google Sheets automatically every month?

Yes. Tolteck's financial export is a file you download by hand, and no Zapier or Make app was found for it. With Cotlet, your agent reads the month's quotes in Tolteck and adds them as rows to your Google Sheet. Nothing is added until you say yes.

Ask your agent: "Add this month's Tolteck quotes to my Google Sheet, one row each with client, date, amount and status."

As an API endpoint: Yes. Save the monthly read as an endpoint in Cotlet and call it on the first of each month from n8n, Make or Zapier to fill the sheet.

#### How do I connect Tolteck to my accountant's software (Pennylane, QuickBooks, Sage) so I stop sending Excel files?

Partly. Tolteck's help centre lists an Excel export for the accountant and Pennylane for receiving e-invoices, and no accounting sync. With Cotlet, your agent reads the invoices in Tolteck and enters them in your accounting tool, both signed in in your Cloud Browser. Nothing is entered until you say yes.

Ask your agent: "Take this month's Tolteck invoices and enter them in Pennylane with client, date, amount and VAT. Show me the list before you enter anything."

As an API endpoint: Yes. Save the invoice read as an endpoint in Cotlet that returns JSON, and pass it to your accounting tool from n8n or Make.

#### How do I send my Tolteck invoices to a certified platform for the 2026/2027 e-invoicing rule? Does Tolteck do it for me?

Partly. Tolteck sets up e-invoicing under Paramètres, Facturation électronique, with a free Pennylane account, and its help centre documents receiving. Sending is described as running through an integrated platform. With Cotlet, your agent checks your invoices and clients for missing details before you send.

Ask your agent: "List my Tolteck clients and invoices with a missing address or company number so I can fix them before e-invoicing starts."

### Library, imports and moving

#### How do I import my client list from Excel into Tolteck without typing each one?

Partly. Tolteck's help centre describes a client export to Excel, but no self-serve client import. With Cotlet, your agent reads your spreadsheet and adds each client in Tolteck with address and phone. Nothing is created until you say yes.

Ask your agent: "Take the clients in this spreadsheet and add each one to Tolteck with address and phone. Show me the first five before you start."

#### How do I import my own works library (ouvrages and prices) into Tolteck? Is there a limit?

Partly. Tolteck's support imports a library for you from an Excel file, up to 250 references. With Cotlet, your agent can also add works and materials one by one in your Tolteck library from your price list. Nothing is created until you say yes.

Ask your agent: "Take the works and prices in this spreadsheet and add each to my Tolteck library. Show me the first five before you start."

#### How do I connect my Point.P or Cédéo account to Tolteck so my negotiated prices appear in my quotes?

Partly. Tolteck connects a Point.P or Cédéo account itself, from its own settings, and a quote can search a supplier for a material. With Cotlet, your agent runs that search for each material of a draft quote and lists prices that look different from your library. Nothing changes until you say yes.

Ask your agent: "For each material in my draft Tolteck quote, search my supplier price and list those that differ from my library. Do not change anything."

#### How do I move all my quotes and clients from Tolteck to another software and keep numbering and history?

Partly. Tolteck exports quotes, invoices and clients to Excel only, and no FEC export is described. With Cotlet, your agent exports them and adds clients and documents in the new software, signed in in the same Cloud Browser. Nothing is created until you say yes.

Ask your agent: "Export my Tolteck clients, quotes and invoices to Excel and keep their numbers and dates, so I can load them into my new software."

#### How do I cancel my Tolteck subscription and get my data out?

Partly. Tolteck's help centre describes no cancel button: you write to its support. With Cotlet, your agent saves your clients, quotes and invoices as Excel files first, so you keep your history. Cancelling stays your step.

Ask your agent: "Export all my Tolteck clients, quotes and invoices to Excel files before I cancel my subscription."

### Terms and safety

#### Is it allowed to let an AI assistant work in my Tolteck account? Will Tolteck suspend it?

Partly. Tolteck's terms, section 5.2, bar AI assistants, agents and scripts from interacting with the service, and section 5.3 allows suspension without notice. With Cotlet, the agent works in your own signed-in session and asks before it changes anything. Read the terms before you allow it.

Ask your agent: "Before you change anything in Tolteck, show me what you plan to do and wait for my OK."

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

## More trades and construction apps on Cotlet

- [Obat MCP](https://github.com/cotlet-connect/obat-mcp)
- [Plancraft MCP](https://github.com/cotlet-connect/plancraft-mcp)
- [Tradify MCP](https://github.com/cotlet-connect/tradify-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [Buildxact MCP](https://github.com/cotlet-connect/buildxact-mcp)
