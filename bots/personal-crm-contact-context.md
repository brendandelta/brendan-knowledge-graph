# Personal CRM Contact Context

| Field | Value |
|---|---|
| **Name** | Personal CRM Contact Context |
| **Bot id** | `89db7e33-9cc6-4c60-9044-923d6879b0bf` (confirm on self-report) |
| **Silo** | Personal |
| **Purpose** | Personal Notion People CRM enrichment (Notion only; never writes Attio). |
| **Status** | live (renamed from Contact Context 2026-11-05; Attio routines paused pending delete) |
| **Repo skill path** | `skills/crm-maintenance/SKILL.md` |
| **Data sources (read)** | pending self-report |
| **Write targets** | pending self-report |
| **Send permissions** | pending self-report |
| **Depends on** | pending self-report |
| **Feeds** | pending self-report |
| **Connectors needed** | pending self-report |
| **Last verified** | — · awaiting self-report |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| CRM enrichment (weekdays) | daily-contact-context | (weekdays 8am, 12pm, 3pm PT — confirm on self-report) | Weekday enrichment windows | Notion CRM enrichment per skills/crm-maintenance | Brendan in this chat when notable |
| CRM enrichment (weekends) | crm-enrichment-weekends | (Sat/Sun 8am, 5pm PT — confirm on self-report) | Weekend enrichment windows | Same job as weekdays | Brendan in this chat when notable |
| Attio contact context | attio-context / attio-contact-context | paused | — | PAUSED pending Brendan delete; owned by Arrow Attio People | — |
| Attio backfill | attio-backfill | paused | — | PAUSED pending Brendan delete; owned by Arrow Attio People | — |

## Full description (verbatim, secrets stripped)

```
pending self-report — owning bot will paste its live description here
```

## Rebuild steps

1. pending self-report

## Arrow duties (if Cross)

If this bot touches Arrow, describe them only generically here and point to [`arrow/BOTS.md`](../arrow/BOTS.md). Never copy Arrow operational detail or personal/Delta/Janey detail across the silo boundary the wrong way.
