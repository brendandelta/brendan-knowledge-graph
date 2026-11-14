# WhatsApp-Attio daily sync

Spec for the daily job that keeps Arrow Attio and the four WhatsApp servers in step, and prepares the Deal Indications, Attio People, and Attio Firms bots for their first runs of the day.

Implemented as Grok Bot routine **"WhatsApp-Attio daily sync"** (folder `whatsapp-attio-daily-sync`), daily ~6:30 AM PT (`30 6 * * *` America/Los_Angeles), flexible ±15 minutes, first run 2026-11-14. This file is the runbook the routine follows. WhatsApp MCP v0.5.0 is live on both Hetzner servers as of 2026-11-13 (release commit `4c602444`). A v0.4 fallback path is defined below and may be run by hand if a server is temporarily back on the older build.

Last updated: 2026-11-13.

## Goal

One shared identity picture in Attio, refreshed every morning before Deal Indications runs:

1. Push phone JIDs, LIDs, and names from every WhatsApp server up to Attio, filling blank fields only.
2. Push Attio names down to every WhatsApp server via `import_names`, filling blanks only.
3. Roll up per-teammate `last_contacted_*` timestamps from `contact_activity`, later-only.
4. Advance a `changes_since` cursor per account so downstream bots read only what is new.
5. Hand those cursors, coverage numbers, and any needs-review collisions to Deal Indications, attio-contact-context, and arrow-attio-firm-context.
6. Publish a coverage report.

Messages themselves stay on the WhatsApp servers and are deleted after 56 days. This job writes facts and identifiers into Attio, never quoted message text. Buyer and seller identities stay out of anything external. Say "Intermediary", never "Broker".

## Accounts and connectors

| Teammate | Connector | Server | Link type Grok Bot holds | Job may write? |
|---|---|---|---|---|
| Brendan Conn | `user-whatsapp-mcp` | Brendan's Hetzner server | Admin | Yes, after the approval gates below |
| Chris Clifford | `user-Chris WhatsApp MCP (Admin)` | Arrow team Hetzner server | Admin | Names only, after standing approval; never draft, send, or change groups |
| Gabe Borden | `user-Gabe WhatsApp MCP (Admin)` | Arrow team Hetzner server | Admin | Names only, after standing approval; never draft, send, or change groups |
| Jillian Nuding | `user-Jillian WhatsApp MCP (Admin)` | Arrow team Hetzner server | Admin | Names only, after standing approval; never draft, send, or change groups |

Attio is Arrow Attio via Composio, workspace `arrow-fund`. Field ids, the JID-first then phone-second match order, and fill-blank / later-only rules: `references/attio-schema.md`. Partner WhatsApp rules: `CONNECTIONS.md`. How the servers work: `references/whatsapp-mcp.md`.

Vaidehi's WhatsApp is not connected. `last_contacted_vaidehi` stays empty until her server exists or a later job can attribute email to her.

## Schedule

Run once daily, early morning Pacific Time, and finish before Deal Indications' first weekday pass at 7:47am PT.

- Live routine on Grok Bot: **"WhatsApp-Attio daily sync"** (folder `whatsapp-attio-daily-sync`), `30 6 * * *` America/Los_Angeles (6:30am PT), flexible ±15 minutes, first run 2026-11-14.
- Weekend Deal Indications starts at 9:47am PT, so the same 6:30am slot still finishes first.
- If the job is still running at 7:40am PT on a weekday, it should stop starting new writes, flush the cursors and coverage report it has, and leave the rest for the next day. Deal Indications must not be delayed.
- Do not run a second copy if the previous run still holds the lock.

State lives on the box at `/workspace/whatsapp-attio-sync/state.json` (create the directory on first run). It holds, per account: the last successful `changes_since` cursor, last run time PT, last coverage snapshot, and a lock with a 45-minute expiry.

## Approval gates

Brendan already approved the design (push identity up, push names down, roll up last-contacted). The job still follows these gates:

1. **Never send, draft, or execute a group change.** Group draft tools from v0.5 exist for humans and for other skills behind draft-then-approve. This job does not call them.
2. **Attio identity writes are blank-only.** Never overwrite `whatsapp_phone_jid`, `whatsapp_lid`, or `standard_arrow_chat_jid`. Never create a person or company just to store a JID. If more than one Attio record matches, skip and list it as needs-review.
3. **Last-contacted writes are later-only.** Write a timestamp only when it is later than the value already set. Never move a date backwards.
4. **Name-down to the servers (`import_names`) needs standing approval, once.** The first production run of name-down should be a dry run: log the exact `{phone_or_jid, name}` list per account, send Brendan that list, and wait for a yes. After he approves standing blank-only name-down, later runs may call `import_names` with the default fill-blanks behavior, no overwrite flag, and a post-run count. Any run that would use the overwrite flag needs a fresh yes on that exact list.
5. **Partner servers stay read-only for everything except that approved blank-only `import_names`.** Never `set_contact_name`, `import_contacts`, `draft_message`, `send_message`, `check_numbers` (live), or group writes on a partner connector.
6. **No money, no external messages, no quoted WhatsApp text in Attio or in the report.**

