---
name: arrow-morning-brief
description: Sun-Thu external markets brief (world/economy, macro, public, private/secondaries, one Company file, All-In-style takes, thought starters) emailed as a mobile-first Arrow Fund HTML article to Brendan and Vaidehi; sends a 10:30am PT thinking note for the weekday 11am Brendan & Vaidehi Get Smart call.
---

# arrow-morning-brief

> **Live recipients (since 2026-11-06).** Sends go to **brendan@arrowfund.co** and **vaidehi@arrowfund.co**. Do **not** add more recipients. The weekday call invite **Brendan & Vaidehi Get Smart** (Mon-Fri 11:00-11:15am PT) was approved and created 2026-11-09. Do not create extra calendar events without Brendan's OK.

**Status:** live · bot 3e40c3a8-3cce-401b-b460-d6ac50940c3c · routines live · recipients Brendan + Vaidehi (approved 2026-11-06) · v0.4 package approved 2026-11-09
**Silo:** Arrow only. Never put personal, Delta, or Janey content here.
**Source of decisions:** Brendan, 2026-11-05 (plan Page + chat; cadence updated same day to Sun-Thu briefs / Mon-Fri reviews, Sunday weekend edition). Sample look: `/workspace/morning-brief/sample-2026-11-05.html`. v0.4 approved 2026-11-09: daily Company file, 11am thinking note, profile tracking, Arrow Fund branding, call named Brendan & Vaidehi Get Smart.

## Bot instructions (keep in sync with the live bot profile)

- Read `AGENTS.md` in this repo first. The bot's own instructions are its working copy; keep this file in sync. If they disagree, follow the bot prompt and tell Brendan.
- Every user-facing reply that names an action, app, email, doc, or web page follows `embed-action-links`.
- Never move money, send, delete, or pay without Brendan's explicit confirm - beyond the approved nightly brief send once live mode is unlocked.
- **Content scope is 100% external.** Never read Attio. Never name Arrow deals, prices, counterparties, book companies, or any internal CRM fact. Private-market color is public news only.
- **No Signal or iMessage in v1.** Delivery is HTML email only (admin@ → recipients).
- Routines pull the repo first, then follow this file.

## 1. Cadence and recipients

| Job | Cron (America/Los_Angeles) | Timing | Recipients |
|---|---|---|---|
| Generate + email brief | `0 19 * * 0-4` | 7:00pm PT **Sun-Thu only**; `lateMinutes: 0` so it may start ~6:30pm. **No send Fri/Sat.** | brendan@arrowfund.co and vaidehi@arrowfund.co |
| Build thinking note | `30 10 * * 1-5` | 10:30am PT **Mon-Fri** (covers the prior evening's brief; Monday note covers Sunday's weekend edition) | same as above |

- **Review call:** 11:00-11:15am PT **Mon-Fri**. Title: `Brendan & Vaidehi Get Smart`. Created 2026-11-09 for Brendan and Vaidehi. Do not create extra calendar events without Brendan's OK.
- **Subject (brief, Mon-Thu evenings):** `Arrow Fund · Morning Markets · for <Weekday>'s Get Smart` (e.g. `Arrow Fund · Morning Markets · for Friday's Get Smart`).
- **Subject (brief, Sunday evening):** `Arrow Fund · Morning Markets · Weekend + week ahead · for Monday's Get Smart`.
- **Subject (thinking note):** `Arrow Fund · Get Smart · <Weekday Mon D>` (e.g. `Arrow Fund · Get Smart · Fri Nov 6`).
- Older threads used `Arrow Morning Markets · for` / `Agenda · Arrow Morning Markets`. When searching admin@ for last night's brief, match either the new subject or that older prefix.
- **Sunday = weekend edition** (see §3b-Sunday): covers everything important from Friday, Saturday and Sunday; longer week-ahead + weekend recap; slightly longer read (target ~12-15 min). Mon-Thu editions stay the standard 8-12 min overnight brief.
- From: **admin@arrowfund.co** via Composio Gmail account id **`gmail_dammer-immi`**, tool `GMAIL_SEND_EMAIL` with `is_html: true`. Set the From display name to **`Brendan Bot`** if Gmail allows it for this mailbox; otherwise leave the default (Arrow Admin / admin@).
- Send once per job. On gather/render failure: retry up to 2 times, 10 minutes apart; if still failing, email Brendan a short plain-text delay note with the same subject + ` (delayed)` and stop. Never send a partial brief.

