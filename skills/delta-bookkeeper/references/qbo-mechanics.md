# QBO Mechanics — Composio API + Web UI

## Composio (primary)
Flow: `COMPOSIO_SEARCH_TOOLS` (toolkit `quickbooks`) → `COMPOSIO_MULTI_EXECUTE_TOOL`. Connected account: admin@deltameridian.com.

Useful tools:
- `QUICKBOOKS_GET_COMPANY_INFO` — **run first every session**; check CompanyName.
- `QUICKBOOKS_QUERY_ACCOUNT` — e.g. `SELECT * FROM Account WHERE Active = true AND AccountType IN ('Bank', 'Credit Card')`. No OR, no parentheses — use IN. Use `Account.Id` (not the name) in AccountRef.
- `QUICKBOOKS_GET_REPORTS` — `ProfitAndLoss`, `BalanceSheet`, `TransactionList` with start/end dates.
- `QUICKBOOKS_QUERY_ENTITIES` — Purchase, Deposit, Transfer, JournalEntry, etc.
- `QUICKBOOKS_GET_PURCHASE` → then update via `QUICKBOOKS_EXECUTE_BATCH_OPERATION` (max 30 ops). Re-fetch immediately before writing (SyncToken). Use sparse updates; on ValidationFault 2020 retry with required top-level fields (e.g. PaymentType) carried from the fetched record. SystemFault 10000 and 429 throttling happen — retry gently, don't hammer.

Known account IDs: Chase 0012 = 144 · Tax Reserve savings 8603 = 145 · Mercury Checking 1344 = 140 · Business Intake 3958 = 141 · Mercury Credit = 142. Look up expense/equity account IDs with QUERY_ACCOUNT when needed.

**API limits (need the web UI):**
- Bank feed "For Review" queue — not visible via API.
- Reconcile workflow — UI only.
- Company tax form / company type — `UPDATE_COMPANY_INFO` silently ignores `CompanyType`. UI only.
- Receipt attachments — do in the UI.
- `UPDATE_COMPANY_INFO` is a full update: always resend every existing field or they may be blanked.

## Web UI (Claude in Chrome, when available)
- Comboboxes (account/category/date) often swallow the first typed text: click → type → if unchanged, Cmd+A → Backspace → retype. Screenshot-verify before saving.
- Report date fields need Return to commit; set both From and To. Default "last 90 days" hides older rows.
- Batch "Make inactive" selection resets after any intermediate save. System accounts, accounts used by products, and parents with subaccounts can't be inactivated.
- Register running-balance column is as-of that row's date — not the current balance.
- Feed tile "Bank: $x" = last feed pull (can lag weeks); "In QuickBooks" = book balance. Neither is live truth — check the bank.
- Attachments: find the hidden `input[type=file]` near "Add attachment" and upload directly; clicking opens a native dialog automation can't handle. 20 MB max; convert HEIC first.
- Global search accepts exact amounts ("1743.13") — fastest way to find a transaction.
- Deleting an invoice with a linked payment: delete the payment first.
- chase.com is blocked for the Chrome extension — ask Brendan for Chase balances/statements. QBO and Mercury work.
