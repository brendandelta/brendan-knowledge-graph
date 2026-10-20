# Skill index

Every skill, what it reads, what it writes, what depends on it. If a skill isn't here it doesn't exist. Update this line when you change the skill.

## Personal (`skills/`)

| Skill | Reads | Writes | Depends on | Depended on by | Status |
|---|---|---|---|---|---|
| brendan-crm | composio:notion, composio:attio, gcal, imessage, signal | composio:notion | profile.md, CONNECTIONS.md | prep-for-meeting, message-triage, week-ahead | live |
| delta-bookkeeper | composio:quickbooks, Delta Intake (Grokbot) | composio:quickbooks | profile.md, CONNECTIONS.md | — | live |
| morning-brief | Calendar, Gmail, iMessage, Signal, open-loops.md | memory/daily/ | brendan-crm | mirror idle screen | stub |
| message-triage | iMessage, Signal, WhatsApp, Gmail | memory/open-loops.md | brendan-crm | mirror | stub |
| prep-for-meeting | Notion People db, Gmail, Calendar | — | brendan-crm | morning-brief | stub |
| week-ahead | Calendar, Notion People db, open-loops.md | — | brendan-crm | — | stub |

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

## Known gaps

- The call-scheduler idea (daily suggested calls, updates Notion view) is not a skill yet. When it becomes one it reads Notion People + Calendar, writes a Notion view, depends on brendan-crm, and brendan-crm's "who's going cold" logic should defer to it. Write that here before building.