## 2. Files and archive

| Path | Purpose |
|---|---|
| `arrow/skills/arrow-morning-brief/SKILL.md` | This procedure (source of truth) |
| `arrow/skills/arrow-morning-brief/template.html` | Email-safe HTML shell; fill tokens, never rewrite structure day to day |
| `arrow/skills/arrow-morning-brief/profiles/INDEX.md` | Covered-company list (never repeat a name listed here) |
| `arrow/skills/arrow-morning-brief/profiles/<slug>.md` | One markdown Company file per covered company |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/brief.html` | Rendered brief archive |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/sources.json` | Citation list used that day |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/company-file.md` | That day's Company file copy |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/agenda.md` | Morning thinking note |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/run.log` | Gather / QA / send log |

Copy `template.html` from the skill folder into `/workspace/arrow-morning-brief/template.html` on first run if missing; prefer the repo copy when they drift.

## 2b. Arrow Fund branding

The brief is an Arrow Fund product. Keep the approved section structure and fill `template.html` rather than inventing a new layout. Official colors (same family as Arrow Weekly):

- Navy `#1F2A44` for the masthead, footer, and takes-desk panel
- Arrow blue `#2E5AAC` for the ARROW FUND wordmark accent, links, and the Company file rule
- Paper `#f4f1ea` behind a white card; alternating band `#EEF2F8` or the template cream `#faf8f3`

Masthead always shows **ARROW FUND** above **Morning Markets Brief**, then the day's title (`For <Weekday>'s Brendan & Vaidehi Get Smart`). Footer: `Arrow Fund · Morning Markets Brief · external news only`. Do not stamp Confidential on this public-news brief. Do not restyle colors or type day to day.

## 3. Procedure - evening brief (`0 19 * * 0-4`, Sun-Thu)

Every routine starts with:

```bash
git -C /home/box/Claude/brendan-knowledge-graph pull --rebase origin main
```

Then read this skill and `template.html`.

### 3a. Gather (~24-36h window ending at run time)

Collect public headlines and figures only from the **free source stack** in §5. Prefer RSS / press pages / primary releases. If a site Cloudflare-blocks automated fetch, use a public Google News result that links the original, or drop the story.

Window: roughly the last 36 hours ending at run time (PT). Anything older than ~36 hours is dropped unless clearly labeled as background ("since Monday…").

Capture for each candidate: headline, 2-4 sentence substance, publish time, canonical URL, optional hero image URL (`og:image` when available and hot-linkable), and the exact figure strings you will quote.

Also collect private-company names that appear in that window (funding, product, regulation, competitor moves). These feed the Company file picker in §3b-company.

### 3b. Write (fixed section order - never reorder)

Target length: **8-12 minute read**, mobile-first (~640px email). Tone: Morning Brew pace + WSJ-grade market seriousness + All-In-style takes. Short paragraphs. No AI self-attribution. No hype adjectives without a cited print.

1. **Masthead** - ARROW FUND wordmark; `Morning Markets Brief`; title `For <Weekday>'s Brendan & Vaidehi Get Smart`; sent date/time PT; recipients line (`To Brendan and Vaidehi · Brendan & Vaidehi Get Smart · 11:00am PT`). Do **not** ask anyone to reply with 3 questions.
2. **Cold open** - 3-5 bullets, the so-what. Every bullet ends with a source link.
3. **World & economy** - 2-4 stories that move policy, capital, or risk. Prefer one hero image per story when a clean `og:image` exists.
4. **Macro** - rates, inflation, central banks, dollar, oil. **Verified figures only.** Omit any print you cannot cite.
5. **Public markets** - indices and sector movers with cited closes / moves. Include a one-line "Why it matters for private markets" when the tape clearly connects; still no Arrow names.
6. **Private markets & secondaries** - funding rounds, continuation vehicles, LP liquidity color, sector snapshots - only when primary-sourced (company / firm release, TechCrunch, Crunchbase News, BusinessWire, etc.).
7. **Company file** - one private company, after private markets and before Takes desk. Follow §3b-company. About 400-600 words.
8. **Takes desk** - 2-3 provocations grounded in today's linked stories (the Company file may be one of them). Label them as opinion, not predictions. No long transcript dumps.
9. **Thought starters** - bot seeds **exactly 3** prompts using the lenses in §7, written for Brendan & Vaidehi Get Smart. Do **not** ask anyone to email 3 questions back.
10. **Sources used today** - footer list of every citation URL used in the body.

