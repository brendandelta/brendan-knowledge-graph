# Nightly audit (prompt for the agent)

Read every SKILL.md under skills/ and arrow/skills/, plus INDEX.md and profile.md.

Report to memory/audit/YYYY-MM-DD.md and copy to memory/audit/latest.md:

1. Skills on disk not in INDEX.md.
2. INDEX rows with no skill on disk.
3. Skills that reference a system, database, field, or other skill that no other file mentions (likely stale).
4. Two skills that give conflicting instructions about the same data (e.g. who owns "who's going cold").
5. Anything in arrow/ that mentions Delta, iMessage, Signal, the personal CRM, or Brendan's own deals.
6. Skills marked "live" in INDEX that are still placeholders.

Keep it short. One line per finding. No findings → write "clean".
