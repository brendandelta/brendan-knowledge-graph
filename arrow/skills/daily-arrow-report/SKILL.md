---
name: daily-arrow-report
description: Daily 10am PT HTML digest of yesterday in Attio (meetings, new contacts, people talked to, deal moves) emailed to the team.
---

# daily-arrow-report

**Status:** DRAFT on Grokbot; never scheduled (awaiting approval). Protocol v1 below.
**Source:** Grokbot export 2026-10-20 (`references/grokbot-export-2026-10-20.md`). Text below is the running prompt, lightly reformatted. Edit here, then update the engine.

## Bot instructions (synced 2026-10-29 with the live Grokbot "Daily Arrow Report" profile)
- Read AGENTS.md in this repo first. The bot's own instructions are its working copy. Keep this file in sync with them. If the two disagree, keep following the bot's prompt and tell Brendan.
- Every user-facing reply that names an action, app, email, CRM record, doc or web page follows the `embed-action-links` skill: a clickable markdown deep link in the same sentence, never an invented URL.
- Source of truth is Attio only. Signal, WhatsApp, iMessage and Gmail are not report sources unless Brendan says so.
- Never send, delete, move money or pay without Brendan's explicit confirmation. The daily send waits on Brendan's approval of the Oct 8 test, and it is still undecided whether each morning's email auto-sends or is drafted for him to send.
- Routines: none (not scheduled).

Migration notes: not running. template.html and render.py live on Grokbot's box; copy them here when migrating.

# Arrow Daily Report: Protocol (v1)

Owner: Brendan Conn · Workspace: Attio "Arrow Fund" · Timezone: America/Los_Angeles (PT)

## 1. Schedule and send rules
- **When:** every day at **10:00 AM PT**. The report covers the **previous PT calendar day** (00:00:00 to 23:59:59 PT).
  - UTC window = `[D 00:00 PT, D+1 00:00 PT)`. During PDT that is `D 07:00Z → D+1 07:00Z`; during PST it is `D 08:00Z → D+1 08:00Z`. Always compute it with `zoneinfo`; never hard-code the offset.
- **To:** Chris@arrowfund.co, Gabe@arrowfund.co, vaidehi@arrowfund.co, jillian@arrowfund.co (all four in To; no CC/BCC).
- **Subject:** `Arrow Daily Report – <Weekday, Mon D>` (en dash), e.g. `Arrow Daily Report – Thursday, Oct 8`.
- **Body:** the rendered HTML from `template.html`, plus a plain-text alternative with the same content.
- **Send once per day.** If the Attio pull fails or comes back partial, do not send a partial report. Retry up to 3 times, 10 minutes apart. If it still fails, send a short note: "Report delayed: Attio unavailable" (same recipients, same subject with "(delayed)").
- **Test mode** (where we are now): render the files only and send nothing.

## 2. Files
| File | Purpose |
|---|---|
| `template.html` | The single master template. **Its structure never changes day to day.** Page tokens `{{...}}` plus row partials between `<!--PARTIALS:BEGIN-->` and `<!--PARTIALS:END-->`. |
| `render.py` | `python3 render.py <YYYY-MM-DD> <out.html> [footer]`. Stage 1 (`build`) normalizes the raw Attio pull into `data/report-<date>.json`. Stage 2 (`render`) fills the template. All values are HTML-escaped. It fails loudly if any token is left unfilled. |
| `data/attio-<date>.json`, `data/attio-<date>-extra.json` | The raw Attio pull for the day, kept as an audit trail. |
| `test-2026-10-08.html` / `.png` | Test render and screenshot. |

Screenshot: `google-chrome --headless=new --no-sandbox --force-device-scale-factor=2 --window-size=680,6200 --screenshot=out.png file://.../report.html`, then crop the trailing background.

## 3. Data gathering (Attio only, via the Composio `attio` toolkit)
Run the steps through `COMPOSIO_MULTI_EXECUTE_TOOL`. For bulk work, use `COMPOSIO_REMOTE_WORKBENCH` with `run_composio_tool`. Let `S` and `E` be the UTC bounds of the PT day.

1. **Team roster:** `ATTIO_LIST_WORKSPACE_MEMBERS` maps member id/email to first name. Current members: Brendan, Chris, Gabriel, Jillian, Vaidehi. A "team member" is any `@arrowfund.co` address.
2. **Meetings:** `ATTIO_LIST_MEETINGS {ends_from:S, starts_before:E, timezone:"America/Los_Angeles", sort:"start_asc", limit:200}`. Keep paging while `data.pagination.next_cursor` is set.
   - Normalize: a timed meeting uses `start.datetime`/`end.datetime` (UTC ISO plus an IANA `timezone`). An all-day meeting uses `start.date`/`end.date`, and **end is exclusive** (RFC 5545). Include an all-day event only if `start.date ≤ D < end.date`.
   - Post-filter strictly with `start < E and end > S`, then sort by start.
   - For each meeting, call `ATTIO_LIST_CALL_RECORDINGS {meeting_id}`. If a recording exists, call `ATTIO_GET_CALL_RECORDING` to get the transcript.
