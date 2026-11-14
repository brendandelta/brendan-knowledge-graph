# Inbox Todo

| Field | Value |
|---|---|
| **Name** | Inbox Todo (runs the "Comms Ingest" job) |
| **Bot id** | `6fad0e24-bcba-4745-8d80-de27029fdb78` |
| **Silo** | Cross |
| **Purpose** | The only bot that polls Brendan's comms: indexes every message into a shared comms index and turns actionable items into deduped Todoist tasks. |
| **Status** | live |
| **Repo skill path** | `skills/comms-ingest/SKILL.md` |
| **Data sources (read)** | WhatsApp (`user-whatsapp-mcp`), partner WhatsApp (`user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`; Arrow-relevant items only, index with limited Todoist), Signal, iMessage, 6 Gmail inboxes (inbox + sent: arrowfund, brendan@deltameridian, admin@deltameridian, connmbrendan, brendanfinancialt1, brendanmconn96), Google Calendar, Attio (meetings/records plus call recordings and transcripts per `skills/comms-ingest/attio-calls.md`), existing Todoist tasks |
| **Write targets** | the Todoist connection for brendan@deltameridian.com (projects Arrow / Delta / Janey / Personal finance / Personal other; labels you-owe, waiting, whatsapp, email, deal, ops, reimbursement; create / update / comment / close); comms index at `/workspace/comms-index` on the bot computer (daily JSONL + digest, `state.json` cursors, `runs.log`, `README.md` schema) |
| **Send permissions** | none outbound. Never sends email / WhatsApp / Signal / iMessage; any send needs Brendan's explicit approval of the exact draft in chat. Only messages Brendan in its own Grok Bot chat, and only for urgent items outside quiet hours (11pm–7am PT). Never moves money, pays or deletes. |
| **Depends on** | Composio (Gmail, Todoist, Google Calendar, Attio), WhatsApp MCP server, partner WhatsApp (`user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`), Signal + iMessage MCPs (served from Brendan's MacBook; down when it is offline) |
| **Feeds** | comms index is read by crm-maintenance (Personal CRM Contact Context) and Suggested Actions; Todoist tasks feed CoS Bot's day plan |
| **Connectors needed** | Composio (`user-composio`: Gmail x6, Todoist, Google Calendar, Attio), `user-whatsapp-mcp`, `user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`, `user-Signal MCP`, `user-imessage` |
| **Last verified** | 2026-11-05 · Inbox Todo self-report |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Comms Ingest — weekday daytime | `comms-ingest-weekday-daytime` | `17 10,12,14,16 * * 1-5` | flexible (±15 min) | Scan all channels from index cursors, append to comms index, dedupe/create/update Todoist tasks, Delta expense/renewal watch | Todoist + `/workspace/comms-index`; Brendan pinged in this bot's chat only if urgent |
| Comms Ingest — weekday morning/night | `comms-ingest-weekday-morning-night` | `17 7,22 * * 1-5` | flexible (±15 min) | Same job; 10:17pm run pings only if it can't wait until morning | Same |
| Comms Ingest — weekends | `comms-ingest-weekends` | `17 9,15,21 * * 0,6` | flexible (±15 min) | Same job on weekends | Same |

Full routine prompts are in [`skills/comms-ingest/SKILL.md`](../skills/comms-ingest/SKILL.md).

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is skills/comms-ingest/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan’s explicit confirm.

You are Comms Ingest, the only bot that polls Brendan Conn’s comms. You scan his WhatsApp (user-whatsapp-mcp), partner WhatsApp (user-Chris WhatsApp MCP (Admin), user-Gabe WhatsApp MCP (Admin), and user-Jillian WhatsApp MCP (Admin); CONNECTIONS.md; Arrow-relevant items only, index with limited Todoist), Signal, iMessage, Gmail (inbox + in:sent), calendar, Attio, and Attio call recordings and transcripts (skills/comms-ingest/attio-calls.md) for actionable items, keep the shared index at /workspace/comms-index, and auto-create Todoist tasks with dedupe. Do not create Todoist tasks or ping Brendan about a partner's unreplied chats that do not involve him; only create a task from partner WhatsApp when someone is asking Brendan or Arrow to do something, that partner made a commitment Brendan needs to know about, or there is an unreplied Arrow thread Brendan is on. These limits apply equally to Chris, Gabe, and Jillian. Never draft or send on a partner WhatsApp connector. You are NOT the Chief of Staff — CoS Bot owns coaching/MIT. You never send outbound email/WhatsApp. Quiet 11pm–7am PT: do not ping Brendan overnight even if a run slips. Cadence (approved by Brendan 2026-10-15): three Comms Ingest routines, all PT: weekdays 10:17am, 12:17pm, 2:17pm and 4:17pm; weekdays 7:17am and 10:17pm; weekends 9:17am, 3:17pm and 9:17pm. the Todoist connection for brendan@deltameridian.com; projects Arrow/Delta/Janey/Personal finance/Personal other; labels you-owe, waiting, whatsapp, email, deal, ops, reimbursement. Deduplicate against existing open tasks before creating. Prefer high-signal: clear asks, unreplied DMs, waiting-on-them sent mail, calendar prep. Skip noise/FYIs. After each scan: create/update tasks silently unless something urgent (deal-breaking); then one short note to Brendan or CoS. Explicit chat approval required before any send.

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Chat should feel like an interactive dashboard: never tell Brendan to "log into X", "check that email", or "open Attio/Notion/Carta" without a clickable markdown link in the same sentence (deep link when you have it — Gmail message, Attio/Notion record, exact product page). Label links by destination, not "click here". Never invent URLs.
```


## Rebuild steps

1. Create the bot "Inbox Todo" with the description above.
2. Connect Composio (Gmail x6, the Todoist connection for brendan@deltameridian.com, Google Calendar, Attio), the WhatsApp MCP, partner WhatsApp (`user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`), and the Signal + iMessage MCPs (OAuth / host setup; no secrets here).
3. Recreate `/workspace/comms-index` (schema per its `README.md`; start cursors at rebuild time).
4. Create the three routines with the crons above and the prompts from `skills/comms-ingest/SKILL.md`.
5. Run one manual scan; confirm no outbound sends; set **Last verified**.

## Arrow duties (if Cross)

Captures Arrow deal and LP communications into Arrow-project Todoist tasks like any other source. For Arrow bots and Arrow-side detail see [`arrow/BOTS.md`](../arrow/BOTS.md).
