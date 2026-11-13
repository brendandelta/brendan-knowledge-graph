---
name: carta-investor-onboarding
description: Use when taking an investor from the fundraise pipeline to a funded, countersigned subscription in Carta Closings for an Arrow fund or SPV (pipeline sheet → Attio → Gmail → Carta). Covers fund-level Carta setup, the numbered per-investor order of operations, the emails Arrow sends outside Carta, and the approval gates. Hubble (AF Space Fund 1 LP) is the worked example.
---

# Carta investor onboarding (Arrow funds and SPVs)

Status: draft v2 (2026-11-02). Source: Marcela Cardoso dos Santos (Carta Fund Administration, Implementation PM), "Arrow Fund <> Carta - Closings Demo", Oct 16 2026, recorded in Attio. This is the primary basis for the Carta mechanics. Stage names come from the Hubble Fundraise Pipeline sheet's Lists tab.

## Hard rules

1. **Nothing goes out without Brendan's explicit OK.** That covers every email, every message, Carta **Send Invites**, **Countersign**, removing a signature, and capital calls. Draft it, show it, and wait.
2. **Money is propose-only.** Never initiate, approve, or confirm a wire. Only report what the bank or Carta shows.
3. **Pipeline sheet and Attio writes need an OK** until Brendan makes them standing. Propose the exact cell or field change.
4. **Countersigned means legally binding.** After countersign, changes go through fund counsel, not Carta edits. Before countersign, fix errors by removing the signature so the LP re-signs.
5. **Never ask for SSNs, tax forms, IDs, or bank account numbers by email.** Carta's questionnaire and KYC collect them.
6. **The minimum and the cap come from the fund documents.** Anything below the minimum or above the remaining allocation needs a written GP exception before the Carta invite.
7. Attio is the system of record for people. The pipeline sheet is the deal team's working tracker. Keep both in step, and log each step in both.
8. Keep this file generic. No personal or non-Arrow material belongs here.

## Systems

| System | What lives there | Who writes |
|---|---|---|
| Pipeline sheet (one per deal, e.g. "Hubble – Fundraise Pipeline") | One row per prospect: Stage, Subscription $$, Commitment ($), Team Owner, Last touch, CRM match, Next step, Due | Deal team. Bot proposes edits. |
| Attio (arrow-fund) | Person/company record, call recordings, notes, deal stage | Bot after an OK. When a run needs a recording or transcript, fetch it with `skills/comms-ingest/attio-calls.md`. This skill does not run a scheduled scan. |
| Gmail (brendan@ / chris@arrowfund.co) | Arrow's own emails: info request, heads-up, countersigned, funded | Brendan sends. Bot drafts. |
| Carta → Entities → [vehicle] → Closings | Sub docs, prospects, questionnaire, signatures, countersign, capital call | Brendan/Chris. Bot only clicks on an explicit OK. |
| Drive | Executed LPA, sub agreement template, countersigned copies | Deal team |

**Sheet Stage dropdown (Lists tab), in order:** Initial call → AF to Send Investor Email → Partner to send Investor email → Intro email sent → VDR requested → VDR sent / whitelisted → Follow up → Interest Indication → In diligence → Official IOI → Carta Invite sent → Deal Closed.
Use only these values. Map off-list values when you see them: "Send Carta" → Official IOI (amount agreed, invite not sent); "AF Follow up" → Follow up; "Pass" / "Not a Fit" are terminal. Flag them, and don't rewrite without an OK.

## A. Fund setup in Carta (once per vehicle, before any invite)

Per Marcela (Oct 16):

