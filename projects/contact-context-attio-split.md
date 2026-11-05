# Contact Context split plan (FINAL draft; no bots created, no routines changed. Brendan does those)

## Today
**Contact Context** (89db7e33-9cc6-4c60-9044-923d6879b0bf) runs two jobs:
1. **Personal Notion CRM**, `skills/crm-maintenance/SKILL.md`:
   - Weekday routine (folder `daily-contact-context`): 8am, 12pm and 3pm PT.
   - Weekend routine (folder `crm-enrichment-weekends`): Sat/Sun 8am and 5pm PT.
   - Uses the Notion Run Log as its cursor. Never writes Attio.
2. **Arrow Attio people**, `arrow/skills/attio-contact-context/SKILL.md` v2.2:
   - "Attio contact context" (folder `attio-context`): weekdays 8:30am, 12:30pm and 3:30pm PT; weekends 8:30am and 5:30pm PT.
   - "Attio backfill" (folder `attio-backfill`): `15,45 7-21 * * *`.
   - State is in `/workspace/attio-context/state.json`, the queue in `/workspace/attio-backfill/`.

Splitting matches the constitution: personal (Notion) and Arrow (Attio, `arrow/`) are separate systems of record, and only Arrow is shared with the team.

## After: four bots (FINAL, per Brendan 2026-11-05)
| Bot | Runs | Writes | Cron (CRON_TZ=America/Los_Angeles) |
|---|---|---|---|
| **Contact Context** (existing, slimmed) | crm-maintenance only | Notion People (append only) | `0 8,12,15 * * 1-5` (daily-contact-context) · `0 8,17 * * 0,6` (crm-enrichment-weekends), both unchanged |
| **Arrow Attio People** (new; inherits the routines) | attio-contact-context v2.2 | Attio People (owned fields only) | `30 8,12,15 * * 1-5` + `30 8,17 * * 0,6` ("Attio contact context") · `15,45 7-21 * * *` ("Attio backfill"), both unchanged |
| **Arrow Attio Firms** (new) | arrow-attio-firm-context (PROPOSE mode) | Attio Companies (firm-intel attributes and notes, after each approval) | `0 9,13,16 * * 1-5` + `0 10 * * 0,6` ("Attio firm context") |
| **Arrow Log** (new; its own bot) | arrow-weekly-log | The week's Arrow Log doc + /workspace/arrow-log/ | `30 9 * * *` + `0 12,19 * * *` ("Arrow weekly log") · `30 6 * * 5` ("Arrow log Friday wrap"). Plan: arrow-log-bot-plan.md |

The log is a **general shared Arrow source**: any Arrow bot may read it (read-only); only Arrow Log writes it. Current report readers:
- Daily Arrow Report: `0 10 * * *`.
- Arrow Weekly: `0 7 * * 5`.
Both are created only after Brendan approves their sends.

---
## 1. Arrow Attio People (new bot)
**Name:** Arrow Attio People
**Description:**
```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/attio-contact-context/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

Arrow Attio People. Keeps Arrow Fund's Attio People records current from Arrow comms (comms index, Arrow Gmail, WhatsApp/Signal/iMessage Arrow threads, calendar, Attio calls) under arrow/skills/attio-contact-context. Attio People only, owned fields only, human-edit guard always on. Never Notion, never Companies (that is Arrow Attio Firms), never personal or Delta material. Inherits the existing Attio contact-context and Attio backfill routines, /workspace/attio-context/state.json cursor, /workspace/attio-backfill/ queue and ATTIO_CONTEXT_TOKEN from Contact Context. Results go to Brendan in this chat only when notable. Never sends anything to anyone else. The Arrow Weekly Log is a general shared source any Arrow bot may read; this bot does not write it (Arrow Log does).

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Never tell Brendan to open something without a clickable markdown deep link in the same sentence. Never invent URLs.
```
**Routines** (moved unchanged from Contact Context; prompts verbatim from attio-contact-context §Live routine prompts):
- "Attio contact context", folder `attio-context`. Weekdays 8:30am, 12:30pm and 3:30pm PT; weekends 8:30am and 5:30pm PT. Prompt: "Load arrow/skills/attio-contact-context/SKILL.md and run that skill for one event run. Results go to Brendan in this Grok Bot chat, and only when something is notable…" (the full verbatim prompt is in the skill).
- "Attio backfill", folder `attio-backfill`, CRON_TZ=America/Los_Angeles `15,45 7-21 * * *` (Backfill mode, tiers A–C).
**Inheritance (decided):** Arrow Attio People takes over both Attio routines unchanged, plus `/workspace/attio-context/state.json` (cursor), the `/workspace/attio-backfill/` queue, lock and log, and the same ATTIO_CONTEXT_TOKEN (if set), so the human-edit guard still recognizes its own actor.
**Hand-over steps (in order, to avoid double runs):**
1. Pause both Attio routines on Contact Context.
2. Create the new bot and its routines, pointing at the same `/workspace/attio-context/state.json` and `/workspace/attio-backfill/` queue, lock and log (the box is shared, so the cursor carries over and the backfill resumes where it stopped).
3. Run one event run by hand and check the cursor advanced.
4. Delete the two Attio routines from Contact Context.

