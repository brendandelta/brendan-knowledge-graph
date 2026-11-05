---
name: arrow-attio-firm-context
description: Keeps Arrow's Attio Company records current with dated, sourced firm intel (firm type, holdings and marks, role for Arrow, key people) scraped from Arrow Gmail, Arrow WhatsApp groups, Signal (incl. "Data OS input"), Arrow calendar and SEC data. Companies only; never people. Propose-then-approve until Brendan switches it to auto.
---

# arrow-attio-firm-context

**Status:** v1.1 FINAL draft (2026-11-05). Approved for go-live (PROPOSE mode). Not running until the "Arrow Attio Firms" bot and its routine are created. Every write is shown to Brendan and waits for his yes. No firm data or notes have been written yet; StepStone below is only an example (`arrow_role` = Seller, confirmed).
**Source:** structure mirrors `arrow/skills/attio-contact-context/SKILL.md` v2.2, adapted for the Attio `companies` object.
**Bot:** "Arrow Attio Firms" (new bot, see contact-context-split-plan.md).

## Migration notes
- New skill. Twice-rule basis: firm intel has come up repeatedly (StepStone marks via the Signal "Data OS input" chat, the N-PORT scraper's holder data, the Attio backfill learning about firms while enriching people). The people skill can't hold it (its silos ban marks and Attio notes).
- Approved by Brendan 2026-11-05:
  - (a) the seven company attributes below. They were created that day with their select options.
  - (b) public-filing marks may be stored on company records.
- Still pending: switching from PROPOSE to AUTO, which is Brendan's call, later.

## Purpose
When a firm comes up in Arrow comms or filings, the Attio company record should say, with a date and a source link: what kind of firm it is, what it holds and at what marks, what it is to Arrow (seller, buyer, co-investor, competitor, intermediary), and who the key people are. The team should be able to open the company in Attio and get it in 20 seconds.

## Scope
- Object: `companies` only (Attio workspace `arrow-fund`). Never people (that's arrow-attio-people / attio-contact-context), never deals, blocks or interests.
- Arrow involvement test (same spirit as the people skill): the firm (a) appears in an Arrow source (@arrowfund.co mail, an Arrow WhatsApp group, the Signal "Data OS input" chat or another Arrow Signal group, an Arrow calendar event with ≥1 @arrowfund.co attendee), AND (b) is Arrow-relevant: holds/marks/trades private shares, is a fund/LP/allocator/intermediary/bank, or is linked to a block, interest or deal.
- Brendan-only items (no other Arrow team member, no Arrow record link, not in an Arrow source) are never added or suggested. Personal, Delta and Janey material is never read into this process (filter: arrow-weekly-log §Filter).
- Existing records only. Creating a new company record is always a PROPOSE item, even in AUTO mode.

## Attio companies schema (read 2026-11-05, read-only)
Existing, writable, relevant: `name`, `domains`, `description` (system), `categories` (multi-select, Clearbit-style: Finance, Financial Services, B2B…), `tags` (select; options are messy: Senahill, 3D Printing, Accounting…), `team` (people, inverse of person.company), `close_contacts` (people, inverse of person.close_with), `associated_deals`, `fund_investments` (deals via investing_fund_link), `brokered_deals` (deals via intermediary_company_link), `sourced_blocks_fund` (blocks via seller_fund_link), `sourced_blocks` (blocks via intermediary_firm), `linkedin`, `primary_location`.
Missing: anything for firm type, Arrow role, holdings, marks, intel date or source. So the skill needs new attributes.

## Firm-intel attributes (created 2026-11-05 on Companies, approved by Brendan)
| api_slug | Title | Type | Rule |
|---|---|---|---|
<!-- attribute_ids: firm_type 0e658a48-54f0-455a-a098-85b863003bb7 · arrow_role 1e230418-b6b3-480c-8648-729c49201b16 · firm_context bcc8f111-80cf-4cde-b588-3007c02d3661 · holdings_marks 02113d60-5598-4eae-9b26-8e3961912521 · key_people 54532ddf-9526-44ce-9220-74db69e5f70e · firm_intel_updated cd6a8606-71bc-478e-836f-7e0ccd83a55f · firm_intel_source 915c4f03-0809-4dd4-884b-fb6a7f0e3ec5 -->
| `firm_type` | Firm type | select (single) | VC · Growth / PE · Secondaries fund · Fund of funds / Allocator · Interval / Evergreen fund · Crossover / Hedge fund · Family office · RIA / Wealth manager · Private bank / Broker-dealer · Placement agent / Intermediary · Sovereign / Pension / Endowment · Corporate / Strategic · Operating company (portfolio co). Set when empty; change only if the current value was set by this process. |
| `arrow_role` | Role for Arrow | multi-select | Seller · Buyer · Co-investor · Competitor · Intermediary / Source · LP prospect · Service provider. Add-only. Never remove a human-set option. |
| `firm_context` | Firm context | text (≤700 chars) | Rewritten in full each update, newest facts first, same style as person `arrow_context`. Facts only: what they are, what they do with Arrow, what they hold. Ends with "Updated YYYY-MM-DD". |
| `holdings_marks` | Holdings & marks | text (≤700 chars) | Latest known position per portfolio company, one line each: `Company — $size @ $price/sh (as of YYYY-MM-DD, source)`. Public filings (N-PORT, N-CSR, Form D, 13F, published reports) only, unless Brendan approves a private source item by item. |
| `key_people` | Key people | record-reference → people (multi) | Add-only. Only people already in Attio. Never creates people. |
| `firm_intel_updated` | Firm intel updated | date | Set by this process on every write. |
| `firm_intel_source` | Latest intel source | text (URL) | Link to the newest source used (SEC URL, Gmail permalink, Drive file, "Signal: Data OS input YYYY-MM-DD"). |

Notes:
- `key_people` is a one-way reference to People, with no reverse attribute created on People.
- To add a new select option, Brendan must approve it first. The process never creates one on its own.

## Dated note (history)
Each time new intel lands, add ONE Attio note on the company (parent_object `companies`). Notes are append-only history; the attributes are the current snapshot.

Title: `Firm intel · YYYY-MM-DD · <source type>`
Body template:
```
Firm type: <value> (new | unchanged)
Role for Arrow: <values> — <why, one line>
Holdings & marks (as of <period date>):
  • <Company> — $<size> @ $<price>/sh (prev $<price> as of <date>, <x>× / <±%>) — <source link>
Key people: <Name (title)> [Attio link] …
What changed: <one line>
Sources: <link 1>, <link 2>
Added by Arrow Attio Firms (proposed <date>, approved by Brendan <date> | auto)
```
Rules: no quotes of messages, no buyer/seller identities from a live deal, no Arrow terms/fees, no personal or Delta material. Numbers only from a source opened this run; never estimated.

### Example: StepStone Group (record 571bedd8-ff12-487a-899b-21d991d18b10)
[StepStone Group in Attio](https://app.attio.com/arrow-fund/company/571bedd8-ff12-487a-899b-21d991d18b10). Existing record (Clearbit data, Connection strength "Very weak"); no firm intel today.
- `firm_type`: Fund of funds / Allocator (StepStone Private Venture & Growth Fund "SPRING" is an evergreen N-PORT filer; adviser StepStone Private Wealth LLC, sub-adviser StepStone Group LP).
- `arrow_role`: **Seller** (confirmed by Brendan 2026-11-05). (They also hold top-100 names as an allocator; seller is the role for Arrow.)
- `holdings_marks` (from nport.db, SEC N-PORT for SPRING, period 2026-06-30, [filing](https://www.sec.gov/Archives/edgar/data/1918642/000119312526376298/primary_doc.xml)):
  - Databricks — ~$308.9M @ $222.74/sh (prev $190.00 at 2026-03-31)
  - Hadrian — ~$16.1M @ $70.65/sh (prev $18.39 at 2026-03-31; ~3.8× in one quarter)
  - Poolside — $12.2M @ $76.21/sh in SPRING at 2026-06-30. **Brendan's figure is $116M.** That doesn't match SPRING's N-PORT. It probably comes from the Data OS input chat or totals several StepStone vehicles. Verify before writing.
- Note title: `Firm intel · 2026-11-05 · SEC N-PORT + Signal (Data OS input)`.

## Silos (never write)
- Never write person records, deals, blocks, interests or lists.
- **Exception to the people-skill silo (approved by Brendan 2026-11-05):** marks and position sizes from PUBLIC filings may go in `holdings_marks` and firm-intel notes. Private marks from comms (e.g., a broker's indication in Signal) are PROPOSE-only every time, with the source named. Never write Arrow's own pricing, terms, fees, or the identity of a buyer/seller on a live Arrow deal.
- Never quote a message. Summarize facts.

## Never
- Never overwrite human-entered data (Human-edit guard).
- Never create people. Never create companies without approval.
- Never send, post, email or message anyone. Results go to Brendan in this bot's chat only.
- Never write to the weekly log doc (that's arrow-weekly-log; it reads Attio changes from this process instead).
- Never read personal/Delta/Janey sources.

## Human-edit guard
Before every write, read the attribute's current value with `created_by_actor`. Write only if the value is empty, or its actor is this process's own actor (ATTIO_CONTEXT_TOKEN actor if set, else the Composio Attio app actor `b07dc751-ba38-4209-82ca-f2bd28526da4`). Otherwise skip, and put "conflict: human value X vs found Y" into the proposal. Multi-selects and record-references: add only. Use PATCH, never PUT.

## Sources (read-only)
1. **SEC (public):** `/workspace/nport/nport.db` (fund_holdings, spvs, fund_managers, person_scores), built by N-PORT Scraper. Read only; never write that DB. New quarterly sets get picked up after N-PORT Scraper's monthly refresh on the 1st.
2. **Arrow Gmail:** brendan@arrowfund.co via Composio `gmail_tagish-stuss`. Other team inboxes only through Attio email metadata.
3. **WhatsApp (whatsapp MCP), Arrow groups only:** groups whose name contains "Arrow"/"ArrowFund"/"Arrow Fund", or that include a non-Brendan Arrow team member, or that sit in `/workspace/attio-firms/arrow-groups.json` (allowlist, Brendan-approved). Example current Arrow groups: "Kells Capital <> Arrow fund", "Rift Capital <> Arrow Fund", "William Blair x Arrow Fund", "Arrow Fund & D Boral Capital", "Y Invest x Arrow". Ambiguous groups ("Velvet, The Underground I Sundance 2026", "BM 2026 AXIS MUNDI", "Long Feng Community", "dealio", "Poly") are skipped unless allowlisted.
4. **Signal (Signal MCP), Arrow chats only:** the "Data OS input" chat (primary firm-intel feed) plus allowlisted Arrow groups. Reachable only while Brendan's Mac is online.
5. **Arrow calendar:** Attio meetings (`ATTIO_LIST_MEETINGS`) with ≥1 @arrowfund.co participant, and the firm-domain attendees.
6. **Comms index:** `/workspace/comms-index/` (read-only, written by Comms Ingest) for the Arrow-tagged slice when present.
7. **Attio:** company record, its deals/blocks/interests links, people at the company (`team`) for key-people candidates.

## Access
- Composio: Attio `attio_bosser-wooded` (workspace arrow-fund); Gmail `gmail_tagish-stuss`.
- If Signal MCP `status` fails (Mac offline): skip Signal, log "signal: offline, catch-up pending since <ts>", keep the Signal cursor where it was, and catch up on the next run that can reach it. Never treat offline as "nothing new".

## State and log
- State (cursor): `/workspace/attio-firms/state.json` — `{gmail_ts, whatsapp_ts, signal_ts, signal_pending_since, calendar_ts, nport_quarter_seen, proposals_open:[…]}`.
- Proposal queue: `/workspace/attio-firms/proposals.json` (id, company record_id, attribute → old/new, note draft, sources, status proposed|approved|rejected|written).
- Log: `/workspace/attio-firms/log.md` (one line per run: time PT, sources read, firms found, proposals, writes, skips, conflicts).
- Lock: `/workspace/attio-firms/lock` (skip the run if a lock younger than 40 min exists).

## Event run
1. Pull: read state; pull each source since its cursor (Gmail, WhatsApp Arrow groups, Signal Arrow chats if reachable, Attio meetings, nport.db if a new quarter).
2. Extract firm mentions: org names, domains, fund/vehicle names, SEC filer names. Resolve each to an Attio company (domain first, then exact name, then fuzzy with a confidence score). Unresolved Arrow-relevant firms go in a "new company?" proposal.
3. For each resolved firm, build the delta: firm_type, arrow_role (with the evidence), holdings_marks (public sources), key_people (existing Attio people with that company, or named in an Arrow source), firm_context rewrite, and the dated note.
4. Run the Human-edit guard. Drop anything already known (same value and same source).
5. PROPOSE mode: add the items to proposals.json and send Brendan one chat message with a compact table: firm (Attio link) · change · source link · Approve? He replies with "approve all", "approve 1,3" or edits. On approval, write with PATCH, add the note, and log it.
   AUTO mode (only after Brendan switches it): write public-source facts directly. Private-source marks, new companies, `arrow_role` = Competitor, and conflicts still go to PROPOSE.
6. Advance the cursors only for the sources that were read successfully. Release the lock.

## Backfill mode
One-off, after approval: walk Attio companies linked to any deal/block/interest plus every N-PORT holder already matched in nport.db `attio_matches`, 10 firms per run. Same propose rules. Queue in `/workspace/attio-firms/backfill.json`.

## Schedule (proposed)
- Event runs: weekdays 9:00am, 1:00pm and 4:00pm PT, plus weekends at 10:00am PT. Offset 30 minutes from the people runs (8:30, 12:30, 3:30) so the two don't collide on the Attio rate limit.
  CRON_TZ=America/Los_Angeles `0 9,13,16 * * 1-5` and `0 10 * * 0,6`.
- After N-PORT Scraper's monthly refresh (1st of the month, ~8am PT), the 9am run on the 1st also diffs the new quarter's marks for firms already in Attio.
- Backfill (after approval): every 60 minutes, 8am–8pm PT, until the queue is empty, then the routine is removed.

## Notify Brendan
In PROPOSE mode, message him only when there are proposals: one compact message per run, with links. In AUTO mode, send a digest only when something notable happened: a mark move ≥50% in a quarter, a new seller or buyer signal, a firm newly tied to a live Arrow deal, or a conflict. Otherwise stay silent.

## Live routine prompts (draft)
- **"Attio firm context"** (folder `attio-firms`):
  "Load arrow/skills/arrow-attio-firm-context/SKILL.md and run one Event run. Mode: PROPOSE. Results go to Brendan in this chat only when there are proposals or notable changes. Never write without his approval in PROPOSE mode. Never send anything to anyone else."

## Changelog
- 2026-11-05 v0.1 draft (Grok Bot, for Brendan's review).
- 2026-11-05 v1.1: StepStone `arrow_role` = Seller (Brendan). Go-live approved in PROPOSE mode.
- 2026-11-05 v1.0: Brendan approved the attributes and the public-marks exception. The 7 attributes and their select options were created in Attio. No record writes. The bot is "Arrow Attio Firms", and the weekly log is owned by the separate "Arrow Log" bot.
