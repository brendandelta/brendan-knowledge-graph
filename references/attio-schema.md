# Attio schema reference (Arrow Fund)

Workspace: [Arrow Fund](https://app.attio.com/arrow-fund) (`arrow-fund`). Reached through Arrow Attio via Composio.

This file is the Attio field reference for bots that map WhatsApp (and later other channels) onto People and Companies. Brendan approved the People schema cleanup on 2026-11-13; the changes below are live. The LP intake fields in the last section were added the same day. Last-contact fields stay documented in `arrow/skills/attio-contact-context/SKILL.md`. Firm-intel fields stay documented in `arrow/skills/arrow-attio-firm-context/SKILL.md`.

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

On People, `last_whatsapp_contact`, `last_whatsapp_contact_by`, `last_contacted`, `last_contacted_by`, `last_contact_channel`, and `days_since_last_contact` are owned by attio-contact-context. When that skill matches a WhatsApp contact (1:1 or a counted group), it uses the JID-first, phone-second rule above, then refreshes those six fields and the per-teammate last-contacted timestamps. See that skill for attribute ids and rollup rules.

### Per-teammate last-contacted (created 2026-11-13)

Timestamp attributes. Write only a later timestamp than the value already set. Never overwrite with an earlier time. Email is rolled into these only when a daily job can attribute sent mail to a specific teammate; Attio's built-in email interaction is not split per user via the API.

| Slug | Title | Type | Attribute id |
|---|---|---|---|
| `last_contacted_brendan` | Last contacted (Brendan) | timestamp | `c599c6fa-92df-4dfb-96c2-47a3a252e4ab` |
| `last_contacted_chris` | Last contacted (Chris) | timestamp | `5e3355af-07ef-4fff-84d1-5ef5f9ad3b83` |
| `last_contacted_gabe` | Last contacted (Gabe) | timestamp | `eac496dd-8f4c-4bc8-8ade-1d8fa77a8892` |
| `last_contacted_jillian` | Last contacted (Jillian) | timestamp | `720e1c9e-1241-4427-a326-80b21ea98efd` |
| `last_contacted_vaidehi` | Last contacted (Vaidehi) | timestamp | `6d57a92a-d5a4-42fd-b2e6-f60b27734057` |

Gabe's and Jillian's WhatsApp were connected on Brendan's Grok Bot account on 2026-11-13 (`user-Gabe WhatsApp MCP (Admin)` and `user-Jillian WhatsApp MCP (Admin)`). Those two fields are filled from their servers (later-only; leave blank when that teammate has no counted contact). What counts (Brendan, 2026-11-13): 1:1 chats plus groups of 15 or fewer members, where a teammate's post counts for every non-Arrow member and a member's own post counts for that member; in bigger groups only @-mentions and quoted replies; Arrow teammates never count as contacting each other. Source is `contact_activity`: combined fields from whatsapp-mcp v0.5.1, direct-chat rows on v0.5.0 (`references/whatsapp-mcp.md`). Vaidehi's WhatsApp is not connected yet, so `last_contacted_vaidehi` stays empty until her server exists or email attribution is added.

## People schema cleanup (approved and applied 2026-11-13)

Title-only renames (slugs unchanged, so API clients keep working):

| Slug | Old title | New title |
|---|---|---|
| `referrals_6` | Referrals | Interest referrals |
| `referrals_4` | Referrals | Block referrals |
| `brokered_interests` | Brokered Interests | Intermediary interests |
| `brokered_blocks_contact` | Brokered Blocks (Contact) | Intermediary blocks |
| `check_size_range` | Check Size range | Check size range |

Archived (never deleted), after data was moved or confirmed empty:

| Slug | Old title | What happened first |
|---|---|---|
| `stated_location` | Stated Location | Copied into empty `primary_location` where the text parsed to a location |
| `private_companies_interest` | Private Co. Interest | Matched names added to `companies_of_interest`; original text saved as an Attio note |
| `target_company_3` | Target Company (status) | The one "Tether" value moved to a note and linked on `companies_of_interest` |
| `other_sectors` | Other Sectors | Confirmed empty |

Do not write the archived slugs. New website inquiries must not post to `stated_location` or `private_companies_interest`.

Select-option cleanup on People:

- `sector_of_interest`: titles `defense` and `crypto` renamed to `Defense` and `Crypto` (same option ids). Unused options archived: Industrials, E-commerce, Marketplaces, Logistics & Supply Chain, Edtech, Future of Work, Materials. PNC was left live because one person still has it. The already-archived duplicate "AI Applications" option was left archived; live values are on the live option.
- `tags`: unused options Secondary Buyers and Late-Stage Primaries archived. FO, Blacklist, NY Events, Unethical, and Nvidia Venture Cap stayed live.
- `inquiry_role`: an `Intermediary` option was added (`e3239162-fc5e-4f42-8f29-d631a864a218`). The one person on `Broker-Dealer` was moved to Intermediary. `Broker-Dealer` was not archived, because the website form still posts that role. Map both `Broker-Dealer` and Intermediary to What they are (`lp_fo_ria`) Intermediary. Never use the word "Broker" in new options, values, or prose.

The website inquiry form lives at [arrowfund.co/request-access](https://www.arrowfund.co/request-access) and posts JSON to `POST /api/inquiries`. The client source is the Next.js `RequestAccessForm` on that site. The server mapping was not in this knowledge repo or in any public `brendandelta` GitHub repo. Form field names and the option strings the browser sends are documented in the 2026-11-13 changelog entry so the form can be updated in the same change as any further Attio option work.


### People data hygiene (junk records)

Machine-sender and service-desk People records are removed weekly by `arrow/skills/attio-people-cleanup` (bot "Attio Cleanup"), which lists the protective fields it checks and keeps the run history (2026-11-13: 210 deleted, 8 renamed, 102 keep-listed). Any new linking or LP field added here should also be added to that skill's protections.

## People LP intake fields (created 2026-11-13)

Brendan approved these five People attributes on 2026-11-13 after comparing a partner LP intake survey with Arrow's [Request access](https://www.arrowfund.co/request-access) form. They are optional. The website form and People-view updates are a follow-up (not in this repo).

| Slug | Title | Type | Attribute id | Options (title, option id) |
|---|---|---|---|---|
| `investment_structure` | Investment Structure | select, multi | `a690ac7c-090f-4abd-9c86-54144a7965f1` | Direct (`e7876b6d-d8d3-46d7-b9f7-1381a4889c70`), SPVs (`50d2438c-bff4-4b43-9d71-19a3e7849f04`), Fund commitments (`8ccd3495-a9f9-49e1-95c8-0ac18062987d`), Secondaries (`64dced25-2266-47c5-900c-f3a71a2d137d`), Co-investments (`2c0b76da-2791-4ccb-b9c8-f5389dce83f7`) |
| `decision_timeline` | Decision Timeline | select, single | `3f53b1f0-ce14-4ea3-96a4-5f79beb51119` | Under 2 weeks (`28778b48-3fc9-4526-b9c2-c1e002928eb4`), 2-4 weeks (`8c5da56e-5c5e-442b-8b29-9950426210b9`), 1-2 months (`2befa2b5-8f8c-4804-a9c9-7e39ca0d132b`), More than 2 months (`059d575e-c249-448f-b49b-1bb3241db94a`), Varies by deal (`85910dfd-811c-4ef6-8b26-0d40312dcd08`) |
| `liquidity_interest` | Liquidity Interest | select, single | `5d3cb0ae-6e0a-4427-a117-035a6958ecdc` | Yes (`17a1ff6b-d899-4120-9a07-fb80d63ba8b8`), Possibly (`31b298b7-ad16-46a4-ab27-1f0fd5e08d73`), No (`54216edc-80bc-4306-ab4f-59efc46c6d3b`) |
| `liquidity_holdings` | Holdings They'd Sell | record-reference to Companies, multi | `27007815-dce1-44d4-946f-bbbd3cbd13de` | Allowed object is Companies (`b1428e8d-e770-4c68-be6f-b8af71cefa3c`). No reverse relationship. Link only to existing Companies. |
| `notable_past_investments` | Notable Past Investments | text | `e3b2b39d-c855-47a3-8385-d01ddc9f43fe` | Raw text only. |

Rules:

- Blocks and Interests are indications of supply or demand, not qualified inventory. They become qualified only when a teammate adds them to the deal-board lists: Qualified Blocks (`qualified_blocks`, list id `618270d1-a785-4fa3-8534-c35ac11b47bf`, parent object `blocks`) or Qualified Interests (`qualified_interests`, list id `67267bb5-c261-4681-b806-52404a70d1e5`, parent object `interests`). Matching, and any buy/sell sheet, should key off that list membership, not off `deal_classification`. The Morning Buy/Sell Sheet skill was retired (see DECISIONS.md); there is no live buy/sell skill to update.
- A website form answer of `liquidity_interest` Yes plus at least one existing Company on `liquidity_holdings` creates a normal Block immediately, one per company, the same way any other indication is written. Link `underlying_company` to that Company, and link the person as seller on Blocks `seller_contact_link` (inverse on People is `sourced_blocks_contact`). Status is Available. `deal_classification` is Secondary, because this is a sale of existing holdings. Do not invent a special unconfirmed path, and do not add the Block to Qualified Blocks. The website must not create a Company for an unmatched name; those names stay in the inquiry note, and no Block is created for them. Search for an existing Block with the same seller and company first, and update it rather than duplicating.
- The option `Unconfirmed` on Blocks `deal_classification` means Primary vs Secondary is not yet known. It is not a qualification flag. Existing Deal Indications writes use it when the type is unknown (47 of the 100 most recently created Blocks). Do not set it just because the Block came from the form.
- `liquidity_interest` Possibly stays on the person only. No Block is created.
- Deal Indications later confirms or enriches website-created Blocks from WhatsApp or email: update the existing record (terms, size, overview), and do not create a second Block. Qualification is still a human adding the record to Qualified Blocks.
- How a Block is qualified: POST `/v2/lists/qualified_blocks/entries` with `parent_object` `blocks` and `parent_record_id` set to the Block's record id (Composio: `ATTIO_PUT_V2_LISTS_LIST_ENTRIES`). No extra entry values are required. The list has one custom attribute, `push_to_deal_board` (checkbox, "Push to Deal Board"): when checked, that qualified Block is shown on the LP portal deal board. The form must not set this. Same pattern for Interests on `qualified_interests`. As of 2026-11-13 there are four Qualified Blocks entries, added by teammates, not by a bot.
- `liquidity_holdings` stores supply (holdings they might sell). Link only to existing Companies. Never create a Company or an Interest from this field.
- `notable_past_investments` is track record. Never create Company records from this text. Keep it separate from `companies_of_interest`, which is current demand.
- `investment_structure` is how they prefer to invest. The live `Secondaries` option on `sector_of_interest` stays for existing data; new answers about how they invest go on this field.
- `decision_timeline` is how long they typically take to decide once diligence begins. Option titles must match exactly, including `2-4 weeks` and `1-2 months`.
- attio-contact-context fills `investment_structure` and `decision_timeline` when empty, from chats or email, and never overwrites a human-set or form-set value. It may mention confident past-investment or liquidity facts in `arrow_context` without copying prices or creating records. It does not create Blocks.

Related existing fields, confirmed and not changed on 2026-11-13:

| Slug | Title | Type | Attribute id | Notes |
|---|---|---|---|---|
| `check_size_range` | Check size range | select, multi | `8682152a-e908-41a8-9c58-e25ea2775fbe` | Already multi-select. Nine live options: `$0-100K`, `$100-200K`, `$200-500K`, `$500-$1M`, `$1M-$5M`, `$5M-$10M`, `$10M-$15M`, `$15M-20M`, `$20M+`. The website form is still single-select and still has three stale values; that is a form follow-up, not a schema change. |
| `companies_of_interest` | Companies of Interest | record-reference to Companies, multi | `a780dbfd-2aa2-4988-89ec-349904c565a9` | Current appetite. Allowed object is Companies (`b1428e8d-e770-4c68-be6f-b8af71cefa3c`). No reverse relationship. Add only, never replace. Match existing Companies only; never create a Company from typed names. |

## Blocks write path (for website indications and Deal Indications)

Object slug `blocks`, object id `92e38fdb-18d1-44c5-92ff-3a53fe698c54`. Existing names follow "Person - Company", sometimes with a size in parentheses.

The only writable attribute that is required on create is `underlying_company`. `created_at` is required but system-set and not writable. Do not write archived slugs: `intermediaries`, `seller_fund`, `seller_contacts`, `associated_deal`, `associated_deals`, `source_8`.

| Slug | Title | Type | Required | Notes |
|---|---|---|---|---|
| `block_name` | Block Name | text | no | "{Person full name} - {Company name}" |
| `underlying_company` | Underlying Company | record-reference to Companies, single | yes | Existing Company id only. Allowed object `b1428e8d-e770-4c68-be6f-b8af71cefa3c`. |
| `seller_contact_link` | Seller Contact Link | record-reference to People, multi | no | The seller. Inverse is People `sourced_blocks_contact`. Use this for a website submitter who said they would sell. Do not put a direct seller on `intermediaries_2` or `intermediary_contact_link` (those are for the person who brought the indication). |
| `deal_classification` | Deal Classification | select, single | no | Live options: Primary (`488867f2-8e0b-4f2b-9244-669df3be116b`), Secondary (`5145eeb3-2c0e-4c7d-b6ab-5464c1cf8c50`), Unconfirmed (`4a5465f8-5928-46c6-88a9-00a690eedb77`). Attribute id `86b89ef6-2692-4c70-a668-f1634492ba63`. Website Yes-and-sell: Secondary. Unconfirmed means type is unknown, not "unqualified." |
| `status` | Status | status, single | no | Live titles: Available (`5ec49e91-364f-4d1c-a3f6-76bcc3afd623`), Reserved (`ebd4775b-276b-4e46-9674-d77b0413a79c`), Sold (`a26c6be5-9bad-4714-b8c9-b574d18e24e9`), Withdrawn (`0d0e7059-b736-4347-85ea-9c220d464975`). New indications use Available. Attribute id `effd2aab-735a-4b8c-b7ee-3f583b0c03f1`. |
| `source` | Source | text | no | Provenance, e.g. "website (11/13) - inquiry", matching "chris (10/27) - email". Description: "Where this block was dictated/indicated." Do not write archived `source_8`. |
| `overview` | Overview | text | no | Context: website form, person said they would sell this company, size unknown if unknown. There is no separate notes attribute; use this. |
| `terms` | Terms | text | no | Fill when known; otherwise say unknowns here or in overview. Do not invent numbers. |
| `block_size` | Block Size | currency, USD | no | Fill only when known. |
| `run_sync` | Run Sync | checkbox | no | Deal Indications sets this true on its writes; the form should too. |

Qualification is a separate step, described in the LP intake rules above. The form creates the Block and stops.
