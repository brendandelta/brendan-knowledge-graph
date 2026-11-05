# Marwan Deal Board Bot

| Field | Value |
|---|---|
| **Name** | Marwan Deal Board Bot |
| **Bot id** | 98d627fd-8f39-4c96-b70e-9b08d4950eed |
| **Silo** | Arrow |
| **Purpose** | Two-way sync between Spartan's OffBook secondaries board and Attio: (1) mirror Marwan Chahboun's (The Spartan Group) board into Attio blocks/interests tagged to him; (2) post Arrow's recent Attio blocks/interests that have terms to Brendan's own side of the board (no investor names). |
| **Status** | Live (both routines enabled) |
| **Repo skill path** | `(no skill in repo yet)` — logic lives in the routine prompts below |
| **Data sources (read)** | OffBook board https://secondaries.spartangroup.io/my-board/spartan (Supply + Demand tabs) and /my-board, read in the box browser signed in via Google as brendan@arrowfund.co; Attio workspace `arrow-fund` (blocks, interests, companies, people) |
| **Write targets** | Attio blocks + interests (Marwan-sourced, always linking person 582d6212-d6fc-4169-9f11-726d7f129f09 and The Spartan Group company 4100acbb-59de-435f-9541-e517a3437b69); Brendan's OffBook board (/my-board) Sell-Side and Buy-Side positions posted by this bot only; box state files `/workspace/marwan-board/state.json`, `/workspace/marwan-board/my-board-state.json`, `/workspace/marwan-board/attio-schema.md` |
| **Send permissions** | Messages only Brendan, in its own chat. Standing OK (since 2026-11-02) to post, edit and remove positions this bot posted on Brendan's OffBook board without asking. Must get Brendan's explicit yes before adding a Marwan deal that matches our book to his board. Never edits Brendan's hand-added positions, Spartan deals or Master Board; never puts investor/seller/intermediary names on the board. No email/Slack/WhatsApp sends. |
| **Depends on** | Attio data entered by Arrow team / other Arrow Attio bots; OffBook Google sign-in in the box browser |
| **Feeds** | Morning Buy/Sell Sheet and other Attio readers (Marwan-sourced blocks/interests); Spartan's view of Arrow supply/demand via OffBook |
| **Connectors needed** | Composio (`user-composio`, Attio tools); box browser (computer use) for OffBook |
| **Last verified** | 2026-11-05 · self-reported |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Marwan board → Attio sync | `marwan-board-attio-sync` | `30 8,15 * * 1-5` | flexible ±15 min (server fires 8:17 / 3:17) | Read Marwan's OffBook board, diff vs state.json, create/update Attio blocks & interests tagged to Marwan, list disappeared positions, propose strong matches to Brendan's board (add only after his yes) | Brendan in this bot's chat; silent when nothing changed |
| My board refresh from Attio | `my-board-refresh-from-attio` | `30 7 * * 1-5` | flexible ±15 min (server fires 7:36) | Post Arrow's live Attio blocks (Sell-Side) and interests (Buy-Side) from the last 7 days that have size + valuation/price to Brendan's OffBook board; edit changed terms, remove closed ones; exclude Marwan-sourced, DealOwl, LP portal, NEEDS REVIEW | Brendan in this bot's chat; silent when nothing changed |

## Full description (verbatim, secrets stripped)

```
(empty — this bot's profile description and title are blank; behavior is defined by memory + the two routine prompts)
```

## Rebuild steps

1. Create a bot named "Marwan Deal Board Bot" in the Arrow section.
2. Connect Composio with Attio (workspace `arrow-fund`).
3. In the box browser, sign in to https://secondaries.spartangroup.io with Google as brendan@arrowfund.co.
4. Recreate both routines above with the same cron/timing. Prompts (key rules): Marwan sync — Supply→blocks, Demand→interests; name 'Marwan - <Company> <layer> <size>'; source 'marwan (MM/DD) - spartan board'; status Available/Target; link Marwan person + Spartan company (set intermediary_contact_link in a separate update after intermediaries_2, re-read to confirm); search before create; skip hidden-name demand; don't change status of disappeared rows; read-only on the board. Board refresh — Buy-Side form has no valuation field so terms go in Notes; layer from terms (direct = L0, default L1); pick company type-ahead only on exact name match; never post names.
5. Re-seed `/workspace/marwan-board/state.json` and `my-board-state.json` from the current board + Attio if the box was lost (match Attio ids to existing positions before posting anything new, to avoid duplicates).
