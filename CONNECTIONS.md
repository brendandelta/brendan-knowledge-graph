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
| attio | Arrow Fund workspace, full read-write. Meetings and call recordings: `skills/comms-ingest/attio-calls.md`. Prefer `ATTIO_CONTEXT_TOKEN` when present (never print it); Composio is the read fallback. | `attio_bosser-wooded` | Arrow | deal-indications, crm-maintenance, brendan-crm, suggested-actions, attio-contact-context, arrow-attio-firm-context, arrow-weekly-log, comms-ingest |
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
| whatsapp (Brendan) | Brendan's own WhatsApp. Own Hetzner server, 24/7. On the v0.5.0 build since 2026-11-13 (31 tools on the admin link, 15 on a view-only link, 56 day body purge with `contact_activity` and the change log kept, auto updates from the `release` branch); see `references/whatsapp-mcp.md` | paper, until 1Password | comms-ingest, crm-maintenance, deal-indications, suggested-actions, chief-of-staff, attio-contact-context, arrow-attio-firm-context, arrow-weekly-log |
| whatsapp-chris | Chris Clifford's WhatsApp. Arrow team Hetzner server, v0.5.0 since 2026-11-13 (31 tools on the admin link, 15 on a view-only link); custom MCP on Brendan's Grok Bot account as `user-Chris WhatsApp MCP (Admin)` (added 2026-11-10, renamed 2026-11-13). Do not store the endpoint URL or access token in this repo. | paper, until 1Password | attio-contact-context, arrow-attio-firm-context, deal-indications, arrow-weekly-log, comms-ingest (index only, limited Todoist), chief-of-staff (limited) |
| whatsapp-gabe | Gabe Borden's WhatsApp. Arrow team Hetzner server, v0.5.0 since 2026-11-13 (31 tools on the admin link, 15 on a view-only link); custom MCP on Brendan's Grok Bot account as `user-Gabe WhatsApp MCP (Admin)` (connected 2026-11-13). Do not store the endpoint URL or access token in this repo. | paper, until 1Password | attio-contact-context, arrow-attio-firm-context, deal-indications, arrow-weekly-log, comms-ingest (index only, limited Todoist), chief-of-staff (limited) |
| whatsapp-jillian | Jillian Nuding's WhatsApp. Arrow team Hetzner server, v0.5.0 since 2026-11-13 (31 tools on the admin link, 15 on a view-only link); custom MCP on Brendan's Grok Bot account as `user-Jillian WhatsApp MCP (Admin)` (connected 2026-11-13). Do not store the endpoint URL or access token in this repo. | paper, until 1Password | attio-contact-context, arrow-attio-firm-context, deal-indications, arrow-weekly-log, comms-ingest (index only, limited Todoist), chief-of-staff (limited) |

## Partner WhatsApp (Chris, Gabe, and Jillian)

Brendan's own WhatsApp is `user-whatsapp-mcp` (whatsapp (Brendan) above). Partner mailboxes are separate connectors and must never be mixed with his. As of 2026-11-13 all three partner accounts are connected on Brendan's Grok Bot account:

- Chris Clifford: `user-Chris WhatsApp MCP (Admin)` (connected 2026-11-10, renamed 2026-11-13). v0.5.0 since 2026-11-13.
- Gabe Borden: `user-Gabe WhatsApp MCP (Admin)` (connected 2026-11-13). v0.5.0 since 2026-11-13.
- Jillian Nuding: `user-Jillian WhatsApp MCP (Admin)` (connected 2026-11-13). v0.5.0 since 2026-11-13.

How the server works, the full tool list, link types, retention and deploys: `references/whatsapp-mcp.md`. Read it before using any WhatsApp connector.

Link types (v0.5.0, enforced by the server, not by convention):

- Admin link: read and write (31 tools). Brendan holds the admin link for every partner account. Chris also uses the admin link for his own account.
- View-only link: the 15 read tools only, including `contact_activity`, `changes_since`, `export_contacts`, `resolve_person`, and `coverage_report`. The send, draft, `import_contacts`, `import_names`, and group draft tools do not exist on it. Gabe and Jillian get view-only links for their own accounts.
- A bot can tell which it has from the last line of `status`.

Rules for every partner WhatsApp connector:

1. Always address the connector by its exact namespace. Never use a partner connector when the task is Brendan's WhatsApp, and never the reverse.
2. Default is read-only. Never call `draft_message` or `send_message` on a partner connector unless Brendan explicitly asks to send as that partner.
3. Arrow-relevant chats only. Skip personal, family, and non-Arrow threads. Same silo as personal, Delta, and Janey material.
4. Source attribution must name the partner and the mailbox, e.g. `chris (11/10) - chris-whatsapp`, `gabe (11/13) - gabe-whatsapp`, `jillian (11/13) - jillian-whatsapp`.
5. Dedup: a shared Arrow group or a thread with the same person may appear in Brendan's WhatsApp and on more than one partner server. Match and dedupe by phone JID (or E.164 when no JID is stored). Treat identical content at the same time as one item; do not double-write Attio, the weekly log, or Todoist.
6. Skills should refer to this section rather than restating a one-off setup. A new partner is a new CONNECTIONS.md row, the same filters, and a skill update that names the new connector.

## Attio WhatsApp identity fields

Arrow Attio (workspace `arrow-fund`) stores WhatsApp ids on People and Companies so bots can match a chat by JID first and phone second. Full field ids, write rules, and the standard-chat test: `references/attio-schema.md`.

- People: `whatsapp_phone_jid` (`{E.164 without plus}@s.whatsapp.net`), `whatsapp_lid` (`{n}@lid`), and `standard_arrow_chat_jid` (`{n}@g.us`).
- Companies: `standard_arrow_chat_jid` only.
- Fill only empty values. Never overwrite. Never create a person or firm just to store a JID. If more than one Attio record matches, skip and log it.
- `standard_arrow_chat_jid` is only for a group that is clearly the standing Arrow chat with one person or one firm (for example `X <> Arrow Fund`). Skip internals, multi-party chats, deal-specific threads, first-name-only titles, and duplicate chats for the same counterpart.

Skills that map WhatsApp to Attio (attio-contact-context, arrow-attio-firm-context, deal-indications, comms-ingest) must follow this matching order. Partner WhatsApp stays read-only unless Brendan explicitly asks to send as that partner.

## Attio meetings and call recordings

Attio call recordings and transcripts are a context source alongside WhatsApp and email for every bot that writes context from comms. The shared procedure, working endpoints, paging, empty-transcript retries, and the never-write-from-the-shared-step rule are in `skills/comms-ingest/attio-calls.md`. Each bot keeps its own cursor and seen-recordings list and writes only its own silo.


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
