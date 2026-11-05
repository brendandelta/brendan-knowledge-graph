# Decisions

## 2026-10-18

**One repo, one shared subfolder.** Considered two fully separate repos with a mount rule; rejected as too much to hold in his head. `arrow/` is a subfolder that is also its own repo. Visibility line is a folder boundary.

**The UI stores nothing.** Previous Arrow web app (CRM + entity tracker) failed because it became a system of record. The mirror renders what the agent returns and sends what Brendan says. Notion, Gmail, Attio, QBO stay the systems of record.

**Instructions live in one place.** `skills/` and `arrow/skills/`. Not in claude.ai, not in Notion pages, not in Grok memory. Those are engines and data.

**Mac mini is the brain, Pi is the face.** iMessage requires macOS, so the mini is non-negotiable as the server. The wall screen is a Pi 5 thin client in kiosk mode so the mini can live anywhere.

**Voice before touch.** Skip a touchscreen for v1.

**Phase 1 before any hardware.** Morning brief as a scheduled job on an existing screen proves the brain works. No wall monitor until that runs daily.

## 2026-10-19

**Update policy.** Instructions change only deliberately, in a commit that also touches `INDEX.md` and `CHANGELOG.md`. Data changes constantly and never touches the repo. A change to a data *structure* (redesigning a Notion db) is an instruction change because the skill that reads it changes. Rule: if a skill would behave differently, it's a commit.

**Self-auditing over vigilance.** A nightly job reads all skills + INDEX and flags contradictions, stale references, and skills not in the index. Brendan does not hold the whole system in his head.

## 2026-10-20

**Money is propose-only.** Every movement across Delta, Arrow, Janey is a proposal into a weekly queue; Brendan approves each one. Reconsidered the threshold idea; rejected for now. Revisit with a track record.

**Four contexts, not three.** Janey added. Personal and Delta are intertwined; Arrow is the only shared one.

**Arrow is sealed.** `arrow/` shareable outward; nothing flows up from it into the parent brain. Attio→Notion one-way automatic; Notion→Attio only via the approval feed.

**Composio stays.** Standard apps go through Composio or its agent-native successor; custom MCP only where nothing exists (iMessage, Signal, WhatsApp bridge).

**BARVIS parked.** The prior CFO system is not absorbed now and not run in parallel. Revisit when the accounting workflow is built.

**Build by workflow.** New skills get written as real workflows come up, not planned in advance.

**Chat is the interface.** Approvals and feeds arrive as chat with confirm actions. Dashboards later.

**Mercury is dead.** Chase Business Checking is Delta's operating account; Chase Business Savings is the tax reserve.

**No confidence tags** in output to Brendan. Say when inferring instead.

**Constitution adopted.** Core (repo as brain, systems of record stay, sealed arrow/, propose-only money) is frozen; shell (engines, models, hardware, skills, tooling) evolves. Rebuild test and twice rule in CONSTITUTION.md. Reason: BARVIS and the Arrow web app were both full rebuilds that lost prior work.

**Stubs deleted; Attio reads allowed (UNCONFIRMED).** Four pre-written stub skills removed under the twice rule. brendan-crm may read Attio directly because Attio→Notion is the agreed one-way flow; writes to Attio only via crm-sync-feed after approval. Brendan has not confirmed the read half.

**Grokbot prompts are the skills.** Imported nearly verbatim rather than rewritten. Fix seams after migration, not before. Reason: rebuild test.

**No send exceptions.** Grokbot's two auto-sends (deal-indications Signal ack, private-markets-desk team email) are removed on migration. Confirm-before-send is absolute.

**No confidence tags anywhere.** Private Markets Desk says "unverified" in words.

**Repo is source of truth for instructions only.** Data (messages, records, the comms index) lives in databases and logs. The Delta CRM Spec in Notion becomes a pointer to skills/brendan-crm.

**Composio for everything.** No Claude Desktop connectors. Custom MCP only where Composio has no toolkit.

**1Password.** Account opened after the mini is set up. Four vaults by entity.

**Money moves only through real payment APIs.** No bank scraping. Consumer bank transfers stay manual, agent-prepared.

