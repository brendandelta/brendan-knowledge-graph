---
name: arrow-morning-brief
description: Sun–Thu external markets brief (world/economy, macro, public, private/secondaries, All-In-style takes, discussion prompts) emailed as a mobile-first HTML article to Brendan and (once approved) Vaidehi; compiles their 3-question email replies into a 10:30am PT agenda for the 11am review.
---

# arrow-morning-brief

> **TEST MODE (locked 2026-11-05).** Every send goes to **brendan@arrowfund.co only**. Do **not** add vaidehi@arrowfund.co, create calendar invites, or auto-send past test until Brendan explicitly approves in this bot's chat. Flip this flag in the same commit as the live description / registry update.

**Status:** live in TEST MODE · bot 3e40c3a8-3cce-401b-b460-d6ac50940c3c · routines live 2026-11-05 (Brendan-only)
**Silo:** Arrow only. Never put personal, Delta, or Janey content here.
**Source of decisions:** Brendan, 2026-11-05 (plan Page + chat; cadence updated same day to Sun–Thu briefs / Mon–Fri reviews, Sunday weekend edition). Sample look: `/workspace/morning-brief/sample-2026-11-05.html`.

## Bot instructions (keep in sync with the live bot profile)

- Read `AGENTS.md` in this repo first. The bot's own instructions are its working copy; keep this file in sync. If they disagree, follow the bot prompt and tell Brendan.
- Every user-facing reply that names an action, app, email, doc, or web page follows `embed-action-links`.
- Never move money, send, delete, or pay without Brendan's explicit confirm — beyond the approved nightly brief send once live mode is unlocked.
- **Content scope is 100% external.** Never read Attio. Never name Arrow deals, prices, counterparties, book companies, or any internal CRM fact. Private-market color is public news only.
- **No Signal or iMessage in v1.** Delivery is HTML email only (admin@ → recipients).
- Routines pull the repo first, then follow this file.

## 1. Cadence and recipients

| Job | Cron (America/Los_Angeles) | Timing | Recipients (test) | Recipients (live, pending) |
|---|---|---|---|---|
| Generate + email brief | `0 19 * * 0-4` | 7:00pm PT **Sun–Thu only**; `lateMinutes: 0` so it may start ~6:30pm. **No send Fri/Sat.** | brendan@arrowfund.co only | + vaidehi@arrowfund.co after Brendan's OK |
| Build agenda | `30 10 * * 1-5` | 10:30am PT **Mon–Fri** (covers the prior evening's brief; Monday agenda covers Sunday's weekend edition) | same as above | same |

- **Review call:** 11:00–11:15am PT **Mon–Fri**. Title when created: `Arrow Morning Markets · 15m`. **Calendar invite is NOT created yet** — pending Brendan's approval (weekdays-only is now the working assumption).
- **Subject (brief, Mon–Thu evenings):** `Arrow Morning Markets · for <Weekday>'s 11am` (e.g. `Arrow Morning Markets · for Friday's 11am`).
- **Subject (brief, Sunday evening):** `Arrow Morning Markets · Weekend + week ahead · for Monday's 11am`.
- **Subject (agenda):** `Agenda · Arrow Morning Markets · <Weekday Mon D>` (e.g. `Agenda · Arrow Morning Markets · Fri Nov 6`).
- **Sunday = weekend edition** (see §3b-Sunday): covers everything important from Friday, Saturday and Sunday; longer week-ahead + weekend recap; slightly longer read (target ~12–15 min). Mon–Thu editions stay the standard 8–12 min overnight brief.
- From: **admin@arrowfund.co** via Composio Gmail account id **`gmail_dammer-immi`**, tool `GMAIL_SEND_EMAIL` with `is_html: true`. Set the From display name to **`Brendan Bot`** if Gmail allows it for this mailbox; otherwise leave the default (Arrow Admin / admin@).
- Send once per job. On gather/render failure: retry up to 2 times, 10 minutes apart; if still failing, email Brendan a short plain-text delay note with the same subject + ` (delayed)` and stop. Never send a partial brief.

