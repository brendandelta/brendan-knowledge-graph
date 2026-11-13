---
name: "delta-meridian-inbox"
description: Sort, rename, and file documents from the "99 - Inbox" folder in the Delta Meridian Holdings Google Drive (brendan@deltameridian.com) into their correct folders, following that drive's README. Clear fits are filed straight away; anything ambiguous is raised with Brendan first. Use when Brendan says he dropped or added files to the Delta inbox, asks to process/clear/sort the Delta inbox, mentions Delta paperwork (formation, OA, EIN, contracts, compliance, tax, payroll, bank statements, Delta invoices or receipts, GridLync) waiting to be filed, whenever any file is being placed anywhere in the Delta drive or a folder is being created there, and on the weekday 12pm PT filing schedule. Do NOT use for the Arrow Fund drive.
---

# Delta Meridian Inbox

Brendan drops Delta Meridian Holdings LLC documents into `99 - Inbox` at the root of the Delta My Drive.

**How:** follow the shared procedure in `arrow/skills/document-filing/SKILL.md` exactly (it lives in `arrow/` so the Arrow team gets it too; it is generic and carries nothing Delta), with the settings below. Its APPROVAL LAYER applies here in full: no folder is created in this drive without Brendan's explicit approval and a README amendment written in the same step.
**Open every file:** the shared procedure's HARD RULE applies here in full. On every run (scheduled or asked), open and read the actual contents of every file in this inbox, including files carried over from prior days, before filing, recommending, or asking. Download each file and extract the text (pdftotext/OCR) or export Docs/Sheets; never judge by filename, and never report that a file was not re-read. The weekday 12pm PT routine prompt restates this rule.
**What goes where:** the drive's README holds the folder map, folder IDs, naming conventions and confirmed filing rules. Read it in full every run. Rules are added to the README, not here.

## Settings

| Setting | Value |
|---|---|
| Composio account (always pass it) | `googledrive_decare-benami` (brendan@deltameridian.com) |
| Drive | Brendan's My Drive in the Delta workspace (no shared drive) |
| Inbox | `99 - Inbox` — `103Y4mfhniazUGgTuDdq5WllTSVswHB1P` (created 2026-11-02) |
| Archive (originals) | `99 - Inbox / 0 Archive` — `1YTp_o4LNEcCOismkZwm4xtbZE4rFtK92` |
| README | `00 — READ ME — Delta Meridian Drive (Structure and Conventions)` — Google Doc `1r9_MWLdYSFnxV7z-5gj9TbclfQDGUcnzgAg_f6pWxic` (My Drive root, v1 2026-11-02) |

If an ID fails, search by name.

## Delta-specific mechanics

- **My Drive, not a shared drive:** listings use `corpora: user`; `supports_all_drives: true` is harmless and can stay on moves. New shortcuts land in My Drive root (`'root'`), which is also the drive root.
- **Receipts go in Drive and QBO.** Every receipt is filed in the drive (README section 5); reimbursement backup goes in `Brendan Reimbursements / {Vendor}`. Books live in QuickBooks (see `skills/delta-bookkeeper`): when a receipt or reimbursement document is filed, mention it in Next steps so it can be attached to the QBO transaction and its Delta Intake row updated. Never write to QBO from this skill.
- **Siloed:** Arrow, Janey or personal documents found in this inbox are flagged, never filed here or moved into another account's drive.
- Credential files (e.g. `Access Creds`, Mercury backup codes) are never opened or moved by this skill; flag only.
