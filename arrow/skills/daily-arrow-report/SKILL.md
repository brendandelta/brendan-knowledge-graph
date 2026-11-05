---
name: daily-arrow-report
description: Daily 10am PT HTML email of yesterday's Arrow activity, formatted from the Arrow Weekly Log doc plus current Attio status (meetings, new people, people updates, deals and blocks moved, commitments, firm intel), emailed to the team. Formatter and sender; never scans comms itself.
---

# daily-arrow-report

**Status:** v2 FINAL draft (2026-11-05). Brendan approved this design: the report reads the log plus current Attio status. It is not scheduled. Test mode applies until Brendan approves the process.
**Source:** v1 protocol (Grokbot export 2026-10-20), rewritten 2026-11-05 so the report reads `arrow/skills/arrow-weekly-log`.

## Bot instructions (keep in sync with the live "Daily Arrow Report" profile)
- Read AGENTS.md in this repo first. The bot's own instructions are its working copy. Keep this file in sync with them. If the two disagree, keep following the bot's prompt and tell Brendan.
- Every user-facing reply that names an action, app, email, CRM record, doc or web page follows the `embed-action-links` skill: a clickable markdown deep link in the same sentence, never an invented URL.
- **Sources (only these two):**
  1. **The Arrow Log** says what happened. Read the doc in Drive folder [Arrow Weekly Log](https://drive.google.com/drive/folders/1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5) (`1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5`) for the week that contains yesterday (PT).
  2. **Current Attio status** says where things stand now: current deal stages and owners, the pipeline snapshot, record links, and the fields on new people.
  - Never read Gmail, WhatsApp, Signal, iMessage or the calendar directly. The Arrow Log bot does that.
- Everything follows the §4a Liability guard: Attio status never brings back what the log's Liability filter kept out.
- If the log and Attio disagree on a current stage or owner, Attio wins. Note the disagreement in Data notes.
- Never add a fact that is in neither source. Shorten log entries but don't reword their facts. Keep their source links.
- Never send, delete, move money or pay without Brendan's explicit confirmation, beyond the approved daily send once that approval exists.
- Routines: "Daily Arrow Report", CRON_TZ=America/Los_Angeles `0 10 * * *`. Create it only after Brendan approves the process.

Migration notes: template.html and render.py live on Grokbot's box. Copy them here when migrating. render.py needs a new `build` stage that takes `data/log-<date>.json` + `data/attio-<date>.json`, and template.html needs the section changes in §4.

# Arrow Daily Report: Protocol (v2)

Owner: Brendan Conn · Workspace: Attio "Arrow Fund" · Timezone: America/Los_Angeles (PT)

## 1. Schedule and send rules
- **When:** every day at **10:00 AM PT**. The report covers the **previous PT calendar day** (00:00:00 to 23:59:59 PT).
  - UTC window = `[D 00:00 PT, D+1 00:00 PT)`. During PDT that is `D 07:00Z → D+1 07:00Z`; during PST it is `D 08:00Z → D+1 08:00Z`. Always compute it with `zoneinfo`; never hard-code the offset.
- **To:** Chris@arrowfund.co, Gabe@arrowfund.co, vaidehi@arrowfund.co, jillian@arrowfund.co (all four in To; no CC/BCC).
- **Subject:** `Arrow Daily Report – <Weekday, Mon D>` (en dash), e.g. `Arrow Daily Report – Thursday, Oct 8`.
- **Body:** the rendered HTML from `template.html`, plus a plain-text alternative with the same content.
- **Send once per day.** If the log read or the Attio status pull fails or comes back partial, do not send a partial report. Retry up to 3 times, 10 minutes apart. If it still fails, send a short note: "Report delayed: Attio unavailable" (same recipients, same subject with "(delayed)").
- **Test mode** (where we are now): render the files, show Brendan the render in chat, and send nothing. Per DECISIONS 2026-10-29, once Brendan approves the process the report auto-sends once a day from brendan@arrowfund.co via Composio `gmail_tagish-stuss`, with no further confirmation.
- **Log freshness gate:** before building, check that the Arrow Log doc's "Last updated" line is 9:30am PT today or later. If it isn't, wait 15 minutes and recheck, up to 3 times. If it's still stale: in test mode, tell Brendan; in live mode, send the "(delayed)" note described above, with the reason "Arrow Log not updated".

## 2. Files
| File | Purpose |
|---|---|
| `template.html` | The single master template. **Its structure never changes day to day.** Page tokens `{{...}}` plus row partials between `<!--PARTIALS:BEGIN-->` and `<!--PARTIALS:END-->`. |
| `render.py` | `python3 render.py <YYYY-MM-DD> <out.html> [footer]`. Stage 1 (`build`) normalizes the raw Attio pull into `data/report-<date>.json`. Stage 2 (`render`) fills the template. All values are HTML-escaped. It fails loudly if any token is left unfilled. |
| `data/log-<date>.json` | Yesterday's entries parsed from the Arrow Log doc (audit trail). |
| `data/attio-<date>.json` | The current-status Attio pull for the day (audit trail). |
| `test-2026-10-08.html` / `.png` | Test render and screenshot. |

Screenshot: `google-chrome --headless=new --no-sandbox --force-device-scale-factor=2 --window-size=680,6200 --screenshot=out.png file://.../report.html`, then crop the trailing background.

## 3. Data gathering (log + current Attio status)
Let `D` be yesterday (PT), and `S`/`E` the UTC bounds of that PT day.

**A. Arrow Log (what happened)**
1. Find the doc titled `Arrow Log — week of <Monday of D's week>` in folder `1Ge4r1wH2tMllGqom6sMQX96xkmBk58I5`. Use `GOOGLEDRIVE_FIND_FILE` on `googledrive_left-bim` with `'<folder>' in parents`. If D is a Sunday, that's the week that started 6 days earlier.
2. Read it with `GOOGLEDOCS_GET_DOCUMENT_PLAINTEXT` on `googledocs_colius-skiv`.
3. Parse the bullets under each heading: Deals and blocks moved · New people · People updates · Meetings · Commitments · Firm intel · Data notes.
   - Keep the bullets whose date prefix is D, plus any bullet marked "updated <D>".
   - For Commitments, also keep every bullet still open that is past its due date.
4. Save the result to `data/log-<D>.json`.

**B. Current Attio status (where things stand), read-only via the Composio `attio` toolkit**
5. Team roster: `ATTIO_LIST_WORKSPACE_MEMBERS`, to map members to first names.
6. Deals: run `ATTIO_LIST_DEAL_RECORDS {limit:500}`. For each deal named in the log:
   - read its current stage, owner and size (if present). Never price, terms or counterparties (§4a);
   - add stage moves dated D that the log missed, from `ATTIO_LIST_DEAL_RECORD_ATTRIBUTE_VALUES {attribute:"stage", show_historic:true}` where `active_from ∈ [S,E)`.
   The pipeline snapshot is the count of all deals by current stage (Lead → In Progress → Won 🎉 / Lost).
7. People: for every person in the log's New people and People updates, resolve the Attio record by email or name. Read company, job title and `created_by` (who added them), and the record link. Also run the v1 query of people created in `[S,E)` to catch any new contact the log missed; those rows get the source "Attio (not in log)".
8. Meetings: run `ATTIO_LIST_MEETINGS` for `[S,E)` only to cross-check the log's Meetings list. Add any meeting that's missing, with the summary "No log entry".
9. Batch-resolve company names. Save everything to `data/attio-<D>.json`.
10. Run `render.py`.

## 4. Section rules (fixed order, never reorder)
0. **Header:** "Arrow Fund · Daily Report", then the long date and window line. Four stat tiles: **Meetings · New contacts · People updates · Deal moves**.
1. **01 Meetings**, from Log: Meetings, cross-checked with Attio. Each row: time PT · title · kind tag (External / Event / Internal) · team members · external attendees with company (unless confidential; §4a) · the log's neutral one-line outcome, else "No notes."
2. **02 New Contacts**, from Log: New people plus the Attio fields. Each row: name · company · role · added by · source (the log's "how they came in"). Then a "New company records" line from Attio.
3. **03 People Updates**, from Log: People updates. One row per person: name · firm · role · channels · Arrow team member · the log's facts (role/firm change, what they're working on or looking for, topics, asks, next steps) · Attio link. Email-only contact rows (no topics) are grouped by team member.
4. **04 Deal Movement**, from Log: Deals and blocks moved. Each row: deal/block name · From → To · size (if known) · owner (current, from Attio) · time · who changed it. Never price, terms or counterparties. Then "Notable updates", then the **Pipeline snapshot** (Attio).
5. **05 Commitments**, from Log: Commitments (any party). Show what was new or completed yesterday, plus anything open and overdue. Format: Owner to action for party · due.
6. **06 Firm Intel**, from Log: Firm intel. Public-filing marks are allowed. Never show buyer or seller identities on a live deal.
7. **Data notes**, always present. It carries the log's Data notes for D (e.g. "Signal offline"), every log-vs-Attio disagreement, items added from Attio that weren't in the log, and any exclusions.

**Empty sections** always render the `empty` partial: *"Nothing to report."* Never drop a section header.

## 4a. Liability guard (applies to everything taken from Attio status or the log)
The report goes to the whole team, so it follows the same **Liability filter** as the Arrow Log (`arrow/skills/arrow-weekly-log/SKILL.md`, items 1–11 and the writing rules). Attio status must never bring back what the log filter kept out.
- **Log content:** use log bullets as written. Never "re-expand" a neutral fallback line ("Discussed [deal] with [counterparty type]; details with Brendan") by looking the item up in Attio or anywhere else.
- **Attio note text, call transcripts and email content:** never copied in. Meeting and person summaries come only from the log's neutral one-liners. If the log has none, the row says "No notes."
- **Deals, blocks and interests from Attio:** show **name, stage, owner, size, dates and counts**. Deal names and sizes are OK (Brendan-approved 2026-11-05).
  - Never show **price** per share, share price, valuation multiples used as pricing, **terms**, fees, fee splits, finder or broker or referral compensation, or side arrangements.
  - Never show **counterparties**: the identity of a buyer or seller. Use a counterparty type such as "fund LP" or "secondary seller" instead, or omit.
  - Never show cap tables, data-room contents, or financials received under NDA.
- **People fields:** never show accreditation, KYC/AML, tax, suitability, or any personal data from Attio records (personal emails, phones and addresses included). Never repeat characterizations stored in Attio text fields.
- **No return or performance language** anywhere: no projected multiples, no "great opportunity", and nothing that could read as an offer or solicitation. Public-filing marks may appear only as factual Firm intel with the SEC link.
- **Final self-check before rendering or sending:** re-read every row against the Liability filter. Drop anything doubtful, or reduce it to the neutral fallback line, and flag it to Brendan privately in this bot's chat with the source link and rule number. Nothing about held-back items appears in the email.

## 5. Tone and accuracy
- Short, factual, third person, no adjectives or hype. Summaries are 1-2 lines (~220 characters) and are taken from the Arrow Log's neutral entries only (§4a). Never use Attio note text or transcripts.
- **Never fabricate.** If Attio has no notes, the row says so. Never infer what an email said, and never guess a company or role. The fallbacks are "Company not set", "Role not set", or the email domain labeled "(email domain)".
- All times are in PT with the "PT" label.
- Keep names as stored in Attio, with whitespace normalized.

## 6. Known limitations
- The log only covers what the Arrow Log bot can read. If Signal or iMessage is offline (Brendan's Mac), those items show up late, and Data notes says so.
- Attio never exposes email content. "What about" lines for other team members' email come from the log only when an Arrow source shows the content; otherwise the row says "Email (no content)".
- A meeting being listed in Attio does not confirm anyone attended.
- Deal stage history in Attio is reliable. Attio has no workspace-wide `updated_at` filter, so "Notable updates" covers only new deals, stage changes, and what the log reports.

## Changelog
- 2026-11-05 v2.2: go-live. People Talked To → People Updates (from log). Commitments are any-party. Deal names and sizes OK; prices, terms and counterparties never. Log is a general shared source any Arrow bot may read.
- 2026-11-05 v2.1: added the §4a Liability guard (Brendan, "no liability inducing information"). No prices, terms or counterparties; names and sizes OK. No Attio note or transcript text, a final self-check, and private flags to Brendan.
- 2026-11-05 v2: Brendan approved. The report now formats the Arrow Log plus current Attio status, and no longer scans comms. Sections 05 Commitments and 06 Firm Intel are new, and 03 People Talked To now comes from the log. There is a log freshness gate. Send terms are unchanged (DECISIONS 2026-10-29).
