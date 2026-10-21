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
