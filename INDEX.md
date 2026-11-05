# Skill index

Every skill, what it reads, what it writes, what depends on it. If a skill isn't here it doesn't exist. Update its row when you change the skill. New rows appear only when a workflow has come up twice (CONSTITUTION.md).

Status key: **live** = running today (engine in parentheses) · **draft** = written, never run · **planned** = workflow has come up, not written.

## Personal (`skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| brendan-crm | composio:notion, composio:attio (read only), gcal, imessage, signal | composio:notion only (never writes Attio; 2026-11-04 silo) | profile.md | live (Claude), v4.3 |
| delta-bookkeeper | composio:quickbooks, Delta Intake | composio:quickbooks | profile.md | live (Claude); becomes accounting-and-tax |
| chief-of-staff | todoist, gmail (arrow), whatsapp, signal, imessage, gcal x2, attio, other bots' transcripts | todoist, chat | profile.md | live (Grokbot) |
| comms-ingest | signal, imessage, whatsapp, 6 gmails, gcal, attio | comms index, todoist | — | live (Grokbot); migrate. Infrastructure: three skills read its index. |
| crm-maintenance | comms index, direct thread reads (whatsapp, signal, imessage, 5 gmails, gcal), attio (read only, incl. calls) | composio:notion only (append only); never writes Attio | comms-ingest, brendan-crm | live (Grokbot, bot "Personal CRM Contact Context", Notion only; Attio routines deleted 2026-11-05); migrate |
| suggested-actions | comms index or direct reads, notion, attio | chat (drafts only) | comms-ingest, deal-indications | live (Grokbot); migrate |
| call-cadence | notion People CRM, imessage, whatsapp, signal, gcal | chat (call list, drafts), gcal (15-min call events) | brendan-crm | live (Grokbot), v1.1 |
| grok-bot | knowledge repo, connectors per task | bots/MAP.md, bots/grok-bot.md, chat | AGENTS.md bot registry | live (Grokbot); routine "Bot registry refresh" 7pm PT |
| money-queue | QBO x3, Chase, Ramp, Shopify, Meta | proposal list only | hard rules | planned |
| crm-sync-feed | composio:notion, composio:attio | attio, after approval | brendan-crm | superseded 2026-11-04: Notion never feeds Attio (silo); Attio People are written only by attio-contact-context |
| accounting-and-tax | QBO x3, Gusto, banks | QBO | delta-bookkeeper | planned |
| delta-meridian-inbox | composio:googledrive (googledrive_decare-benami: 99 - Inbox, Delta Drive README) | gdrive (copy to destination, original to 0 Archive), chat | arrow/skills/document-filing; Delta Drive README (Doc 1r9_MWLdYSFnxV7z-5gj9TbclfQDGUcnzgAg_f6pWxic) | draft; thin wrapper, weekdays 12pm PT (clear fits only, questions to chat) |

