# WhatsApp MCP: how it works

Instant context for any agent that touches a WhatsApp connector. Code lives in the private repo `brendandelta/whatsapp-mcp`. No endpoint URLs, tokens or server addresses are stored in this repo.

Last verified live: 2026-11-13 (Arrow team server and Brendan's server, v0.4.0). Chris's, Gabe's, and Jillian's accounts are connected on Brendan's Grok Bot account as `user-Chris WhatsApp MCP (Admin)`, `user-Gabe WhatsApp MCP (Admin)`, and `user-Jillian WhatsApp MCP (Admin)`. Skills that read partner WhatsApp read all three, under CONNECTIONS.md Partner WhatsApp rules.

## What it is

- One always-on collector per WhatsApp account, built on Baileys 7.0.0-rc14 (a linked-device client, the same mechanism as WhatsApp Web).
- WhatsApp gives a linked device no way to fetch or search old history on demand. The collector writes each message to a SQLite database as it arrives, and every tool reads from that database. One database per account, on the server that hosts the account. Text only, no media files.
- An MCP server (Streamable HTTP) sits on top. Only the collector talks to WhatsApp; the MCP server queues sends and live lookups for it.

## Where it runs

| Server | Accounts | Version |
|---|---|---|
| Arrow team server (Hetzner, Arrow account) | Chris Clifford, Gabe Borden, Jillian Nuding | v0.4.0 since 2026-11-12 |
| Brendan's own server (Hetzner) | Brendan | v0.4.0 since 2026-11-13. Same code, single account layout: the web server runs on the machine itself and the connector address did not change. A timer checks the `release` branch every 3 minutes and rebuilds |

The two servers are separate on purpose. Same code, different machines. Each account has its own container, database, tokens and settings.

## Retention (Brendan's rule, 2026-11-12)

- Messages older than 56 days are permanently deleted every day on every account: the row, the raw copy, the search index entry, old drafts and sent records. The database file is then rebuilt so the data cannot be recovered.
- Older messages that arrive late through history sync are never stored.
- Kept: chat and group names, group membership, contact names, phone and LID mappings (no message content).
- The window is a per-account setting (`RETENTION_DAYS`, default 56). `status` reports the window and the oldest stored date.
- Consequence for skills: anything needed for longer than 56 days must be written to its system of record (Attio, Notion, the weekly log) before it ages out.

## Link types

- Admin link: all 18 tools.
- View-only link: the 10 read tools. Write tools are not exposed at all.
- The last line of `status` says which link the caller is on.

## Tools (v0.4.0)

Read (both link types):

- `status`: counts, LID resolution, send cap, retention window, link type, version.
- `list_chats(limit, cursor, groups_only, directs_only)`: paginated, no hidden cap. Returns `total` and `next_cursor`.
- `list_groups(limit, cursor, query)`: every group with member counts.
- `get_group_info(group, refresh)`: subject, owner, created date and the full participant list (phone, name, admin role, JID). `group` is a group JID, or a name to search; if several match it returns the candidates. `refresh=true` re-reads it live.
- `list_contacts(query, addressbook_only, limit, cursor)`.
- `get_messages(chat, limit, since, before)`, `recent_activity(hours, limit, exclude_groups)`, `search_messages(query, chat, limit)`, `unreplied(hours, limit)`, `unresolved_contacts(limit)`.

Admin link only:

- `draft_message(to, text, allow_new_contact)` then `send_message(draft_id)`. Two steps, always. `to` must be a phone number or a JID. Names are refused and the tool returns matching candidates with their numbers instead.
- `list_drafts`, `cancel_draft`.
- `set_contact_name(phone_or_jid, name)`: label in the database only.
- `import_contacts(content, default_country_code)`: paste the full text of a vCard (.vcf) or CSV export. Numbers are normalized to E.164 and stored as address-book names for that account.
- `check_numbers(phones)`: live check of which numbers are on WhatsApp (up to 500 per call).
- `refresh_groups`: re-read every group and its participants live.

## Sending rules

- Sending works only when the account has it switched on, and only from an admin link.
- Cap: 200 sends per hour per account by default, adjustable, 0 means unlimited. Sends are spaced a few random seconds apart.
- Brendan can send from every account. Chris can send from his own. Gabe and Jillian are view-only on their own accounts.
- The partner-connector rules in CONNECTIONS.md still apply to bots: never draft or send on a partner connector unless Brendan explicitly asks to send as that partner, and every send needs his approval of the exact draft.

## Identity

- Chats are keyed by JID. A person can appear as a phone JID or as a LID (WhatsApp's privacy id). The collector stores every LID to phone mapping it learns, including from group participant lists, and merges the LID identity into the phone identity.
- `status` shows how many direct chats are still LID only. On 2026-11-12: Chris 174, Gabe 172, Jillian 122.
- Group names differ between phones. Always key groups by JID.
- Attio stores those JIDs on People and Companies (`whatsapp_phone_jid`, `whatsapp_lid`, `standard_arrow_chat_jid`). Match a WhatsApp chat to Attio by JID first and E.164 phone second. See `references/attio-schema.md`.

## Deploys

- Servers track the `release` branch of `brendandelta/whatsapp-mcp` and check it every 3 minutes. A release builds first, then restarts accounts one at a time behind a health check. Sessions and databases survive restarts; nobody has to re-link.
- Pairing a new account is a web page with a QR code, reached with that account's admin link.

## Not built yet

Shared contact map across the team accounts, `resolve_person`, coverage report for Attio lists, per-teammate contact log, server-side Attio sync, group create and edit tools, and bulk sends. The Attio JID fields and a first manual standard-chat backfill exist as of 2026-11-13 (`references/attio-schema.md`); a server-side standard Arrow chat finder is not built yet. When the remaining tools ship, this file and CONNECTIONS.md are updated in the same commit.
