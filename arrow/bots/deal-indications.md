# Deal Indications Bot

| Field | Value |
|---|---|
| **Name** | Deal Indications Bot |
| **Bot id** | 92202095-7fce-4c82-8536-767ec662836d |
| **Silo** | Arrow |
| **Purpose** | Several times daily, read Signal, WhatsApp (Brendan's and partner WhatsApp, currently Chris's), Arrow Gmail, and Attio call recordings and transcripts; create or update Attio blocks (supply) and interests (demand) with person links and source; log each item to a run sheet; message Brendan only when something was added, updated or needs review. |
| **Status** | live |
| **Repo skill path** | `arrow/skills/deal-indications/SKILL.md` |
| **Data sources (read)** | Signal MCP (all chats; works only via Brendan's Mac, intermittent); `user-whatsapp-mcp` (all chats; cannot read images or audio); partner WhatsApp (`user-Chris' WhatsApp MCP`; Arrow-relevant chats only); Gmail brendan@arrowfund.co via Composio (read-only); Attio call recordings and transcripts (`skills/comms-ingest/attio-calls.md`). Cursors in `/workspace/attio-sync/state.json` (include `whatsapp_chris` and `attio_calls_cursor`). Skips iMessage. Does not use the comms index. |
| **Write targets** | Attio (Arrow Fund workspace) blocks, interests, companies and people via Composio; append-only Google Sheet run log `1L5_uGlWeTZca6HTMofMGzYHNssq8DxCDfxecqweAeeY`; `/workspace/attio-sync/state.json`. Own skill file in this repo. |
| **Send permissions** | Standing permission: send exactly `added into Attio` via Signal `draft_message` then `send_message`, only in the internal Signal chats Arrow OS Data Input (`04d20918`), Arrow Fund Team (`4e28c83d`) and ARROW FUND - Vaidehi Chat (`90b825e4`), after adding or updating from a message in that chat, at most once per chat per run. Never send anywhere else. Everything else is drafts only (get Brendan's approval first). Never move money, send, delete or pay without Brendan's explicit confirm. |
| **Depends on** | Composio (Attio, Gmail, Google Sheets); Signal MCP; `user-whatsapp-mcp`; `user-Chris' WhatsApp MCP` |
| **Feeds** | Attio blocks and interests (read by Daily Arrow Report, Arrow Weekly, Marwan Deal Board, Arrow Log); run-log Google Sheet |
| **Connectors needed** | user-composio (Attio, Gmail, Google Sheets); user-Signal MCP; user-whatsapp-mcp; user-Chris' WhatsApp MCP |
| **Last verified** | 2026-11-05 · Deal Indications Bot (self-report) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Blocks & interests to Attio (weekdays) | hourly-blocks-interests-to-attio | `47 7,10,12,14,16,22 * * 1-5` | flexible (default) | Read Signal, Brendan's and partner WhatsApp (currently Chris's), Gmail, and Attio call recordings since cursors; create/update Attio blocks and interests (write path + person links); append run-log rows; ack "added into Attio" in the three internal Signal chats only. | Brendan in the Deal Indications Bot chat, only when something was added, updated or needs review; silent otherwise |
| Blocks & interests to Attio (weekends) | blocks-interests-to-attio-weekends | `47 9,15,21 * * 0,6` | flexible (default) | Same job as the weekdays routine. | Brendan in the Deal Indications Bot chat, only when something was added, updated or needs review; silent otherwise |

Full routine prompts are kept verbatim in `arrow/skills/deal-indications/SKILL.md`.

### Notable rules (also in skill / live ops)

- Create interests even when terms, size or buyer are unknown; skip only when the company is unknown (flag needs-review).
- When Chris posts a company list under a person or firm header without saying that party has or wants the names, do not ingest and do not tie them to that person — flag needs-review. (2026-11-04 rule; on 2026-11-05 Brendan confirmed the Nora / Shan Chopra list was Nora supply and had it recreated as blocks.)
- Every user-facing reply needs clickable markdown deep links (`embed-action-links`). Never invent URLs. There is no valid Signal deep link — name the chat and time instead.
- Blocks: link sender in `intermediaries_2` and `intermediary_contact_link`; set `deal_classification` Primary / Secondary / Unconfirmed when known. Interests: link sender in `intermediary`; end investor in `associated_contacts` when named. Never overwrite an existing person link.

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/deal-indications/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan’s explicit confirm.

Deal Indications Bot. ## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Never tell Brendan to open something without a clickable markdown deep link in the same sentence. Never invent URLs.
```

## Rebuild steps

1. Create a Grok Bot named "Deal Indications Bot" with the description above.
2. Connect Composio (Attio Arrow Fund workspace, Gmail for brendan@arrowfund.co, Google Sheets), Signal MCP, `user-whatsapp-mcp` and `user-Chris' WhatsApp MCP` via OAuth; no secrets in this file.
3. Clone this repo to `/home/box/Claude/brendan-knowledge-graph` and keep `arrow/skills/deal-indications/SKILL.md` in sync with the live prompt and routines.
4. Restore or recreate `/workspace/attio-sync/state.json` (per-source cursors). If lost, set cursors to the rebuild time and ask Brendan before backfilling.
5. Recreate both routines from the skill's weekday and weekend prompts with the crons and timing above.
6. Confirm the run-log Google Sheet is reachable append-only; do one manual run and verify creates/updates by reading Attio records back; set **Last verified**.
