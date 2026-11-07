---
name: nport-scraper
description: N-PORT Scraper bot (Arrow Fund). Builds a database of who holds and marks the top-100 private companies, using SEC N-PORT fund holdings and Form D SPVs, then maps and ranks the people for Brendan's secondaries outreach.
---

# N-PORT Scraper

## Architecture (approved by Brendan 2026-10-29)
- Raw data lives in a SQLite database on the bot computer at /workspace/nport/nport.db. It holds filings, fund holdings, SPVs, related persons, people, scores and Attio matches. Scripts are in /workspace/nport/.
- People go to Attio (the Arrow Fund workspace, through the Composio connector), but only after Brendan explicitly approves. Each person is tagged by company and role. Brendan sends every outreach message himself.
- The readable view is the Page "N-PORT Private Marks Tracker", which is edited in place and never recreated.
- Notion is not used as the store.

## Sources
- SEC quarterly N-PORT bulk data sets, downloaded with a User-Agent header and parsed locally by bulk_ingest.py. Company match patterns are in companies.json.
- Form D SPVs from EDGAR full-text search: issuer, related persons (GP, managers, directors) and sales-compensation recipients (brokers). Clean collisions with formd_clean.py.
- Portfolio managers come from prospectuses, 497K, N-CSR and adviser filings.

## People groups
decision-maker (PMs, SPV GPs) / holder-controller (fund and SPV entities, officers, signers) / broker (placement agents, sales compensation).
Score = position size + fund age + live Arrow deal in name + existing Attio relationship.

## Run loop
Routine "N-PORT monthly refresh" (renamed from "N-PORT top-100 queue worker", approved by Brendan 2026-10-31): runs on the 1st of each month at about 8am PT, flexible by ±30 minutes. Cron: CRON_TZ=America/Los_Angeles 0 8 1 * *.
The top-100 queue is done: all 103 companies on the list are fully scraped and enriched.
Each run:
- checks for a newer SEC DERA quarterly N-PORT bulk set (2026q3 is the latest loaded) and re-ingests it;
- rescans Form D for new SPVs;
- adds PMs, advisers and signers for new holders and scores them;
- re-checks Attio read-only;
- edits the N-PORT Private Marks Tracker Page in place;
- sends Brendan a digest of new SPVs, changed positions and live-deal overlaps, or sends nothing if nothing changed.
The destination for the dataset is Attio (a cap-table-companies view on the firms tracker, updated after each monthly refresh). That write path is being built; until Brendan approves it live, do not write to Attio and do not email the workbook to Vaidehi.

## Rules
Never write to Attio without Brendan's explicit confirm. Never send messages or contact anyone without Brendan's explicit confirm. Never fabricate numbers; every row links to its SEC filing.
