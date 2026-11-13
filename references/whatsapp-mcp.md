# WhatsApp MCP: how it works

Instant context for any agent that touches a WhatsApp connector. Code lives in the private repo `brendandelta/whatsapp-mcp`. No endpoint URLs, tokens or server addresses are stored in this repo.

Last verified live: 2026-11-13 (Arrow team server and Brendan's server, v0.5.0, release commit `4c602444`). Chris's, Gabe's, and Jillian's accounts are connected on Brendan's Grok Bot account as `user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`. Skills that read partner WhatsApp read all three, under CONNECTIONS.md Partner WhatsApp rules.

## What it is

- One always-on collector per WhatsApp account, built on Baileys 7.0.0-rc14 (a linked-device client, the same mechanism as WhatsApp Web).
- WhatsApp gives a linked device no way to fetch or search old history on demand. The collector writes each message to a SQLite database as it arrives, and every tool reads from that database. One database per account, on the server that hosts the account. Text only, no media files.
- An MCP server (Streamable HTTP) sits on top. Only the collector talks to WhatsApp; the MCP server queues sends and live lookups for it.

## Where it runs

| Server | Accounts | Version |
|---|---|---|
| Arrow team server (Hetzner, Arrow account) | Chris Clifford, Gabe Borden, Jillian Nuding | v0.5.0 since 2026-11-13 (release commit `4c602444`, 3:25 PM PT) |
| Brendan's own server (Hetzner) | Brendan | v0.5.0 since 2026-11-13 (same release). Same code, single account layout: the web server runs on the machine itself and the connector address did not change. A timer checks the `release` branch every 3 minutes and rebuilds |

The two servers are separate on purpose. Same code, different machines. Each account has its own container, database, tokens and settings.

## Retention (Brendan's rule, 2026-11-12)

- Message bodies, media payloads, the search index, and old drafts and sent records older than 56 days are permanently deleted every day on every account. The database file is then rebuilt so the data cannot be recovered.
- Older messages that arrive late through history sync are never stored.
- Kept: chat and group names, group membership, contact names, phone and LID mappings (no message content). From v0.5.0, `contact_activity` and the change log also survive the body purge. Activity counts are not reduced by later purges. The change log stores ids and operations, not bodies.
- The window is a per-account setting (`RETENTION_DAYS`, default 56). `status` reports the window and the oldest stored date.
- Consequence for skills: anything needed for longer than 56 days must be written to its system of record (Attio, Notion, the weekly log) before it ages out. Last-contact facts should come from `contact_activity`, which survives the purge.

## Link types

- Admin link: all 31 tools (15 read, 16 write).
- View-only link: the 15 read tools. Write tools are not exposed at all, including `import_names` and the group draft tools.
- The last line of `status` says which link the caller is on.

## Tools (v0.5.0)

Read (both link types):

- `status`: counts, LID resolution, send cap, retention window, link type, version.
- `list_chats(limit, cursor, groups_only, directs_only)`: paginated, no hidden cap. Returns `total` and `next_cursor`.
- `list_groups(limit, cursor, query)`: every group with member counts.
- `get_group_info(group, refresh)`: subject, owner, created date and the full participant list (phone, name, admin role, JID). `group` is a group JID, or a name to search; if several match it returns the candidates. `refresh=true` re-reads it live.
- `list_contacts(query, addressbook_only, limit, cursor)`.
- `get_messages(chat, limit, since, before)`, `recent_activity(hours, limit, exclude_groups)`, `search_messages(query, chat, limit)`, `unreplied(hours, limit)`, `unresolved_contacts(limit)`.
- `contact_activity(since, phone_or_jid, no_outbound_days, limit, cursor)`: per direct chat, plus one row per sender in each group. First/last inbound and outbound, counts, phone JID, LID, E.164. No message text. Survives the 56-day body purge. `since` filters rows whose activity record changed at or after that ISO time. `phone_or_jid` accepts a phone, phone JID, or LID; names are rejected. `no_outbound_days` returns direct chats with no outbound in that many days, including never; group rows are left out.
- `changes_since(cursor, limit)`: incremental feed of chats, contacts, LID-to-phone links, and messages added, updated, or deleted since an opaque cursor. Omit the cursor to start at the beginning. Pass `next_cursor` back on every later call, including when `has_more` is false. Do not build a cursor. Message text is included only while the message is still stored. A delete is an id and `op=delete`, with no body. Default page size 200, max 1000.
- `export_contacts(limit, cursor)`: one row per known person (contacts, direct chats, LID mappings, group participants): phone JID, LID (several are comma-separated, sorted), E.164, saved name, push name, and name source (`addressbook`, `whatsapp`, `push`, or `none`).
- `resolve_person(query)`: lookup from a phone, JID, LID, or name. Returns every identifier and the matching chats. A phone, JID, or LID resolves only when that identity is already stored. A name resolves only when it equals exactly one full saved, WhatsApp, or push name. A shorter or partial match is `ambiguous: true`, even when only one person fits. It does not choose a send recipient. `draft_message` still refuses every name.
- `coverage_report`: counts only, no parameters. Resolved phone-number direct chats vs LID-only, named vs unnamed, and distinct people active in the last 7, 30, and 56 days (windows are cumulative). Active counts come from the activity log, so they survive the body purge.

Admin link only:

- `draft_message(to, text, allow_new_contact)` then `send_message(draft_id)`. Two steps, always. `to` must be a phone number or a JID. Names are refused and the tool returns matching candidates with their numbers instead.
- `list_drafts`, `cancel_draft`.
- `set_contact_name(phone_or_jid, name)`: label in the database only.
- `import_contacts(content, default_country_code)`: paste the full text of a vCard (.vcf) or CSV export. Numbers are normalized to E.164 and stored as address-book names for that account. Still overwrites.
- `import_names(names, overwrite)`: bulk `{phone_or_jid, name}`, 1 to 5000 entries. Fills blanks by default (no address-book name yet, including a contact that only has a WhatsApp name). `overwrite=true` replaces an address-book name that is already set. Does not parse a vCard.
- `check_numbers(phones)`: live check of which numbers are on WhatsApp (up to 500 per call).
- `refresh_groups`: re-read every group and its participants live.
- Group drafts: nothing reaches WhatsApp until `execute_group_draft`. Each stage call returns a draft id and an exact preview (`ran: false`). Participants are phone numbers or JIDs, never names. A group may be given by name only when that name matches one group; several matches require the group JID. The preview always shows the resolved JIDs.
  - `draft_group_create(subject, participants)`
  - `draft_group_add(group, participants)`
  - `draft_group_remove(group, participants)`
  - `draft_group_rename(group, subject)`
  - `execute_group_draft(draft_id)`
  - `cancel_group_draft(draft_id)` (only while status is `draft`)
  - `list_group_drafts(limit)`
- Group actions are limited by `GROUP_ACTION_LIMIT_PER_HOUR`, default 20 (0 means unlimited), separate from the 200/hour message cap. They are not tied to `SEND_ENABLED`.

## Sending rules

- Sending works only when the account has it switched on, and only from an admin link.
- Cap: 200 sends per hour per account by default, adjustable, 0 means unlimited. Sends are spaced a few random seconds apart.
- Brendan can send from every account. Chris can send from his own. Gabe and Jillian are view-only on their own accounts.
- The partner-connector rules in CONNECTIONS.md still apply to bots: never draft or send on a partner connector unless Brendan explicitly asks to send as that partner, and every send needs his approval of the exact draft.

## Identity

- Chats are keyed by JID. A person can appear as a phone JID or as a LID (WhatsApp's privacy id). The collector stores every LID to phone mapping it learns, including from group participant lists, and merges the LID identity into the phone identity.
- `status` shows how many direct chats are still LID only. On 2026-11-13 after the v0.5.0 deploy: Brendan 0, Chris 174, Gabe 172, Jillian 122. `coverage_report` is the counts tool going forward.
- Group names differ between phones. Always key groups by JID.
- Attio stores those JIDs on People and Companies (`whatsapp_phone_jid`, `whatsapp_lid`, `standard_arrow_chat_jid`). Match a WhatsApp chat to Attio by JID first and E.164 phone second. See `references/attio-schema.md`.

## Deploys

- Servers track the `release` branch of `brendandelta/whatsapp-mcp` and check it every 3 minutes. A release builds first, then restarts accounts one at a time behind a health check. Sessions and databases survive restarts; nobody has to re-link.
- The first start after 0.5.0 takes a write lock and backfills `contact_activity` from messages still stored, then applies known LID mappings, before the process listens. The health check allows about a minute. Migrations are idempotent (new tables use `IF NOT EXISTS`; each backfill commits with its `schema_migrations` row). A restart cannot double-count.
- New tables: `contact_activity`, `changes`, `group_drafts`, `schema_migrations`.
- Pairing a new account is a web page with a QR code, reached with that account's admin link.

## Not built yet

Shared contact map across the team accounts, server-side Attio sync, and bulk sends. The daily WhatsApp-Attio sync job is specified in `references/whatsapp-attio-daily-sync.md` and is not a running skill yet; it depends on the v0.5.0 tools, which are now live. The Attio JID fields and a first manual standard-chat backfill exist as of 2026-11-13 (`references/attio-schema.md`); a server-side standard Arrow chat finder is not built yet. When the remaining tools or the daily job ship, this file and CONNECTIONS.md are updated in the same commit.
