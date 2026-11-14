# Skill index

Every skill, what it reads, what it writes, what depends on it. If a skill isn't here it doesn't exist. Update its row when you change the skill. New rows appear only when a workflow has come up twice (CONSTITUTION.md).

Status key: **live** = running today (engine in parentheses) · **draft** = written, never run · **planned** = workflow has come up, not written.

Connection account ids, workspace ids and endpoints are not kept in this public repo. Skills name connections generically (for example "Arrow Attio via Composio", "Arrow Gmail connection"); bots pick the matching connection by account.

## Personal (`skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| brendan-crm | composio:notion, composio:attio (read only; call transcripts pulled by crm-maintenance via skills/comms-ingest/attio-calls.md), gcal, imessage, signal | composio:notion only (never writes Attio; 2026-11-04 silo) | profile.md | live (Claude), v4.3 |
| delta-bookkeeper | composio:quickbooks, Delta Intake (Deltameridian Notion) | composio:quickbooks | profile.md | live (Claude); becomes accounting-and-tax; Notion IDs refreshed 2026-11-10 |
| chief-of-staff | todoist, gmail (arrow), whatsapp (Brendan's + limited partner: Chris's, Gabe's, and Jillian's), signal, imessage, gcal x2, attio, other bots' transcripts | todoist, chat | profile.md | live (Grokbot) |
| comms-ingest | signal, imessage, whatsapp (Brendan's + partner: Chris's, Gabe's, and Jillian's; index-only for partner, limited Todoist), 6 gmails, gcal, attio (meetings plus call recordings/transcripts; shared procedure skills/comms-ingest/attio-calls.md) | comms index, todoist | attio-schema.md, attio-calls.md | live (Grokbot); migrate. Infrastructure: three skills read its index. Shared Attio call scan lives at skills/comms-ingest/attio-calls.md. |
| crm-maintenance | comms index, direct thread reads (whatsapp, signal, imessage, 5 gmails, gcal), attio (read only, incl. calls via skills/comms-ingest/attio-calls.md) | composio:notion only (append only); never writes Attio | comms-ingest, brendan-crm, attio-calls.md | live (Grokbot, bot "Personal CRM Contact Context", Notion only; Attio routines deleted 2026-11-05; deferred backlinks 2026-11-12); migrate |
| suggested-actions | comms index or direct reads, notion, attio (incl. call transcripts via skills/comms-ingest/attio-calls.md) | chat (drafts only) | comms-ingest, deal-indications, attio-calls.md | missing from live fleet (2026-11-06); skill retained for rebuild |
| call-cadence | notion People CRM, imessage, whatsapp, signal, gcal | chat (call list, drafts), gcal (15-min call events) | brendan-crm | live (Grokbot), v1.2 |
| grok-bot | knowledge repo, `/home/box/agent-data/agents/` (registry refresh only), WhatsApp (all four accounts, daily sync), composio:attio (daily sync), connectors per task | bots/MAP.md, bots/grok-bot.md, composio:attio (blank-only identity and later-only last_contacted on the daily sync), `/workspace/whatsapp-attio-sync/`, chat | AGENTS.md bot registry, whatsapp-attio-daily-sync.md | live (Grokbot); routines "Bot registry refresh" 7pm PT inventories live profiles, "WhatsApp-Attio daily sync" (folder whatsapp-attio-daily-sync) 6:30am PT flexible ±15, first run 2026-11-14 |
| money-queue | QBO x3, Chase, Ramp, Shopify, Meta | proposal list only | hard rules | planned |
| crm-sync-feed | composio:notion, composio:attio | attio, after approval | brendan-crm | superseded 2026-11-04: Notion never feeds Attio (silo); Attio People are written only by attio-contact-context |
| accounting-and-tax | QBO x3, Gusto, banks | QBO | delta-bookkeeper | planned |
| delta-meridian-inbox | composio:googledrive (the Google Drive connection for brendan@deltameridian.com: 99 - Inbox, Delta Drive README) | gdrive (copy to destination, original to 0 Archive), chat | arrow/skills/document-filing; Delta Drive README (Doc 1r9_MWLdYSFnxV7z-5gj9TbclfQDGUcnzgAg_f6pWxic) | draft; thin wrapper, weekdays 12pm PT (clear fits only, questions to chat); follows document-filing hard rule to open every inbox file |

