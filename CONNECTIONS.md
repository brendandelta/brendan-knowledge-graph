# Connections

The "hands" layer. Every way the agent reaches a system of record. If a skill talks to something, it's listed here. Skills reference these by name; they don't describe how to connect.

| Name | What it reaches | How | Account | Used by |
|---|---|---|---|---|
| composio:notion | Notion (People db, Delta workspace) | Composio toolkit `notion`, account `notion_pace-adiate` | brendan@deltameridian.com | brendan-crm |
| composio:attio | Attio (Arrow pipeline) | Composio toolkit `attio` | brendan@deltameridian.com | brendan-crm (read), arrow skills |
| composio:quickbooks | QuickBooks Online (Delta) | Composio toolkit `quickbooks` | admin@deltameridian.com | delta-bookkeeper |
| gdrive | Arrow shared Google Drive | Google Drive connector / MCP | brendan@deltameridian.com | arrow-fund-inbox |
| gmail | Email | Gmail MCP | brendan@deltameridian.com | morning-brief, message-triage, prep-for-meeting |
| gcal | Calendar | Google Calendar MCP | brendan@deltameridian.com | morning-brief, prep-for-meeting, week-ahead |
| imessage | iMessage | your iMessage MCP server (macOS only) | — | message-triage, morning-brief |
| signal | Signal | your Signal MCP server | — | message-triage |
| whatsapp | WhatsApp | WhatsApp MCP | — | message-triage |
| todoist | Tasks | Composio, account `todoist_kino-vend`; projects Arrow/Delta/Janey/Personal finance/Personal other | Delta | chief-of-staff, comms-ingest |
| composio:gmail x6 | arrowfund `gmail_tagish-stuss`; brendan@delta `gmail_antes-dyce`; admin@delta `gmail_crept-situla`; connmbrendan `gmail_gifola-hexine`; brendanfinancialt1 `gmail_cerris-heed`; brendanmconn96 | Composio | each | comms-ingest, crm-maintenance, chief-of-staff |
| composio:attio | account `attio_bosser-wooded` | Composio | Arrow | deal-indications, crm-maintenance |
| composio:googlesheets | run-log sheet, account `googlesheets_cush-citron` | Composio | Arrow | deal-indications |

## Composio

Composio is a single MCP endpoint that holds OAuth to many apps (Notion, Attio, QuickBooks). One connection, many tools, found with `COMPOSIO_SEARCH_TOOLS` and run with `COMPOSIO_MULTI_EXECUTE_TOOL` or `COMPOSIO_REMOTE_WORKBENCH`. The skills already use it this way. On the mini it's one MCP server entry in the runtime config, same as in Claude Desktop.

## Where these are configured

- Claude Desktop today: the connectors menu.
- Mac mini later: `services/mcp.json` (one entry per row above). Same servers, different host.
