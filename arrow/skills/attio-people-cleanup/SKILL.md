---
name: attio-people-cleanup
description: Weekly Arrow Fund Attio People cleanup. Auto-delete obvious machine-sender and service-desk junk with a live protection re-check and a snapshot before each delete, send borderline records to Brendan for approval, never touch the keep list, and track repeat senders to block in Attio email sync.
---

# Attio People junk cleanup (weekly)

Workspace: Arrow Fund Attio (`arrow-fund`), via Composio.
Reference classifier: `/workspace/attio-cleanup/classify.py`. Brendan's do-not-touch list: `/workspace/attio-cleanup/keep-list.json`.
Approved by Brendan 2026-11-13. First approved cleanup run on that date deleted 210 records (log: `deleted-2026-11-13.csv`).

Goal: remove machine senders and service desks (Attio, Chase alerts, no-reply, newsletters, calendar relays) from People, never a real person and never anything a teammate has touched. Keep Arrow material siloed: use only Arrow connections (Arrow Attio and Arrow Gmail).

## 1. Data the bot must read on every run (all read-only)

| Data | How | Used for |
|---|---|---|
| All People records | `ATTIO_PEOPLE_LIST_PERSONS`, limit 500, page by offset until a short page | name, emails, company, every protective field |
| Company names | `POST /objects/companies/records/query` (paged) | company name for the report and name-equals-company checks |
| Notes | `GET /notes` (limit 50, paged to exhaustion) | `parent_object=people` record ids with a note |
| Tasks | `GET /tasks` (limit 500, paged) | `linked_records` record ids |
| People lists | `GET /lists`, then `POST /lists/{slug}/entries/query` for every list whose parent is `people` | list membership |
| Cross-object references | `POST /objects/{blocks,interests,deals,companies}/records/query`, collect every record-reference whose `target_object` is `people` | links that may not have a People-side inverse |
| Workspace members | `GET /workspace_members` | owner / creator names |

If any of these reads fails or comes back truncated, the run stops and deletes nothing.

## 2. Junk signals (computed per email address, then per record)

**M. Machine sender local part** (whole local part, optional `+tag`, case-insensitive): `no-reply`, `noreply`, `do-not-reply`, `donotreply`, `do_not_reply`, `auto-noreply`, `notification(s)`, `notify`, `alert(s)`, `*alerts`, `billing`, `invoice(s)`, `receipts`, `statements`, `payables`, `support`, `help`, `helpdesk`, `customer service/support/care/success`, `care`, `cs`, `newsletter(s)`, `news`, `breakingnews`, `update(s)`, `digest`, `marketing`, `communications`, `promo(tions)`, `offers`, `insights`, `mailer-daemon`, `postmaster`, `bounce(s)`, `mailer`, `calendar`, `calendar-notification/invite/server`, `invitation(s)`, `feedback`, `recommendations`, `recommended-*`, `security`, `verify`, `verification`, `recovery`, `automated`, `system`, `robot`, `bot`/`*bot` (not on gmail/yahoo/outlook/icloud/etc.), `confirm(ation)s`, `welcome`, `subscription(s)`, `unsubscribe`, `survey(s)`, `rewards`, `order(s)`, `booking`, `reservation(s)`, `listings`, `webcasts`, `onlinebanking`, `onlinebk`, `the-messenger`, `share`, `ship`, `dse`/`dse_naN` (DocuSign), `adobesign`, `proxymaster`, `conferenceroom*`, `room-*`, `return(s)`.
Also machine if the local part contains `no-reply`/`noreply`/`no.reply`, `do-not-reply`, `notification`, `mailer-daemon`, `bounce`, `newsletter`, `unsubscribe` anywhere.
Also machine if the local part equals the sending brand's root domain (`ebay@ebay.com`, `chase@mcmap.chase.com`), but only when the record name is empty, a brand, or a service, never when it is a person's name.

**B. Bulk or relay mail domain**: first DNS label is a sending subdomain (`e, em, email, mail, mailer, news, info, reply, notification(s), notify, message(s), engage, send, alerts, marketing, feedback, explore, discover, inspire, c, t, m, mcmap, camail, eumail, nuwelcome, customers, team, comms, updates, mg, bounce, go, express, calendar, events, fin, tsg, mkt, eq, mail-production, contact, ms`) and the domain has at least 3 labels; or the domain is a known ESP or relay: `luma-mail.com` (except `user.luma-mail.com`), `lu.ma`, `docusign.net/.com`, `adobesign.com`, `stellaconnect.net`, `jobberemail.com`, `getinvoicesimple.com`, `msgsndrreach.com`, `pxsmail.com`, `medallia.com`, `sendgrid.net`, `mcsv.net`, `mailchimp.com`, `hubspotemail.net`, `intercom-mail.com`, `amazonses.com`, `substack.com`, `beehiiv.com`, `*.onmicrosoft.com`, `notices-*.com`.
`user.luma-mail.com` is the masked address of a real event attendee and is NOT junk by itself.

