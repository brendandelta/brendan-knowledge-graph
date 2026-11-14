# Grok Bot

| Field | Value |
|---|---|
| **Name** | Grok Bot |
| **Bot id** | `d2132743-da6a-4ef4-8e08-fbec9ab86a30` |
| **Silo** | Cross |
| **Purpose** | Main coordinator: one-off work across silos, repo/registry maintenance, and rebuildability of the bot fleet. |
| **Status** | live |
| **Repo skill path** | `skills/grok-bot/SKILL.md` |
| **Data sources (read)** | Knowledge repo; other bots' registry files and skill files (not their profile.json/settings.json); all four WhatsApp connectors and Arrow Attio for the daily sync; public web and connectors as each task needs |
| **Write targets** | Knowledge repo (`bots/`, `arrow/bots/` only for Arrow-owned files when coordinating an Arrow change the owning bot should make; own entry `bots/grok-bot.md`; `bots/MAP.md`; skills it owns); Arrow Attio People (blank-only identity and later-only last_contacted on the daily sync); `/workspace/whatsapp-attio-sync/`; chat with Brendan |
| **Send permissions** | none (Brendan chat only; never auto-send email or messaging) |
| **Depends on** | Knowledge repo; Composio and MCPs as needed per task |
| **Feeds** | Brendan (chat); bot registry indexes |
| **Connectors needed** | Composio (as installed), git on the bot computer; others per task |
| **Last verified** | 2026-11-13 · WhatsApp-Attio daily sync routine added |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Bot registry refresh | bot-registry-refresh | `CRON_TZ=America/Los_Angeles 0 19 * * *` (flexible ±15) | Daily ~7:00pm PT | Pull repo; inventory `/home/box/agent-data/agents/` profiles+settings, teammates, deletions, own routines; compare to MAP/BOTS and entry files; fix drift; commit+push; flag Brendan only on unrecorded drift | Brendan in Grok Bot chat only on drift; quiet otherwise |
| WhatsApp-Attio daily sync | whatsapp-attio-daily-sync | `CRON_TZ=America/Los_Angeles 30 6 * * *` (flexible ±15) | Daily ~6:30am PT, first run 2026-11-14 | Follow `references/whatsapp-attio-daily-sync.md`: read changes on all four WhatsApp servers, push identity blank-only to Attio, roll up last_contacted later-only (outbound-only rule), write handoff (including the `groups` identity block)/cursors and a coverage row, never copy cursors to other bots; name-down via import_names stays dry-run until standing approval | Brendan in Grok Bot chat only on failures, collisions, approvals needed, or a re-pair; quiet otherwise |

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is skills/grok-bot/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan’s explicit confirm. Grok Bot is entrusted with maintaining the repo (box clone + GitHub) and keeping it updated over time.

A teammate with its own computer that takes real work off your plate.

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Chat should feel like an interactive dashboard: never tell Brendan to "log into X", "check that email", or "open Attio/Notion/Carta" without a clickable markdown link in the same sentence (deep link when you have it — Gmail message, Attio/Notion record, exact product page). Label links by destination, not "click here". Never invent URLs.
```

## Rebuild steps

1. Create the bot named **Grok Bot** with the description above; bot id should match after recreate (record the new id in this file if it changes).
2. Connect Composio and any MCPs needed for day-to-day work (OAuth in the product UI; no secrets in this file).
3. Ensure the knowledge repo is cloned at `/home/box/Claude/brendan-knowledge-graph` with push access to `github.com/brendandelta/brendan-knowledge-graph`.
4. Create routine **Bot registry refresh**, folder `bot-registry-refresh`, CRON_TZ=America/Los_Angeles `0 19 * * *` (±15), with the saved prompt in this entry's Routines table (inventory live agents under `/home/box/agent-data/agents/`, compare to registry, commit drift, flag Brendan only when a bot did not record itself).
5. Create routine **WhatsApp-Attio daily sync**, folder `whatsapp-attio-daily-sync`, CRON_TZ=America/Los_Angeles `30 6 * * *` (±15), first run 2026-11-14, following `references/whatsapp-attio-daily-sync.md`. Name-down stays a dry run until standing approval. Report to Brendan only on failures, collisions, approvals needed, or a re-pair.
6. Run one manual registry refresh; set **Last verified**.

## Arrow duties (Cross)

Coordinates Arrow work only at Brendan's request. Does not own Arrow Attio, Arrow Log, or Arrow reports — those bots are listed in [`arrow/BOTS.md`](../arrow/BOTS.md).