### 3b-company. Daily Company file

**Placement:** after Private markets & secondaries, before Takes desk.

**Length and structure:** detailed but short, about 400-600 words. Compact version of [Sequoia's company-breakdown guide](https://sequoiacap.com/article/writing-a-business-plan), with a16z's market / product / team / "why now" lens:

1. Why this company today (the news hook that ties it to the day's tape)
2. Purpose, in one sentence
3. Problem, and who feels it
4. What they sell, and who the customers are
5. Why now
6. How they make money
7. Competition and alternatives
8. Scale we can cite (funding, valuation, customers), only if a source printed it
9. Team, briefly, if it is public
10. One open question, to feed Brendan & Vaidehi Get Smart

If a field is not sourced, cut it rather than guess. Still 100% external: no Attio, no Arrow deal names.

**Picker (never repeat a company):**

1. Read `arrow/skills/arrow-morning-brief/profiles/INDEX.md` and also scan local `/workspace/arrow-morning-brief/*/company-file.md` so a failed git write cannot cause a repeat.
2. Each night, scan that day's news for private companies that are not on the covered list (treat obvious aliases as the same company).
3. Among uncovered names, **news relevance wins**.
4. If two names are equally tied to the day, pick the larger or later-stage one.
5. If a pre-revenue or very early company is clearly the story of the day, that one wins even if it is not a giant.
6. If the day's news does not name a strong private-company hook, pick the largest uncovered company that still has a light, honest tie to the tape (a sector move, a competitor, a rate or regulation story) and say so in "Why this company today".
7. Sunday's weekend edition still gets one Company file.

**After a successful send:** write `arrow/skills/arrow-morning-brief/profiles/<slug>.md` (kebab-case company name), append a row to `profiles/INDEX.md`, copy the same markdown to `/workspace/arrow-morning-brief/YYYY-MM-DD/company-file.md`, then `git pull --rebase origin main`, commit **only** those two profile files, and push. Do not bundle unrelated dirty files. If the commit or push fails, say so to Brendan; the local archive copy is the backup so the next run still sees the name.

### 3b-Sunday. Weekend edition extras (Sunday 7pm only)

When the run day is Sunday (PT):

1. **Window:** gather from Friday 00:00 PT through Sunday run time (≈60-72h), not the usual ~36h. Label older Friday items as weekend context, not "breaking".
2. **Add / expand sections** (keep the fixed order; enlarge these):
   - After **Cold open**, add a short **Weekend recap** subsection (or fold 2-3 recap bullets into cold open) covering Fri-Sun so-whats.
   - Expand **Private markets & secondaries** with any material weekend releases.
   - After **Takes desk** (or just before thought starters), add **Week ahead** - 3-5 dated catalysts for the coming Mon-Fri (jobs, CPI, Fed speakers, earnings clusters, known secondaries conferences / known public catalysts). Every catalyst needs a source or a clear "scheduled / consensus" label; never invent dates.
3. **Length:** target **12-15 min** read. Still mobile-first; do not balloon past ~15 stories total plus the Company file.
4. **Masthead title:** `For Monday's Brendan & Vaidehi Get Smart · Weekend edition`.
5. **Thought starters:** seed prompts should skew week-ahead ("what do we hunt Mon-Wed if X prints hot?").
6. Mon-Thu runs skip this block entirely.

### 3c. Render

Fill `template.html` tokens (see file comments). HTML-escape all inserted text. Inline CSS only (email-safe tables). Images: absolute HTTPS URLs, `width="592"`, `style="display:block;width:100%;max-width:592px;height:auto;border-radius:4px;"`. If an image URL fails a HEAD/GET check, omit the image rather than leave a broken one.

Write the filled HTML to `/workspace/arrow-morning-brief/YYYY-MM-DD/brief.html` and `sources.json`. Also write `/workspace/arrow-morning-brief/YYYY-MM-DD/company-file.md` (the markdown Company file) at render time so a later send failure still leaves a local copy.

Known template issue: the nested `<!-- omit tr ... -->` inside the head comment closes it early and can leak placeholder copy. Strip the head comment before sending.

### 3d. Quality gate (hard stop if any fail)

- [ ] Every factual claim has a working source link in the same story/bullet.
- [ ] No invented numbers, quotes, or dates. Unverifiable → cut.
- [ ] No Attio / Arrow deal / book / counterparty / price / internal content.
- [ ] No news older than ~36h unless labeled background (Sunday weekend edition: Fri-Sun window OK; label Friday items as weekend context).
- [ ] Spot-check links (at least every cold-open + section lead link) return 2xx or a readable article page.
- [ ] Deduped vs yesterday's brief archive (don't rehash the same lead without a new development).
- [ ] Company file is present, 400-600 words, follows the 10-part structure with unsourced fields cut, and the company is not already in `profiles/INDEX.md` or a prior local `company-file.md`.
- [ ] Masthead shows ARROW FUND and Morning Markets Brief; colors match the template (navy `#1F2A44`, Arrow blue `#2E5AAC`).
- [ ] Recipients are **brendan@arrowfund.co** and **vaidehi@arrowfund.co** (the approved live list).
- [ ] From account is `gmail_dammer-immi` / admin@.

Fail any check → fix or cut; never send a failing brief.

### 3e. Send

Call Composio `GMAIL_SEND_EMAIL` on account **`gmail_dammer-immi`**:

- `to`: `brendan@arrowfund.co` and `vaidehi@arrowfund.co`
- `subject`: per §1
- `body`: the rendered HTML
- `is_html`: true
- From display name `Brendan Bot` when the tool/account supports it

Do **not** BCC, CC extra people, or attach files in v1. After send, append a line to `run.log` (time, message id if returned, recipient list, pass/fail notes), then write and commit the Company file as in §3b-company. Post a one-line confirmation to Brendan in this bot's chat with the subject and archive path. Stay silent to others.

## 4. Procedure - morning thinking note (`30 10 * * 1-5`, Mon-Fri)

```bash
git -C /home/box/Claude/brendan-knowledge-graph pull --rebase origin main
```

Do **not** ask anyone to bring 3 questions. This email is a short provocation for Brendan & Vaidehi Get Smart.

1. Find last night's brief thread in **admin@arrowfund.co** (search subject `Arrow Fund · Morning Markets` or the older `Arrow Morning Markets · for`, matching today's review weekday, or the message-id saved in `run.log`). On Monday that is Sunday's weekend edition.
2. Re-read last night's so-whats and the Company file (from the thread or `/workspace/arrow-morning-brief/<brief-date>/`).
3. Read any replies from Brendan or Vaidehi. They are no longer required to send questions; if either emailed thoughts anyway, fold those in. Ignore signatures and quoted reply text.
4. Write `/workspace/arrow-morning-brief/YYYY-MM-DD/agenda.md` using the facilitator template in §8:
   - 2 or 3 sharper takes that push on last night's so-whats (including the Company file when it is the day's hook)
   - One "what would change our mind"
   - One "so what this week" (generic, no book names)
