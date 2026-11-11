# Bot map (Personal · Delta · Janey · Cross)

Non-Arrow Grok Bots. Arrow-only bots are indexed in [`arrow/BOTS.md`](../arrow/BOTS.md) and must never gain personal, Delta or Janey detail there.

**How to update:** the bot that owns an entry updates `bots/<slug>.md` and this map in the same commit whenever it is created, renamed, deleted, or its description, routines, cadence or send permissions change. Grok Bot runs "Bot registry refresh" daily at 7pm PT and flags drift. See `AGENTS.md`.

Template: [`bots/_TEMPLATE.md`](_TEMPLATE.md).

## Cross (serves more than one silo)

A Cross bot may mention Arrow only generically and must point to [`arrow/BOTS.md`](../arrow/BOTS.md) for Arrow detail. It must not copy Arrow operational content into `bots/`, and must not put personal/Delta/Janey content into `arrow/`.

| Bot | File | Skill | Status in registry |
|---|---|---|---|
| Grok Bot | [grok-bot.md](grok-bot.md) | `skills/grok-bot` | live (registry refresh 2026-11-06) |
| CoS Bot (live name: Brendan · Chief of Staff) | [cos-bot.md](cos-bot.md) | `skills/chief-of-staff` | live (name drift synced 2026-11-06) |
| Inbox Todo | [inbox-todo.md](inbox-todo.md) | `skills/comms-ingest` | live (self-reported 2026-11-05) |
| Suggested Actions | [suggested-actions.md](suggested-actions.md) | `skills/suggested-actions` | missing from live (flagged 2026-11-06 registry refresh) |

## Personal

| Bot | File | Skill | Status in registry |
|---|---|---|---|
| Personal CRM Contact Context | [personal-crm-contact-context.md](personal-crm-contact-context.md) | `skills/crm-maintenance` | live (Notion only; Attio routines deleted 2026-11-05) |
| Call Cadence | [call-cadence.md](call-cadence.md) | `skills/call-cadence` | live (self-reported 2026-11-05) |
| Global Entry Watch | [global-entry-watch.md](global-entry-watch.md) | none (temporary inbox watch) | live, short-term (Gmail readable and 8am/6pm PT routine reported live 2026-11-10) |

## Delta

None known yet. When a Delta-only bot exists, add `bots/<slug>.md` and a row here.

## Janey

None known yet. When a Janey-only bot exists, add `bots/<slug>.md` and a row here.

## Arrow

See [`arrow/BOTS.md`](../arrow/BOTS.md).
