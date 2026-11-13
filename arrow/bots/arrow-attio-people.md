# Arrow Attio People

| Field | Value |
|---|---|
| **Name** | Arrow Attio People |
| **Bot id** | 994d8e46-47dd-4600-9038-81749023d856 |
| **Silo** | Arrow |
| **Purpose** | Keeps Arrow Fund's Attio People records current (Arrow context, relationship links, investor fields, empty contact fields, last-contact rollup for email and WhatsApp) from Arrow comms. People only; Companies belong to Arrow Attio Firms. |
| **Status** | live since 2026-11-05 (took over the Attio People work from Contact Context, now Personal CRM Contact Context) |
| **Repo skill path** | `arrow/skills/attio-contact-context/SKILL.md` |
| **Data sources (read)** | comms index; Brendan's Gmail inboxes incl. Arrow; Google Calendar; WhatsApp (Brendan's `user-whatsapp-mcp` and partner WhatsApp, currently `user-Chris' WhatsApp MCP`; Arrow-relevant threads only), Signal (incl. Arrow team groups) and iMessage; Attio People, blocks/interests, lists; Attio call recordings and transcripts on every event run (`skills/comms-ingest/attio-calls.md`; cursor in /workspace/attio-context/state.json); public professional web pages. Never Notion. Arrow Weekly Log may be read. |
| **Write targets** | Attio (Arrow Fund workspace) People only, owned fields: `arrow_context`, `introduced_by` (single; inverse `introduced`), `knows`, `close_with`, `contact_owner`, `context_updated`, `check_size_range`, `sector_of_interest`, `lp_fo_ria` (+ FO tag), empty contact fields, and last-contact fields (`last_whatsapp_contact`, `last_whatsapp_contact_by`, `last_contacted`, `last_contacted_by`, `last_contact_channel`, `days_since_last_contact`); human-edit guard always on (last-contact fields are computed and always refreshed). Box files: /workspace/attio-context/state.json (event cursor plus `attio_calls_cursor`, `attio_calls_seen`, `attio_calls_pending_transcripts`), relationship-provenance.jsonl; /workspace/attio-backfill/ (queue.json, relationship_checked.json, log.md, lock, pending-questions.md). Its own skill file in this repo. |
| **Send permissions** | None to anyone but Brendan. Messages Brendan only in its own Grok Bot chat, only when notable. Never sends, drafts or replies on email, Slack, WhatsApp, Signal or iMessage. Never moves money, deletes or pays. |
| **Depends on** | comms-ingest (comms index); Composio Attio connection; WhatsApp, partner WhatsApp (`user-Chris' WhatsApp MCP`), Signal and iMessage connectors; Gmail/Calendar via Composio |
| **Feeds** | Attio People, which Daily Arrow Report, Arrow Weekly, Arrow Log and Marwan Deal Board read |
| **Connectors needed** | Composio (Attio, Gmail, Google Calendar); `user-whatsapp-mcp`; `user-Chris' WhatsApp MCP`; Signal MCP; iMessage MCP. `ATTIO_CONTEXT_TOKEN` env var optional (preferred when present; not set on this bot as of 2026-11-05, Composio used instead). |
| **Last verified** | 2026-11-05 · Grok Bot registry refresh (description synced from live) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Attio contact context | attio-contact-context | `30 8,12,15 * * 1-5` and `30 8,17 * * 0,6` | flexible ±15 min | One event run of the skill: required Attio calls step (`skills/comms-ingest/attio-calls.md`), update in-scope People touched since the cursor, run the last-contact pass (WhatsApp outbound + email rollup; full days-since refresh on the first PT run of the day), advance the cursors. Live routine still needs the last-contact paragraph and the Attio calls paragraph from the skill. | Brendan in this bot's chat, only when notable (people worth adding, conflicts, blocked source) |
| Attio backfill | attio-backfill | `15,45 7-21 * * *` | flexible ±10 min | One 25-person batch of Backfill mode (tiers A–C, D skipped), plus A–C relationship checks; temporary until A–C done. | Brendan in this bot's chat, only when a tier finishes, a failure, or a conflict |
| Caplight recap watch | (temporary) | weekdays hourly 9 AM to 6 PM PT, until the end of Tue Nov 17, 2026 | flexible | Wait for Hank Caulkins's (hank@caplight.com) recap email of the Nov 13 Caplight call, add it as an Attio note on Hank (f94edb9b-4d56-453f-9e8b-e81c9e3d81aa) and Caplight (85b9eb2b-7efb-4465-8b4d-3f353bb1885c), then delete this routine. | Brendan in this bot's chat, only when the note is written or the watch ends |

Full routine prompts are kept verbatim in the skill's "Live routine prompts" section.

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/attio-contact-context/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

Arrow Attio People. Keeps Arrow Fund's Attio People records current from Arrow comms (comms index, Arrow Gmail, WhatsApp including partner WhatsApp currently Chris's, Signal/iMessage Arrow threads, calendar, and Attio call recordings and transcripts on every event run per skills/comms-ingest/attio-calls.md) under arrow/skills/attio-contact-context. Attio People only, owned fields only, human-edit guard always on. Never Notion, never Companies (that is Arrow Attio Firms), never personal or Delta material. Inherits the existing Attio contact-context and Attio backfill routines, /workspace/attio-context/state.json cursor, /workspace/attio-backfill/ queue and ATTIO_CONTEXT_TOKEN from Contact Context. Results go to Brendan in this chat only when notable. Never sends anything to anyone else. The Arrow Weekly Log is a general shared source any Arrow bot may read; this bot does not write it (Arrow Log does).

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Never tell Brendan to open something without a clickable markdown deep link in the same sentence. Never invent URLs.
```

## Rebuild steps

1. Create a Grok Bot named "Arrow Attio People" with the description above.
2. Connect Composio (Attio Arrow Fund workspace, Gmail, Google Calendar), WhatsApp, partner WhatsApp (`user-Chris' WhatsApp MCP`), Signal and iMessage connectors. Optionally set `ATTIO_CONTEXT_TOKEN`.
3. Clone this repo to /home/box/Claude/brendan-knowledge-graph.
4. Restore or recreate /workspace/attio-context/state.json (cursor) and /workspace/attio-backfill/ (queue.json etc.); if lost, set the cursor to the rebuild time and ask Brendan before rebuilding the backfill queue.
5. Recreate the two recurring routines from the skill's "Live routine prompts" with the crons and timing above.
6. Do one manual event run and verify writes by reading records back.