| # | Item | Where | Notes |
|---|---|---|---|
| F1 | Entity KYC complete | Carta KYC task link from the Carta team | Blocks the Closings tool. |
| F2 | Upload the LPA / operating agreement and the subscription agreement | Closings → Setup (or send them to the Carta team to upload) | **Only the sub doc is required for closings run through Carta.** Docs are read-only to LPs. LPs answer **Carta's standard questionnaire**, not Arrow's. Before signing, they agree to the LPA and sub doc terms, and that agreement is binding. |
| F3 | Configure preferences | Closings → Setup | (a) **Manager name = the GP entity name, not a person**, plus the firm address. (b) Optional welcome note explaining that you use Carta and how it works. (c) Qualified Purchaser and Qualified Client: **Require** or **Do not show**. "Require" forces an answer, but an accredited-only LP can still answer No. (d) Extra document uploads only when counsel needs them, e.g. a driver's license for third-party KYC. The standard questionnaire already collects the tax ID and W-9/W-8. (e) Capital call: **automatic on countersign** (N days to pay, notice sent at once) **or later** from Carta's capital-call area. Add the purpose and team contacts. (f) Submit. |
| F4 | Bank / wire instructions for the vehicle | Bank → Carta | Needed for the capital call notice. |
| F5 | Deal terms on hand | Fund docs | Minimum, cap/allocation, fees (e.g. a 105% wire = 100% + 5% fee), funding window, Outside Date and any extension notice. |
| F6 | Pipeline sheet + Attio list exist | Drive / Attio | Each sheet row has a CRM match to an Attio person. |
| F7 | Closing offline instead? | Carta team | Provide the countersigned sub docs, and Carta uploads those LPs offline. |

## B. Per-investor order of operations

Each step lists: **system → what gets updated → gate.** "OK" means Brendan's explicit approval in chat.

| # | Step | System → update | Gate |
|---|---|---|---|
| 1 | **Get them on the pipeline.** Add a row with name, firm, type, email, partner/source, and phone. Link the Attio person (create one on OK). | Sheet: new row, Stage = Initial call, CRM match. Attio: person + note on the source. | OK for Attio create |
| 2 | **Intro and materials.** Send the investor email, the webinar or recording, and the data room (VDR). | Gmail: intro email. Sheet Stage → Intro email sent → VDR requested → VDR sent / whitelisted. Attio note. | OK per send |
| 3 | **Interest and diligence.** Hold calls and answer questions. | Sheet Stage → Interest Indication → In diligence. Attio: call recording/notes linked. | none (read) |
| 4 | **Negotiate the subscription.** Agree on the amount (check it against the minimum and the remaining cap), the fee/wire math, the investing entity, and timing. Below-minimum or over-cap amounts need a GP exception. | Sheet: Subscription $$ (ask) and **Commitment ($) (agreed)**. Stage → **Official IOI**. Attio: amount + entity. | GP OK on any exception |
| 5 | **Info request email.** Ask for the exact legal name and entity type, signatory + co-signers/collaborators, accredited (and QP/QC) basis, the bank the wire comes from, funding timing, and any existing holdings in the issuer. Note that Carta will collect the tax/ID info. | Gmail draft → Brendan sends. Sheet: Last touch, Next step "await info". | **OK to send** |
| 6 | **Pre-screen the reply.** Is the entity eligible? Is there a trust or entity KYC need (formation docs, 25% owners + control person)? Any conflicts or source-credit issues? | Attio note. Sheet: Next step. | Flag issues to Brendan |
| 7 | **Enter them in Carta.** Closings → Overview → **Add Prospective Investor**: legal name (editable later), one or more collaborators (name + email, e.g. every trustee), commitment **Fixed** (default) or **LP-entered** (only for flexibility, e.g. anchors). The prospect shows as *Not invited*. | Carta: prospect saved (not invited). | **OK on the exact values before Save** |
| 8 | **Heads-up email (Arrow, outside Carta).** Tell them a Carta invite is coming from Carta, what they'll sign (LPA + sub agreement), the amount and the wire math, and the deadline. | Gmail draft → Brendan sends. | **OK to send** |
| 9 | **Send the docs = Send Invites.** Select the prospect → **Send Invites**. New Carta users get an email to join. Existing users get an email plus a task in Carta. | Carta: status *Invited*. Sheet Stage → **Carta Invite sent**. Attio stage. | **OK to click Send Invites** |
| 10 | **They complete and sign.** Watch *Invited → In progress → Submitted*. If they're stuck, use ⋯ → view/edit questionnaire to see where. | Carta status. Sheet: Last touch/Next step. Nudges are drafts only. | OK per nudge |
| 11 | **Pre-countersign review.** Check the legal name, the amount = Commitment ($), signatures from every collaborator, the accredited/QP/QC answers against what the fund requires, and that KYC is clear. To fix anything: **remove the signature**, correct it, and they re-sign. | Carta (read). Summary to Brendan. | **OK before removing a signature** |
| 12 | **Countersign** (the GP signatory). This is legally binding. If the capital call is set to auto, the notice goes out now. | Carta: *Countersigned*. Sheet: note the countersign date. Attio note. Drive: save the executed copy. | **Explicit OK to countersign** |
| 13 | **"Your documents are countersigned" email (Arrow, separate).** Confirm acceptance, attach or link the executed docs, and state the wire amount, the due date, and that wire instructions come only through Carta. Never put wire details in the email body. | Gmail draft → Brendan sends. | **OK to send** |
| 14 | **Capital call and wire.** Confirm the call notice went out (auto or manual), then watch for the wire and match it to the amount. | Carta capital call. Bank (read). Sheet: Next step. | **OK for a manual call. Never move money.** |
| 15 | **Funded confirmation email (Arrow, separate) and close-out.** Confirm receipt. | Gmail draft → Brendan sends. Sheet Stage → **Deal Closed**, Commitment ($) final. Attio: Funded. Drive filed. Track fund-level Form D/blue-sky timing with counsel. | **OK to send** |