## 2. Arrow Attio Firms (new bot)
**Name:** Arrow Attio Firms
**Description:**
```
## Knowledge repo (read first)
<same standard paragraph, with skill file arrow/skills/arrow-attio-firm-context/SKILL.md>

Arrow Attio Firms. Keeps Arrow Fund's Attio Company records current with dated, sourced firm intel (firm type, holdings and marks, role for Arrow, key people) from Arrow Gmail, Arrow WhatsApp groups, Signal (incl. "Data OS input"), Arrow calendar and SEC N-PORT/Form D data (read-only from /workspace/nport/nport.db). Starts in PROPOSE mode: every write is shown to Brendan with sources and waits for his yes. Attio Companies only; never people, deals or blocks; never creates records without approval; human-edit guard always on. Public-filing marks are allowed on Companies (approved). Example: StepStone Group arrow_role = Seller. Never personal or Delta material. Never sends anything to anyone.

## Embed action links (required)
<same standard block>
```
**Routine:**
- "Attio firm context", folder `attio-firms`. CRON_TZ=America/Los_Angeles `0 9,13,16 * * 1-5` and `0 10 * * 0,6`. Prompt as in arrow-attio-firm-context §Live routine prompts.
- (Later, after approval) "Attio firm backfill", hourly 8am–8pm PT until the queue is empty.
**Already done (2026-11-05):** Brendan approved the 7 company attributes and their select options, which were created in Attio, and the public-marks exception. No firm data has been written yet.

## 3. Contact Context: exact changes
- **Description:** keep the Knowledge repo paragraph (skill file `skills/crm-maintenance/SKILL.md`) and the Embed action links block. Change the line "Contact Context." to:
  "Contact Context. Maintains Brendan's personal Notion People CRM only (skills/crm-maintenance). Never reads or writes Attio for updates; Arrow Attio work is done by the Arrow Attio People and Arrow Attio Firms bots."
  (Attio stays read-only for lookups, as crm-maintenance already allows.)
- **Routines:** keep `daily-contact-context` (weekdays 8am, 12pm and 3pm PT) and `crm-enrichment-weekends` (Sat/Sun 8am and 5pm PT). **Remove** "Attio contact context" and "Attio backfill", but only after the new bot's routines have been verified (hand-over steps above).
- **Memory:** remove its Attio-specific working notes after the move, or copy them to the new bot (the Attio cleanup and audit notes from 2026-11-04).

## 4. Repo changes (one commit, after approval)
- INDEX.md: add rows for arrow-attio-firm-context (bot Arrow Attio Firms) and arrow-weekly-log (bot Arrow Log). Update the rows for attio-contact-context (live on: Arrow Attio People), crm-maintenance (Attio jobs moved), daily-arrow-report and arrow-weekly (source: the weekly log). Draft rows are in index-changelog-drafts.md.
- CHANGELOG.md: one entry.
- No change to the attio-contact-context skill except its "Live (Grokbot)" bot name.

## 5. Arrow Log bot
See arrow-log-bot-plan.md for its name, its full description (with the Knowledge repo paragraph), routines and setup order. It is a separate bot. Contact Context and Arrow Attio People never write the log. The log covers the whole Arrow team (Commitments and People updates for any party), not Brendan alone. Personal/mixed items on Arrow accounts are excluded by content.
