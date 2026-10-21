# Skill index

Every skill, what it reads, what it writes, what depends on it. If a skill isn't here it doesn't exist. Update this line when you change the skill. New rows appear only when a workflow has come up twice (CONSTITUTION.md).

## Personal (`skills/`)

| Skill | Reads | Writes | Depends on | Depended on by | Status |
|---|---|---|---|---|---|
| brendan-crm | composio:notion, composio:attio (read only, Arrow→personal), gcal, imessage, signal | composio:notion | profile.md, CONNECTIONS.md | crm-sync-feed (planned) | live |
| delta-bookkeeper | composio:quickbooks, Delta Intake (Grokbot) | composio:quickbooks | profile.md, CONNECTIONS.md | — | live |

## Shared (`arrow/skills/`)

| Skill | Reads | Writes | Depends on | Depended on by | Status |
|---|---|---|---|---|---|
| arrow-fund-inbox | gdrive ("99 - Inbox", README) | gdrive | CONNECTIONS.md | — | live |

## Planned (not yet written)

| Skill | Reads | Writes | Depends on | Notes |
|---|---|---|---|---|
| money-queue | QBO (Delta, Arrow, Janey), Chase, Ramp, Shopify, Meta | a proposal list only; never moves money | profile.md hard rules | weekly review, each item approved individually |
| crm-sync-feed | composio:notion, composio:attio | Attio (only after approval) | brendan-crm | weekly list of Notion people missing from Attio; Attio→Notion stays automatic |
| accounting-and-tax | QBO x3, Gusto, bank accounts | QBO | delta-bookkeeper (absorbs it) | reimbursement rules, salary vs K-1, tax reserve; Delta first, then Arrow, Janey |

## Ideas (not yet seen twice)

- call-scheduler: daily suggested calls, updates a Notion view. Would read Notion People + Calendar, depend on brendan-crm, and take over its "who's going cold" logic.
- morning brief, message triage, meeting prep, week-ahead: obvious candidates; written when they come up for real.
