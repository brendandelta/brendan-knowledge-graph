# Attio schema reference (Arrow Fund)

Workspace: [Arrow Fund](https://app.attio.com/arrow-fund) (`arrow-fund`, workspace id `945181e9-ddcd-4d63-978e-ea968ceae3a8`). Reached through Composio (`user-composio`, connection `attio_bosser-wooded`).

This file is the Attio field reference for bots that map WhatsApp (and later other channels) onto People and Companies. It does not replace the field-by-field People schema overhaul still waiting on Brendan's approval. Last-contact fields stay documented in `arrow/skills/attio-contact-context/SKILL.md`. Firm-intel fields stay documented in `arrow/skills/arrow-attio-firm-context/SKILL.md`.

## WhatsApp identity fields (created 2026-11-13)

Text attributes. Fill only when empty. Never overwrite a value already set. Never create a People or Company record just to store a JID.

| Object | Slug | Title | Type | Attribute id | Meaning |
|---|---|---|---|---|---|
| People | `whatsapp_phone_jid` | WhatsApp phone JID | text | `3b7c92fd-fc61-45c9-9c84-947eba3df049` | Canonical phone JID, `{E164_without_plus}@s.whatsapp.net`. Strip a `:0` (or other) device suffix before writing. Never store a LID local-part as a phone JID. |
| People | `whatsapp_lid` | WhatsApp LID | text | `4decbbbb-a151-4e4d-82b5-65c1d06d77a1` | WhatsApp privacy id, `{n}@lid`, when the server exposes a mapping for that person. |
| People | `standard_arrow_chat_jid` | Standard Arrow chat JID | text | `304310b0-d2c3-4f92-b7ea-73578611c363` | Group JID (`{n}@g.us`) of the one standard Arrow chat with that person. |
| Companies | `standard_arrow_chat_jid` | Standard Arrow chat JID | text | `b80e24d0-55f4-486e-8257-ca271639f55d` | Group JID (`{n}@g.us`) of the one standard Arrow chat with that firm. |

## How to match a WhatsApp chat to Attio

Use this order everywhere a skill maps a WhatsApp chat onto a person or a firm:

1. **JID first.** Query People `whatsapp_phone_jid` or `whatsapp_lid` with the canonical JID (phone JID after stripping any device suffix, or `@lid`). For a group, query People and Companies `standard_arrow_chat_jid` with the `@g.us` JID.
2. **Phone second.** If there is no JID hit, normalize the number to E.164 (`+` and digits) and match People `phone_numbers`.
3. **Exactly one confident match.** Use that record. If more than one People or Company record matches, skip and log the collision. Do not write the JID fields, last-contact fields, or `standard_arrow_chat_jid` onto every collision. Do not guess.
4. **Do not create** a person or a firm just to store a WhatsApp id. LID-only directs with no phone and no existing JID field stay unmatched until a phone mapping exists.
5. **Fill only empty values.** If `whatsapp_phone_jid`, `whatsapp_lid`, or `standard_arrow_chat_jid` is already set, leave it.

Canonical phone JID is always built from the matched E.164 phone, not by treating a LID as a phone number.

## Standard Arrow chat (`standard_arrow_chat_jid`)

Set this only when a group is clearly the standing Arrow chat with one person or one firm. Typical names: `X <> Arrow`, `X <> Arrow Fund`, `Arrow <> X`, `Arrow Fund x X`, and the same idea with `+` or `/`.

Skip and leave the field empty when any of these are true:

- Internal or ops chats (internal, invoice, dealflow, investor materials, and similar).
- Multi-party chats (two named counterparties plus Arrow).
- Deal-specific or one-off threads (a company plus a deal name, a dollar size, or a third platform in the title).
- First-name-only or incomplete names (`Adam <> Arrow Fund`, `David L <> Arrow Fund`) unless the identity is otherwise certain.
- Two or more groups for the same counterpart (pick none until a human names the standard one).
- The name does not uniquely match one existing People or Company record.

First backfill: 2026-11-13, from Brendan's and Chris's WhatsApp groups whose names include "Arrow". Only unique, confident matches were written. Uncertain and duplicate chats were left unset.

## Related last-contact fields

On People, `last_whatsapp_contact`, `last_whatsapp_contact_by`, `last_contacted`, `last_contacted_by`, `last_contact_channel`, and `days_since_last_contact` are owned by attio-contact-context. When that skill matches a 1:1 chat, it now uses the JID-first, phone-second rule above, then refreshes those six fields. See that skill for attribute ids and rollup rules.
