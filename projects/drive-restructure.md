# Reconcile all Google Drives

**Status:** after 1Password

## Goal
Reconcile all Google Drives.

## Done when
Four Drive accounts (Arrow, Delta, Janey, Personal) each with a documented folder structure; junk removed; Janey folders moved off connmbrendan; structures recorded as references for document-filing.

## Decisions
- 2026-11-02: Janey gets its own document-filing wrapper later, on the same pattern as Arrow and Delta: one shared procedure skill (skills/document-filing) plus a thin per-drive wrapper, with the drive's structure and filing rules in a README stored in the Janey Drive itself. Not built yet; needs the Janey Drive (brendan@janeyhealth.com) connected in Composio first.

- 2026-11-02: the shared filing procedure lives at arrow/skills/document-filing (not skills/), because teammates' agents load only arrow/skills/ and arrow-fund-inbox depends on it. It is generic (no Delta, Janey or personal content); the Delta and future Janey wrappers in skills/ point down into it, which the sealed-arrow rule allows (nothing flows up).
- 2026-11-02: Delta Drive restructured and documented (Brendan's decisions): Delaware LLC; folders renumbered 00–06 + 99 - Inbox; new Contracts, Compliance, Tax, Payroll, Bank Statements folders; GridLync consolidated under 06 GridLync; files named YYYY-MM-DD — {Vendor/Entity} — {Doc type} [Status].ext; every receipt also filed in Drive; reimbursements by vendor. README: Google Doc 1r9_MWLdYSFnxV7z-5gj9TbclfQDGUcnzgAg_f6pWxic at Delta My Drive root.

## Steps
- [ ] (written when the project starts)

## Log
- 2026-10-20 created
- 2026-11-02 Janey document-filing wrapper noted as planned (not built)
- 2026-11-02 Delta Drive restructured (folders renumbered, 6 folders created, 12 files renamed), Delta README v1 created; Arrow README amendment v7 appended
