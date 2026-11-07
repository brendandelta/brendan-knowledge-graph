# N-PORT Scraper

| Field | Value |
|---|---|
| **Name** | N-PORT Scraper |
| **Bot id** | 2bfe373c-e261-4439-a322-12d8087354c8 |
| **Silo** | Arrow |
| **Purpose** | Builds a database of who holds and marks the top-100 private companies from SEC N-PORT fund holdings and Form D SPVs, then maps and ranks the people for Brendan's secondaries outreach. |
| **Status** | live |
| **Repo skill path** | `arrow/skills/nport-scraper/SKILL.md` |
| **Data sources (read)** | SEC EDGAR N-PORT bulk data sets (`FUND_REPORTED_HOLDING`); EDGAR full-text Form D search; prospectus, 497K, N-CSR and adviser filings for PM names; Attio (Arrow Fund workspace) read-only via Composio; WhatsApp read-only for relationship checks. |
| **Write targets** | SQLite `/workspace/nport/nport.db` plus `STATUS.md`, scripts and `review/` under `/workspace/nport`; Page [N-PORT Private Marks Tracker](https://cursor.com/canvas/store-ab1d6be2-aec4-4a0f-b8c1-e71bada4fb11/957c6fb4-ea1a-4c78-891d-0575274e5bdf) (edited in place); Page [OpenAI Holder & Relationship Report](https://cursor.com/canvas/store-ab1d6be2-aec4-4a0f-b8c1-e71bada4fb11/eea29d8d-8a19-434f-956f-48f0e57b265d); workbook and PDF reports in `/workspace/nport/reports` (full all-company export via `export_all_workbook.py`). Attio writes only after Brendan explicitly approves. |
| **Send permissions** | Gmail drafts only: after each monthly refresh that has new data, create a draft (never send) from `brendan@arrowfund.co` to `vaidehi@arrowfund.co` attaching the full all-company holder workbook. Brendan sends it himself. No other sends, posts, or contacts. Never move money, send, delete, or pay without Brendan's explicit confirm. |
| **Depends on** | SEC EDGAR (DERA N-PORT bulk + Form D); Attio (read); WhatsApp (read) |
| **Feeds** | N-PORT Private Marks Tracker Page; digests to Brendan in this bot's Grok Bot chat; Gmail drafts to Vaidehi after refreshes with new data; marks/holders signal into Arrow Attio Firms (people staged to Attio only after Brendan approves) |
| **Connectors needed** | `user-composio` (Attio; Gmail as `brendan@arrowfund.co`). `user-whatsapp-mcp` (read-only). User skills: `private-holder-scraper`, `getting-started`. |
| **Last verified** | 2026-11-06 · N-PORT Scraper (self-report) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| N-PORT monthly refresh | n-port-top-100-queue-worker | `CRON_TZ=America/Los_Angeles 0 8 1 * *` | 1st of each month ~8am PT, flexible ±30 min | Check for newer SEC DERA quarterly N-PORT bulk set and re-ingest; rescan EDGAR Form D for new SPVs; add PMs/advisers/signers for new holders and score them; re-check Attio read-only; edit Tracker Page in place; if data changed, rebuild the all-company workbook and draft an email to vaidehi@arrowfund.co | Digest to Brendan in this bot's Grok Bot chat, including the Gmail draft link when a Vaidehi draft was created; silent when nothing changed |

Replaced the every-2-hours "N-PORT top-100 queue worker" on 2026-10-31 after all 103 companies on the list were fully scraped and enriched.

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/nport-scraper/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan’s explicit confirm.
```

Title in live profile: (empty).

## Rebuild steps

1. Create the bot named "N-PORT Scraper" with bot id `2bfe373c-e261-4439-a322-12d8087354c8` (or a new id on rebuild) and the description above.
2. Connect Composio (Attio, Arrow Fund workspace) and WhatsApp MCP read-only via OAuth; no secrets in this file.
3. Install / load user skills `private-holder-scraper` and `getting-started`; keep `arrow/skills/nport-scraper/SKILL.md` as the operating playbook (architecture, sources, people groups, run loop, rules).
4. Recreate `/workspace/nport/` (SQLite `nport.db`, scripts, `STATUS.md`, `review/`, `reports/`) from backup or by re-running the ingest pipeline against SEC DERA and EDGAR Form D.
5. Create the routine "N-PORT monthly refresh" in folder `n-port-top-100-queue-worker` with cron `CRON_TZ=America/Los_Angeles 0 8 1 * *` (±30 min) and the prompt summary above.
6. Point Pages at the Tracker and OpenAI report canvases (or recreate them) and run one manual refresh; set **Last verified**.
