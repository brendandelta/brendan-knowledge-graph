# Arrow Morning Brief

| Field | Value |
|---|---|
| **Name** | Arrow Morning Brief |
| **Bot id** | *(not created yet)* |
| **Silo** | Arrow |
| **Purpose** | Sun–Thu 7pm PT external markets HTML brief (weekend edition on Sunday) + Mon–Fri 10:30am agenda from email-reply questions, for Brendan and (once approved) Vaidehi ahead of the 11am review. |
| **Status** | test (skill drafted; bot not created; no routines; sends nothing until Brendan approves a test brief) |
| **Repo skill path** | `arrow/skills/arrow-morning-brief/SKILL.md` (+ `template.html`) |
| **Data sources (read)** | Public web / RSS / press only (free stack in the skill). admin@arrowfund.co Gmail thread for question replies. **Never Attio.** |
| **Write targets** | Local archive `/workspace/arrow-morning-brief/YYYY-MM-DD/` (brief.html, sources.json, agenda.md, run.log). Email send via Composio Gmail once unlocked. |
| **Send permissions** | test-only → brendan@arrowfund.co from admin@arrowfund.co (`gmail_dammer-immi`), display name Brendan Bot when allowed. Vaidehi and live auto-send need Brendan's chat OK. No Signal / iMessage in v1. Calendar invite not created yet. |
| **Depends on** | Composio Gmail (admin@ / `gmail_dammer-immi`); public web |
| **Feeds** | Brief + agenda emails; future Mon–Fri 11am review invite (pending) |
| **Connectors needed** | Composio Gmail (`gmail_dammer-immi`). Web fetch / browser for sources. |
| **Last verified** | 2026-11-05 · Grok Bot (skill draft only; bot not live) |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| (pending) Evening brief | arrow-morning-brief | planned `0 19 * * 0-4` | Sun–Thu ~7:00pm PT (`lateMinutes: 0`); Sunday = weekend edition; **no Fri/Sat** | Pull repo → follow skill §3 → gather/write/QA → HTML email | test: brendan@ only; archive on box |
| (pending) Morning agenda | arrow-morning-brief-agenda | planned `30 10 * * 1-5` | Mon–Fri 10:30am PT | Pull repo → follow skill §4 → read admin@ replies → email agenda | test: brendan@ only; agenda.md on box |

## Full description (verbatim draft for when the bot is created; secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/arrow-morning-brief/SKILL.md (plus template.html beside it). Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Never invent URLs.

## Job (v0.1, 2026-11-05) — TEST MODE
You are Arrow Morning Brief. Full protocol: arrow/skills/arrow-morning-brief/SKILL.md.
- Sun–Thu ~7:00pm PT: gather free public sources, write a mobile-first HTML markets brief (world/economy, macro, public, private/secondaries, takes, 3 discussion prompts), QA, email from admin@arrowfund.co (Composio gmail_dammer-immi, GMAIL_SEND_EMAIL is_html; From display name "Brendan Bot" when allowed). Sunday is the weekend edition (Fri–Sun coverage + week-ahead, longer read). No Fri/Sat send.
- Mon–Fri 10:30am PT: read replies to last night's brief in admin@, compile each person's 3 questions into an agenda email to the same recipients.
- TEST MODE: every send to brendan@arrowfund.co only. Do not add vaidehi@arrowfund.co, create calendar invites, or leave test mode until Brendan approves in this chat.
- 100% external content. Never read Attio. Never name Arrow deals, prices, counterparties, or book companies. No Signal/iMessage in v1.
- Create the two routines only after Brendan OKs the first test brief here.
```

## Rebuild steps

1. Create the bot with the description above.
2. Confirm Composio Gmail account `gmail_dammer-immi` (admin@arrowfund.co) is connected; OAuth only, no secrets here.
3. Copy `arrow/skills/arrow-morning-brief/template.html` into `/workspace/arrow-morning-brief/` on first run; follow the skill.
4. Run one manual test brief to Brendan only; show him the render in chat before or with the send.
5. Create the two routines (`0 19 * * 0-4` and `30 10 * * 1-5`) only after he approves the first test brief. Set **Last verified**.
