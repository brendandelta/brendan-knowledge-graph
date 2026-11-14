# Brendan <> Vaidehi Market Reports

| Field | Value |
|---|---|
| **Name** | Brendan <> Vaidehi Market Reports |
| **Bot id** | 3e40c3a8-3cce-401b-b460-d6ac50940c3c |
| **Silo** | Arrow |
| **Purpose** | Sun-Thu 7pm PT Arrow Fund-branded external markets HTML brief (weekend edition on Sunday, daily Company file) + Mon-Fri 10:30am thinking note, for Brendan and Vaidehi ahead of Brendan & Vaidehi Get Smart (11:00-11:15am PT weekdays). |
| **Status** | live v0.4 (approved 2026-11-09) · recipients Brendan + Vaidehi · display name "Brendan <> Vaidehi Market Reports"; first test brief sent 2026-11-05 (Gmail id 1a1adefbeab7c03d) |
| **Repo skill path** | `arrow/skills/arrow-morning-brief/SKILL.md` (+ `template.html` + `profiles/`) |
| **Data sources (read)** | Public web / RSS / press only (free stack in the skill). admin@arrowfund.co Gmail thread for any replies. `profiles/INDEX.md` for covered companies. **Never Attio.** |
| **Write targets** | Local archive `/workspace/arrow-morning-brief/YYYY-MM-DD/` (brief.html, sources.json, company-file.md, agenda.md, run.log). Company files in `arrow/skills/arrow-morning-brief/profiles/`. Email send via Composio Gmail (`GMAIL_SEND_EMAIL`, is_html) to Brendan and Vaidehi. |
| **Send permissions** | brendan@arrowfund.co and vaidehi@arrowfund.co from admin@arrowfund.co (the Gmail connection for admin@arrowfund.co), display name Brendan Bot when allowed (Composio GMAIL_SEND_EMAIL has no display-name field as of 2026-11-05, so From shows the account default). No Signal / iMessage in v1. Recurring Get Smart invite created 2026-11-09 on brendan@arrowfund.co (event id nfu8do9k1g5o1i355g0lrkbarg). |
| **Depends on** | Composio Gmail (admin@ / the Gmail connection for admin@arrowfund.co); Composio Google Calendar (the Google Calendar connection for brendan@arrowfund.co) for the existing invite only; public web |
| **Feeds** | Brief + thinking-note emails; weekday Brendan & Vaidehi Get Smart call |
| **Connectors needed** | Composio Gmail (the Gmail connection for admin@arrowfund.co). Web fetch / browser for sources. |
| **Last verified** | 2026-11-09 · v0.4 package live |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Evening brief | `evening-brief` | `0 19 * * 0-4` | Sun-Thu 7:00pm PT, flexible, `lateMinutes: 0` (may start ~6:45pm); Sunday = weekend edition; **no Fri/Sat**. Dedupe guard: skips sending if today's `run.log` already shows a sent brief | Pull repo → follow skill §3 → gather/write (incl. Company file)/render/QA → HTML email → save profile | brendan@ + vaidehi@; archive `/workspace/arrow-morning-brief/YYYY-MM-DD/` |
| Morning agenda | `morning-agenda` | `30 10 * * 1-5` | Mon-Fri 10:30am PT, flexible, `lateMinutes: 0` | Pull repo → follow skill §4 → write thinking note → email | brendan@ + vaidehi@; agenda.md on box |

## Full description (live bot description; secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/arrow-morning-brief/SKILL.md (with template.html beside it); your registry entry is arrow/bots/arrow-morning-brief.md. Keep them in sync: whenever your instructions or a routine change, make the same change in the repo, committed with a CHANGELOG line (INDEX.md only when a file is added or removed). If this prompt and the files ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

You are Brendan Conn's Arrow Morning Brief bot for Arrow Fund (America/Los_Angeles). Sun-Thu at 7pm PT you write and email a beautiful, mobile-first, Arrow Fund-branded HTML markets brief (world and economy news, macro, public markets, private markets and secondaries, one daily Company file, All-In-style takes, thought starters) for the next day's weekday 11am PT 15-minute call, Brendan & Vaidehi Get Smart. Sunday is a beefed-up weekend edition covering Fri-Sun plus the week ahead. Mon-Fri at 10:30am PT you email a short thinking note on the prior night's brief (sharper takes, what would change our mind, so what this week), folding in any thoughts they already sent. Follow arrow/skills/arrow-morning-brief exactly.

Rules: you send from admin@arrowfund.co via Composio Gmail (the Gmail connection for admin@arrowfund.co), display name 'Brendan Bot' when allowed. Recipients: brendan@arrowfund.co and vaidehi@arrowfund.co. Content is 100% external: never use Attio or name Arrow deals, prices or counterparties. Free sources first, every fact linked, nothing invented. No Signal or iMessage. Don't create extra calendar events without Brendan's OK. Message only Brendan, in this chat.
```

## Rebuild steps

1. Create the bot with the description above.
2. Confirm the Composio Gmail connection for admin@arrowfund.co is connected; OAuth only, no secrets here.
3. Create both routines at setup: "Evening brief" (folder `evening-brief`, `0 19 * * 0-4`, flexible, `lateMinutes: 0`, with the run.log dedupe guard) and "Morning agenda" (folder `morning-agenda`, `30 10 * * 1-5`, flexible, `lateMinutes: 0`). Both pull the repo, then follow the skill.
4. Copy `arrow/skills/arrow-morning-brief/template.html` into `/workspace/arrow-morning-brief/` on first run; follow the skill.
5. Recipients are live (Brendan + Vaidehi). Weekday call Brendan & Vaidehi Get Smart exists on brendan@arrowfund.co (Mon-Fri 11:00-11:15am PT). Set **Last verified**.
