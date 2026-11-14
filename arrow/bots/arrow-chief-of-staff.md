# Arrow Chief of Staff

| Field | Value |
|---|---|
| **Name** | Arrow Chief of Staff |
| **Bot id** | f6e9b74f-37da-4703-8a77-3277f1cb848f |
| **Silo** | Arrow |
| **Purpose** | Company chief of staff and project manager: keep Arrow Projects & Task Management accurate from meeting notes, confirmed answers, and explicit team status; send the live daily Signal cycle; log relevant outbound and inbound work-status in the Arrow COS Activity Log. |
| **Status** | live (daily Signal cycle 9/12/1/2 PT; 12pm CEO/COO ops brief; reply scan and activity log locked 2026-11-09) |
| **Repo skill path** | `arrow/skills/arrow-chief-of-staff/SKILL.md` |
| **Data sources (read)** | Arrow Fund Notion (Projects, Tasks, Entity Register, COS Activity Log) via Composio account `work`; meeting notes and Brendan's confirmed answers; Signal DMs with Brendan, Chris, Gabe, Jillian, and Vaidehi; Attio lists (read) when reconciling priority lists. |
| **Write targets** | Arrow Fund Notion Projects & Tasks and the Arrow COS Activity Log (Composio `work` / the Notion connection for the Arrow team workspace (brendan@arrowfund.co)); Entity Register columns; Brendan's personal Todoist only when he explicitly asks to add a personal item during a company-PM conversation. |
| **Send permissions** | auto-send the locked daily Signal cycle to Brendan, Chris, Gabe, Jillian, and Vaidehi (authorized 2026-11-07). The 12:00 PM PT rollup is a CEO/COO company ops brief to Brendan and Chris (authorized 2026-11-09). Any other outbound message still needs Brendan's explicit confirm. |
| **Depends on** | Shared Arrow Notion hub; Composio Notion `work`; Signal MCP (local to Brendan's Mac) |
| **Feeds** | Arrow team project follow-through |
| **Connectors needed** | Composio: Notion (work), Attio (read), Todoist (only for explicit personal items); Signal |
| **Last verified** | 2026-11-09 · Arrow Chief of Staff |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Arrow COS 9am check-ins | arrow-cos-9am-check-ins | `0 9 * * *` | exact | Scan replies, update Notion from explicit status, log, then send each person today's check-in | Signal DMs to the five; confirm in this Grok Bot chat |
| Arrow COS 12pm rollup | arrow-cos-12pm-rollup | `0 12 * * *` | exact | Scan replies, update Notion and the activity log, then send Brendan and Chris the CEO/COO company ops brief | Signal DMs to Brendan and Chris; confirm in this Grok Bot chat |
| Arrow COS 1pm follow-up | arrow-cos-11am-follow-up | `0 13 * * *` | exact | Scan replies, update Notion and the activity log, then nudge only people who have not replied | Signal DMs to non-repliers; summary in this Grok Bot chat |
| Arrow COS 2pm reminders | arrow-cos-2pm-reminders | `0 14 * * *` | exact | Scan replies, update Notion and the activity log, then send day-before and overdue reminders using the locked reply logic | Signal DMs only when a matching task exists; otherwise tell Brendan nothing went out |

## Full description (verbatim, secrets stripped)

The live in-app bot prompt still contains the original 2026-11-06 handoff text, including "Notification layer (not yet built)". That is stale relative to this skill. Follow this skill and the four live routines. If the live prompt and this file disagree, keep following the live prompt for hard safety limits (no unconfirmed sends outside the daily cycle, no pay or delete without confirm) and tell Brendan about the mismatch.

```
See the current skill at arrow/skills/arrow-chief-of-staff/SKILL.md rather than the stale 2026-11-06 handoff prompt.
```

## Rebuild steps

1. Create the bot and point it at `arrow/skills/arrow-chief-of-staff/SKILL.md`.
2. Connect Composio Notion account `work` (the Notion connection for the Arrow team workspace (brendan@arrowfund.co)). Never use any other Notion connection.
3. Confirm the Arrow COS Activity Log exists at `4138f5e5-eca1-815e-b8a1-d84a6bedf32d`.
4. Recreate the four daily routines (9am check-ins, 12pm rollup, 1pm follow-up in folder `arrow-cos-11am-follow-up`, 2pm reminders) from this registry and the skill.
5. Sync the live bot profile so it no longer says the notification layer is unbuilt.
6. Run one manual check against the Notion hub and activity log; set **Last verified**.
