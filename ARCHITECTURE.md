# Architecture

## Three layers

**Brain** — the Mac mini. Always on. Runs the MCP servers, the agent runtime, the scheduler, the memory store, and a small web app. All state lives here or in the systems of record.

**Hands and senses** — MCP servers. iMessage, Signal, WhatsApp, Gmail, Google Calendar, Google Drive, Notion, Attio, QuickBooks. These expose tools; they hold no instructions.

**Faces** — the wall screen, the phone, the laptop. Each is a client of the brain. Same skills, same memory, different screen.

## What runs on the mini

| Piece | Job | Status |
|---|---|---|
| MCP servers (iMessage, Signal) | expose messages as tools | running on laptop, move to mini |
| Agent runtime | load profile + skill + memory, call model with tools, loop, write memory | not started |
| Scheduler (launchd) | morning brief, inbox triage, week preview, nightly reflection, nightly audit | not started |
| Memory store | `memory/` in this repo | started |
| Web app | serves the mirror over WebSocket; stores nothing | not started |
| Tailscale | phone and laptop reach the mini; nothing public | not started |

## Model routing

One function, `call_model(task_type, ...)`. Config decides which provider and model per task type. Cheap/fast for classification, strong for drafting and reasoning. Claude and Grok are both providers; neither is "the" model.

## Memory

- `memory/profile-facts.md` — durable facts the agent has learned about Brendan (not the system prompt; that's `profile.md`)
- `memory/people/` — one file per person, mirrors the Notion People database, agent-written notes only
- `memory/daily/YYYY-MM-DD.md` — one log per day
- `memory/open-loops.md` — things waiting on someone

Nightly reflection job reads the day, writes the above.

## The personal / Arrow line

`arrow/` is a git submodule (or subtree) of `arrow-os`, the team repo. When Brendan's agent runs, it loads `skills/` and `arrow/skills/`. When a teammate's agent runs `arrow-os`, it loads only `arrow/skills/`. Delta, personal deals, iMessage, Signal, the CRM never go in `arrow/`.

Test for which side a thing goes on: would a teammate need it to do Arrow work? Yes → `arrow/`. No → here.

## Security

- Never port-forwarded. LAN + Tailscale only.
- API keys in macOS keychain or a gitignored `.env`.
- Every send / delete / payment goes through a confirm step on whatever face is in use.
- The mini does not auto-update macOS.

## Build order

0. Mini + Tailscale + MCP servers under launchd. Repo exists.
1. Morning brief as a scheduled job writing an HTML page.
2. Chat endpoint with tools, text input from phone/laptop.
3. Monitor on the wall, Pi thin client, kiosk mode.
4. Voice pipeline.
5. Memory reflection, proactive triggers, idle screen.