5. Email that note as a short HTML or plain+HTML note to the same recipient list as the brief (`brendan@arrowfund.co` and `vaidehi@arrowfund.co`), subject per §1, from the same admin@ account.
6. Do not create or modify calendar events as part of this job (the Get Smart invite already exists). Putting the note into the event description is optional and only if Brendan asks.
7. If nobody replied, still send the thinking note. Do not include a line asking them to bring 3 questions to the call.

## 5. Free source stack (v1)

| Section | Sources | Notes |
|---|---|---|
| World / economy | Axios top RSS (`https://api.axios.com/feed/top/`); AP / Reuters via public pages or Google News RSS; Guardian live blogs | Headlines + links; some sites block bots |
| Macro | Fed press RSS (`https://www.federalreserve.gov/feeds/press_all.xml`); FRED (`https://fred.stlouisfed.org/`); CNBC rates/jobs; BoE / ECB press | Never invent prints |
| Public markets | CNBC / MarketWatch / AP markets; public index closes from those stories | Cite the article that carried the print |
| Private / secondaries | Crunchbase News; TechCrunch; BusinessWire / company PE releases; NEPC public notes; SEC EDGAR when useful | CVs and mega-rounds when primary-sourced |
| Company file | Same private/secondaries stack, plus the company's own site or press page when needed for product / customer / model facts | Cut anything you cannot cite |
| Takes | All-In / BG2 / Acquired public notes; grounded takes from today's links | No long verbatim transcript dumps |

Paid / gated sources are out of scope for v1. If a free source repeatedly fails fetch, note it in `run.log` and rotate to another free source the same day; tell Brendan only if the brief would be thin without it.

## 6. Tone guide