## 2026-10-21

**Default send-from is brendan@arrowfund.co** for nearly everything; agents ask when another address plainly fits better.

**Attio writes from the personal CRM:** check Attio; if absent, ask before adding; if present, fill only Attio-tracked structured fields (contact, check size, investor type, sectors) and only where empty.

**Renamed brendan-knowledge-graph → brendan-knowledge-graph** (2026-10-29). Same thing, name now says what it is.

## 2026-10-29

**Send exceptions (approved by Brendan).** Private Markets Desk daily pack, Arrow Weekly and Daily Arrow Report go out as emails (Arrow Weekly and Daily Arrow Report auto-send only after Brendan approves the process). Deal Indications posts "added into Attio" acks on Signal. The Buy/Sell Sheet goes only to Gabe on WhatsApp, once Brendan approves it (not done yet). Call Cadence sends calendar invites. Everything else still waits for Brendan. Supersedes the 10/20 "no send exceptions".

**Sync-and-flag.** Each bot keeps its own instructions and keeps its skill file here matching how it runs. Where a bot and the repo disagree, the bot's current way wins, but only after the gap is flagged to Brendan and he approves each change. Each bot edits only its own skill file; Grok Bot commits with INDEX.md and CHANGELOG.md.

**Follow-up split.** Call Cadence owns relationship calls, Inbox Todo owns reply-owed tasks, CoS only ranks, brendan-crm workflow 5 runs on demand only. Inbox Todo also scans admin@arrowfund.co.

**No confidence tags** in private-markets-desk (reaffirmed).

**Notion Delta CRM Spec deleted.** skills/brendan-crm is the only spec.

## 2026-10-30

**Entry file is AGENTS.md.** CLAUDE.md renamed, no duplicate or symlink, so every bot and agent reads one file first. CLAUDE.md stays only as a one-line pointer to AGENTS.md for Claude Code.

## 2026-11-03

**Attio context process (Brendan approved, chat t67u/t69u/t70u).** New arrow/skills/attio-context (renamed attio-contact-context 2026-11-04) writes 2–4 plain business sentences (Arrow context) plus relationship links (Introduced by, Knows, Close with, Relationship owner, Context updated) on Attio People, so teammates like Gabe can find who someone is without knowing the schema. It replaces, for those six fields only, the brendan-crm privacy rule "if you are about to write a sentence into Attio, stop" and the 2026-10-13 v3 rejection of Relationship owner. Why the old way failed: keeping everything in Brendan's private Notion left the Arrow team with names and no context, and Brendan deletes messages, so the context was being lost. Guardrails: business-only inclusion test, silos (no quotes, dates, prices, opinions, personal facts, Notion bodies), human-edit guard, never sends. It also reads Notion People Role, Company and Based in into Attio directly, an approved exception to "Notion→Attio only via the approval feed" for those three properties. The CRM process must not write the six fields.

## 2026-11-04

**Notion CRM and Attio context are two separate processes (Brendan approved, chat t74u–t78u).** Inputs are shared; outputs are siloed. Both read the same sources (Comms Index, WhatsApp, Signal incl. team groups, iMessage, Attio calls/transcripts, the 5 Gmail inboxes, Calendar). The Notion CRM (brendan-crm, crm-maintenance) writes only Notion and treats Attio as read-only. arrow/skills/attio-contact-context (renamed from attio-context) is the only writer of Attio People context, relationship, investor and empty contact fields, and never reads or writes Notion. This supersedes the 2026-11-03 exception that read Notion Role/Company/Based in into Attio, and the planned crm-sync-feed. Why: the old mirror specs contradicted each other (sectors in, sectors banned) and two writers on the same Attio fields made provenance unclear. Label mapping for What they are: VC/fund manager → GP; family office → LP + FO tag; broker/placement/finder → Intermediary; the word "Broker" is never used. Scope: people only Brendan has spoken to (no other Arrow member on a call, thread, email or group, no block/interest/list link) are never added or suggested for Attio, and existing records with only Brendan-only context get neutral contact fields at most; nothing is deleted. Teammates' own records never get a Relationship owner.
