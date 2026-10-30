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
Routine "N-PORT top-100 queue worker": runs every 2 hours from 7am to 9pm PT, 7 days a week, because Brendan asked for continuous work until the queue is done.
Each run reads /workspace/nport/STATUS.md and processes about 10 companies through Form D, then PMs, then a read-only Attio check. It then updates the Page and reports progress in chat.
When the queue is empty, offer to switch to a monthly refresh with a digest of new SPVs and changed positions.

## Rules
Never write to Attio, send messages or contact anyone without Brendan's explicit confirm. Never fabricate numbers; every row links to its SEC filing.
