---
# Bot registry entry template
# Copy to bots/<slug>.md (Personal / Delta / Janey / Cross) or arrow/bots/<slug>.md (Arrow only).
# Strip secrets, tokens, account numbers, and personal credentials before saving.
# The bot that owns this entry updates it whenever its description, routines, cadence, or send permissions change, in the same commit as arrow/BOTS.md or bots/MAP.md plus a one-line CHANGELOG.md entry. Touch INDEX.md only when a skill or file is added or removed.
---

# <Bot display name>

| Field | Value |
|---|---|
| **Name** | |
| **Bot id** | |
| **Silo** | Arrow / Personal / Delta / Janey / Cross |
| **Purpose** | (1 line) |
| **Status** | live / test / paused |
| **Repo skill path** | `skills/…` or `arrow/skills/…` |
| **Data sources (read)** | |
| **Write targets** | |
| **Send permissions** | none / draft-only / test-only / auto-send to &lt;whom&gt; |
| **Depends on** | |
| **Feeds** | |
| **Connectors needed** | (names only, no secrets) |
| **Last verified** | YYYY-MM-DD · &lt;who&gt; |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| | | | | | |

## Full description (verbatim, secrets stripped)

```
(paste the live bot description here)
```

## Rebuild steps

1. Create the bot with the description above.
2. Connect the listed connectors (OAuth; no secrets in this file).
3. Create each routine with the cron and prompt above.
4. Run one manual check; set **Last verified**.

