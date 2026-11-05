# Arrow Document Filing Bot

Arrow-facing record only: this bot also serves a second, separate entity Drive. AGENTS.md requires a full cross-silo entry under `bots/`; relocation is pending a separately scoped update. Non-Arrow details are intentionally omitted here.

| Field | Value |
|---|---|
| **Name** | Arrow Document Filing Bot |
| **Bot id** | 7ba25253-3d1f-43c9-910a-4c3778eb60b8 |
| **Silo** | Cross; Arrow-facing details only |
| **Purpose** | Daily filing of Arrow Fund shared-drive `99 - Inbox`, plus questions and a summary for Brendan in its chat; also serves a second, separate entity Drive. |
| **Status** | live |
| **Repo skill path** | `arrow/skills/document-filing`, `arrow/skills/arrow-fund-inbox` |
| **Data sources (read)** | Google Drive via Composio, Arrow account `googledrive_left-bim`, and Drive README. Read-only QuickBooks/Gmail receipt lookups when Brendan approves. Non-Arrow accounts omitted. |
| **Write targets** | Arrow Drive: copies, moves to archive, renames, README Google Docs; knowledge repo. Non-Arrow targets omitted. |
| **Send permissions** | Sends no emails or messages to anyone; posts only to Brendan in its own chat. Saved send rules allow Composio connector calls without asking; not blanket permission to send externally. |
| **Depends on** | Repo, filing procedure, per-Drive wrapper and README; explicit Drive account selection. |
| **Feeds** | Filed documents, archived originals, questions and weekday chat summaries. |
| **Connectors needed** | `user-composio` (`googledrive`, `googledocs`); approved receipt lookups require QuickBooks/Gmail access. GitHub push uses the existing repo-only token in the git credential store, never copied into registry. |
| **Last verified** | 2026-11-05 · owner-bot self-report supplied for registry |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Daily inbox filing (Arrow + separate entity); live name omitted to preserve silo | Omitted: live folder contains non-Arrow entity name; restore from private registry | `0 12 * * 1-5` | Weekdays, flexible ±15 min; server picked 11:49am PT start | Pull repo; load document-filing and per-Drive wrappers; file clear fits by copying to destination then moving original to `0 Archive`, never delete; ask about the rest. | Summary and questions to Brendan in this bot's Grok Bot chat every weekday, even when empty. |

## Full description (verbatim, secrets stripped)

The profile description is empty (no title either).

There is no description text to quote. The purpose above is a summary, not an invented live description.

## Rebuild steps

1. Create the bot with an empty profile description and no title; retain prior bot id as reference.
2. Connect listed connectors with explicit Arrow account selection; use existing repo-only git credentials without exposing them.
3. Load shared document-filing and Arrow inbox skills plus the Drive README. Restore separate-entity configuration and exact routine name/folder from a private registry entry, never from Arrow files.
4. Restore weekday noon America/Los_Angeles routine, flexible ±15 min, with copy-then-archive filing, no deletion, and chat summaries even when empty.
5. Run one manual check and update Last verified. Registry documentation does not alter live routines or send permissions.
