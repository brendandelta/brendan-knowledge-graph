# Changelog

## 2026-10-19
- Repo created. Scaffolding, ARCHITECTURE, INDEX, profile, DECISIONS, RUNBOOK.
- Stubs for morning-brief, message-triage, prep-for-meeting, week-ahead.
- Placeholders for brendan-crm, delta-bookkeeper, arrow-fund-inbox (copy in from claude.ai).
- Real brendan-crm, delta-bookkeeper (with references/), arrow-fund-inbox copied in. Placeholders removed.
- CONNECTIONS.md added: the hands layer, Composio documented.

## 2026-10-20
- profile.md rewritten with Brendan: four contexts, org and money flow, email and systems tables, bank accounts, hard rules (propose-only money, sealed arrow/).
- Shorthand section removed.
- INDEX: planned skills money-queue, crm-sync-feed, accounting-and-tax.
- DECISIONS: nine entries from the 10/20 session.
- Open tasks moved to TASKS.md.
- CONSTITUTION.md added; CLAUDE.md points to it first.
- INDEX: four stubs removed; brendan-crm row notes Attio read-only; Ideas section replaces Known gaps.
- Grokbot export imported: 8 skills created from running prompts (chief-of-staff, comms-ingest, crm-maintenance, suggested-actions; arrow: deal-indications, private-markets-desk, arrow-weekly, daily-arrow-report). Export kept verbatim in references/.
- INDEX rewritten: live/draft/planned with engine; seams list; build order.
- profile.md: two standing send exceptions recorded.
- CONNECTIONS: Composio connection ids for Gmail x6, Attio, Todoist, Sheets.
- Send exceptions removed from profile.md and the two skills; confidence tags removed from private-markets-desk.
- comms-ingest: schema tightening notes. TASKS: recording capture project, CRM Spec pointer.
- ARCHITECTURE build order: comms index is step 1 on the mini.
- CONNECTIONS rewritten from live Composio state: 23 active connections, 3 custom servers, to-connect list, money-movement design note, 1Password decided.
- projects/ added with five project files and README; INDEX Projects table; TASKS lines now point at projects.
- Sweep: profile.md now says 1Password. Audit gains decision-consistency and project checks.

## 2026-10-21
- brendan-crm v4: repo is the spec; Grokbot → crm-maintenance; Attio ask-before-add and sectors in mirror; send-from arrowfund; Source "Agent"; wipe moved to TASKS.
- projects/health-insurance.md added; INDEX project row.
- TASKS: Attio wipe, Grokbot list import.

## 2026-10-27
- brendan-crm v4.1: Janey relevance field, Janey/Delta circles.

## 2026-10-29
- Renamed to brendan-knowledge-graph. Folder: ~/Claude/brendan-knowledge-graph. Remote: github.com/brendandelta/brendan-knowledge-graph.

## 2026-10-29
- Bots synced their own skill files to how they run today: arrow-weekly, daily-arrow-report, deal-indications, private-markets-desk, comms-ingest, crm-maintenance, suggested-actions.
- deal-indications: "added into Attio" Signal acks to the 3 internal chats kept (supersedes 10/20 note); step 7 PERSON LINK, blocks and interests always linked to the sender's Attio person (interests via intermediary).
- crm-maintenance and call-cadence point to skills/brendan-crm; Notion Delta CRM Spec deleted (repo is source of truth).
- New skills: call-cadence (v1.1), morning-buy-sell-sheet, nport-scraper (v1). INDEX rows added.
- DECISIONS: 10/29 send exceptions, sync-and-flag, follow-up split. INDEX seam 5 superseded.

## 2026-10-30
- CLAUDE.md renamed to AGENTS.md (no duplicate kept). Every bot reads AGENTS.md first.
- CLAUDE.md re-added as a one-line pointer to AGENTS.md, so Claude Code still auto-loads it.
- chief-of-staff synced to live CoS Bot (Brendan approved all 9 mismatches): morning prep 9:10 + nudges, brief order, standing #1 Carta/Hubble + open items, wider sources, approval rule for all messages and no calendar writes, embed-action-links, temporary Galo wire watch, substance-over-systems + Attio-first inventory, interrupt only for true fires. INDEX row updated.

## 2026-10-31
- nport-scraper v1.1 synced to live N-PORT Scraper (Brendan approved): routine "N-PORT top-100 queue worker" renamed "N-PORT monthly refresh", 1st of each month ~8am PT ±30 min (CRON_TZ=America/Los_Angeles 0 8 1 * *); all 103 top-100 companies scraped and enriched; monthly run re-ingests newer DERA N-PORT sets (2026q3 latest), rescans Form D, adds and scores new holders' PMs/advisers/signers, read-only Attio check, edits tracker Page in place, digest only if something changed. Rules unchanged. INDEX row updated.

## 2026-11-02
- Janey document-filing noted as planned, not built: projects/drive-restructure.md (Decisions + Log), INDEX document-filing row, CONNECTIONS Janey Drive row. Same pattern as Arrow/Delta: shared procedure skill + thin per-drive wrapper + README in the Janey Drive.
