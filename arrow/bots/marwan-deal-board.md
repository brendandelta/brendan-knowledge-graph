# Marwan Deal Board Bot

| Field | Value |
|---|---|
| **Name** | Marwan Deal Board Bot · OffBook board ↔ Attio sync |
| **Bot id** | 98d627fd-8f39-4c96-b70e-9b08d4950eed |
| **Silo** | Arrow |
| **Purpose** | Two-way sync between Spartan's OffBook secondaries board and Attio: (1) mirror Marwan Chahboun's (The Spartan Group) board into Attio blocks/interests tagged to him; (2) post Arrow's recent Attio blocks/interests that have terms to Brendan's own side of the board (no investor names). |
| **Status** | Live (both routines enabled); title "OffBook board ↔ Attio sync" |
| **Repo skill path** | `arrow/skills/marwan-deal-board/SKILL.md` (Part A = board → Attio sync, Part B = my board refresh) |
| **Data sources (read)** | OffBook board https://secondaries.spartangroup.io/my-board/spartan (Supply + Demand tabs) and /my-board, read in the box browser signed in via Google as brendan@arrowfund.co; Attio workspace `arrow-fund` (blocks, interests, companies, people) |
| **Write targets** | Attio blocks + interests (Marwan-sourced, always linking person 582d6212-d6fc-4169-9f11-726d7f129f09 and The Spartan Group company 4100acbb-59de-435f-9541-e517a3437b69); Brendan's OffBook board (/my-board) Sell-Side and Buy-Side positions posted by this bot only; box state files `/workspace/marwan-board/state.json`, `/workspace/marwan-board/my-board-state.json`, `/workspace/marwan-board/attio-schema.md` |
| **Send permissions** | Messages only Brendan, in its own chat. Standing OK (since 2026-11-02) to post, edit and remove positions this bot posted on Brendan's OffBook board without asking. Must get Brendan's explicit yes before adding a Marwan deal that matches our book to his board. Never edits Brendan's hand-added positions, Spartan deals or Master Board; never puts investor/seller/intermediary names on the board. No email/Slack/WhatsApp sends. |
| **Depends on** | Attio data entered by Arrow team / other Arrow Attio bots; OffBook Google sign-in in the box browser |
| **Feeds** | Attio readers (Marwan-sourced blocks/interests); Spartan's view of Arrow supply/demand via OffBook |
| **Connectors needed** | Composio (`user-composio`, Attio tools); box browser (computer use) for OffBook |
| **Last verified** | 2026-11-06 · Grok Bot registry refresh (live display name); was 2026-11-05 · self-reported |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Marwan board → Attio sync | `marwan-board-attio-sync` | `30 8,15 * * 1-5` | flexible ±15 min (server fires 8:17 / 3:17) | `git pull` the repo, then follow skill Part A: read Marwan's OffBook board, diff vs state.json, create/update Attio blocks & interests tagged to Marwan, list disappeared positions, propose strong matches to Brendan's board (add only after his yes) | Brendan in this bot's chat; silent when nothing changed |
| My board refresh from Attio | `my-board-refresh-from-attio` | `30 7 * * 1-5` | flexible ±15 min (server fires 7:36) | `git pull` the repo, then follow skill Part B: post Arrow's live Attio blocks (Sell-Side) and interests (Buy-Side) from the last 7 days that have size + valuation/price to Brendan's OffBook board; edit changed terms, remove closed ones; exclude Marwan-sourced, DealOwl, LP portal, NEEDS REVIEW | Brendan in this bot's chat; silent when nothing changed |

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it. On the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first. That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/marwan-deal-board/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either. Keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

You are Brendan Conn's Marwan Deal Board Bot for Arrow Fund (America/Los_Angeles). You keep Spartan's OffBook secondaries board (https://secondaries.spartangroup.io) and Arrow's Attio in sync, following arrow/skills/marwan-deal-board:
- Part A: weekdays at about 8:30am and 3:30pm PT, mirror Marwan Chahboun's (The Spartan Group) board into Attio blocks and interests tagged to him. Propose strong matches with our book for Brendan's board, and add them only after his yes.
- Part B: weekdays at about 7:30am PT, post Arrow's live, recent Attio blocks (Sell-Side) and interests (Buy-Side) that have terms to Brendan's board, with no names. Brendan gave a standing OK on 2026-11-02.
Message only Brendan, in this chat, with Attio deep links, and stay silent when nothing changed. Never touch Brendan's hand-added positions, Spartan deals or Master Board.
```

## Rebuild steps

1. Create a bot named "Marwan Deal Board Bot" in the Arrow section.
2. Connect Composio with Attio (workspace `arrow-fund`).
3. In the box browser, sign in to https://secondaries.spartangroup.io with Google as brendan@arrowfund.co.
4. Recreate both routines above with the same cron/timing; each prompt is: `git -C /home/box/Claude/brendan-knowledge-graph pull`, then follow `arrow/skills/marwan-deal-board/SKILL.md` (Part A or Part B), plus a short inline fallback of the key rules.
5. Re-seed `/workspace/marwan-board/state.json` and `my-board-state.json` from the current board + Attio if the box was lost (match Attio ids to existing positions before posting anything new, to avoid duplicates).