3. **Notes:** `ATTIO_LIST_NOTES {limit:50, offset:n}`, paging until a page comes back with fewer than 50. The API does **not** reliably return newest-first, so page everything, then filter on `created_at ∈ [S,E)`. Join a note to a meeting by `meeting_id`, or else by `parent_record_id` = a meeting participant's person record.
4. **New contacts:** `ATTIO_POST_V2_OBJECTS_OBJECT_RECORDS_QUERY {object:"people", filter:{$and:[{created_at:{$gte:S}},{created_at:{$lt:E}}]}}`. Do the same for `companies` to get the "New company records" line.
   - Who added: `created_by`. A `workspace-member` means "Added manually by <name>". `system` means auto-created by Attio. In that case, the source is "Email sync – <owner>'s mailbox" when `first_email_interaction` falls in the window; otherwise it is "Auto-created by Attio (no source recorded)". Also check the custom `source` attribute.
   - List adds: `ATTIO_POST_V2_LISTS_LIST_ENTRIES_QUERY {list, filter:{created_at in window}}` for every list from `ATTIO_LIST_LISTS`. Append "added to <List>" to the source.
5. **People talked to:**
   - Email: query `people` with `last_email_interaction.interacted_at ∈ [S,E)`. The team member is `owner.id`, i.e. whose mailbox.
   - Meetings: the external participants of the day's meetings, matched to person records by email. A cross-check is `last_calendar_interaction.interacted_at ∈ [S,E)`.
   - Notes: the parent person records of the day's notes.
   - Merge by normalized name, so duplicate records collapse into one row. Exclude automated senders using the domain/local-part rules in `render.py` (`is_automated`): Luma, DocuSign, newsletters, `e.`/`mail.`/`events.` subdomains, `support@`, `offerings@`, etc.
6. **Deals:** `ATTIO_LIST_DEAL_RECORDS {limit:500}`. For each deal, call `ATTIO_LIST_DEAL_RECORD_ATTRIBUTE_VALUES {record_id, attribute:"stage", show_historic:true}`.
   - A **deal move** is a historic stage value whose `active_from ∈ [S,E)` and which has a previous value. The move goes from the previous `status.title` to the new one, and `created_by_actor` tells you who changed it.
   - **Notable updates:** deals with `created_at` in the window. Optionally, the same history check on `owner` and `value`.
   - **Pipeline snapshot:** a count of deals by current stage.
   - Pipeline shape (discovered): object `deals`, attribute `stage` (status), with stages **Lead → In Progress → Won 🎉 / Lost**. Owner is `owner` (actor-reference), value is `value` (currency), plus `associated_company` and `associated_people`. There is no separate deal pipeline list; all 16 lists are people lists.
7. **Company names:** batch-resolve referenced company ids with `ATTIO_POST_V2_OBJECTS_OBJECT_RECORDS_QUERY {object:"companies", filter:{record_id:{$in:[...]}}}`.
8. Save the raw pulls to `data/attio-<date>.json`, then run `render.py`.

## 4. Section rules (fixed order, never reorder)
0. **Header:** "Arrow Fund · Daily Report", then the long date and the window line. Below that, four stat tiles in a fixed order: **Meetings · New contacts · Talked to · Deal moves**.
1. **01 Meetings:** every in-window meeting across all team calendars, sorted by start.
   - Each row shows: time in PT (plus the local time when the meeting's timezone is not PT/UTC), title, a kind tag, the team members, the external attendees with company, and a summary.
   - Kind tag: `External` means a non-Arrow attendee. `Event` means a Luma/Partiful invite or a HOLD. `Internal` means team only.
   - Summary priority: note text first, then the transcript, then "<Luma/Partiful> event · <venue>. No notes in Attio." (for events), then "No notes or call recording in Attio."
2. **02 New Contacts:** real people created in the window. Each row shows: name · company, role (job title, else the record description, else "Role not set"), added by, and source.
   - Automated senders and duplicate aliases are left out of the list and counted in Data notes.
   - Follow the list with a "New company records" line.
3. **03 People Talked To:** two parts.
   - First, "Meetings & notes": one row per person with channels (Meeting / Email / Note), the team member, and a 1-2 line summary from notes.
   - Then "Email · <Team member> (n)": compact rows of name · company and last interaction time in PT.
   - Ends with the caption that email content is not available.
4. **04 Deal Movement:** stage moves as `Deal`, `From → To`, owner, time, and who changed it. Then "Notable updates" (new deals and other changes), then "Pipeline snapshot".
5. **Data notes:** always present. It lists every gap, exclusion, and Attio limitation that affected the day's report.

**Empty sections** always render the `empty` partial: *"Nothing to report."* Never drop a section header.

## 5. Tone and accuracy
- Short, factual, third person, no adjectives or hype. Summaries are 1-2 lines (~220 characters) and are taken or condensed from Attio note or transcript text only.
- **Never fabricate.** If Attio has no notes, the row says so. Never infer what an email said, and never guess a company or role. The fallbacks are "Company not set", "Role not set", or the email domain labeled "(email domain)".
- All times are in PT with the "PT" label.
- Keep names as stored in Attio, with whitespace normalized.

## 6. Known Attio limitations (as of Oct 2026)
- **Email content is never exposed** by the Attio API (the `email:read` scope is metadata-only, and the current connection does not have it). We only know that an email was exchanged, when, and in whose mailbox.
- `last_email_interaction` holds only the latest interaction. If the report runs at 10 AM, anyone emailed yesterday *and again this morning* drops out of yesterday's list. An `email:read` scope or a webhook-based daily log would fix this.
- Call recordings and transcripts only exist if Attio's call recorder is in use. As of Oct 8, 2026 there were none.
- RSVP status is often `pending` for team members. A meeting being listed does not confirm anyone attended.
- Deal stage history is reliable. Other "record updated" activity has no workspace-wide `updated_at` filter (Attio does not support it), so notable updates are limited to new deals and stage changes.
