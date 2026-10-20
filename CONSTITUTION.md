# Constitution

What never gets rebuilt. Read before proposing any structural change.

## The core (frozen)

1. **The repo is the brain.** Not a folder, not a Notion page, not a database. Any idea that begins "what if the brain lived in..." is rejected.
2. **Systems of record stay put.** Notion for people, QuickBooks for books, Attio for Arrow pipeline, Drive for files, the messaging apps for messages. The repo tells agents how to use them; it never replaces them.
3. **`arrow/` is sealed.** Shareable outward, nothing flows up.
4. **Money is propose-only; sends need a confirm.**

Changing any of these requires a dated entry in DECISIONS.md naming what it replaces and why the old way failed. A commit, not a chat.

## The shell (free to evolve)

Engines (OpenClaw, Hermes, whatever follows), models, hardware, interfaces, individual skills, Composio vs. custom MCP. These swap without touching the core.

## Two tests

**Rebuild test.** If a change means re-entering information already entered once, it is a rebuild. Rebuilds are banned. BARVIS → this repo was a rebuild. OpenClaw → Hermes with the repo intact is not.

**Twice rule.** No new skill, tool, or structure until the workflow that needs it has come up twice. First time by hand. Second time it becomes a skill.