## 2. Files and archive

| Path | Purpose |
|---|---|
| `arrow/skills/arrow-morning-brief/SKILL.md` | This procedure (source of truth) |
| `arrow/skills/arrow-morning-brief/template.html` | Email-safe HTML shell; fill tokens, never rewrite structure day to day |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/brief.html` | Rendered brief archive |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/sources.json` | Citation list used that day |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/agenda.md` | Compiled questions + facilitator note |
| `/workspace/arrow-morning-brief/YYYY-MM-DD/run.log` | Gather / QA / send log |

Copy `template.html` from the skill folder into `/workspace/arrow-morning-brief/template.html` on first run if missing; prefer the repo copy when they drift.

## 3. Procedure — evening brief (`0 19 * * 0-4`, Sun–Thu)

Every routine starts with:

```bash
git -C /home/box/Claude/brendan-knowledge-graph pull
```

Then read this skill and `template.html`.

### 3a. Gather (~24–36h window ending at run time)

Collect public headlines and figures only from the **free source stack** in §5. Prefer RSS / press pages / primary releases. If a site Cloudflare-blocks automated fetch, use a public Google News result that links the original, or drop the story.

Window: roughly the last 36 hours ending at run time (PT). Anything older than ~36 hours is dropped unless clearly labeled as background ("since Monday…").

Capture for each candidate: headline, 2–4 sentence substance, publish time, canonical URL, optional hero image URL (`og:image` when available and hot-linkable), and the exact figure strings you will quote.

### 3b. Write (fixed section order — never reorder)

Target length: **8–12 minute read**, mobile-first (~640px email). Tone: Morning Brew pace + WSJ-grade market seriousness + All-In-style takes. Short paragraphs. No AI self-attribution. No hype adjectives without a cited print.

1. **Masthead** — `Arrow Fund · Morning Markets Brief`; title `For <Weekday>'s 11am review`; sent date/time PT; recipients line; "Reply with your 3 questions before 11:00am PT".
2. **Cold open** — 3–5 bullets, the so-what. Every bullet ends with a source link.
3. **World & economy** — 2–4 stories that move policy, capital, or risk. Prefer one hero image per story when a clean `og:image` exists.
4. **Macro** — rates, inflation, central banks, dollar, oil. **Verified figures only.** Omit any print you cannot cite.
5. **Public markets** — indices and sector movers with cited closes / moves. Include a one-line "Why it matters for private markets" when the tape clearly connects; still no Arrow names.
6. **Private markets & secondaries** — funding rounds, continuation vehicles, LP liquidity color, sector snapshots — only when primary-sourced (company / firm release, TechCrunch, Crunchbase News, BusinessWire, etc.).
7. **Takes desk** — 2–3 provocations grounded in today's linked stories. Label them as opinion, not predictions. No long transcript dumps.
8. **Discussion prompts** — bot seeds **exactly 3** questions using the lenses in §7. Reminder that each person still replies with their own 3.
9. **Sources used today** — footer list of every citation URL used in the body.


### 3b-Sunday. Weekend edition extras (Sunday 7pm only)

When the run day is Sunday (PT):

1. **Window:** gather from Friday 00:00 PT through Sunday run time (≈60–72h), not the usual ~36h. Label older Friday items as weekend context, not "breaking".
2. **Add / expand sections** (keep the fixed order; enlarge these):
   - After **Cold open**, add a short **Weekend recap** subsection (or fold 2–3 recap bullets into cold open) covering Fri–Sun so-whats.
   - Expand **Private markets & secondaries** with any material weekend releases.
   - After **Takes desk** (or just before discussion prompts), add **Week ahead** — 3–5 dated catalysts for the coming Mon–Fri (jobs, CPI, Fed speakers, earnings clusters, known secondaries conferences / known public catalysts). Every catalyst needs a source or a clear "scheduled / consensus" label; never invent dates.
