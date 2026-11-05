# Arrow Morning Brief

| Field | Value |
|---|---|
| **Name** | Arrow Morning Brief |
| **Bot id** | 3e40c3a8-3cce-401b-b460-d6ac50940c3c |
| **Silo** | Arrow |
| **Purpose** | Sun–Thu 7pm PT external markets HTML brief (weekend edition on Sunday) + Mon–Fri 10:30am agenda from email-reply questions, for Brendan and (once approved) Vaidehi ahead of the 11am review. |
| **Status** | test (routines live, Brendan-only) · bot created 2026-11-05; first test brief sent to Brendan 2026-11-05 (Gmail id 1a1adefbeab7c03d) |
| **Repo skill path** | `arrow/skills/arrow-morning-brief/SKILL.md` (+ `template.html`) |
| **Data sources (read)** | Public web / RSS / press only (free stack in the skill). admin@arrowfund.co Gmail thread for question replies. **Never Attio.** |
| **Write targets** | Local archive `/workspace/arrow-morning-brief/YYYY-MM-DD/` (brief.html, sources.json, agenda.md, run.log). Email send via Composio Gmail (`GMAIL_SEND_EMAIL`, is_html); TEST MODE = Brendan only. |
| **Send permissions** | test-only → brendan@arrowfund.co from admin@arrowfund.co (`gmail_dammer-immi`), display name Brendan Bot when allowed (Composio GMAIL_SEND_EMAIL has no display-name field as of 2026-11-05, so From shows the account default). Vaidehi and live auto-send need Brendan's chat OK. No Signal / iMessage in v1. Calendar invite not created yet. |
| **Depends on** | Composio Gmail (admin@ / `gmail_dammer-immi`); public web |
| **Feeds** | Brief + agenda emails; future Mon–Fri 11am review invite (pending) |
| **Connectors needed** | Composio Gmail (`gmail_dammer-immi`). Web fetch / browser for sources. |
| **Last verified** | 2026-11-05 · Arrow Morning Brief bot (first test brief sent; routines live) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Evening brief | `evening-brief` | `0 19 * * 0-4` | Sun–Thu 7:00pm PT, flexible, `lateMinutes: 0` (may start ~6:45pm); Sunday = weekend edition; **no Fri/Sat**. Dedupe guard: skips sending if today's `run.log` already shows a sent brief | Pull repo → follow skill §3 → gather/write/render/QA → HTML email | test: brendan@ only; archive `/workspace/arrow-morning-brief/YYYY-MM-DD/` |
| Morning agenda | `morning-agenda` | `30 10 * * 1-5` | Mon–Fri 10:30am PT, flexible, `lateMinutes: 0` | Pull repo → follow skill §4 → read admin@ replies → email agenda | test: brendan@ only; agenda.md on box |

## Full description (live bot description; secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull --rebase origin main` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/arrow-morning-brief/SKILL.md (plus template.html beside it). Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

## Job — TEST MODE (routines live 2026-11-05)
You are Arrow Morning Brief. Follow arrow/skills/arrow-morning-brief/SKILL.md exactly.
- Sun–Thu 7:00pm PT (routine "Evening brief"): gather free public sources, write the mobile-first HTML markets brief (cold open, world/economy, macro, public, private/secondaries, takes, 3 discussion prompts, sources), QA, and email it. Sunday is the weekend edition (Fri–Sun coverage + week ahead, longer read). No Fri/Sat send. Skip the send if today's run.log already shows a sent brief.
- Mon–Fri 10:30am PT (routine "Morning agenda"): read replies to last night's brief in admin@, compile each person's 3 questions into the agenda email.
- Send from admin@arrowfund.co via Composio gmail_dammer-immi (GMAIL_SEND_EMAIL, is_html), From display name "Brendan Bot" when the tool allows it.
- TEST MODE: every send to brendan@arrowfund.co only. Add vaidehi@arrowfund.co only after Brendan approves in this chat.
- 100% external content. Never read Attio. Never name Arrow deals, prices, counterparties, or book companies. Free sources only. Every fact linked; never invent numbers, quotes, or dates.
- No Signal or iMessage. No calendar events without Brendan's OK. Message only Brendan.
```

## Rebuild steps

1. Create the bot with the description above.
2. Confirm Composio Gmail account `gmail_dammer-immi` (admin@arrowfund.co) is connected; OAuth only, no secrets here.
3. Create both routines at setup: "Evening brief" (folder `evening-brief`, `0 19 * * 0-4`, flexible, `lateMinutes: 0`, with the run.log dedupe guard) and "Morning agenda" (folder `morning-agenda`, `30 10 * * 1-5`, flexible, `lateMinutes: 0`). Both pull the repo, then follow the skill.
4. Copy `arrow/skills/arrow-morning-brief/template.html` into `/workspace/arrow-morning-brief/` on first run; follow the skill.
5. Run one manual test brief to Brendan only. Keep TEST MODE until he approves going live in chat. Set **Last verified**.
