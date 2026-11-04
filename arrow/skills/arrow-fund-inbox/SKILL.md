---
name: "arrow-fund-inbox"
description: Sort, rename, and file documents from the "99 - Inbox" folder in the Arrow Fund shared Google Drive into their correct folders, following the drive's README. Files that clearly fit an existing folder are filed straight away; anything ambiguous is discussed with Brendan first. Then surfaces next steps (payments due, missing vendor paperwork, signatures, replies). Use this skill whenever the user says they dropped, placed, threw, added, or uploaded files into the inbox, asks to "process/clear/sort/file the inbox", mentions invoices, contracts, W-9s, financial reports, or other paperwork waiting to be filed, or asks what needs doing from recent documents, even if they don't say "skill" or name the folder. Also use it whenever any file is being placed anywhere in the Arrow Fund shared drive or a folder is being created there, even outside the inbox. Also runs on the weekday 12pm PT filing schedule.
---

# Arrow Fund Inbox

Brendan and the team drop documents into `99 - Inbox` at the root of the Arrow Fund shared drive.

**How:** follow the shared procedure in `arrow/skills/document-filing/SKILL.md` (next to this skill) exactly, with the settings below. Its APPROVAL LAYER applies here in full: no folder is created in this drive without Brendan's explicit approval and a README amendment written in the same step.
**What goes where:** the drive's README (amendments v2–v7; v7 of 2026-11-02 holds the key folder IDs) holds the folder map, folder IDs, naming patterns and every confirmed filing rule (Apex Horizon, carry side letters, Harvey Esquire, S11/Hubble, S12–S16, Delaware SR confirmations, Carta, Taiga, deal-attributable vendor costs, shortcuts, invoices Arrow issues). Read it in full every run. Rules are added to the README, not here.

## Settings

| Setting | Value |
|---|---|
| Composio account (always pass it) | `googledrive_left-bim` (brendan@arrowfund.co) |
| Drive | Arrow Fund shared drive, root `0AP9pttwIqgIZUk9PVA` |
| Inbox | `99 - Inbox` — `1L1YNUipRP08o2N_jJF8O7_0dWoKJFQQX` |
| Archive (originals) | `99 - Inbox / 0 Archive` — `19GtsWWa-6lWPSCBqJuTbicB-b-qLtbmk` |
| README | `00 — READ ME — Arrow Fund Drive (Structure and Conventions)` — Google Doc `1K08QQ67KSYkOGYAJwHClV7nLwAW9m4OOd0x-P0D4nog` |
| My Drive root of this account (where new shortcuts land) | `0AMiCrWCJ-2LpUk9PVA` |

If an ID fails, search by name; folders get moved or recreated (the old shared-drive inbox ID broke this way).

## Arrow-specific mechanics

- **"The inbox"** means the shared-drive one. Brendan's own My Drive inbox (`99 - Inbox`, `1pDMd4h37KFvo4j3_Eg5kWWDtlO8_XVXJ`, same account) is processed only when he says "my personal inbox".
- **Shared drive:** every list, copy and move needs `supportsAllDrives` / `supports_all_drives: true`. Moves between shared-drive folders work only through Composio with this account.
- **Shortcuts** (README v5): `GOOGLEDRIVE_CREATE_SHORTCUT_TO_FILE` creates the shortcut in this account's My Drive root; then `GOOGLEDRIVE_MOVE_FILE` it into place with `remove_parents` = `0AMiCrWCJ-2LpUk9PVA`.
- **Harvey tracker:** after filing Harvey Esquire invoices or receipts, offer to update Vaidehi's `Harvey invoices` sheet (ID in the README). Writing to it needs Composio's `googlesheets` toolkit on this account.
- **Delaware SR confirmations:** acceptance can be checked on icis.corp.delaware.gov or at (302) 739-3073 (README rule).
- **Composio accounts:** four Google accounts are connected; the default is a personal Gmail that can't see the shared drive, hence the explicit account. (The old "standard" Google Drive connector refused moves between shared-drive folders; if any tool refuses, try Composio's `googledrive` toolkit on this account before asking Brendan to do it by hand.)
- **Always "needs your call" here** (README open items): S10 Ausum vs Parafi, S11/S12 questions not settled in the README, entities the README lists as unconfirmed, and anything restricted (S5).
- **Established name patterns:**
  ```
  YYYY-MM-DD — Vendor Name — Invoice {number} ({short description}, ${amount}).pdf
  2026-08-31 — Arrow Fund Holdings — Month-End Financial Report (Aug 2026).pdf
  2026-08-31 — Arrow Fund Holdings — General Ledger (Aug 2026).xlsx
  ```
  The accountant's older `05 2026 …` names in Monthly Statements don't follow the README; name new ones per the README and offer to rename the old ones.
- **Next steps** here include Delaware franchise tax and registered-agent deadlines, accountant questions in the month-end package (uncategorized lines, unexplained receivables), and offers to draft Gmail (brendan@arrowfund.co), Calendar reminders or Attio updates, drafted first and only sent or changed with approval.
- Everything in this folder is shareable with the Arrow team. Nothing personal or Delta goes here or into the Arrow drive.
