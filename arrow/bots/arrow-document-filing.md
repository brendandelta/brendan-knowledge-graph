# Arrow Document Filing Bot

Arrow-facing record only: this bot also serves a second, separate entity Drive. AGENTS.md requires a full cross-silo entry under `bots/`; relocation is pending a separately scoped update. Non-Arrow details are intentionally omitted here.

| Field | Value |
|---|---|
| **Name** | Arrow Document Filing Bot |
| **Bot id** | 7ba25253-3d1f-43c9-910a-4c3778eb60b8 |
| **Silo** | Cross; Arrow-facing details only |
| **Purpose** | Daily filing of Arrow Fund shared-drive `99 - Inbox`, plus questions and a summary for Brendan in its chat; also serves a second, separate entity Drive. |
| **Status** | live |
| **Repo skill path** | `arrow/skills/document-filing` (shared procedure, incl. the always-post daily summary), `arrow/skills/arrow-fund-inbox` (Arrow wrapper); second-entity wrapper lives outside `arrow/` |
| **Data sources (read)** | Google Drive via Composio, Arrow account `googledrive_left-bim`, and Drive README. Read-only QuickBooks/Gmail receipt lookups when Brendan approves. Non-Arrow accounts omitted. |
| **Write targets** | Arrow Drive: copies, moves to archive, renames, README Google Docs; knowledge repo. Non-Arrow targets omitted. |
| **Send permissions** | Sends no emails or messages to anyone; posts only to Brendan in its own chat. Saved send rules allow Composio connector calls without asking; not blanket permission to send externally. |
| **Depends on** | Repo, filing procedure, per-Drive wrapper and README; explicit Drive account selection. |
| **Feeds** | Filed documents, archived originals, questions and weekday chat summaries. |
| **Connectors needed** | `user-composio` (`googledrive`, `googledocs`); approved receipt lookups require QuickBooks/Gmail access. GitHub push uses the existing repo-only token in the git credential store, never copied into registry. |
| **Last verified** | 2026-11-05 · owner-bot self-report (title, description, slim routine set) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Daily inbox filing (Arrow + separate entity); live name omitted to preserve silo | Omitted: live folder contains non-Arrow entity name; restore from private registry | `0 12 * * 1-5` | Weekdays, flexible ±15 min; server picked 11:49am PT start | Slim: `git -C /home/box/Claude/brendan-knowledge-graph pull`, read AGENTS.md, then follow `document-filing` (scheduled-run section) with the per-drive wrappers; Arrow first, separate pass for the second entity; all logic lives in the skills. | Summary and questions to Brendan in this bot's Grok Bot chat every weekday, even when empty. |

## Full description (verbatim, secrets stripped)

**Title:** Daily Drive inbox filing

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill files in the repo are arrow/skills/document-filing/SKILL.md (shared procedure) and arrow/skills/arrow-fund-inbox/SKILL.md (Arrow wrapper). Keep the two in sync: whenever your instructions or a routine change, make the same change in those skill files, committed together with INDEX.md and CHANGELOG.md. If this prompt and the files ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

You are Brendan's document filing bot. Every weekday around 12pm PT you clear the Arrow Fund shared drive's `99 - Inbox` (and, in a fully separate pass, a second entity's Drive inbox via its own wrapper skill under skills/, outside arrow/). Each drive's README in that drive is the data (structure, IDs, rules); the skills are the steps. File only clear fits (copy to destination with the README naming, original to `0 Archive`, never delete, never create folders without approval), ask everything else as questions in this chat, and post a summary every weekday run, even when empty.
```

## Rebuild steps

1. Create the bot with the title and description above, verbatim; retain prior bot id as reference.
2. Connect listed connectors with explicit Arrow account selection; use existing repo-only git credentials without exposing them.
3. Load shared document-filing and Arrow inbox skills plus the Drive README. Restore separate-entity configuration and exact routine name/folder from a private registry entry, never from Arrow files.
4. Restore weekday noon America/Los_Angeles routine, flexible ±15 min, with copy-then-archive filing, no deletion, and chat summaries even when empty.
5. Run one manual check and update Last verified. Registry documentation does not alter live routines or send permissions.
