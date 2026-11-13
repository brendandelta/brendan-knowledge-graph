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
| googledrive | brendan@deltameridian.com | `googledrive_decare-benami` | Delta | delta-meridian-inbox |
| googlesheets | brendan@arrowfund.co | `googlesheets_cush-citron` | Arrow | deal-indications |
| googledocs | brendan@arrowfund.co | `googledocs_colius-skiv` | Arrow | arrow-weekly (draft); edits to the Arrow Drive README |
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
| whatsapp (Brendan) | Brendan's own WhatsApp. Own Hetzner server, 24/7. On the v0.4.0 build since 2026-11-13 (18 tools on the admin link, 56 day retention, auto updates from the `release` branch); see `references/whatsapp-mcp.md` | paper, until 1Password | comms-ingest, crm-maintenance, deal-indications, suggested-actions, chief-of-staff, attio-contact-context, arrow-attio-firm-context, arrow-weekly-log |
| whatsapp-chris | Chris Clifford's WhatsApp. Arrow team Hetzner server, v0.4.0 since 2026-11-12 (18 tools on the admin link, 10 on a view-only link); custom MCP on Brendan's Grok Bot account (added 2026-11-10). Do not store the endpoint URL or access token in this repo. | paper, until 1Password | attio-contact-context, arrow-attio-firm-context, deal-indications, arrow-weekly-log, comms-ingest (index only, limited Todoist), chief-of-staff (limited) |
| whatsapp-gabe | Gabe Borden's WhatsApp. Arrow team Hetzner server, v0.4.0, linked 2026-11-12. Connector not yet added to any bot. Do not store the endpoint URL or access token in this repo. | paper, until 1Password | none yet |
| whatsapp-jillian | Jillian Nuding's WhatsApp. Arrow team Hetzner server, v0.4.0, linked 2026-11-12. Connector not yet added to any bot. Do not store the endpoint URL or access token in this repo. | paper, until 1Password | none yet |

## Partner WhatsApp (Chris now; Gabe and Jillian later)

Brendan's own WhatsApp is `user-whatsapp-mcp` (whatsapp (Brendan) above). Partner mailboxes are separate connectors and must never be mixed with his. As of 2026-11-10:

- Chris Clifford: `user-Chris' WhatsApp MCP` (connected). v0.4.0 since 2026-11-12.
- Gabe Borden and Jillian Nuding: WhatsApp accounts linked and running v0.4.0 since 2026-11-12. Their connectors have not been added to a bot yet. When Brendan adds them, update the rows above and reuse the rules below. Do not rewrite skills.

How the server works, the full tool list, link types, retention and deploys: `references/whatsapp-mcp.md`. Read it before using any WhatsApp connector.

Link types (v0.4.0, enforced by the server, not by convention):

- Admin link: read and write. Brendan holds the admin link for every partner account. Chris also uses the admin link for his own account.
- View-only link: read tools only. The send, draft, import and group tools do not exist on it. Gabe and Jillian get view-only links for their own accounts.
- A bot can tell which it has from the last line of `status`.

Rules for every partner WhatsApp connector:

1. Always address the connector by its exact namespace. Never use a partner connector when the task is Brendan's WhatsApp, and never the reverse.
2. Default is read-only. Never call `draft_message` or `send_message` on a partner connector unless Brendan explicitly asks to send as that partner.
3. Arrow-relevant chats only. Skip personal, family, and non-Arrow threads. Same silo as personal, Delta, and Janey material.
4. Source attribution must name the partner and the mailbox, e.g. `chris (11/10) - chris-whatsapp`.
5. Dedup: a shared Arrow group or a Brendan-partner thread may appear in both Brendan's WhatsApp and a partner's WhatsApp. Treat identical content at the same time as one item; do not double-write Attio, the weekly log, or Todoist.
6. Skills should refer to this section rather than restating a one-off Chris-only setup, so adding Gabe or Jillian later is a new row plus the same filters.

## Git access from the box

- The box can push to `brendandelta/brendan-knowledge-graph` (since 2026-11-02). Auth is a fine-grained GitHub token scoped to this one repo only (Contents: read/write), stored in the box's git credential helper. Every bot on the box can use it through plain `git push`.
- The token itself is never written into this repo, a skill, a log, memory or chat. If it must be rotated, Brendan creates a new one on GitHub and it goes straight into the credential helper.

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
