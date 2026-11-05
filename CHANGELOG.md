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
- Delta Drive restructured and documented (Brendan's decisions). Delta Meridian Holdings is a Delaware LLC taxed as an S-corp: "California LLC" corrected in skills/delta-bookkeeper (SKILL.md, books-manual sections 1, 4 and 8, which now say Delaware LLC operating in California where CA tax is discussed). Books-manual rule 9: every receipt is also filed in the Delta Drive. Delta Drive folders renumbered 00–06 + 99 - Inbox, new Contracts / Compliance / Tax / Payroll / Bank Statements folders, GridLync consolidated under 06 GridLync, 12 files renamed to YYYY-MM-DD — {Vendor/Entity} — {Doc type} [Status].ext. Delta README v1 created as a Google Doc (1r9_MWLdYSFnxV7z-5gj9TbclfQDGUcnzgAg_f6pWxic); Arrow README amendment v7 appended (folder IDs and confirmed filing rules moved out of the skill).
- Filing skills split into procedure + per-drive wrapper: new arrow/skills/document-filing (generic shared procedure; in arrow/ because teammates' agents load only arrow/skills/, and it carries nothing Delta or personal); arrow/skills/arrow-fund-inbox rewritten as a thin wrapper (account, IDs, Arrow mechanics; rules now live in the README); new skills/delta-meridian-inbox wrapper (Delta inbox, archive, README ID). Scheduled weekdays 12pm PT, clear fits only, questions to chat (replaces "runs only when asked"). INDEX rows updated (document-filing moved to Shared). projects/drive-restructure.md decisions and log updated.
- CONNECTIONS: box git push access to brendan-knowledge-graph (fine-grained, repo-only token in the box's git credential helper; the token is never written down); delta-meridian-inbox and README edits added to Used by.

## 2026-11-03
- New skill: arrow/skills/attio-context, now arrow/skills/attio-contact-context (draft v1, Brendan approved). Plain business context and relationship links on every Attio person (arrow_context, introduced_by, knows, close_with, contact_owner, context_updated), strict silos, human-edit guard, event-driven run with backfill, 5 weekday/weekend times. New Attio attribute People `close_with` ↔ Companies `close_contacts`. Pilot on the four J.P. Morgan Private Bank contacts. INDEX Shared row added.
- Carve-outs: skills/brendan-crm (v4.2, after the privacy rule + changelog) and skills/crm-maintenance (migration notes only; live prompts not yet changed) must not write the attio-contact-context fields.
- DECISIONS: 2026-11-03 Attio context process.

## 2026-11-04
- Silo (Brendan approved): the Notion CRM and the Attio process are now two separate processes. Inputs shared (same comms sources), outputs siloed.
- skills/brendan-crm v4.3: Notion only, Attio read-only. Removed mirror_fields, mirror rule, Attio list membership and contact fill; workflow 1 step 6 is a read-only lookup that only sets Notion `Attio record ID`.
- skills/crm-maintenance: Rule B (never writes Attio), section 7 "Attio: hands off", read-only Attio setup, enrichment never touches Attio and never overwrites Company, CONFLICT-only run log line, Michael Tucker Attio ID open item removed (fixed in Notion), weekend prompt writes Notion only. Synced to the live prompts verbatim.
- arrow/skills/attio-context renamed to arrow/skills/attio-contact-context, v2 (live): never reads or writes Notion; only Attio People writer; owns investor fields (check_size_range, sector_of_interest, lp_fo_ria + FO tag) with the GP/LP+FO/Intermediary mapping ("Broker" never used) and empty contact fields; explicit Sources section; Brendan-only scope rule; Backfill mode (queue /workspace/attio-backfill/queue.json: A 322, B 1045, C 1482, D 1544 skipped; 25 per batch; lock); cursor/log moved to /workspace; contact_owner never on teammates' records; ATTIO_CONTEXT_TOKEN preferred. New routine prompt "Attio backfill".
- INDEX: brendan-crm, crm-maintenance, attio-contact-context rows updated; crm-sync-feed marked superseded.
- DECISIONS: 2026-11-04 silo entry.
- arrow/skills/attio-contact-context v2.1: check_size_range options renamed $0-100K / $100-200K / $200-500K (in place in Attio); sector_of_interest options added (Secondaries, Climate, Healthcare, Media, Energy, Private Credit) and mapping updated.
- arrow/skills/attio-contact-context v2.2: sector_of_interest expanded to 60 structured options in Attio (37 added; none renamed or archived), mapping guidance extended.