**R. Generic role inbox** (a real small firm may answer it): `hello, hi, info, team, contact, admin, office, event(s), community, media, press, sales, enterprise, partners, invest(ments/ors), ir, deals, account(s), member(s), membership, jobs, careers, recruiting, hr, legal, service(s), inquiries, enquiries, reception, general, mail, email, reply, start, us, plus, guest, operations, ops, apps, finance, shipping, dispatch, concierge, data, advisory, trademarks, submissions, social, howdy, platformteam, offerings, for-you, licensing, project.*, dg-*`, plus short non-dotted local parts on government domains (`.gov`, `.gov.xx`, `.state.xx.us`) when the record has no person name.
Role inboxes are never auto-deleted on their own.

**N. Name signals**
- Brand name: the record name (minus "The", "Inc", "Team", "Support") is on the curated consumer/SaaS brand list in `classify.py` (`BRANDS`: Attio, Chase, Google, Carta, Mercury, Gusto, Shopify, Tesla, eBay, Pinterest, NYT, WSJ, SVB, First Citizens, and so on). Add brands to that list; do not loosen the pattern.
- Service words in the name: customer service/support, help desk, notification(s), alert(s), billing, invoice, receipts, newsletter, digest, team, insights, rewards, account services, accounts payable, event(s), news, updates, calendar, no reply, bot, admin, business team.
- Firm words in the name: Inc, LLC, Ltd, Corp, Bank, Partners, Capital, Ventures, Holdings, Group, Advisors, Fund, Foundation, Gallery, Financial, Hotel, Services, Communications, Initiative, Airlines, NYC, School, Tour, Post, Series, Summit, Dinner, Club, and similar.
- Person name: 2 to 4 capitalized name tokens with no brand, service, or firm word.

## 3. Protective checks

**Hard protections.** Any one of these means the record is never auto-deleted:
- People links: `sourced_blocks_contact`, `brokered_blocks_contact`, `brokered_interests`, `associated_interests_8`, `associated_deals`, `referrals_4`, `referrals_6`, `companies_of_interest`, `liquidity_holdings`, `introduced`, `introduced_by`, `close_with`.
- Cross-object references from Blocks (`seller_contact_link`, `intermediary_contact_link`, `intermediaries_2`, `referrals_source`), Interests (`intermediary`, `associated_contacts`, `referrals_source`, `external_owner`), Deals (`associated_people`), and Companies (`key_people`, `close_contacts`). Companies `team` is auto-populated by domain and is NOT protective.
- LP / intake data: `check_size_range`, `investment_structure`, `decision_timeline`, `liquidity_interest`, `notable_past_investments`, `submitted_website_inquiry`, `inquiry_role`, `latest_inquiry_message`, `accredited_self_reported`, `approval_for_lp_portal`, `source`.
- WhatsApp and teammate contact: `whatsapp_phone_jid`, `whatsapp_lid`, `standard_arrow_chat_jid`, `last_whatsapp_contact`, `last_contacted_brendan/chris/gabe/jillian/vaidehi`.
- Membership in any People list (Priority lists, Website Inquiries, Outreach, Prospects, event lists, and so on). Qualified Blocks / Qualified Interests are covered through the Block and Interest links above.
- Any Attio note or task on the record.
- `contact_owner` set, `lp_fo_ria` (What they are) set, or any `tags` (including Blacklist, which is kept on purpose).
- Any email on `arrowfund.co`, a look-alike of it (`arowfund.co`), or "Arrow" in the record name.

**Soft protections.** These block auto-delete unless every address on the record is a machine sender or a bulk/relay domain:
- a phone number;
- a calendar meeting (`last_` or `next_calendar_interaction`), since calendar bots get these too;
- created by a workspace member, not by the email sync;
- `arrow_context` filled.

**Not protective:** Attio's auto-derived `last_contacted`, `last_email_interaction`, connection strength, and LinkedIn/avatar enrichment. These get filled for no-reply senders too.

## 4. Decision order (first match wins)

