# SAP MCP Server

**English** · [Deutsch](docs/README.de.md)

**Connect 3 SAP products to Claude, ChatGPT and Copilot through one MCP server.** Powered by [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp).

SAP MCP Server connects 3 SAP products to Claude, ChatGPT, Copilot and Cursor through one MCP endpoint: 37 tools in total. Pick the systems you run, add their credentials, and each becomes a set of MCP tools. It runs on AnythingMCP Cloud or self-hosted with Docker, with encrypted credentials and an audit log.

**Last verified:** 2026-09-26 against the systems marked verified in the table (each one's own verification is linked from its row).  
**Adapter synced:** <!-- synced -->2026-09-26

Maintained by [helpcode.ai](https://helpcode.ai), the team that builds and maintains [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp).

## Systems

| System | Region | Tools | Auth | Verified live | Cloud | Dedicated repo |
|---|---|---|---|---|---|---|
| SAP Business One | DE | 12 | Login session | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-business-one) | [sap-business-one-mcp-server](https://github.com/HelpCode-ai/sap-business-one-mcp-server) |
| SAP S/4HANA Cloud | Global | 15 | OAuth 2.0 | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-s4hana-cloud) | – |
| SAP Concur | Global | 10 | OAuth 2.0 | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-concur) | – |

"Verified live" means someone ran the connector against a real system; its own repository says how and when. "Not yet" means it follows the vendor's API documentation and has not been confirmed there; reports are welcome.


## Your SAP system isn't listed?

