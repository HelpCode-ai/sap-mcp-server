# SAP MCP Server

**Connect 4 SAP products to Claude, ChatGPT and Copilot through one MCP server.** Powered by [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp).

SAP MCP Server connects 4 SAP products to Claude, ChatGPT, Copilot and Cursor through one MCP endpoint: 76 tools in total. Pick the systems you run, add their credentials, and each becomes a set of MCP tools. It runs on AnythingMCP Cloud or self-hosted with Docker, with encrypted credentials and an audit log.

**Last verified:** 2026-09-27 against the systems marked verified in the table (each one's own verification is linked from its row).  
**Adapter synced:** <!-- synced -->2026-10-09

Maintained by [helpcode.ai](https://helpcode.ai), the team that builds and maintains [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp).

## Systems

| System | Region | Tools | Auth | Verified live | Cloud | Dedicated repo |
|---|---|---|---|---|---|---|
| SAP Business One | DE | 24 | Login session | yes, 2026-10-09 | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-business-one) | [sap-business-one-mcp-server](https://github.com/HelpCode-ai/sap-business-one-mcp-server) |
| SAP S/4HANA Cloud | Global | 20 | OAuth 2.0 | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-s4hana-cloud) | – |
| SAP S/4HANA (OData) | Global | 12 | User + password | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-s4hana-odata) | – |
| SAP S/4HANA (HANA SQL) | Global | 10 | DB user | yes, 2026-09-27 | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-s4hana-hana) | [sap-hana-mcp-server](https://github.com/HelpCode-ai/sap-hana-mcp-server) |
| SAP Concur | Global | 10 | OAuth 2.0 | not yet | [install](https://cloud.anythingmcp.com/connectors/store?install=sap-concur) | – |

"Verified live" means someone ran the connector against a real system; its own repository says how and when. "Not yet" means it follows the vendor's API documentation and has not been confirmed there; reports are welcome.


## Your SAP system isn't listed?

Connect it through what it already exposes: its OData services ([odata-to-mcp](https://github.com/HelpCode-ai/odata-to-mcp)), its REST API ([openapi-to-mcp](https://github.com/HelpCode-ai/openapi-to-mcp)), its SOAP services ([soap-to-mcp](https://github.com/HelpCode-ai/soap-to-mcp)) or its SQL database, read-only ([sql-to-mcp](https://github.com/HelpCode-ai/sql-to-mcp)). That covers custom and on-premises builds that no catalog adapter will ever know.

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
#### SAP Business One (24)

| Tool | What it does | Access |
|---|---|---|
| `b1_list_business_partners` | List business partners (customers, suppliers, leads) with their code, name, type and balance. | read |
| `b1_get_business_partner` | Read one business partner by CardCode: addresses, contacts, payment terms and balance. | read |
| `b1_list_items` | List inventory items with their code, name, prices and stock per warehouse (ItemWarehouseInfoCollection). | read |
| `b1_get_item` | Read one item by ItemCode: prices, units, groups and stock in each warehouse. | read |
| `b1_list_orders` | List sales orders with their customer, dates, totals and status (bost_Open / bost_Close). | read |
| `b1_get_order` | Read one sales order by DocEntry (integer key, not DocNum), with all its lines. | read |
| `b1_create_order` | Create a new sales order. | write |
| `b1_list_invoices` | List A/R invoices (sales invoices to customers). | read |
| `b1_get_invoice` | Read one A/R invoice by DocEntry (integer key, not DocNum), with lines and taxes. | read |
| `b1_list_quotations` | List sales quotations with their customer, validity date, totals and status. | read |
| `b1_list_delivery_notes` | List delivery notes (goods shipped to customers) with their customer, date and lines. | read |
| `b1_list_credit_notes` | List A/R credit memos (credit notes issued to customers) with their totals and status. | read |
| `b1_list_purchase_invoices` | List A/P invoices (supplier invoices). | read |
| `b1_get_purchase_invoice` | Read one A/P invoice in full, with its lines, taxes and withholding tax. | read |
| `b1_list_purchase_credit_notes` | List A/P credit memos (credit notes received from suppliers). | read |
| `b1_list_vendor_payments` | List outgoing payments to suppliers, with the invoices each one settles (PaymentInvoices). | read |
| `b1_list_incoming_payments` | List incoming payments from customers, with the invoices each one settles (PaymentInvoices). | read |
| `b1_list_journal_entries` | List journal entries with their lines (JournalEntryLines: account, debit, credit). | read |
| `b1_get_journal_entry` | Read one journal entry by JdtNum (TransId) with all its lines: account, debit and credit. | read |
| `b1_list_chart_of_accounts` | List G/L accounts of the chart of accounts with their code, name, type and balance. | read |
| `b1_list_bank_statements` | List imported bank statements with their account, date and balances. | read |
| `b1_list_external_reconciliations` | List external (bank) reconciliations of G/L or business partner accounts in a date or number range. | read |
| `b1_get_external_reconciliation` | Read one external (bank) reconciliation: amount, date, type and the journal entry and bank statement lines it matched. | read |
| `b1_get_company_info` | Sanity check: returns company metadata (admin info). | read |

#### SAP S/4HANA Cloud (20)

| Tool | What it does | Access |
|---|---|---|
| `s4_cloud_list_services` | List the OData services this connector reaches. | read |
| `s4_cloud_describe_service` | The entity sets of an OData service with their business labels, keys and whether they are analytical or parameterised. | read |
| `s4_cloud_describe_entity` | The fields of one entity set: labels, types, keys, the currency or unit field of each amount, dimensions and measures, and required filters. | read |
| `s4_cloud_query` | Read rows from an entity set, with field names checked against the service model and server paging followed. | read |
| `s4_cloud_get_entity` | Read one entity by its key, optionally with related entities. | read |
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

#### SAP S/4HANA (OData) (12)

| Tool | What it does | Access |
|---|---|---|
| `s4_list_services` | List the OData services published by the SAP Gateway (V2 and V4 catalog), filtered by words in their name, title or description. | read |
| `s4_describe_service` | The entity sets of an OData service with their business labels, keys and whether they are analytical or parameterised. | read |
| `s4_describe_entity` | The fields of one entity set: labels, types, keys, the currency or unit field of each amount, dimensions and measures, and required filters. | read |
| `s4_query` | Read rows from an entity set, with field names checked against the service model and server paging followed. | read |
| `s4_get_entity` | Read one entity by its key, optionally with related entities. | read |
| `s4_guide` | How to read SAP S/4HANA through OData: the workflow (overview), filter syntax, analytical services and KPIs, finance, sales, inventory and pitfalls. | read |
| `s4_journal_entry_items` | Journal entry line items from the general ledger (API_JOURNALENTRYITEMBASIC_SRV / A_JournalEntryItemBasic): company code, fiscal year, G/L account, posting… | read |
| `s4_billing_documents` | Billing documents (API_BILLING_DOCUMENT_SRV / A_BillingDocument): invoices, credit memos and cancellations with date, sales organization, sold-to party, net… | read |
| `s4_sales_orders` | Sales orders (API_SALES_ORDER_SRV / A_SalesOrder) with type, sales organization, sold-to party, creation date, net amount, currency and processing status. | read |
| `s4_business_partners` | Business partners (API_BUSINESS_PARTNER / A_BusinessPartner): customers, suppliers and contacts with name, category and grouping. | read |
| `s4_material_stock` | Material stock (API_MATERIAL_STOCK_SRV / A_MatlStkInAcctMod) per material, plant, storage location, batch and stock type, in the material's base unit. | read |
| `s4_products` | Products (API_PRODUCT_SRV / A_Product) with type, group and base unit. | read |

#### SAP S/4HANA (HANA SQL) (10)

| Tool | What it does | Access |
|---|---|---|
| `sap_guide` | The SAP data model explained for SQL: how to work (overview), client and data types (basics), finance, sales, inventory, procurement, operations, pitfalls… | read |
| `sap_org_structure` | Company codes (with currency, chart of accounts and fiscal year variant), controlling areas, plants, sales organizations and purchasing organizations of… | read |
| `sap_search_tables` | Find SAP tables and database views by name pattern or by words in their description (e.g. "billing document", "VBRK", "ZSD%"). | read |
| `sap_describe_table` | Every field of an SAP table with its business label, key flag, type, the currency or unit field that goes with each amount or quantity, and the check table… | read |
| `sap_find_fields` | Find which tables hold a business field, by words in its label (e.g. "payment terms", "net due date") or by data element name. | read |
| `sap_field_values` | What the codes in a field mean: the fixed values of its domain with their descriptions (e.g. VBRK VBTYP, ACDOCA KOART). | read |
| `sap_table_relations` | How a table joins to others: its foreign keys with the check table and the join condition, and the text table that holds its descriptions. | read |
| `sap_search_cds_views` | Search SAP's CDS views (the S/4HANA virtual data model) by name or label, e.g. "journal entry", "billing", "stock". | read |
| `sap_describe_cds_view` | Columns of a CDS view with their labels, default aggregation (SUM marks a measure), currency and unit fields, text fields and foreign-key associations. | read |
| `sap_query` | Run one read-only SQL SELECT against the SAP HANA database and return up to 1000 rows. | read |

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
Yes, this repository: SAP Business One, SAP S/4HANA (Cloud Public Edition, plus on-premise and Private Cloud through OData or HANA SQL) and SAP Concur as MCP tools for Claude, ChatGPT and Copilot, through AnythingMCP.

### Which SAP systems are covered?
SAP Business One (Service Layer), SAP S/4HANA Cloud Public Edition (OData APIs, OAuth 2.0 via BTP), SAP S/4HANA on-premise and Private Cloud (OData through SAP Gateway, or SQL on the HANA database) and SAP Concur (expense and travel). ECC and BW with SAP Gateway connect through the OData connector as well.

### OData or HANA SQL for S/4HANA on-premise?
OData goes through SAP Gateway: SAP checks the technical user's authorizations on every call and only published services are reachable, which is what SAP's API policy expects from third-party integrations. HANA SQL reads every table and CDS view, with SAP's data dictionary as the map, and aggregates on the database; it bypasses SAP's application authorizations, so the database grants are the boundary, and your HANA licence must allow third-party SQL access. Both can run on the same MCP server.

### Can the AI change data in SAP?
SAP Business One can create sales orders and Concur can submit expense reports and act on approvals. The S/4HANA tools only read: the OData built-ins are read-only, and the HANA session runs `SET TRANSACTION READ ONLY`. Roles decide which tools each MCP server exposes.

### Have these been tested against live SAP systems?
The HANA SQL connector was verified on 2026-09-27 against an SAP S/4HANA 2025 Private Cloud system, every tool. The OData engine was verified against public OData services, not yet against a live SAP Gateway. The other adapters follow SAP's API documentation; the "Verified live" column says so per product.

## Related

- [sap-business-one-mcp-server](https://github.com/HelpCode-ai/sap-business-one-mcp-server): SAP Business One MCP server: Claude & ChatGPT read partners, items, orders, invoices, payments and journal entries, and create sales orders.
- [sap-hana-mcp-server](https://github.com/HelpCode-ai/sap-hana-mcp-server): SAP HANA MCP server: Claude & ChatGPT read SAP S/4HANA with SQL, guided by SAP's own data dictionary and CDS views. Read-only.
- [odata-to-mcp](https://github.com/HelpCode-ai/odata-to-mcp): OData to MCP: turn any OData V2 or V4 service, SAP Gateway included, into MCP tools for Claude & ChatGPT. Reads $metadata, no code.
- [erp-mcp-server](https://github.com/HelpCode-ai/erp-mcp-server): ERP MCP server: connect 17 ERPs (SAP, Odoo, JTL-Wawi, Xentral, weclapp, ERPNext…) to Claude & ChatGPT. Self-hosted or cloud.
- [soap-to-mcp](https://github.com/HelpCode-ai/soap-to-mcp): Turn any SOAP/WSDL web service into MCP tools for Claude & ChatGPT. Legacy SOAP APIs as AI tools, no code, self-hosted.
- [AnythingMCP](https://github.com/HelpCode-ai/anythingmcp): the open-source MCP server and gateway this repository is built on.

## License

AGPL-3.0-only. The adapter definition in `adapter/` comes from AnythingMCP (AGPL-3.0).
