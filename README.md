# brendan-os

This repo is the system. Everything else (the Mac mini, the MCP servers, Notion, Gmail, the wall screen) either runs this repo or is data this repo reads.

Start with `CONSTITUTION.md`, then `ARCHITECTURE.md`. Then `INDEX.md`. Then `profile.md`.

## Layout

```
CONSTITUTION.md   what never gets rebuilt
profile.md        system prompt: who Brendan is and how to work with him
INDEX.md          registry of every skill, what it touches, what depends on it
ARCHITECTURE.md   the design, three layers, the rules
DECISIONS.md      dated log of why things are the way they are
CHANGELOG.md      what changed, when. Every instruction change gets a line.
RUNBOOK.md        when something breaks
TASKS.md          open loops that are not skills
CONNECTIONS.md    every connection to an outside system, Composio included

skills/           instructions. The only place instructions live.
arrow/            shared with the Arrow Fund team. Its own git repo. Nothing personal goes here.
memory/           what the agent has learned. Written by the agent, read by you.
services/         launchd plists for the always-on box
mirror/           the wall screen UI (renders only, stores nothing)
scripts/          small tools: audit, brief, etc.
```

## The rules

1. Instructions live in `skills/` or `arrow/skills/`. Nowhere else.
2. Data lives in Notion, Gmail, Calendar, Attio, QuickBooks, iMessage. Never here.
3. The mirror has no database.
4. An instruction change is a commit that also touches `INDEX.md` and `CHANGELOG.md`.
5. Anything that sends, deletes, or pays asks first.
6. `arrow/` is a subfolder of this repo and its own repo. Personal mounts shared; shared never sees personal.
