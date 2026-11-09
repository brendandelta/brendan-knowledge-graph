---
name: marwan-deal-board
description: Two-way sync between Spartan's OffBook secondaries board and Arrow's Attio. (1) Mirror Marwan Chahboun's (The Spartan Group) board into Attio blocks/interests tagged to him; (2) post Arrow's recent Attio blocks/interests that have terms to Brendan's own side of the board, without names.
---

# marwan-deal-board

**Status:** LIVE on the Grokbot bot "Marwan Deal Board Bot". Routines: "Marwan board → Attio sync" (`30 8,15 * * 1-5` PT, flexible ±15) runs Part A; "My board refresh from Attio" (`30 7 * * 1-5` PT, flexible ±15) runs Part B. Each routine pulls this repo first, then follows this file.
**Source:** moved here 2026-11-05 from the two live routine prompts. Keep this file and the routines in sync.

## Shared setup

- OffBook: https://secondaries.spartangroup.io. The box browser is signed in via Google as brendan@arrowfund.co. Pages: `/my-board/spartan` (Marwan's board, Supply + Demand tabs), `/my-board` (Brendan's board), `/my-board/master` (never touch). If any page redirects to login, stop and ask Brendan in the bot's chat to sign in again (hand him the box screen).
- Attio workspace `arrow-fund`, via Composio Attio tools. Conventions in `/workspace/marwan-board/attio-schema.md`.
- Marwan's person record: `582d6212-d6fc-4169-9f11-726d7f129f09`. The Spartan Group company: `4100acbb-59de-435f-9541-e517a3437b69`.
- State files on the box: `/workspace/marwan-board/state.json` (Part A: board position → Attio record + value fingerprints + notes), `/workspace/marwan-board/my-board-state.json` (Part B: Attio id → board position + posted values).
- Message only Brendan, in the bot's chat. Every record mentioned gets an Attio deep link: `https://app.attio.com/arrow-fund/custom/blocks/record/<id>` or `.../custom/interests/record/<id>`. Stay silent when nothing changed.

## Part A — Marwan board → Attio sync

1. **Read** `/my-board/spartan`, both tabs, every position including expanded group sub-positions, with all fields: layer L0/L1/L2, structure tags, available size, min ticket, price/share, valuation, upfront fee all-in, mgmt/carry, val incl fees, updated date, description; demand size sought and added date. Read-only on this page.
2. **Diff** against `state.json`: new, changed, and disappeared positions.
3. **Write to Attio.** Supply → blocks, Demand → interests. Search before creating (no duplicates).
   - Name (`block_name` / `name`): `Marwan - <Company> <layer> <size>`.
   - `source`: `marwan (MM/DD) - spartan board`.
   - Terms as bullets. Status Available (blocks) / Target (interests) unless already further along. Set `deal_classification`, `overview` (short description + "From Marwan's Spartan board, updated <date>"), `run_sync` on, `underlying_company` / `asset` linked; create missing companies.
   - Always link Marwan: blocks `intermediaries_2` and `intermediary_contact_link`; interests `intermediary`. Link The Spartan Group as the intermediary firm/company.
   - Attio quirk: writing `intermediaries_2` empties `intermediary_contact_link`, so set `intermediary_contact_link` in a separate update afterwards and re-read to confirm both hold Marwan.
   - Interest allocation sizes go as a plain number. Never overwrite an existing person link; only fill empty ones. Skip demand rows with hidden company names. Follow notes saved in `state.json`.
   - Disappeared positions: do not change Attio status; list them as possibly sold/withdrawn.
   - Verify writes, then update `state.json`.
4. **Matches.** Check new or changed positions against Arrow's own book (blocks/interests not sourced from Marwan). Only for strong matches (company plus size and price/structure fit), ask Brendan with a short question whether to add it to `/my-board`. Never add without his explicit yes.
5. **Report:** short list of added, updated and disappeared, with Attio links, plus any strong matches.

## Part B — My board refresh from Attio

Standing OK from Brendan (2026-11-02): posting, editing and removing positions this job posted needs no further approval. Spartan sees everything the moment it's posted.

**What qualifies**
- Attio interests → Buy-Side; blocks → Sell-Side. Records created or updated in the last 7 days that are still live: interests not Closed/On Hold, blocks Available.
- Exclude: anything Marwan-sourced (intermediary is Marwan, source says spartan board, or id in `state.json`); DealOwl notifications; LP portal records; buyers already in talks on our own deals; anything marked NEEDS REVIEW or unclear buy vs sell.
- Must have terms: a size plus a valuation or price per share. Skip otherwise. Layer from the terms (direct = L0); default L1.

**Posting ("+ New position", submit "Post position")**
- Sell-Side: company, layer, available ($m, range OK) and valuation ($bn) required; optional price/share, min ticket, fees.
- Buy-Side: company, side, layer, currency, demand ($m), buyer type (leave blank), sector, notes. No price/valuation field, so put terms in Notes, e.g. "Valuation under $30bn. L1, no carry."
- Company type-ahead: pick a suggestion only when its name matches exactly; otherwise keep the typed text. (Blindly taking the first suggestion once posted rows as Anduril.)
- Picking a company suggestion auto-fills the Company description, which can name founders, backers and stale marks (seen on Stripe, 2026-11-09). After posting, open the position and replace any description that names people or investors with a neutral one-liner (e.g. "Financial infrastructure / payments platform"). An empty description doesn't save; the site restores the auto-fill.
- Notes (Spartan-visible) carry only a short version of the terms. Never put investor, seller or intermediary names anywhere on the board.

**Refresh**
- Track everything posted in `my-board-state.json` (seed from the board if missing; match existing positions before posting to avoid duplicates).
- Post new qualifying records; edit positions whose Attio terms changed; remove positions this job posted whose record is now closed, sold, withdrawn or passed. Older-than-7-day positions stay while the record is live.
- Never edit or remove positions Brendan added by hand. Never touch Spartan deals or Master Board.

**Report:** one short message listing posted, updated and removed, each with its Attio link, plus a link to https://secondaries.spartangroup.io/my-board. Nothing changed → send nothing.