1. No junk signal: **KEEP**.
2. A hard protection or an Arrow domain, and the name is a person's with at least one personal-looking address (or a role inbox but no machine address): **KEEP**. A protected human with one stray machine address on the record is still a human.
3. A hard protection and at least one personal-looking address, with no brand or service name: **KEEP** (for example "Simon X / Omni Ventures").
4. Any other hard protection, Arrow domain, Arrow look-alike, "Arrow" in the name, or an empty shell (no name and no email): **BORDERLINE**.
5. No email, a soft protection, and not a brand name (a hand-entered placeholder such as "Adam (Saudi defense fund)"): **KEEP**.
6. The name matches a protected contact's name and the address is a relay or machine address (a likely alias): **BORDERLINE**.
7. A finance-ops desk (`billing`, `invoices`, `payables`, `statements`, `receipts`, `accounts`) at a domain where Attio has other named people: **BORDERLINE**. It could be an LP's or portfolio company's AP desk.
8. Every address is machine or bulk/relay, but the name is a person's and their name is in the local part (a Tesla advisor on `reply.tesla.com`, a newsletter author): **BORDERLINE**.
9. Every address is machine or bulk/relay: **JUNK_AUTO**. Soft signals are ignored here.
10. Brand name with no personal-looking address (or brand plus a role inbox), and no soft signal: **JUNK_AUTO**.
11. Person name with a personal-looking address and no machine or bulk address: **KEEP** (the name equals the company only because the company came from a personal domain).
12. Person name on a role inbox with a soft signal: **KEEP** (a named human at their own firm's hello@ or info@ that a teammate added or met).
13. Anything else with a junk signal: **BORDERLINE**.

## 5. Weekly run procedure

1. **Read** all inputs in section 1. If any read fails or is truncated, stop and tell Brendan. Delete nothing.
2. **Load the keep list** (`keep-list.json`). Any record_id on it is skipped entirely: never deleted, never re-sent as borderline, never edited.
3. **Classify** every remaining People record with sections 2 to 4 into JUNK_AUTO, BORDERLINE, and KEEP.
4. **Volume guard.** Let N be this run's JUNK_AUTO count and P be last week's deleted count (from the previous run log; 210 for the 2026-11-13 batch, but compare against normal weekly runs once one exists).
   - If N > 50, delete nothing automatically. Send Brendan the full list and ask him to approve the batch.
   - If P > 0 and N > 3 x P, stop the same way and send the list.
5. **Snapshot, then delete each JUNK_AUTO record, one at a time:**
   - Re-read the record right before deleting it (`GET /v2/objects/people/records/{id}`), plus its list entries (`GET /v2/objects/people/records/{id}/entries`), notes (`GET /v2/notes?parent_object=people&parent_record_id={id}`), tasks (`GET /v2/tasks?linked_object=people&linked_record_id={id}`), and fresh Blocks, Interests, Deals, and Companies references.
   - Re-run every hard protection. If any soft signal appeared that was not there at classification time, skip the record and log it.
   - Write the full JSON (record, list entries, notes, tasks, timestamp) to `/workspace/attio-cleanup/snapshots/{record_id}.json` before deleting.
   - Delete with `DELETE /v2/objects/people/records/{id}`. Then confirm with a GET that it returns 404.
   - Append to `deleted-YYYY-MM-DD.csv`: record_id, name, emails, group, reason, snapshot_path, deleted_at.
6. **Borderline goes to Brendan.** Send him new BORDERLINE records (not on the keep list and not already sent and pending), each with its Attio link (`https://app.attio.com/arrow-fund/person/{id}/overview`), emails, reason, and protective signals. Delete only the ids he approves, one at a time, with the same re-check and snapshot. Ids he says to keep are added to `keep-list.json`. Never delete, merge, or rename a borderline record without his explicit approval for that record.
7. **Track repeat senders.** Keep `repeat-senders.json` (sender address and domain to a list of dates deleted). If an address or domain is deleted in two or more weekly runs, Attio's email sync is re-creating it. Recommend to Brendan that he block it in Attio's email sync settings (Settings > Email and calendar > excluded addresses or domains), listing each repeat sender and how many times it came back. Do not change Attio settings yourself.
8. **Report** to Brendan: counts deleted, skipped (and why), borderline sent for review, repeat senders to block, and the log path. Use clickable Attio links. No em dashes.
9. **Restore path.** A wrongly deleted record can be recreated from its snapshot JSON (name, emails, phone, company, and other fields with `POST /v2/objects/people/records`). Re-add list entries from the snapshot. Email history re-syncs from Attio on its own.

## 6. Never

- Never delete anything on the keep list, anything with a hard protection, or any arrowfund.co address.
- Never delete without a snapshot written first.
- Never edit fields, merge records, or change Attio settings in the weekly job. Renames happen only when Brendan approves them, and touch only the `name` attribute.
- Never auto-delete more than 50 in one run.

## 7. History

- 2026-11-13: dry run over 4,313 People found 201 JUNK_AUTO, 111 BORDERLINE, and 4,001 KEEP. Brendan approved deleting the 201, plus the 7 brand-named oddities and 2 fully empty records. All 210 were deleted after the live re-check (0 skipped). He also approved renaming the firm-named records where the real name is certain (8 renamed, 15 left as they were), and keeping the remaining 102 borderline records (now `keep-list.json`).

## 8. Schedule and owner

Owned by the Attio Cleanup bot. Weekly routine "Weekly Attio People cleanup" runs Sundays around 6:00 PM PT, flexible ±15 (`0 18 * * 0`, first scheduled run 2026-11-15). It reports to Brendan in that bot's Grok Bot chat and stays silent when nothing was deleted and nothing needs review.
