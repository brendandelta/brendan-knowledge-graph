# Brendan · Chief of Staff

| Field | Value |
|---|---|
| **Name** | Brendan · Chief of Staff (also known as CoS Bot) |
| **Bot id** | `0104f7f8-4b0a-4a12-8779-c24513602227` |
| **Silo** | Cross |
| **Purpose** | Chief of Staff: weekday MIT coaching via morning session, midday check, EOD commit, and Monday weekly lock — without dumping lists. |
| **Status** | live (wire-watch routine paused) |
| **Repo skill path** | `skills/chief-of-staff/SKILL.md` (also uses `to-do-bot-operating-playbook`, `embed-action-links`) |
| **Data sources (read)** | Todoist; Gmail (brendan@arrowfund.co via Composio); Google Calendar (Arrow + personal); WhatsApp / Signal / iMessage MCPs, plus partner WhatsApp (`user-Chris' WhatsApp MCP`) only for threads that involve Brendan or an explicit ask of Brendan or Arrow; Attio via Composio (first source for deal inventory); Google Drive/Sheets; Carta (box browser); teammate bot transcripts |
| **Write targets** | Todoist (create / date / complete / priority); bot store `morning-brief/<YYYY-MM-DD>.md`; knowledge repo `skills/chief-of-staff/SKILL.md` + `INDEX.md` + `CHANGELOG.md`; Gmail sends only after Brendan's explicit approval of exact text |
| **Send permissions** | draft-only. Never sends email or messaging without Brendan's explicit approval of the exact text (DraftExternalMessage card, or chat draft + yes then Composio Gmail send). Never moves money, sends, deletes, or pays without explicit confirm. Saved Allow rules: Composio connector calls without asking; updating prompts of CRM enrichment (weekends), Midday accountability check, and Monday weekly priorities lock routines. |
| **Depends on** | Inbox Todo (Todoist feed); Call Cadence / Suggested Actions / Personal CRM Contact Context and Arrow bots' transcripts for morning roll-up; knowledge repo |
| **Feeds** | Brendan in CoS Bot chat (briefs, nudges, coaching) |
| **Connectors needed** | `user-composio` (Gmail, Todoist, Attio, Google Sheets/Drive/Calendar); `user-whatsapp-mcp`; `user-Chris' WhatsApp MCP` (limited); `user-Signal MCP`; `user-imessage`; `x`; `cursor-origin`; `cursor-github` |
| **Last verified** | 2026-11-06 · Grok Bot registry refresh (display name synced from live profile) |

## Routines

Every routine prompt starts with the standard "Knowledge repo (read first)" block. Operating detail lives in [`skills/chief-of-staff/SKILL.md`](../skills/chief-of-staff/SKILL.md). Temporary (2026-11-11, until Brendan says finished): morning, midday, and EOD checks carry the WhatsApp MCP upgrade + Attio People schema overhaul as a top accountability item and name the next step he owes. Those prompts also restate "no em dashes".

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Morning session (prep + sit-down nudges) | `hourly-priority-brief` | `10 9 * * 1-5`; `30 9 * * 1-5`; `50 9 * * 1-5`; `10 10 * * 1-5` | flexible, up to 5 min late | 9:10 builds morning brief (Todoist, Gmail, WhatsApp, Signal, iMessage, limited partner WhatsApp, calendars, teammate transcripts) into bot store `morning-brief/<date>.md` with `status: ready`; 9:30 / 9:50 / 10:10 send a short sit-down nudge if not yet delivered; until Brendan says finished, the brief includes the WhatsApp MCP + Attio schema standing item and the next step he owes; no em dashes | Brendan in CoS Bot chat (full brief only on "go") |
| Midday accountability check | `midday-accountability-check` | `20 12 * * 1-5` | default timing | Asks whether the morning MIT moved; forces finish / renegotiate / demote; at most one new fire; until Brendan says finished, also the WhatsApp MCP + Attio schema standing item and the next step he owes; no em dashes | CoS Bot chat |
| Monday weekly priorities lock | `monday-weekly-priorities-lock` | `0 8 * * 1` | flexible ±15 min | Proposes ≤3 weekly rocks, gets confirmation, reflects them in Todoist | CoS Bot chat |
| EOD commit | `eod-commit` | `20 17 * * 1-5` | default timing | What moved vs slipped; recommends tomorrow's MIT; syncs Todoist Today; quiet until morning; until Brendan says finished, also the WhatsApp MCP + Attio schema standing item and the next step he owes; no em dashes | CoS Bot chat |
| Watch banker for fund wire instructions | `watch-galo-for-space-fund-wire` | `0,30 9-17 * * 1-5` | flexible ±15 min; **PAUSED** | Watches Gmail + a Drive folder for fund wire instructions; alerts only when new | CoS Bot chat (only on find) |

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is skills/chief-of-staff/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan’s explicit confirm.

Your Chief of Staff: weekday morning day plan (Todoist, email, WhatsApp, calendar), MIT coaching, and follow-ups you owe — without dumping lists.

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Chat should feel like an interactive dashboard: never tell Brendan to "log into X", "check that email", or "open Attio/Notion/Carta" without a clickable markdown link in the same sentence (deep link when you have it — Gmail message, Attio/Notion record, exact product page). Label links by destination, not "click here". Never invent URLs.
```

## Rebuild steps

1. Create the bot named **CoS Bot · Chief of Staff** with the description above; bot id should match after recreate (record the new id in this file if it changes). Title: Chief of Staff.
2. Connect Composio (Gmail for brendan@arrowfund.co, Todoist, Attio, Google Calendar/Drive/Sheets), WhatsApp MCP, partner WhatsApp (`user-Chris' WhatsApp MCP`), Signal MCP, and iMessage MCP (OAuth / host setup; no secrets in this file). Box browser may need Carta sign-in for fund-admin checks.
3. Ensure the knowledge repo is cloned at `/home/box/Claude/brendan-knowledge-graph` and the bot store path for morning briefs exists: `/cursor/stores/store-a500b914-a256-414f-830b-15a9ae5bc294/morning-brief/`.
4. Create the five routines with the crons and folders above; prompts open with the Knowledge-repo block and load `skills/chief-of-staff/SKILL.md` / `to-do-bot-operating-playbook`. Leave the banker wire-watch **paused** unless Brendan reopens it.
5. Confirm Saved Allow rules: Composio connector calls; prompt updates for Midday accountability check, Monday weekly priorities lock, and CRM enrichment (weekends).
6. Run one manual morning prep (write a `status: ready` brief, send nothing); set **Last verified**.

## Arrow duties (Cross)

Tracks Arrow deal follow-ups and closes at a coaching level (MITs, countersigns, banker intros, LP invite status) without owning Arrow inventory bots. Mentions Arrow only generically here. For Arrow bots and Arrow-side operational detail see [`arrow/BOTS.md`](../arrow/BOTS.md). Never copy personal, Delta or Janey material into `arrow/`.
