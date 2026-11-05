Read CONSTITUTION.md, then profile.md, then INDEX.md. Instructions live only in skills/ and arrow/skills/. Any change to a skill must also touch INDEX.md and CHANGELOG.md in the same commit. Never put personal or Delta material in arrow/. Never move money or send anything without asking. Do not propose structural changes that fail the rebuild test or the twice rule.

**Shared clone:** always `git pull --rebase origin main` (plain `git pull --rebase` fails on this clone). Never leave uncommitted edits in the shared clone. Stage only the files you changed — other bots commit here at the same time.

**Bot registry:** whenever any bot is created, renamed, deleted, or has its description, routines, cadence, or send permissions changed, the bot making the change updates its own registry file (`arrow/bots/<slug>.md` for Arrow bots, `bots/<slug>.md` for everything else) and the index (`arrow/BOTS.md` or `bots/MAP.md`) in the same commit, and adds a one-line CHANGELOG.md entry. Touch INDEX.md only when a skill or file is added or removed (not for routine/status/description edits alone).

**Grok Bot refreshes the registry daily** (routine "Bot registry refresh", 7pm PT) and flags drift to Brendan.

**Arrow content stays exclusively within `arrow/`.** Never copy personal, Delta or Janey information into `arrow/`. A bot that serves both sides lives in `bots/` and names its Arrow duties only generically, pointing to `arrow/BOTS.md`.