3. **Length:** target **12–15 min** read. Still mobile-first; do not balloon past ~15 stories total.
4. **Masthead title:** `For Monday's 11am review · Weekend edition`.
5. **Prompts:** seed questions should skew week-ahead ("what do we hunt Mon–Wed if X prints hot?").
6. Mon–Thu runs skip this block entirely.

### 3c. Render

Fill `template.html` tokens (see file comments). HTML-escape all inserted text. Inline CSS only (email-safe tables). Images: absolute HTTPS URLs, `width="592"`, `style="display:block;width:100%;max-width:592px;height:auto;border-radius:4px;"`. If an image URL fails a HEAD/GET check, omit the image rather than leave a broken one.

Write the filled HTML to `/workspace/arrow-morning-brief/YYYY-MM-DD/brief.html` and `sources.json`.

### 3d. Quality gate (hard stop if any fail)

- [ ] Every factual claim has a working source link in the same story/bullet.
- [ ] No invented numbers, quotes, or dates. Unverifiable → cut.
- [ ] No Attio / Arrow deal / book / counterparty / price / internal content.
- [ ] No news older than ~36h unless labeled background (Sunday weekend edition: Fri–Sun window OK; label Friday items as weekend context).
- [ ] Spot-check links (at least every cold-open + section lead link) return 2xx or a readable article page.
- [ ] Deduped vs yesterday's brief archive (don't rehash the same lead without a new development).
- [ ] Recipients match **TEST MODE** (Brendan only) or the approved live list.
- [ ] From account is `gmail_dammer-immi` / admin@.

Fail any check → fix or cut; never send a failing brief.

### 3e. Send

Call Composio `GMAIL_SEND_EMAIL` on account **`gmail_dammer-immi`**:

- `to`: test → `brendan@arrowfund.co`; live → Brendan + Vaidehi (only after chat approval)
- `subject`: per §1
- `body`: the rendered HTML
- `is_html`: true
- From display name `Brendan Bot` when the tool/account supports it

Do **not** BCC, CC extra people, or attach files in v1. After send, append a line to `run.log` (time, message id if returned, recipient list, pass/fail notes). Post a one-line confirmation to Brendan in this bot's chat with the subject and archive path. Stay silent to others.

## 4. Procedure — morning agenda (`30 10 * * 1-5`, Mon–Fri)

```bash
git -C /home/box/Claude/brendan-knowledge-graph pull
```

1. Find last night's brief thread in **admin@arrowfund.co** (search subject `Arrow Morning Markets · for` matching today's review weekday, or the message-id saved in `run.log`).
2. Read replies from Brendan and (when live) Vaidehi. Parse up to 3 questions each. Ignore signatures, quoted reply text, and anything that is not a question.
3. Merge duplicates / near-duplicates. Prefer the sharper wording. Cap the agenda at **6 questions** (3 each); if someone sent fewer, note the gap.
4. Write `/workspace/arrow-morning-brief/YYYY-MM-DD/agenda.md` using the facilitator template in §8.
5. Email the agenda as a short HTML or plain+HTML note to the same recipient list as the brief (test = Brendan only), subject per §1, from the same admin@ account.
6. **Do not create or modify calendar events** until Brendan approves the invite step. When that lands, the agenda text may also go into the event description — note only, do not invent the event now.
7. If no replies by 10:30: still send an agenda that lists the bot's 3 seed prompts and a one-line "no replies yet — bring your 3 to the call".

## 5. Free source stack (v1)

| Section | Sources | Notes |
|---|---|---|
| World / economy | Axios top RSS (`https://api.axios.com/feed/top/`); AP / Reuters via public pages or Google News RSS; Guardian live blogs | Headlines + links; some sites block bots |
| Macro | Fed press RSS (`https://www.federalreserve.gov/feeds/press_all.xml`); FRED (`https://fred.stlouisfed.org/`); CNBC rates/jobs; BoE / ECB press | Never invent prints |
| Public markets | CNBC / MarketWatch / AP markets; public index closes from those stories | Cite the article that carried the print |
| Private / secondaries | Crunchbase News; TechCrunch; BusinessWire / company PE releases; NEPC public notes; SEC EDGAR when useful | CVs and mega-rounds when primary-sourced |
| Takes | All-In / BG2 / Acquired public notes; grounded takes from today's links | No long verbatim transcript dumps |