Carta statuses: Not invited → Invited → In progress → Submitted (ready to countersign) → Countersigned → Called → Funded.

## C. Email templates (drafts only; Brendan sends from brendan@arrowfund.co, cc the deal partner)

**Info request (step 5).** Subject: "[Issuer] via Arrow: next steps for your subscription"
Confirm the amount from step 4. Ask for:
1. Exact legal name of the investing person or entity, and its type
2. Signatory, plus anyone else who must sign or see the docs (name + email)
3. Basis for accredited status (and QP/QC if the fund requires it)
4. The bank the wire will come from
5. Funding timing against the window
6. Any existing [Issuer] holdings or rights

Then say Carta will collect tax, ID, and KYC securely, and that a Carta invite will follow.

**Heads-up (step 8).** "You'll get an email from Carta in the next day to review and sign the [vehicle] LPA and subscription agreement for $[amount] (wire $[amount × fee factor]). It takes about 15 minutes."

**Countersigned (step 12/13).** "We've countersigned your subscription in [vehicle]; your executed documents are in Carta. Please wire $[x] by [date] using only the instructions in Carta's capital call notice. Call us to verify before sending."

**Funded (step 15).** "We received your $[x] wire on [date]. You're fully in [vehicle]. Thank you."

## D. Worked example: Hubble (AF Space Fund 1 LP)

- Sheet: "Hubble – Fundraise Pipeline" (Arrow Drive), tabs Pipeline / Summary / Webinar / Lists.
- Terms (Sub Agreement + LPA): minimum $500k per Sub Agreement §3(a) (lower amounts were quoted case by case, so these need a GP exception); 105% wire (5% fee); fund within 3 business days of acceptance; $10M cap; Outside Date Oct 30 2026, extendable to Nov 30 by GP written notice (LPA §14.24).
- Carta: entity KYC done; sub agreement sent to Carta Oct 19; preferences/capital call pending the JPM wire instructions as of Nov 2 2026.
- The Carta default questionnaire covers tax ID/W-9. Decide QP/QC Require vs Do not show with counsel.

## Open decisions (carry until Brendan answers)

1. Who countersigns in Carta (Chris as GP signatory, or Brendan)? May the bot click Save / Send Invites after an explicit OK?
2. Capital call: auto on countersign with N = 3 business days, or manual? Can the notice show the 105%?
3. QP/QC: Require or Do not show for each vehicle.
4. Standing GP exception for sub-minimum tickets? What's the floor?
5. Should the deal team standardize the sheet on the Lists stages (retire "Send Carta", "AF Follow up")?
