# Skill index

Every skill, what it reads, what it writes, what depends on it. If a skill isn't here it doesn't exist. Update its row when you change the skill. New rows appear only when a workflow has come up twice (CONSTITUTION.md).

Status key: **live** = running today (engine in parentheses) · **draft** = written, never run · **planned** = workflow has come up, not written.

## Personal (`skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| brendan-crm | composio:notion, composio:attio, gcal, imessage, signal | composio:notion; attio (list membership + empty structured fields, ask before adding anyone) | profile.md | live (Claude), v4 |
| delta-bookkeeper | composio:quickbooks, Delta Intake | composio:quickbooks | profile.md | live (Claude); becomes accounting-and-tax |
| chief-of-staff | todoist, gmail (arrow), whatsapp, gcal x2 | todoist, chat | profile.md | live (Grokbot); migrate |
| comms-ingest | signal, imessage, whatsapp, 6 gmails, gcal, attio | comms index, todoist | — | live (Grokbot); migrate. Infrastructure: three skills read its index. |
| crm-maintenance | comms index, direct thread reads, attio calls | composio:notion (append only), attio (empty fields only) | comms-ingest, brendan-crm, Delta CRM Spec | live (Grokbot); migrate |
| suggested-actions | comms index or direct reads, notion, attio | chat (drafts only) | comms-ingest, deal-indications | live (Grokbot); migrate |
| money-queue | QBO x3, Chase, Ramp, Shopify, Meta | proposal list only | hard rules | planned |
| crm-sync-feed | composio:notion, composio:attio | attio, after approval | brendan-crm | planned |
| accounting-and-tax | QBO x3, Gusto, banks | QBO | delta-bookkeeper | planned |
| document-filing | gdrive x4, gmail, messages | gdrive | arrow-fund-inbox | planned |

## Shared (`arrow/skills/`)

| Skill | Reads | Writes | Depends on | Status |
|---|---|---|---|---|
| arrow-fund-inbox | gdrive (99 - Inbox, README) | gdrive | — | live (Claude) |
| deal-indications | signal, whatsapp, gmail (arrow) | composio:attio (blocks, interests), run-log sheet, Signal ack to 3 internal chats | attio-schema.md (to write) | live (Grokbot); migrate |
| private-markets-desk | web | chat, one email to the team | — | live (Grokbot); migrate |
| arrow-weekly | composio:attio | docx, email to leadership | — | draft |
| daily-arrow-report | composio:attio | html email to team | — | draft |

## Projects (`projects/`)

| Project | Status | Next step |
|---|---|---|
| binder-build | in progress | comb brendan-crm |
| mac-mini-setup | next | buy the mini |
| 1password-migration | after mini | open account |
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
5. DECIDED: no send exceptions. deal-indications drops the Signal ack; private-markets-desk email goes out only on a tap.
6. comms index must be rebuilt on the mini before crm-maintenance or suggested-actions can run there.

## Build order

1. comms-ingest (everything downstream needs the index)
2. deal-indications (Arrow revenue)
3. crm-maintenance
4. chief-of-staff + suggested-actions
5. money-queue (new)