Connect it through what it already exposes: its REST/OData API ([openapi-to-mcp](https://github.com/HelpCode-ai/openapi-to-mcp)), its SOAP services ([soap-to-mcp](https://github.com/HelpCode-ai/soap-to-mcp)) or its SQL database, read-only ([sql-to-mcp](https://github.com/HelpCode-ai/sql-to-mcp)). That covers custom and on-premises builds that no catalog adapter will ever know.

## Self-hosted (Docker)

```bash
git clone https://github.com/HelpCode-ai/sap-mcp-server.git && cd sap-mcp-server
./scripts/install.sh   # installs every system whose credentials are set in .env
```

## Connect Claude, ChatGPT, Copilot or Cursor

- **Claude (claude.ai, Desktop, mobile):** *Customize → Connectors → Add custom connector*, paste your MCP server URL and sign in. Claude connects from Anthropic's cloud, so the URL must be public HTTPS: your AnythingMCP Cloud URL, or your own instance behind TLS.
- **Claude Code:**

  ```bash
  claude mcp add --transport http sap-mcp-server http://localhost:4000/mcp --header "X-API-Key: <MCP_API_KEY>"
  ```
- **Cursor** (`.cursor/mcp.json`) and **VS Code / GitHub Copilot** (`.vscode/mcp.json`, key `servers` instead of `mcpServers`, plus `"type": "http"`):

  ```json
  { "mcpServers": { "sap-mcp-server": { "url": "http://localhost:4000/mcp", "headers": { "X-API-Key": "<MCP_API_KEY>" } } } }
  ```
- **ChatGPT:** add the public HTTPS URL as a connector (app) in ChatGPT's settings. A `localhost` URL does not work there.

## Tools

<!-- tools:start (generated from adapter/*.json, do not edit) -->
#### SAP Business One (12)

| Tool | What it does | Access |
|---|---|---|
| `b1_list_business_partners` | List business partners (customers, suppliers, leads). | read |
| `b1_get_business_partner` | Get one business partner by CardCode. | read |
| `b1_list_items` | List inventory items (articles). | read |
| `b1_get_item` | Get one item by ItemCode. | read |
| `b1_list_orders` | List sales orders. | read |
| `b1_get_order` | Get one sales order by DocEntry (integer primary key). | read |
| `b1_create_order` | Create a new sales order. | write |
| `b1_list_invoices` | List A/R invoices. | read |
| `b1_get_invoice` | Get one A/R invoice by DocEntry. | read |
| `b1_list_quotations` | List sales quotations. | read |
| `b1_list_delivery_notes` | List delivery notes (outgoing goods movements). | read |
| `b1_get_company_info` | Sanity check: returns company metadata (admin info). | read |

#### SAP S/4HANA Cloud (15)

| Tool | What it does | Access |
|---|---|---|
| `s4_list_business_partners` | List business partners (combined customers + suppliers) from API_BUSINESS_PARTNER. | read |
| `s4_get_business_partner` | Get one business partner by its 10-character ID. | read |
| `s4_list_customers` | List customer master records (subset of business partners). | read |
| `s4_list_suppliers` | List supplier master records (subset of business partners). | read |
| `s4_list_sales_orders` | List sales orders (headers). | read |
| `s4_get_sales_order` | Get one sales order by its 10-char number. | read |
| `s4_list_sales_order_items` | List the items (line entries) of a given sales order. | read |
| `s4_list_purchase_orders` | List purchase orders (headers). | read |
| `s4_get_purchase_order` | Get one purchase order by its 10-char number. | read |
| `s4_list_billing_documents` | List billing documents (customer invoices, credit memos). | read |
| `s4_list_outbound_deliveries` | List outbound delivery headers (goods leaving the warehouse). | read |
| `s4_list_journal_entries` | List journal entry items (general ledger postings). | read |
| `s4_list_products` | List materials / products (item master). | read |
| `s4_get_product` | Get one product/material by its ID. | read |
| `s4_list_purchase_requisitions` | List purchase requisition items (pre-PO demand requests). | read |

#### SAP Concur (10)

| Tool | What it does | Access |
|---|---|---|
| `concur_list_reports` | List expense reports for the connected company. | read |
| `concur_get_report` | Get the full detail of one expense report by its ID. | read |
| `concur_list_report_entries` | List the individual expense entries (line items) inside a given report. | read |
| `concur_submit_report` | Submit an expense report for approval. | write |
| `concur_workflow_action` | Approve, send-back or reject a submitted expense report. | write |
| `concur_list_attendees` | List meeting/expense attendees registered in the company directory. | read |
| `concur_list_locations` | List travel/expense locations (cities, regions) available to your tenant. | read |
| `concur_list_users` | List user accounts in the connected Concur tenant. | read |
| `concur_get_user` | Get one user profile by its Concur user ID. | read |
| `concur_list_itineraries` | List travel itineraries (Concur Travel). | read |
<!-- tools:end -->

## FAQ

### Is there an SAP MCP server?
Yes, this repository: SAP Business One, SAP S/4HANA Cloud and SAP Concur as MCP tools for Claude, ChatGPT and Copilot, through AnythingMCP.

### Which SAP systems are covered?
SAP Business One (Service Layer), SAP S/4HANA Cloud Public Edition (OData APIs, OAuth 2.0 via BTP) and SAP Concur (expense and travel). S/4HANA on-premise and ECC connect through their Gateway OData services as a custom REST connector, or read-only through the database.

### Can the AI change data in SAP?
SAP Business One can create sales orders and Concur can submit expense reports and act on approvals. The S/4HANA Cloud tools only read. Roles decide which tools each MCP server exposes.

### Have these been tested against live SAP systems?
Not yet; the "Verified live" column says so per product. The adapters follow SAP's API documentation.

## Related

- [sap-business-one-mcp-server](https://github.com/HelpCode-ai/sap-business-one-mcp-server): SAP Business One MCP server: Claude & ChatGPT read partners, items, orders, invoices and quotations, and create sales orders.
- [erp-mcp-server](https://github.com/HelpCode-ai/erp-mcp-server): ERP MCP server: connect 16 ERPs (SAP, Odoo, JTL-Wawi, Xentral, weclapp, ERPNext…) to Claude & ChatGPT. Self-hosted or cloud.
- [soap-to-mcp](https://github.com/HelpCode-ai/soap-to-mcp): Turn any SOAP/WSDL web service into MCP tools for Claude & ChatGPT. Legacy SOAP APIs as AI tools, no code, self-hosted.
- [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp): the open-source MCP server and gateway this repository is built on.

## License

AGPL-3.0-only. The adapter definition in `adapter/` comes from AnythingMCP (AGPL-3.0).
