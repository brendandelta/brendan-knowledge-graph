# Changelog

## 2026-11-11
- chief-of-staff: temporary standing accountability item (Brendan, via Grok Bot, 2026-11-11). Until he says the project is finished (in CoS chat or Grok Bot's chat), every morning brief, midday check, and EOD check includes the WhatsApp MCP upgrade + Attio People schema overhaul as a top item and names the next step he owes. Steps: (1) Brendan finishes the MCP upgrades with Claude and sends Claude's report to Grok Bot; (2) Grok Bot adds Attio WhatsApp ID fields and updates the repo; (3) Brendan approves the People schema cleanup field by field from the audit Page. Check Grok Bot's transcript for progress; drop the item once he says finished. Morning, midday, and EOD prompts also restate "no em dashes".
- grok-bot: temporary routine "Push: WhatsApp MCP + Attio schema project" (Thu-Fri Nov 12-13, 10 AM, 2 PM, 6 PM PT) pushes Brendan to finish the WhatsApp MCP upgrade with Claude, the Attio WhatsApp ID fields and repo docs, and the People schema overhaul; it self-deletes when done or after the Fri 6 PM run. Brendan · Chief of Staff (personal briefs) asked to carry it in its check-ins too.
- Rule (Brendan, 2026-11-11, all Grok Bots): every skill or routine change is written to the repo skill file and committed and pushed in the same turn, with INDEX.md and CHANGELOG.md. A change is not done until `git push` succeeds.
- attio-contact-context: Phases 1 and 2 of last-contact tracking (Brendan approved). Six People attributes created in Arrow Attio: `last_whatsapp_contact` (timestamp), `last_whatsapp_contact_by` (actor-reference), `last_contacted` (timestamp), `last_contacted_by` (actor-reference), `last_contact_channel` (select: Email, WhatsApp), `days_since_last_contact` (number, whole calendar days in America/Los_Angeles). The "by" fields are workspace-member actor-references, the same type as Relationship owner. Arrow Attio People fills them from outbound 1:1 WhatsApp on Brendan's and Chris's connectors (match by E.164, no new records, only these six fields) and rolls up with Attio's built-in `last_email_interaction`. First backfill ran 2026-11-11 (63 people via WhatsApp, 3014 email-only, 25 unmatched WhatsApp phones). Gabe's and Jillian's WhatsApp are not connected, so their WhatsApp sends are missing. Phase 3 (daily per-owner stale-contact reminders) and Phase 4 (drafted, later opt-in auto check-ins) are planned, not built. The last-contact paragraph was added to the live "Attio contact context" routine on 2026-11-11 (Brendan approved).

## 2026-11-10
- Global Entry Watch: temporary Personal bot (`38b25525-947a-41da-95aa-11eb279181c5`) created to scan connmbrendan@gmail.com at 8:00 AM and 6:00 PM PT for official Global Entry / TTP / CBP updates and notify hard on a real change. No skill. Registry: `bots/global-entry-watch.md`.
- Global Entry Watch: the bot reported it can read connmbrendan@gmail.com and that routine "Global Entry inbox watch" is enabled at 8:00 AM and 6:00 PM PT (flexible ±15). Registry row updated.
- delta-bookkeeper: refreshed live Notion IDs to the Deltameridian workspace Delta Books page (Composio `notion_pace-adiate`). Prior Brendan's Space IDs 404. Rules (live) and Rulings were not found in this workspace.
- memory/profile-facts.md: Global Entry application 138811671 paid 2026-11-10, Pending Review, PASSID 171523070.
- memory/profile-facts.md: visual dashboards and quick access to important information are vital (alongside connecting tools like WhatsApp bots). The existing memory/ folder is the journal home for now; daily notes stay local because memory/daily/ is gitignored. A richer notes surface can wait until this has come up twice.
- Partner WhatsApp: added Chris Clifford's WhatsApp as `user-Chris' WhatsApp MCP` (CONNECTIONS.md Partner WhatsApp section). Skills updated: attio-contact-context, arrow-attio-firm-context v1.3, deal-indications, arrow-weekly-log v1.5, plus limited use in comms-ingest (index only, limited Todoist) and chief-of-staff (only threads that involve Brendan or an Arrow ask). Read-only, Arrow-relevant only, dedup against Brendan's WhatsApp. Gabe and Jillian can be added later as more rows under the same rules. No endpoint URL or token stored in the repo. Personal CRM, call-cadence, daily-arrow-report, arrow-weekly, and private-markets-desk left unchanged.

## 2026-11-09
- arrow-chief-of-staff: the 12:00 PM CEO/COO ops brief keeps the body link-free and puts person task-board links at the end (Brendan, Chris, Gabe, Vaidehi, Jillian).
- arrow-chief-of-staff: the 12:00 PM CEO/COO ops brief has no links (no task, project, or hub links).
- arrow-chief-of-staff: 12:00 PM PT rollup is now a CEO/COO company ops brief to Brendan and Chris (snapshot, needs-you, moving, coming up, remaining projects, then team check-ins), not a who-replied attendance note.
- Arrow Morning Brief v0.4: daily Company file (tracked in `arrow/skills/arrow-morning-brief/profiles/`), 10:30am thinking note instead of 3 emailed questions, Arrow Fund branding (navy #1F2A44, Arrow blue #2E5AAC), weekday call named Brendan & Vaidehi Get Smart.
- arrow-chief-of-staff: 12:00 PM PT rollup now goes to Brendan and Chris Clifford.
- arrow-chief-of-staff: locked the scan-then-update-then-send loop before the 9:00 AM, 12:00 PM, 1:00 PM, and 2:00 PM jobs; added the Arrow COS Activity Log for relevant outbound messages and inbound work-status only; follow-up time is 1:00 PM PT (moved from 11:00 AM).
- marwan-deal-board: Part B now replaces auto-filled company descriptions that name founders/backers with a neutral one-liner (Stripe row fixed on the board).

## 2026-11-06
- Bot registry refresh (Grok Bot): Suggested Actions (`c0d5c0d3…`) missing from live `/home/box/agent-data/agents/` (not in `deleted-agents.json`); marked missing in `bots/suggested-actions.md`, `bots/MAP.md`, INDEX, and `skills/suggested-actions`. Synced CoS live display name to Brendan · Chief of Staff; synced Document Filing and Marwan live display names; fixed corrupted Last verified row on `bots/grok-bot.md`.
- Arrow Chief of Staff: new Arrow company COS / project-management bot (f6e9b74f-37da-4703-8a77-3277f1cb848f) created and handed the Notion PM rewrite; skill `arrow/skills/arrow-chief-of-staff`, registry `arrow/bots/arrow-chief-of-staff.md`. No notification routines yet.
- nport-scraper v1.3: Brendan redirected the dataset destination from monthly Vaidehi email drafts to an Attio cap-table-companies write (to be built into the firms tracker). Monthly refresh no longer drafts email to Vaidehi; Attio writes stay off until that path is approved and live.
- nport-scraper v1.2: after each monthly refresh with new data, rebuild the full all-company holder workbook and create a Gmail draft from brendan@arrowfund.co to vaidehi@arrowfund.co (Brendan sends; never auto-send). First package drafted 2026-11-06 covering all 103 companies.
- arrow-weekly-log v1.4: Brendan — name who a block or interest belongs to; log valuations and PPS; never record fees (core liability). Rule 4 and rule 9 updated.
- document-filing: when asking about an unfiled document that has no already-obvious existing home, first describe what it is from its content and recommend how to file it; never ask by title alone. Documents that already have an obvious home are still filed automatically as clear fits (Brendan, 2026-11-06).
- Arrow Morning Brief v0.3: Brendan approved adding Vaidehi; live recipients are brendan@arrowfund.co and vaidehi@arrowfund.co for the evening brief and morning agenda. Calendar invite still pending.
- arrow/skills/attio-contact-context: Gabe People record id in Access updated to c1ed36e3-ca7a-4c9e-ad79-73d7ffef26a7 (prior 37fd798b 404 after Attio record rotation).


## 2026-11-05
- Bot registry refresh (Grok Bot): synced live drift — Morning Brief display rename to "Brendan <> Vaidehi Market Reports" + description; Arrow Attio People description; Grok Bot description/routine row; `skills/grok-bot` refresh steps now inventory `/home/box/agent-data/agents/` profiles. Updated `arrow/BOTS.md`, `bots/MAP.md`, INDEX.
- Arrow Attio People: one-off routine "Attio questions evening list (Nov 5)" ran, sent open pending-questions.md items to Brendan, and deleted itself; registry row removed.
- Arrow Morning Brief v0.2: bot created (3e40c3a8-3cce-401b-b460-d6ac50940c3c), first test brief sent to Brendan, routines live in TEST MODE (Brendan-only): evening-brief `0 19 * * 0-4` with a run.log dedupe guard, and morning-agenda `30 10 * * 1-5`. Registry, BOTS.md, INDEX and skill status/changelog updated.
- New draft skill arrow/skills/arrow-morning-brief (+ template.html): Sun–Thu 7pm PT external markets HTML brief from admin@ (Brendan Bot display name; test = Brendan only); Sunday weekend edition; Mon–Fri 10:30am agenda from email-reply questions; calendar invite pending. Registry stub arrow/bots/arrow-morning-brief.md + BOTS.md / INDEX rows. No bot, routines, or sends yet.
- Morning Buy/Sell Sheet retired: deleted bot, routine, skill `arrow/skills/morning-buy-sell-sheet`, registry entry `arrow/bots/morning-buy-sell-sheet.md`, and its `arrow/BOTS.md` / INDEX rows. Being replaced by a new block/interest qualification and deal-board system (Brendan, 2026-11-05).
- Note: commit `6ede90f` is mislabeled "registry: Private Markets Desk entry" but contains the Deal Indications registry entry (parallel-bot race); left as-is, no history rewrite. Real Private Markets Desk entry is `7cc7fdf`.
- AGENTS.md + bot registry templates: bot-entry updates add a one-line CHANGELOG and update `arrow/BOTS.md` or `bots/MAP.md`; touch INDEX.md only when a skill or file is added or removed. Shared-clone rules: always `git pull --rebase origin main`, never leave uncommitted edits, stage only your own files.
- arrow-attio-firm-context v1.2: Brendan approved AUTO-WRITE for supported values on existing Companies, with immediate pre-write human-edit guard, add-only key_people, no duplicate merges and firm_context exclusions. New records, uncertain/conflicting values and human-value changes remain proposals; existing private-mark/Competitor approval rules retained. Corrected Signal chat name, held disputed Poolside figure, required per-write log and loaded-skill path/version/commit plus guard/mode report headers. Synced live description, saved routine prompt, registry and INDEX; cadence and external-send ban unchanged.
- New skill arrow/skills/marwan-deal-board (live on Marwan Deal Board Bot): logic moved out of routine prompts; both routines now `git pull` then follow the skill (crons unchanged). Bot title/description set with the knowledge-repo header; registry entry + arrow/BOTS.md row + INDEX row updated.
- document-filing: scheduled runs now always post a daily summary per drive (one line when empty) plus open questions (Brendan, 2026-11-02), replacing "say nothing when empty". Arrow Document Filing Bot routine slimmed to pull + follow skills; title and description set; registry entry updated.
- Inbox Todo (comms-ingest): description rewritten to match the real cadence (three Comms Ingest routines approved 2026-10-15, not every 30 minutes) and channels (WhatsApp, Signal, iMessage, Gmail, calendar, Attio). Same wording synced in `skills/comms-ingest/SKILL.md` and `bots/inbox-todo.md`; stale-cadence mismatch flag removed. INDEX row already correct.
- Personal CRM Contact Context: Brendan confirmed (t86u); deleted Attio routines attio-context, attio-backfill, attio-questions-evening-list-nov-5. Bot is Notion-only (weekday/weekend CRM enrichment). Registry + MAP + crm-maintenance skill updated; Arrow Attio People owns all Attio People work.
- arrow-weekly-log v1.3: live on the Arrow Log bot; first backfill published. Schedule records the actual routines (one "Arrow weekly log" routine with both crons; Friday wrap lateMinutes 0) and the live routine prompts.
- Bot registry: `AGENTS.md` rules; templates `bots/_TEMPLATE.md` and `arrow/bots/_TEMPLATE.md`; indexes `bots/MAP.md` and `arrow/BOTS.md`; pending self-report stubs for known bots; Grok Bot entry `bots/grok-bot.md` + skill `skills/grok-bot` with routine "Bot registry refresh" (`0 19 * * *` PT).
- Personal CRM Contact Context: `skills/crm-maintenance` renamed bot (was Contact Context); Notion-only; weekday/weekend routines unchanged; Attio routines on that bot paused pending Brendan delete; Attio People → Arrow Attio People; firm intel → Arrow Attio Firms; Weekly Log writer → Arrow Log only. Registry: `bots/personal-crm-contact-context.md`.
- attio-contact-context v2.3: relationship evidence rules (direct evidence + per-edge source link, both-endpoint human guard, add-only); introduced_by stays single, Introducer = inverse `introduced`; Backfill adds A–C relationship checks even when Arrow context is filled (relationship_checked.json). Backfill routine prompt updated to match.
- attio-contact-context: Status now live on the Arrow Attio People bot (taken over from Contact Context); routines "Attio contact context" (folder attio-contact-context) and "Attio backfill" (folder attio-backfill). INDEX row updated.
- New draft skills: arrow/skills/arrow-attio-firm-context (Attio company intel, PROPOSE mode; bot-plan.md) and arrow/skills/arrow-weekly-log (weekly Arrow Log doc; bot-plan.md).
- arrow-weekly-log is a general shared Arrow source (any Arrow bot may read); Liability filter approved; Commitments are any-party; People updates; personal/mixed Arrow-account items excluded by content; deal names and sizes OK, prices/terms/counterparties never. Reports carry a matching Liability guard.
- daily-arrow-report and arrow-weekly become formatters/senders that read the weekly log plus current Attio status; neither scans comms. Arrow Weekly's Sep 21 tighter report bar moved from bot memory into the skill.
- Contact Context split: Notion CRM stays on Contact Context (skills/crm-maintenance); attio-contact-context moves to new bot "Arrow Attio People" (inherits routines/state/token; bot-plan.md); arrow-attio-firm-context runs on new bot "Arrow Attio Firms" (PROPOSE; StepStone = Seller); arrow-weekly-log runs on new bot "Arrow Log". Plan: projects/contact-context-attio-split.md.
- Attio: 7 company attributes created (firm_type, arrow_role, firm_context, holdings_marks, key_people, firm_intel_updated, firm_intel_source); public-filing marks allowed on companies.
- Arrow Drive README amendment v8 (applied 2026-11-05): folder 00 — Firm & Admin / Arrow Weekly Log (1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5).


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
- New skill: arrow/skills/carta-investor-onboarding (draft v2). It sets the pipeline → Attio → Gmail → Carta order of operations (15 steps, plus fund setup F1–F7) and approval gates on every send, Send Invites, signature removal, countersign, and capital call. The Carta mechanics come from Marcela (Carta) in the Oct 16 Closings demo. Hubble is the worked example. INDEX Shared row added.
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
