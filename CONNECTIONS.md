# Connections

The "hands" layer. Every way an agent reaches a system of record. If a skill talks to something, it's listed here. Skills reference these by name; they don't describe how to connect.

**Rule (2026-10-20):** everything goes through Composio. Claude Desktop connectors are not used. Where Composio has no toolkit, Brendan builds a custom MCP server (as with iMessage, Signal, WhatsApp).

Verified live against Composio 2026-10-20.

## Composio connections (active)

| Toolkit | Account | Connection id | Context | Used by |
|---|---|---|---|---|
| gmail | brendan@arrowfund.co | `gmail_tagish-stuss` | Arrow | comms-ingest, crm-maintenance, chief-of-staff, deal-indications |
| gmail | admin@arrowfund.co | `gmail_dammer-immi` | Arrow | — |
| gmail | brendan@deltameridian.com | `gmail_antes-dyce` | Delta | comms-ingest, crm-maintenance |
| gmail | admin@deltameridian.com | `gmail_crept-situla` (default) | Delta | comms-ingest, crm-maintenance |
| gmail | connmbrendan@gmail.com | `gmail_gifola-hexine` | Personal | comms-ingest, crm-maintenance |
| gmail | brendanmconn96@gmail.com | `gmail_rebold-anotta` | Personal | comms-ingest |
| gmail | brendanfinancialt1@gmail.com | `gmail_cerris-heed` | Personal finance | comms-ingest (crm-maintenance skips) |
| googlecalendar | brendan@arrowfund.co | `googlecalendar_unclub-taha` | Arrow | chief-of-staff, comms-ingest |
| googledrive | brendanmconn96@gmail.com | `googledrive_octan-lined` (default; change default to arrow) | Personal | — |
| googledrive | connmbrendan@gmail.com | `googledrive_mev-zuni` | Personal; holds some Janey folders | — |
| googledrive | brendan@arrowfund.co | `googledrive_left-bim` | Arrow | arrow-fund-inbox |
| googledrive | brendan@deltameridian.com | `googledrive_decare-benami` | Delta | — |
| googlesheets | brendan@arrowfund.co | `googlesheets_cush-citron` | Arrow | deal-indications |
| googledocs | brendan@arrowfund.co | `googledocs_colius-skiv` | Arrow | arrow-weekly (draft) |
| notion | Deltameridian workspace | `notion_pace-adiate` (default) | Personal | brendan-crm, crm-maintenance |
| notion | Arrow team workspace ("Chris Clifford's Space") | `notion_woy-grouts` (alias work) | Arrow | — (Notion overhaul planned) |
| attio | Arrow Fund workspace, full read-write | `attio_bosser-wooded` | Arrow | deal-indications, crm-maintenance, brendan-crm, suggested-actions |
| quickbooks | Arrow | `quickbooks_godet-board` (default) | Arrow | — |
| quickbooks | Delta | `quickbooks_stid-benchy` | Delta | delta-bookkeeper |
| todoist | brendan@deltameridian.com | `todoist_kino-vend` | all | chief-of-staff, comms-ingest |
| shopify | Janey store | `shopify_styx-dhai` | Janey | — |
| github | brendandelta | `github_worth-cower` | infra | — |
| linkedin | brendanmconn96@gmail.com | `linkedin_odic-bran` | Personal | — |

## Custom MCP servers

| Name | Runs on | Secret lives in | Used by |
|---|---|---|---|
| imessage | the Mac | paper, until 1Password | comms-ingest, crm-maintenance, suggested-actions |
| signal | the Mac | paper, until 1Password | comms-ingest, crm-maintenance, deal-indications, suggested-actions |
| whatsapp v0.3 | Hetzner, 24/7 | paper, until 1Password | comms-ingest, crm-maintenance, deal-indications, suggested-actions, chief-of-staff |

## To connect

| Toolkit | Account | Context | Why |
|---|---|---|---|
| googlecalendar | connmbrendan@gmail.com | Personal | chief-of-staff already expects it |
| googledrive | brendan@janeyhealth.com | Janey | owns the Janey Drive connection; document-filing (Janey wrapper planned, not built) |
| quickbooks | Janey | Janey | accounting-and-tax |
| facebook_ads | Janey Meta ad account | Janey | toolkit exists |
| gusto | payroll | Delta / Arrow | toolkit exists |
| ramp | Arrow cards | Arrow | toolkit exists; has a payments API |
| plaid | Chase, WF, Goldman, Wise | all | read path for money-queue |
| slack, dropbox, docusign | — | — | only if a workflow needs them |

## Money movement (design note for money-queue)

Reading: Plaid for every bank and card. Moving: only through platforms with real payment APIs (Wise, Ramp, QBO payments, a bill-pay service such as Melio or Bill.com for vendors). Consumer banks (Chase personal, Wells Fargo) expose no transfer API; scraping violates terms and gets accounts frozen, so those stay a tap in the bank app with the agent having prepared the exact amounts. Idea to explore: a dedicated agent-payable account or card with a funded cap, swept from Chase, so the agent can pay from it via API. Every movement is approved first regardless.

## Secrets

Every API key, token, and password lives in 1Password (decided; account to be opened once the mini is set up). Vaults: Personal, Delta, Arrow (shareable with Chris and Vaidehi), Janey (shareable with Braeden). Composio OAuth tokens live inside Composio; the 1Password entry is the login itself. The three MCP server secrets are on paper until then. Nothing in this repo, nothing in chat. The mini reads secrets through the 1Password CLI or a gitignored `.env` populated from it.

## Composio usage

One MCP endpoint, many apps. Tools found with `COMPOSIO_SEARCH_TOOLS`, run with `COMPOSIO_MULTI_EXECUTE_TOOL` or `COMPOSIO_REMOTE_WORKBENCH`. Always pass the connection id explicitly when a toolkit has several accounts. Full guide: `references/composio-guide.md`.
