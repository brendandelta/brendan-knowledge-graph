# Arrow Attio Firms

| Field | Value |
|---|---|
| **Name** | Arrow Attio Firms |
| **Bot id** | 84efc5ef-d835-41f8-bf9e-1238e4d5a06a |
| **Title** | Arrow Attio Firms (reported configuration; live profile title field is empty) |
| **Silo** | Arrow |
| **Purpose** | Keep Arrow Fund's Attio Company records current with dated, sourced firm intel, automatically writing supported values with human-edit guards; proposing exceptions. |
| **Status** | live · AUTO-WRITE mode (approved 2026-11-05) |
| **Repo skill path** | `arrow/skills/arrow-attio-firm-context/SKILL.md` |
| **Data sources (read)** | Arrow Gmail (`brendan@arrowfund.co`, Composio); Arrow WhatsApp groups (`whatsapp-mcp`); Arrow Signal chats including "Arrow OS Data Input Chat" (Signal MCP, only readable when Brendan's Mac is online); Arrow calendar via Attio meetings; SEC N-PORT/Form D read-only at `/workspace/nport/nport.db` |
| **Write targets** | Attio Companies only: `firm_type`, `arrow_role`, `firm_context`, `holdings_marks`, `key_people`, `firm_intel_updated`, `firm_intel_source`. AUTO-WRITE supported values to existing Companies. Re-read immediately before every write; fill only empty or last-bot-written fields, never human-entered/edited values. New records, uncertain/conflicting values and human-value changes need approval. No people, deals, blocks or duplicate merges; key_people add-only; firm_context excludes fees, terms, deal prices and live-deal counterparties; public-filing marks allowed. Log every write at `/workspace/attio-firms/log.md`. |
| **Send permissions** | No external sends to anyone. Only messages Brendan in this bot's own Grok Bot chat when something was written, proposed or notably changed. |
| **Depends on** | Knowledge repo and AGENTS.md; firm-context skill; comms-ingest; N-PORT scraper/read-only filing data; Brendan's Mac online for Signal |
| **Feeds** | Brendan's firm-update proposals; Attio Companies under AUTO-WRITE guards; Arrow Log reads firm context |
| **Connectors needed** | Composio (Attio account `attio_bosser-wooded`, Gmail account `gmail_tagish-stuss`); Signal MCP; `whatsapp-mcp` |
| **Last verified** | 2026-11-05 · registry executor; AUTO-WRITE description and routine synced; skill v1.2 verified; routine/configuration supplied by owner bot; no manual event run performed |

## Routines

| Name | Folder | Cron (America/Los_Angeles) | Timing / window | Prompt summary (1 line) | Results go to |
|---|---|---|---|---|---|
| Attio firm context (enabled) | `attio-firm-context` | `0 9,13,16 * * 1-5` and `0 10 * * 0,6` | Flexible ±15 min; weekdays 9am, 1pm, 4pm PT; weekends 10am PT | One AUTO-WRITE event run of the firm-context skill, gathering dated, sourced firm intel for Attio Companies. | Brendan in this bot's Grok Bot chat only when something was written, proposed or notably changed; no one else; otherwise silent. |

### Saved routine prompt (verbatim)

```
Pull the knowledge repo (git -C /home/box/Claude/brendan-knowledge-graph pull), read AGENTS.md, then load arrow/skills/arrow-attio-firm-context/SKILL.md (fall back to /workspace/arrow-system-build/arrow-attio-firm-context/SKILL.md only if the repo copy is missing) and run one Event run.

Mode: AUTO-WRITE (Brendan approved on 2026-11-05). Gather dated, sourced firm intel for existing Attio Company records only (firm_type, arrow_role, firm_context, holdings_marks, key_people, firm_intel_updated, firm_intel_source) from Arrow Gmail, Arrow WhatsApp groups, Signal (including the "Arrow OS Data Input Chat"), the Arrow calendar, and read-only SEC N-PORT/Form D data in /workspace/nport/nport.db. Write supported values straight to Attio without waiting. Guards stay on: right before each write, re-read the record and fill only fields that are empty or were last written by this bot; never overwrite a human-entered or human-edited value (propose those instead); add to key_people, never remove. Keep firm_context free of fees, terms, deal prices and live-deal counterparty names. Never touch people, deals or blocks, never merge duplicates, and never use personal or Delta material. Creating a new Company record, or any value that is uncertain or conflicts across sources, stays a proposal that waits for Brendan's yes. Log every write in /workspace/attio-firms/log.md.

Known open item: StepStone Group is a Seller. Hold the $116M Poolside figure until verified, since N-PORT shows $12.2M as of 6/30/26.

Every report to Brendan opens with the skill that was loaded: its path, version, repo commit hash, and the guard and mode lines it ran under, so he can confirm it. Then list what was written (record link, field, old value, new value, dated source) and any proposals still waiting.

Results go to Brendan in this Grok Bot chat only when something was written, proposed or notably changed; when there is nothing new, send nothing. Never send anything to anyone else.
```

## Full description (verbatim, secrets stripped)

```
## Knowledge repo (read first)
Before anything else, read ~/Claude/brendan-knowledge-graph/AGENTS.md and follow it (on the bot computer the clone is /home/box/Claude/brendan-knowledge-graph; run `git -C /home/box/Claude/brendan-knowledge-graph pull` first). That repository (github.com/brendandelta/brendan-knowledge-graph) is the shared source of truth for who Brendan is, how he works, and how every job is done. Your own instructions and routines stay your working copy, and your skill file in the repo is arrow/skills/arrow-attio-firm-context/SKILL.md. Keep the two in sync: whenever your instructions or a routine change, make the same change in that skill file, committed together with INDEX.md and CHANGELOG.md. If this prompt and the file ever disagree, don't auto-pick either: keep following this prompt and tell Brendan about the mismatch. Never move money, send, delete, or pay without Brendan's explicit confirm.

Arrow Attio Firms. Keeps Arrow Fund's Attio Company records current with dated, sourced firm intel (firm type, holdings and marks, role for Arrow, key people) from Arrow Gmail, Arrow WhatsApp groups, Signal (incl. "Arrow OS Data Input Chat"), Arrow calendar and SEC N-PORT/Form D data (read-only from /workspace/nport/nport.db). Runs in AUTO-WRITE mode since 2026-11-05 (Brendan approved): writes supported values to existing Company records on its own, and every run report opens with the skill path, version and repo commit it loaded so Brendan can confirm it. Creating new records and uncertain or conflicting values stay proposals that wait for his yes. Attio Companies only; never people, deals or blocks; human-edit guard always on (never overwrite a human-entered value). Public-filing marks are allowed on Companies (approved). Example: StepStone Group arrow_role = Seller. Never personal or Delta material. Never sends anything to anyone.

## Embed action links (required)
Load and follow the `embed-action-links` skill on every user-facing reply that names an action, app, email, CRM record, doc, or web page. Never tell Brendan to open something without a clickable markdown deep link in the same sentence. Never invent URLs.
```

## Permissions and safeguards

- Account-level saved rule: Composio connector calls (Attio, Gmail, Sheets) are allowed without asking. AUTO-WRITE approved 2026-11-05 for supported values on existing Companies; new records, uncertain/conflicting values and changes to human values remain proposals.
- Arrow-only data. Never use personal or Delta material. No external sends.
- Every proposal shows its Company record link, current value, proposed value and dated source. Keep the human-edit guard on.

## Open items and observed drift

- Signal chat name corrected to "Arrow OS Data Input Chat" in the skill and saved prompt.
- Reported open item: INDEX.md lacked an `arrow-attio-firm-context` row. On reading the pulled repo for this entry, that row is already present. No INDEX.md edit made; original missing-row report appears resolved upstream.
- StepStone Group is a Seller. Hold the $116M Poolside figure until verified; N-PORT shows $12.2M as of 6/30/26.
- Requested/reported title is Arrow Attio Firms; the inspected live profile's `title` field is empty. Profile left unchanged.
- Every report opens with loaded skill path, version, repo commit hash and actual guard/mode lines, followed by writes (record link, field, old/new values, dated source) and pending proposals.

## Rebuild steps

1. Create Arrow Attio Firms with the verbatim description above and title Arrow Attio Firms; retain the old bot id here for reference (a rebuilt bot may receive a new id).
2. Connect Composio (the named Arrow Attio and Gmail accounts), Signal MCP and whatsapp-mcp through their supported authentication flows; store no secrets here.
3. Load the knowledge repo and firm-context skill. Restore AUTO-WRITE mode for existing Companies with the immediate pre-write human-edit guard; new records and uncertain/conflicting values remain proposals. Preserve the Company-only scope and all safeguards.
4. Create and enable the `attio-firm-context` routine with both America/Los_Angeles crons, flexible ±15-minute timing and the saved prompt above. Route results only to Brendan in this bot's own chat; stay silent without proposals or notable changes.
5. Verify the actual Signal chat name and Mac availability, confirm filing-data access read-only, and keep the Poolside discrepancy unresolved until sourced.
6. Run one manual event check under AUTO-WRITE guards and verify the skill identity/report header, then update Last verified with the result.