Until standing approval for step 4 is on file, the job still does steps 1, 3, 5, and 6, and only logs the name-down list.

## v0.5 tools this job is built on

From WhatsApp MCP v0.5.0 (live on `release` as of 2026-11-13):

| Tool | Link type | This job uses it for |
|---|---|---|
| `contact_activity` | Read | Per-contact first/last inbound and outbound, counts, "no outbound in N days". Survives the 56-day body purge. Source of `last_contacted_*`. |
| `changes_since` | Read, including view-only | Incremental feed of chats, contacts, and messages since an opaque cursor. |
| `export_contacts` | Read | Full contact dump: phone JID, LID, E.164, saved name, push name, name source. |
| `import_names` | Admin | Bulk name fill, blanks by default, explicit overwrite flag. |
| `resolve_person` | Read | One lookup from phone, JID, LID, or name. Returns ambiguity; never guesses. |
| `coverage_report` | Read | Resolved vs LID-only, named vs unnamed, 7/30/56-day activity. |
| Group draft tools | Admin | Not used by this job. |

`import_contacts` (v0.4 address-book import) stays available but is not the name-down path. `set_contact_name` is one-at-a-time and should not be used in the daily job.

## Steps (happy path, v0.5)

### 0. Lock, health, and cheap gate

- Acquire `/workspace/whatsapp-attio-sync/lock`. If a live lock is younger than 45 minutes, exit.
- Call `status` on all four connectors. If a server is down, skip that account, record the failure, and continue with the others. Do not abort the whole run for one account.
- Call `coverage_report` on each account and store a before-snapshot.
- If `changes_since` on every account returns an empty page and Attio has had no identity writes pending from the last run, write a "no changes" coverage row and stop.

### 1. Pull changes

For each account, call `changes_since` with the stored cursor (or a first-run backfill window of 56 days on the first successful v0.5 run). Page until complete. Keep the new cursor in memory; only persist it after steps 2 and 3 succeed for that account.

Dedup across accounts by canonical phone JID (strip any `:device` suffix). A shared Arrow group will appear on several servers; treat it as one item.

### 2. Push identity up to Attio (blank-only)

For each new or changed contact and 1:1 chat:

1. Canonicalize the phone JID (`{E164_without_plus}@s.whatsapp.net`) and LID (`{n}@lid`).
2. Match Attio People by JID first (`whatsapp_phone_jid` or `whatsapp_lid`), E.164 `phone_numbers` second, as in `references/attio-schema.md`.
3. If exactly one confident match: fill empty `whatsapp_phone_jid`, `whatsapp_lid`, and (for a clear standing Arrow group only) `standard_arrow_chat_jid`. Same test as the schema reference: skip internals, multi-party chats, deal-specific threads, first-name-only titles, and two groups for the same counterpart.
4. If the Attio person has no name and the server has a saved or address-book name (not just a push name or a masked number), fill the Attio name only when it is empty.
5. If zero matches: do not create a person. Add the chat to the unmapped confirm-list for a human.
6. If more than one match: skip, do not write, add to needs-review.

Companies only get `standard_arrow_chat_jid`, same test, blank-only.

`export_contacts` is the full-reconciliation tool (weekly, or after a first-run, or if `changes_since` is unavailable). Daily runs prefer `changes_since`.

### 3. Push Attio names down (after standing approval)

Build, per account, the list of contacts whose Attio person has a real name and whose server name is empty. Call `import_names` with fill-blanks (no overwrite). Log accepted, skipped, and conflicted rows.

Until standing approval exists, write that list to `/workspace/whatsapp-attio-sync/name-down-pending/<date>.json` and mention it in the report. Do not call the write tool.

### 4. Roll up per-teammate last-contacted

From `contact_activity`, for each matched person:

- Brendan's server outbound last time → `last_contacted_brendan`
- Chris's → `last_contacted_chris`
- Gabe's → `last_contacted_gabe`
- Jillian's → `last_contacted_jillian`

Write only a later timestamp. Leave the field blank when that teammate has no outbound. This job does not write email into these fields until a later change can attribute sent mail to a specific teammate.

Also refresh `last_whatsapp_contact`, `last_whatsapp_contact_by`, and the six-field rollup owned by attio-contact-context only when this job's timestamp is later and the match is unique. If attio-contact-context and this job might collide, last-write with the later-only rule is safe: both refuse to go backwards.

### 5. Feed the context bots