## Shared (`arrow/skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| arrow-fund-inbox | composio:googledrive (googledrive_left-bim: 99 - Inbox, README) | gdrive, chat | document-filing; Arrow Drive README (rules and IDs in amendment v7) | live (Claude); thin wrapper, weekdays 12pm PT |
| document-filing | composio:googledrive (one account per run), the drive's README | gdrive (copy to destination, original to 0 Archive), chat | per-drive wrapper + README in that drive | live (Arrow Document Filing Bot, weekdays 12pm PT; always posts a summary); generic shared procedure loaded by arrow-fund-inbox and (from skills/) delta-meridian-inbox; Janey wrapper planned later, not built |
| deal-indications | signal, whatsapp, gmail (arrow) | composio:attio (blocks, interests), run-log sheet, Signal ack to 3 internal chats | attio-schema.md (to write) | live (Grokbot); migrate |
| private-markets-desk | web | chat, one email to the team | — | live (Grokbot); migrate |
| marwan-deal-board | OffBook board (secondaries.spartangroup.io, box browser), composio:attio (blocks, interests) | composio:attio blocks/interests tagged to Marwan (Part A); Brendan's OffBook /my-board positions (Part B, standing OK 2026-11-02); /workspace/marwan-board/ state; chat to Brendan | — | live (Grokbot, bot "Marwan Deal Board Bot"): routines "Marwan board → Attio sync" `30 8,15 * * 1-5` + "My board refresh from Attio" `30 7 * * 1-5` PT, flexible ±15 |
| nport-scraper | SEC N-PORT, Form D | holdings/people database, tracker Page, monthly digest to chat | — | live (Grokbot), v1.1, monthly refresh |
| arrow-attio-firm-context | comms-index, gmail (brendan@arrowfund.co), whatsapp/signal Arrow chats (incl. "Arrow OS Data Input Chat"), gcal via Attio meetings, /workspace/nport/nport.db (read), composio:attio | composio:attio Companies only: firm_type, arrow_role, firm_context, holdings_marks, key_people, firm_intel_updated, firm_intel_source (created 2026-11-05) + dated notes, AUTO-WRITE with immediate human-edit guard; new records and uncertain/conflicting values remain proposals; /workspace/attio-firms/ | nport-scraper, comms-ingest | live v1.2; bot "Arrow Attio Firms", AUTO-WRITE mode (approved 2026-11-05); every report identifies loaded skill path/version/commit and guard/mode lines; routine "Attio firm context" `0 9,13,16 * * 1-5` + `0 10 * * 0,6` PT |
| arrow-weekly-log | gmail (brendan@arrowfund.co), whatsapp/signal/imessage Arrow chats, Attio meetings + changes, comms-index | Google Doc "Arrow Log — week of <date>" in Drive 00 — Firm & Admin / Arrow Weekly Log (1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5; googledrive_left-bim + googledocs_colius-skiv), /workspace/arrow-log/ | attio-contact-context, arrow-attio-firm-context | live 2026-11-05; bot "Arrow Log"; routines "Arrow weekly log" (one routine, `30 9 * * *` + `0 12,19 * * *`, flexible ±15) + "Arrow log Friday wrap" (`30 6 * * 5`, lateMinutes 0), PT; any Arrow bot may read (read-only); daily-arrow-report and arrow-weekly are current readers; Liability filter before every write; Commitments any-party; People updates |
| arrow-weekly | arrow-weekly-log doc (Friday wrap + sections); composio:attio (current status); googledrive/googledocs (read) | docx, email to leadership | arrow-weekly-log | draft; `0 7 * * 5` PT after first send approved |
| daily-arrow-report | arrow-weekly-log doc (yesterday's entries); composio:attio (current status); googledrive/googledocs (read) | html email to team | arrow-weekly-log | draft; `0 10 * * *` PT after process approved |
| arrow-morning-brief | public web/RSS (free stack); admin@ Gmail replies | html email (admin@ → Brendan; Vaidehi after OK); /workspace/arrow-morning-brief/ archive | — | live in TEST MODE (Brendan-only); routines live: Sun–Thu `0 19 * * 0-4` evening brief + Mon–Fri `30 10 * * 1-5` agenda PT; Sunday = weekend edition |
| carta-investor-onboarding | deal pipeline sheet (gsheets), composio:attio (people, call recordings), gmail (arrow), Carta Closings (read) | chat drafts; sheet / Attio / Carta (invite, countersign) only after Brendan OKs | fund docs (LPA, sub agreement) per vehicle | draft |
| attio-contact-context | comms-index, gmail (5 inboxes), gcal, whatsapp/signal (incl. team groups)/imessage (read), composio:attio (incl. calls, transcripts); never Notion | composio:attio People only: arrow_context, introduced_by, knows, close_with, contact_owner, context_updated, check_size_range, sector_of_interest, lp_fo_ria (+ FO tag), empty contact fields; /workspace/attio-context/state.json (cursor), relationship-provenance.jsonl (private), /workspace/attio-backfill/ (queue.json, relationship_checked.json, log.md, lock) | comms-ingest | live (Grokbot, bot "Arrow Attio People", taken over from Contact Context 2026-11-05; inherits state.json, backfill queue, token): routines "Attio contact context" (folder attio-contact-context), "Attio backfill" (folder attio-backfill) |

## Bot registry

Rebuild map of live Grok Bots. Arrow-only entries under `arrow/bots/` + [`arrow/BOTS.md`](arrow/BOTS.md). Personal / Delta / Janey / Cross under `bots/` + [`bots/MAP.md`](bots/MAP.md). Template: `bots/_TEMPLATE.md` (mirrored at `arrow/bots/_TEMPLATE.md`). Rules in `AGENTS.md`. Grok Bot refreshes daily 7pm PT.

## Projects (`projects/`)

| Project | Status | Next step |
|---|---|---|
| binder-build | in progress | comb brendan-crm |
| mac-mini-setup | next | buy the mini |
| 1password-migration | after mini | open account |
| contact-context-attio-split | done 2026-11-05 | Personal CRM Attio routines deleted; Arrow Attio People / Firms / Log live |
| drive-restructure | after 1Password | — |
| photo-consolidation | after 1Password | — |
| health-insurance | decide by early Nov | pick route, engage broker |

## Ideas (not yet seen twice)

- call-scheduler: per-person cadence, suggests times, drafts outreach, books on confirm. Run by hand for one person for two weeks first.
- relationship-ranker: who is worth time and how often. Reference file of rules + a Notion field first.

## Seams to fix during migration

1. Four cursor systems (comms-index state.json, attio-sync state.json, Notion Run Log CURSOR, Suggested Actions "in memory"). One state directory.
2. suggested-actions: instructions say use the comms index, routines say there is none. Pick one.
3. DONE: brendan-crm/SKILL.md is the spec; Notion page to become a pointer (TASKS).
4. DECIDED: no confidence tags anywhere; private-markets-desk says 'unverified' in words.
5. SUPERSEDED 2026-10-29: named send exceptions approved (DECISIONS 2026-10-29).
6. comms index must be rebuilt on the mini before crm-maintenance or suggested-actions can run there.

## Build order

1. comms-ingest (everything downstream needs the index)
2. deal-indications (Arrow revenue)
3. crm-maintenance
4. chief-of-staff + suggested-actions
5. money-queue (new)
