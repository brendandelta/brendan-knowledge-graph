# Attio Cleanup

| Field | Value |
|---|---|
| **Name** | Attio Cleanup |
| **Bot id** | 14d5e03f-0c76-4512-8a57-131ffde3bcf1 |
| **Silo** | Arrow |
| **Purpose** | Weekly junk cleanup of Arrow Fund Attio People: auto-deletes obvious machine-sender and service-desk records after a live protection re-check and a snapshot, sends borderline records to Brendan, never touches the keep list, and flags repeat senders to block in Attio email sync. |
| **Status** | live (created 2026-11-13; first approved batch run 2026-11-13) |
| **Repo skill path** | `arrow/skills/attio-people-cleanup/SKILL.md` |
| **Data sources (read)** | Attio (Arrow Fund workspace) People, Companies, Blocks, Interests, Deals, lists, notes, tasks, workspace members; Arrow Gmail (read-only, only for approved name lookups); public web (approved name lookups only). |
| **Write targets** | Attio People: deletes JUNK_AUTO records only (max 50 per run, live re-check first); deletes BORDERLINE records and renames (`name` attribute only) only when Brendan approves each one. Box files under `/workspace/attio-cleanup/`: `snapshots/<record_id>.json` (written before every delete), `deleted-YYYY-MM-DD.csv`, `renamed-YYYY-MM-DD.csv`, `keep-list.json` (Brendan's do-not-touch list), `repeat-senders.json`, `classify.py`. Its own skill file in this repo. |
| **Send permissions** | None to anyone but Brendan. Reports to Brendan in this bot's Grok Bot chat only. Never emails, messages, or changes Attio settings; recommends email-sync blocks for Brendan to apply. Never moves money or pays. |
| **Depends on** | Composio Attio connection; Arrow Gmail via Composio (read-only) |
| **Feeds** | Cleaner Attio People for Arrow Attio People, Arrow Attio Firms, Deal Indications, and the Arrow reports |
| **Connectors needed** | Arrow Attio via Composio; Arrow Gmail via Composio (read-only) |
| **Last verified** | 2026-11-13 · Grok Bot (registry entry created with the skill) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Weekly Attio People cleanup | `weekly-attio-people-cleanup` | `0 18 * * 0` | Sundays ~6pm PT, flexible ±15, first run 2026-11-15 | One run of `arrow/skills/attio-people-cleanup`: read all People and protections, skip the keep list, classify, apply the 50 cap and 3x volume guard, snapshot then delete JUNK_AUTO one at a time with a live re-check, send new BORDERLINE records to Brendan, update repeat senders, add a run-history row. | Brendan in this bot's chat (counts, skips, borderline for approval, repeat senders to block) |

## Full description (verbatim, secrets stripped)

```
(not yet synced from the live bot; the daily "Bot registry refresh" will paste it here)
```

## Rebuild steps

1. Create the bot with the description above.
2. Connect Arrow Attio and Arrow Gmail via Composio (OAuth; no secrets in this file).
3. Create the routine with the cron and prompt above.
4. Copy `keep-list.json` and `classify.py` into `/workspace/attio-cleanup/` (or rebuild the classifier from the skill), run one dry run, and set **Last verified**.