Write a small handoff file `/workspace/whatsapp-attio-sync/handoff.json` that Deal Indications, attio-contact-context, and arrow-attio-firm-context can read on their next run:

- New cursors per account (`whatsapp`, `whatsapp_chris`, `whatsapp_gabe`, `whatsapp_jillian`), also copied into `/workspace/attio-sync/state.json` for Deal Indications so its cheap gate can start from this morning's cursor instead of rescanning 14 days.
- Chats with new activity since the last handoff, already deduped by phone JID, Arrow-relevant only (personal, family, and non-Arrow threads already filtered).
- Needs-review collisions and the unmapped confirm-list delta.
- Coverage snapshot.

The daily sync does not write blocks, interests, `arrow_context`, or `firm_context`. Those stay with their owner skills. It only makes their WhatsApp reads cheap and correctly identified.

### 6. Coverage report and unlock

Call `coverage_report` again. Append one row to the run log (same Deal Indications sheet is fine, or a new tab) with: time PT, messages stored per account, resolved vs LID-only directs, named vs unnamed, Attio JID fills, name-down fills, last-contacted updates, collisions, failures. Release the lock. Message Brendan only when there is a failure, a collision, or a confirm-list batch that needs a yes. Otherwise stay silent.

## Failure handling

- **One server down or unlinked:** skip it, continue the others, report which account and that it will retry tomorrow. Do not advance that account's cursor.
- **`changes_since` error or invalid cursor:** fall back to the v0.4 path for that account for this run. Reset the cursor only after a successful v0.5 page.
- **Attio write error:** retry once. If it still fails, skip that record, keep going, do not advance the cursor past items not written.
- **`import_names` partial failure:** log the failed rows, do not retry with overwrite, do not treat a partial as standing-approval for the failed subset.
- **Lock expired mid-run:** the next run may start. Steps are idempotent (blank-only, later-only, cursor advanced only after success).
- **Partial run at the 7:40am cutoff:** persist cursors only for accounts fully processed; leave the rest for tomorrow.
- **Need Brendan:** a confirm-list, a name-down first-run, a connector that needs a QR re-pair, or a collision on a person who already has deal records. Never auto-merge people in this job.

## v0.4 fallback (if a server is not on v0.5)

Use this only when the v0.5 tools are missing from `status`.

| v0.5 tool | v0.4 stand-in | Limits |
|---|---|---|
| `changes_since` | `recent_activity(hours, limit)` plus `list_chats` since the last run | `recent_activity` caps at 1000 messages and 720 hours. Page `list_chats` by last-activity and then `get_messages(chat, since=...)` for Arrow-relevant chats. |
| `export_contacts` | `list_contacts` (paginated) plus `list_chats(directs_only=true)` and `unresolved_contacts` | LID-only chats have no phone until a mapping appears. |
| `import_names` | Do not fall back to `set_contact_name` in a loop on partner servers. Hold the name-down list until v0.5, or (Brendan's server only, after a yes) a careful `set_contact_name` batch. | `import_contacts` is an address-book import, not a name-down tool. |
| `contact_activity` | Derive last outbound and inbound from `get_messages` within the 56-day window | Dates older than 56 days are gone. Per-teammate last-contacted will shrink until v0.5's surviving activity log is backfilled. |
| `resolve_person` | JID-first, phone-second Attio query as in `references/attio-schema.md` | Same collision rule. |
| `coverage_report` | `status` counts plus a local tally of mapped vs unmapped directs | Less complete, good enough for a daily health line. |

A first v0.5 run should backfill `contact_activity` from stored messages (the server does this) and then walk `export_contacts` once to fill any Attio blanks the daily increments missed.

## What this job does not do

- Send WhatsApp messages or acknowledge in Signal.
- Create, rename, or change WhatsApp groups, even as drafts.
- Create Attio people or companies.
- Merge Attio duplicates (that is a human-approved, separate pass).
- Write blocks, interests, `arrow_context`, or `firm_context`.
- Import Chris's, Gabe's, or Jillian's address books. That remains a one-off Brendan is asked about.
- Touch Brendan's personal, family, or Delta threads, or any partner's personal and family threads.

## First-run checklist

1. Done (2026-11-13): v0.5.0 is on `release` and `status` on all four accounts reports 0.5.0.
2. Done (2026-11-13): Brendan has seen this spec, and Grok Bot created the live routine (folder `whatsapp-attio-daily-sync`, first run 2026-11-14).
3. Dry-run name-down lists are approved, or standing blank-only approval is on file.
4. `/workspace/whatsapp-attio-sync/` exists, is gitignored if it would hold phone numbers in a public repo (it lives on the box, not in the knowledge graph).
5. Deal Indications, attio-contact-context, and arrow-attio-firm-context routine prompts know to read `handoff.json` when present.
6. First production run is watched: coverage before and after, cursor health, and a confirm-list of unmapped chats for Brendan.