- Clear, opinionated, sourced. Second-person is fine in thought starters; body copy stays third-person / desk voice.
- Numbers: use the exact figure the source printed; include the unit and the as-of.
- Quotes: only when you have the source open; attribute by name + outlet.
- Never write "as an AI", never invent "sources say".
- Private-markets bridge lines are allowed ("why it matters for secondaries") without naming Arrow's book.
- Takes desk may be punchy; everything else stays WSJ-clean.

## 7. Thought-starter lenses

Use these so Brendan & Vaidehi Get Smart stays useful. Seed prompts should map to at least one:

1. So what for Arrow's book or pipeline this week? *(generic - no deal names)*
2. What would change our mind on a sector or liquidity view?
3. Which hunt or pause does this imply?
4. Who should we call (person type / firm type - no confidential counterparties)?
5. What risk looks underpriced in the public tape for private marks?
6. What should we ignore so we do not waste cycles?

Do not require emailed questions. The 10:30am note carries the sharper takes.

## 8. Facilitator thinking-note template

```markdown
# Brendan & Vaidehi Get Smart · <Weekday Mon D> · 11:00-11:15am PT

## Cold open (0-3m)
- Align on last night's top 3 bullets (from the brief).

## Sharper takes (3-10m)
1. …
2. …
3. …

## What would change our mind
- …

## So what this week
- … (generic, no book names)

## Thoughts they already sent
- … (omit this heading if neither person emailed)

## Capture (12-15m)
- Owners + due dates (log below after the call when Brendan pastes takeaways).
```

Call shape: 0-3m align → work the provocations → last minutes capture owners + dues. Post-call capture auto-append to Firm Intel is **not** enabled; Brendan pastes manually for now.

## 9. Pending / still open (do not invent answers)

- Recurring Mon-Fri 11:00-11:15am PT invite **Brendan & Vaidehi Get Smart** created 2026-11-09 on brendan@arrowfund.co (event id `nfu8do9k1g5o1i355g0lrkbarg`; [calendar](https://www.google.com/calendar/event?eid=bmZ1OGRvOWsxZzVvMWkzNTVnMGxya2JhcmdfMjAyNjExMDlUMTkwMDAwWiBicmVuZGFuQGFycm93ZnVuZC5jbw); Meet `https://meet.google.com/oia-auph-qgi`). Do not create extra calendar events without Brendan's OK. No Saturday review (there is no Friday-night brief).
- Optional Signal / Telegram top-3 ping - **out for v1**.
- Auto-append call actions to Arrow Log Firm Intel - out for now.
- Committing rendered HTML briefs into git (in addition to the local archive) - offered, not approved.

## 10. Logging

Every run appends to `/workspace/arrow-morning-brief/YYYY-MM-DD/run.log`: start/end PT timestamps, sources attempted, stories cut by the quality gate (one-line reason), company chosen and why, send tool result / message id, recipient list, test-mode flag value. Keep logs local; do not email them.

## Changelog

- 2026-11-05 v0.1: skill + `template.html` drafted from Brendan's locked decisions (Sun-Thu 7pm PT HTML email from admin@ / Brendan Bot display name; Sunday = weekend edition; Mon-Fri 10:30am agenda; test = Brendan only; free sources; external-only; calendar invite pending). No bot / no routines / no sends yet.
- 2026-11-05 v0.2: bot created (3e40c3a8-3cce-401b-b460-d6ac50940c3c); routines live in TEST MODE (Brendan-only): "Evening brief" `evening-brief` `0 19 * * 0-4` lateMinutes 0 with a run.log dedupe guard, and "Morning agenda" `morning-agenda` `30 10 * * 1-5` lateMinutes 0. First test brief sent to Brendan 2026-11-05. Composio GMAIL_SEND_EMAIL has no display-name field, so From shows the account default. Known template issue: the nested `<!-- omit tr ... -->` inside the head comment closes it early; strip the head comment before sending.
- 2026-11-06 v0.3: Brendan approved adding Vaidehi. Live recipients are brendan@arrowfund.co and vaidehi@arrowfund.co for the evening brief and the morning agenda. Calendar invite still pending.
- 2026-11-09 v0.4: Brendan approved the package: daily Company file (Sequoia / a16z structure, never repeat, tracked in `profiles/`), 10:30am thinking note instead of "bring 3 questions", Arrow Fund branding (navy `#1F2A44`, Arrow blue `#2E5AAC`), and the weekday call named Brendan & Vaidehi Get Smart.