## Shared (`arrow/skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| arrow-fund-inbox | composio:googledrive (the Google Drive connection for brendan@arrowfund.co: 99 - Inbox, README) | gdrive, chat | document-filing; Arrow Drive README (rules and IDs in amendment v7) | live (Claude); thin wrapper, weekdays 12pm PT; follows document-filing hard rule to open every inbox file |
| document-filing | composio:googledrive (one account per run), the drive's README | gdrive (copy to destination, original to 0 Archive), chat | per-drive wrapper + README in that drive | live (Arrow Document Filing Bot, weekdays 12pm PT; always posts a summary); generic shared procedure loaded by arrow-fund-inbox and (from skills/) delta-meridian-inbox; Janey wrapper planned later, not built; hard rule: open and read every inbox file every run before filing or asking; asks describe-then-recommend only when no obvious existing home |
| deal-indications | signal, whatsapp (top-priority consumer of all four accounts: Brendan's, Chris's, Gabe's, and Jillian's), gmail (arrow), attio call recordings/transcripts (skills/comms-ingest/attio-calls.md), People `liquidity_interest` and `liquidity_holdings` | composio:attio (blocks, interests; enriches website-created Blocks from Yes plus `liquidity_holdings`, does not duplicate them, and does not treat `deal_classification` Unconfirmed as a qualification flag; qualification is `qualified_blocks` / `qualified_interests` list membership; never creates Companies from `notable_past_investments`), run-log sheet, Signal ack to 3 internal chats | attio-schema.md, attio-calls.md | live (Grokbot); migrate |
| private-markets-desk | web | chat, one email to the team | — | live (Grokbot); migrate |
| marwan-deal-board | OffBook board (secondaries.spartangroup.io, box browser), composio:attio (blocks, interests) | composio:attio blocks/interests tagged to Marwan (Part A); Brendan's OffBook /my-board positions (Part B, standing OK 2026-11-02); /workspace/marwan-board/ state; chat to Brendan | — | live (Grokbot, bot "Marwan Deal Board Bot"): routines "Marwan board → Attio sync" `30 8,15 * * 1-5` + "My board refresh from Attio" `30 7 * * 1-5` PT, flexible ±15 |
| nport-scraper | SEC N-PORT, Form D | holdings/people database, tracker Page, monthly digest to chat; planned monthly cap-table writes into Attio | — | live (Grokbot), v1.3, monthly refresh |
| arrow-attio-firm-context | comms-index, gmail (brendan@arrowfund.co), whatsapp (Brendan's + partner: Chris's, Gabe's, and Jillian's; Arrow groups and Arrow-relevant partner DMs)/signal Arrow chats (incl. "Arrow OS Data Input Chat"), gcal via Attio meetings, attio call recordings/transcripts (skills/comms-ingest/attio-calls.md), /workspace/nport/nport.db (read), composio:attio | composio:attio Companies only: firm_type, arrow_role, firm_context, holdings_marks, key_people, firm_intel_updated, firm_intel_source (created 2026-11-05), standard_arrow_chat_jid (created 2026-11-13, fill when empty) + dated notes, AUTO-WRITE with immediate human-edit guard; new records and uncertain/conflicting values remain proposals; /workspace/attio-firms/ | nport-scraper, comms-ingest, attio-schema.md | live v1.7; bot "Arrow Attio Firms", AUTO-WRITE mode (approved 2026-11-05); every report identifies loaded skill path/version/commit and guard/mode lines; routine "Attio firm context" `0 9,13,16 * * 1-5` + `0 10 * * 0,6` PT |
| arrow-weekly-log | gmail (brendan@arrowfund.co), whatsapp (Brendan's + partner: Chris's, Gabe's, and Jillian's)/signal/imessage Arrow chats, Attio meetings + call recordings (neutral one-liners only, skills/comms-ingest/attio-calls.md) + changes, comms-index | Google Doc "Arrow Log — week of <date>" in Drive 00 — Firm & Admin / Arrow Weekly Log (1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5; the Google Drive connection for brendan@arrowfund.co + the Google Docs connection for brendan@arrowfund.co), /workspace/arrow-log/ | attio-contact-context, arrow-attio-firm-context | live v1.8 (2026-11-13); bot "Arrow Log"; routines "Arrow weekly log" (one routine, `30 9 * * *` + `0 12,19 * * *`, flexible ±15) + "Arrow log Friday wrap" (`30 6 * * 5`, lateMinutes 0), PT; any Arrow bot may read (read-only); daily-arrow-report and arrow-weekly are current readers; Liability filter before every write (v1.4: names/sizes/valuations/PPS OK; name who a block or interest belongs to; fees never); Commitments any-party; People updates |
| arrow-weekly | arrow-weekly-log doc (Friday wrap + sections); composio:attio (current status); googledrive/googledocs (read) | docx, email to leadership | arrow-weekly-log | draft; `0 7 * * 5` PT after first send approved |
| daily-arrow-report | arrow-weekly-log doc (yesterday's entries); composio:attio (current status); googledrive/googledocs (read) | html email to team | arrow-weekly-log | draft; `0 10 * * *` PT after process approved |
| arrow-morning-brief | public web/RSS (free stack); admin@ Gmail replies; `profiles/INDEX.md` | html email (admin@ → Brendan + Vaidehi); /workspace/arrow-morning-brief/ archive; `profiles/<slug>.md` | — | live v0.4 (approved 2026-11-09): Company file + thinking note + Arrow Fund branding; call Brendan & Vaidehi Get Smart; recipients Brendan + Vaidehi; routines live: Sun-Thu `0 19 * * 0-4` evening brief + Mon-Fri `30 10 * * 1-5` thinking note PT; Sunday = weekend edition |
| arrow-chief-of-staff | Arrow Fund Notion (Projects, Tasks, Entity Register, COS Activity Log) via Composio `work`; Signal team DMs; meeting notes; Attio lists (read) | composio:notion (work account only, including the activity log); Signal DMs for the live daily cycle; Todoist only for explicit personal items | — | live (Grokbot, bot "Arrow Chief of Staff" f6e9b74f); daily Signal 9/12/1/2 PT with reply scan and activity log; 12pm CEO/COO ops brief to Brendan and Chris |
| carta-investor-onboarding | deal pipeline sheet (gsheets), composio:attio (people, call recordings via skills/comms-ingest/attio-calls.md when a run needs one), gmail (arrow), Carta Closings (read) | chat drafts; sheet / Attio / Carta (invite, countersign) only after Brendan OKs | fund docs (LPA, sub agreement) per vehicle | draft |
| attio-people-cleanup | composio:attio (People, Companies, Blocks, Interests, Deals, lists, notes, tasks, members); Arrow Gmail (read-only, approved name lookups) | composio:attio People deletes (JUNK_AUTO only, max 50 per run, live re-check, snapshot first; borderline deletes and `name` renames only with Brendan's approval); /workspace/attio-cleanup/ (snapshots/, deleted-*.csv, renamed-*.csv, keep-list.json, repeat-senders.json, classify.py); chat to Brendan | attio-schema.md | live (bot "Attio Cleanup" 14d5e03f); routine "Weekly Attio People cleanup" (folder weekly-attio-people-cleanup) `0 18 * * 0` PT flexible ±15, first run 2026-11-15; run history in the skill (2026-11-13: 210 deleted, 8 renamed, 102 keep-listed) |
| attio-contact-context | comms-index, gmail (5 inboxes), gcal, whatsapp (Brendan's + partner: Chris's, Gabe's, and Jillian's; CONNECTIONS.md)/signal (incl. team groups)/imessage (read), composio:attio (incl. required Attio call scan each event run, skills/comms-ingest/attio-calls.md, cursor in /workspace/attio-context/state.json); never Notion | composio:attio People only: arrow_context, introduced_by, knows, close_with, contact_owner, context_updated, check_size_range, sector_of_interest, lp_fo_ria (+ FO tag), investment_structure, decision_timeline, empty contact fields, last_whatsapp_contact, last_whatsapp_contact_by, last_contacted, last_contacted_by, last_contact_channel, days_since_last_contact, last_contacted_brendan, last_contacted_chris, last_contacted_gabe, last_contacted_jillian, last_contacted_vaidehi, whatsapp_phone_jid, whatsapp_lid, standard_arrow_chat_jid; /workspace/attio-context/state.json (cursor), relationship-provenance.jsonl (private), /workspace/attio-backfill/ (queue.json, relationship_checked.json, log.md, lock) | comms-ingest, attio-schema.md | live (Grokbot, bot "Arrow Attio People", taken over from Contact Context 2026-11-05; inherits state.json, backfill queue, token): routines "Attio contact context" (folder attio-contact-context, includes last-contact pass, on the live routine prompt since 2026-11-11; Attio calls paragraph live 2026-11-13), "Attio backfill" (folder attio-backfill), "Caplight recap watch" (folder caplight-recap-watch, temporary until 2026-11-17) |

## References (`references/`)

Operational docs that skills and routines load. Not skills themselves.

| File | What it is | Used by |
|---|---|---|
| [attio-schema.md](references/attio-schema.md) | Arrow Attio objects, field slugs, WhatsApp identity and last-contacted rules, People LP intake fields, Blocks write path, Qualified Blocks / Qualified Interests lists | attio-contact-context, arrow-attio-firm-context, deal-indications, comms-ingest, attio-people-cleanup |
| [whatsapp-mcp.md](references/whatsapp-mcp.md) | WhatsApp MCP servers, v0.5.0 tools, "How skills read WhatsApp" (shared v0.5 read method for every bot: own `changes_since` cursors, `resolve_person`, daily-sync handoff including the `groups` identity block, `contact_activity` with the outbound-only last-contact rule, groups required as context, `coverage_report`), 56-day body purge (`contact_activity` and the change log are kept), admin vs view-only | CONNECTIONS.md, every WhatsApp-reading skill |
| [whatsapp-attio-daily-sync.md](references/whatsapp-attio-daily-sync.md) | Runbook for the daily job that keeps Attio and all four WhatsApp servers in step (v0.5.0 tools, v0.4 fallbacks, outbound-only last-contact rule, handoff `groups` identity block). Implemented as Grok Bot routine "WhatsApp-Attio daily sync" (folder whatsapp-attio-daily-sync). | live Grok Bot routine, daily 6:30am PT flexible ±15, first run 2026-11-14; feeds deal-indications, attio-contact-context, arrow-attio-firm-context, arrow-weekly-log, and comms-ingest |

## Bot registry

Rebuild map of live Grok Bots. Arrow-only entries under `arrow/bots/` + [`arrow/BOTS.md`](arrow/BOTS.md). Personal / Delta / Janey / Cross under `bots/` + [`bots/MAP.md`](bots/MAP.md). Template: `bots/_TEMPLATE.md` (mirrored at `arrow/bots/_TEMPLATE.md`). Rules in `AGENTS.md`. Grok Bot refreshes daily 7pm PT. Temporary Personal bot: Global Entry Watch (`bots/global-entry-watch.md`), no skill, created 2026-11-10.

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

- journal/notes dashboard: start using the existing memory/ folder for working notes and lessons. A richer journal or visual notes surface can wait until this has come up twice.
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
