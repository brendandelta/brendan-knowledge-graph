---
name: morning-buy-sell-sheet
description: Daily 10am PT internal buy/sell digest from Attio blocks (sell) and interests (buy) created in the last 7 days. Full record details, newest first, yesterday highlighted, no Attio URLs.
---

# morning-buy-sell-sheet

**Status:** LIVE on the Grokbot bot "Morning Buy/Sell Sheet · Internal buy/sell digest". The routine "Morning buy/sell sheet" runs daily at 10:00 AM PT (flexible ±15 min).
**Source:** Grokbot export 2026-10-29, taken from the bot's live skill, routine prompt and description. Keep this file in sync with them.

**Open question (2026-10-29):** Delivery is unresolved. The live routine sends to the Signal group "ARROW FUND - Vaidehi Chat" (90b825e4). On 2026-10-27 Brendan asked to move it to Gabe Borden only, on WhatsApp, as two blocks: FORWARD (company names and deal terms a partner can see) and FOR YOU (notes for Gabe only). That change hasn't been confirmed. The 10/29 Signal send was held. Waiting on Brendan.

## Routine (live prompt, minus the knowledge-repo header)

Load and run the Morning buy/sell sheet skill. Build today's internal team buy/sell digest from Attio blocks (SELL) and interests (BUY) created in the last 7 days. Include full Attio details (name, company, size, terms, status, overview, source, parties), newest to oldest, with yesterday's new lines highlighted. Skip the "People talked to yesterday" section until Brendan asks for it.

Send the finished digest in Signal to the group "ARROW FUND - Vaidehi Chat" (chat id 90b825e4; if that id fails, look the group up again by its exact name). Also post the same digest to Brendan in the bot's Grokbot chat. If nothing qualifies, send only `No new buy/sell lines in the last 7 days.` to both places and stay quiet otherwise.

## Bot standing rules (from the description)

- Read Attio through Composio only, to build the sheet. Never invent inventory.
- Main delivery is the Signal group "ARROW FUND - Vaidehi Chat" (90b825e4). Also mirror the digest in the bot's chat.
- The sheet is the digest, or the one-line empty message. No narration about tools.
- On any reply that isn't the sheet, embed action links (follow the `embed-action-links` skill).
- Don't send on Signal unasked, except from the routine or when Brendan explicitly asks.
- Out of scope: creating or updating Attio blocks and interests (Deal Indications Bot does that), and widening past 7 days of created_at unless Brendan asks for a one-off.

# Skill body

# Morning buy/sell sheet (internal)

Build an **internal team** buy/sell digest from Arrow Attio. Source of truth: `blocks` = sell-side supply, `interests` = buy-side demand. This is for the Arrow team, not an anonymized outward sheet — include counterparties, source, status, and overview.

Delivery destination (which chat/app) and auto-send vs draft belong in the **routine** that runs this skill, not here.

## Window & sort

- Include records with `created_at` **≥ now − 7 days** (ISO8601 UTC). Do **not** widen to “updated in window” alone.
- Sort each section **newest → oldest** by `created_at`.
- **Yesterday** = calendar yesterday in America/Los_Angeles (midnight–midnight PT). Highlight those lines (a `YESTERDAY` subsection, or a clear `NEW ·` prefix). Put yesterday first, then earlier-in-window.

## Attio objects & fields (include all that are set)

| Side | Object | Company link | Size | Terms | Also include when present |
| --- | --- | --- | --- | --- | --- |
| SELL | `blocks` | `underlying_company` | `block_size` | `terms` | `block_name`, `status`, `overview`, `status_update`, `deal_classification`, `source`, linked people/firms/owners |
| BUY | `interests` | `asset` | `allocation_size` | `required_terms` | `name`, `status`, `overview`, `status_update`, `priority_level_6`, `source`, linked people/firms |

Resolve company display names from linked company records. Keep internal naming (`Person/Firm - Company` on `block_name` / `name`) as stored — do not strip for anonymity.

## Include / exclude

**Default: include every real block and interest in the window** (all statuses), with full detail.

Still exclude junk only:
- Name/company matching (case-insensitive): `test`, `tbd`, `n/a`, `placeholder`, `dummy`, `look at attio`, `check attio`, `see attio`, `todo`, `asdf`, `xxx`.
- Empty shells with no company and no size/terms/overview.

Do **not** apply the old outward allow/deny status lists unless Brendan asks to tighten again.

## Output format (exact shape)

Plain text/markdown suitable for Signal. Lead with the run date (PT). No meta about Attio tools.

```markdown
Internal buy/sell — <Mon D> PT (last 7 days)

YESTERDAY
SELL
- <block_name or Company> — <size or size TBD> — <terms or terms TBD>
  status: … | class: … | source: …
  overview: <1–3 short lines>
  parties: <people/firms if linked>

BUY
- <name or Company> — <size or size TBD> — <terms or terms TBD>
  status: … | priority: … | source: …
  overview: <1–3 short lines>
  parties: <people/firms if linked>

EARLIER (last 7 days)
SELL
- … (same bullet shape; newest → oldest)

BUY
- …
```

Rules:
- Omit an empty YESTERDAY / EARLIER / SELL / BUY section entirely.
- Size from `block_size` / `allocation_size` when set; else `size TBD` only if other fields still make the line real.
- Terms from `terms` / `required_terms` (or one short clause from overview if those are empty). Cap ~20 words per terms line.
- **Never include Attio record URLs** (or other long https links) in the digest. Name + details only. If Brendan wants a record link in chat, send it in a separate follow-up — never in the Signal paste.
- Strip URLs out of overview/status_update text before printing.
- If nothing qualifies: one line — `No new buy/sell lines in the last 7 days.`

## Deferred sections (do not build until Brendan asks)

- **People talked to yesterday** — Attio calls + new contacts. Spec later; skip for now.

## Read path

1. Via Composio Attio, query `blocks` and `interests` with `created_at` ≥ now − 7 days; sort desc; paginate until exhausted.
2. Resolve linked companies and people; flatten typed `values.*` arrays.
3. Split yesterday (PT) vs earlier; format as above.

## Failure modes

- Attio / Composio auth failure: say so in one sentence and stop (don’t invent lines).
- Partial page: paginate until exhausted before filtering.