Paid / gated sources are out of scope for v1. If a free source repeatedly fails fetch, note it in `run.log` and rotate to another free source the same day; tell Brendan only if the brief would be thin without it.

## 6. Tone guide

- Clear, opinionated, sourced. Second-person is fine in prompts ("your 3 questions"); body copy stays third-person / desk voice.
- Numbers: use the exact figure the source printed; include the unit and the as-of.
- Quotes: only when you have the source open; attribute by name + outlet.
- Never write "as an AI", never invent "sources say".
- Private-markets bridge lines are allowed ("why it matters for secondaries") without naming Arrow's book.
- Takes desk may be punchy; everything else stays WSJ-clean.

## 7. Question framework (lenses)

Use these so the 11am call stays useful. Bot seed prompts and human replies should map to at least one:

1. So what for Arrow's book or pipeline this week? *(generic — no deal names)*
2. What would change our mind on a sector or liquidity view?
3. Which hunt or pause does this imply?
4. Who should we call (person type / firm type — no confidential counterparties)?
5. What risk looks underpriced in the public tape for private marks?
6. What should we ignore so we do not waste cycles?

Each person replies to the brief email with **exactly 3 questions** before 11:00am PT.

## 8. Facilitator agenda template

```markdown
# Arrow Morning Markets · <Weekday Mon D> · 11:00–11:15am PT

## Cold open (0–3m)
- Align on last night's top 3 bullets (from the brief).

## Questions (3–12m)
### Brendan
1. …
2. …
3. …

### Vaidehi   # omit block entirely while TEST MODE / Vaidehi not on recipients
1. …
2. …
3. …

(Park rabbits. One question at a time.)

## Capture (12–15m)
- Owners + due dates (log below after the call when Brendan pastes takeaways).

## Seed prompts (if replies were thin)
1. …
2. …
3. …
```

Call shape (when the invite exists): 0–3m align → 3–12m work the questions → 12–15m capture owners + dues. Post-call capture auto-append to Firm Intel is **not** enabled; Brendan pastes manually for now.

## 9. Pending / still open (do not invent answers)

- Add Vaidehi to recipients (needs Brendan's chat OK).
- Create the recurring 11:00–11:15am PT **Mon–Fri** calendar invite (needs Brendan's OK; weekdays-only is the working assumption after the Sun–Thu brief cadence lock).
- Optional Signal / Telegram top-3 ping — **out for v1**.
- Auto-append call actions to Arrow Log Firm Intel — out for now.
- Flip TEST MODE → live auto-send — only after Brendan approves a real test brief in the bot's chat.

## 10. Logging

Every run appends to `/workspace/arrow-morning-brief/YYYY-MM-DD/run.log`: start/end PT timestamps, sources attempted, stories cut by the quality gate (one-line reason), send tool result / message id, recipient list, test-mode flag value. Keep logs local; do not email them.

## Changelog

- 2026-11-05 v0.1: skill + `template.html` drafted from Brendan's locked decisions (Sun–Thu 7pm PT HTML email from admin@ / Brendan Bot display name; Sunday = weekend edition; Mon–Fri 10:30am agenda; test = Brendan only; free sources; external-only; calendar invite pending). No bot / no routines / no sends yet.
- 2026-11-05 v0.2: bot created (3e40c3a8-3cce-401b-b460-d6ac50940c3c); routines live in TEST MODE (Brendan-only): "Evening brief" `evening-brief` `0 19 * * 0-4` lateMinutes 0 with a run.log dedupe guard, and "Morning agenda" `morning-agenda` `30 10 * * 1-5` lateMinutes 0. First test brief sent to Brendan 2026-11-05. Composio GMAIL_SEND_EMAIL has no display-name field, so From shows the account default. Known template issue: the nested `<!-- omit tr ... -->` inside the head comment closes it early; strip the head comment before sending.
