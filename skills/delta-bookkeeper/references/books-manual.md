# Delta Meridian — Books Manual (durable rules & structure)

Baseline verified Aug 5, 2026; updated Sept 29, 2026. Balances change — always pull live. Rules here are overridden only by Notion **Rules (live)**.

## Contents
1. Company · 2. Banking map · 3. Chart of accounts · 4. The ten rules · 5. Decision tree · 6. Vendor map · 7. Monthly rhythm · 8. Tax framework

---

## 1. Company
- **Delta Meridian Holdings LLC**, California single-member LLC, **S-corp election effective 2026**. Owner: **Brendan Conn** (admin@deltameridian.com; personal brendanmconn96@gmail.com). Santa Monica, CA.
- S-corp consequences: reasonable salary required (December payroll), distributions tracked separately, basis tracking.
- **Business:** operations consulting. **Sole client: Arrow Fund Holdings LLC** (principals Chris Clifford and Gabe Borden; Chris initiates Arrow's payments).
- **Revenue pattern:** ~**$8,000/month**, lands ~the **28th** by real-time transfer/ACH. History: Feb $10,500; Mar–May 1 $4,000 installments; from May 28 single $8,000/month.
- Related-party context (not Delta entries): $6,500 gift from Chris & Gabe (Jan/Feb 2026) went to Brendan's personal accounts and is correctly NOT income. Arrow's books carry a $25,000 loan receivable from Brendan. Janey Health (related party) appears once as a $0.01 verification deposit.
- **QBO:** Simple Start (no classes/projects). Fiscal year = January. Tax form should be 1120-S (Brendan switching in UI, Sept 29 2026).

## 2. Banking map
**Active**
| Account | Last 4 | QBO name / id | Role |
|---|---|---|---|
| Chase Business Checking | 0012 | "Delta (0012) - 2" / 144 | Operating account |
| Chase Business Premier Savings | 8603 | "CHASE BUS PREM SAV (8603)" / 145 | **Tax Reserve** (added to QBO 9/29/26; ~$15k real) |
| Chase Personal Checking | 0669 | NEVER in QBO | Brendan personal — distributions & reimbursements land here |

**Legacy (drained to $0 on 8/4/26; keep until statements exported, then close ~Oct–Nov):**
- Mercury business org: Checking ••1344 (QBO id 140), Business Intake ••3958 (id 141), Mercury Credit (id 142). Swept $397.66 + $277.46 = $675.12 → Chase 0012 on 8/4.
- Mercury personal org "Delta's Account" — never in QBO; swept $601.16 → personal 0669.
- Wells Fargo ••8796 — Brendan personal, early 2026 only.

No business credit card in QBO as of Sept 2026 (confirm if one appears).

## 3. Chart of accounts (active set)
**Income:** Consulting income — the ONLY revenue account. Product/service item "Operations Consulting" → Consulting income.
**Other income:** Other income (parent) → Credit card rewards · Interest earned (use for Tax Reserve savings interest).
**Expenses:**
- Advertising — all ad spend incl. ads fronted for Arrow (Arrow's reimbursement posts here → nets $0)
- Contract labor — watch $600/person/year 1099-NEC threshold
- Dues and subscriptions — LinkedIn Sales Navigator (Arrow reimburses)
- General business expenses → Bank fees & service charges
- Home office reimbursement — $536.25/month accountable plan
- Legal & accounting services → Accounting fees · Legal fees
- Meals — book 100%; CPA applies 50% limit. Receipts for anything >$75
- Office supplies
- Officer wages — empty until December payroll
- Payroll taxes — empty until December payroll
- Software — QuickBooks $38, OpenAI $20, Supabase ~$25–35, NordVPN $11.99, Google Workspace $16.80
- Taxes & licenses (business) — CA franchise tax
- Travel (parent — never post to the parent) → Airfare · Hotels · Taxis or shared rides · Vehicle rental · Parking & tolls
- Utilities → Phone service (90% business) · Internet & TV services (90% business)
**Equity:** Owner contributions · Shareholder distributions · Retained Earnings · Opening balance equity ($0)
**Never post to:** Uncategorized Asset/Income/Expense, Payments to deposit, Unapplied Cash Payment Income, Sales tax to pay, the stuck "Home office" Other-Expense shell. A/R and A/P should stay $0 (no invoicing — Arrow pays without invoices).
**Inactivated — do not resurrect:** Cash, Inventory Asset, COGS, Billable Expense Income, Management fees, Other business income, Sales of Product Income, Services, Uncategorized Income (income), Memberships & subscriptions, Taxes paid, Property taxes, Home utilities, Homeowner & rental insurance, Rent (Other Expense), Capital Contribution, Owner distributions - Tax payments, Mercury Savings (8180).

## 4. The ten rules
1. **Arrow service payments → Consulting income.** Nothing else is ever revenue.
2. **Arrow expense reimbursements → the same expense account as the original cost** (nets to $0). Never income. QBO auto-suggests Consulting income for any Arrow deposit — override unless it's the ~$8k service payment.
3. **Transfers between Delta-owned accounts → Transfer** (no P&L). Tax-reserve funding from 0012 is a transfer, not a distribution — the reserve is business money.
4. **Money to Brendan personally, or IRS/FTB paid for him → Shareholder distributions.** One account for all of it.
5. **Personal money into the business (rare) → Owner contributions.**
6. **Home office: $536.25/month** accountable-plan reimbursement, paid at month-end for the following month. Lease + calculation + bank statements are the documentation (no per-month rent receipts needed).
7. **Phone and internet: 90% business / 10% personal.** Book the bill to Utilities; the 10% personal share is adjusted out by journal entry (check the Aug 1, 2026 JEs in QBO for the exact pattern before repeating it).
8. **Vehicle: no gas on the business.** Personal car. Mileage reimbursement (~70¢/mile 2026) via the accountable plan — Brendan reports miles after long business-driving days. Parking & tolls deductible on top. Stray gas → Shareholder distributions.
9. **Receipts attach to the QBO transaction** (QBO is system of record). Priority: travel, hotels, flights, meals >$75. Delta Intake in Notion holds receipts until attached.
10. **Pre-Aug-2026 history is verified — never edit it.**

## 5. Decision tree (every bank item)
1. **From Arrow?** ~$8,000 (or the $4,000 pattern) → Consulting income. Any other amount → a reimbursement: find the fronted cost (memo, amount, Delta Intake "Arrow-fronted" rows) → offset to that expense. No match → ask.
2. **Counterparty is a Delta-owned account** (0012, 8603 savings, legacy Mercury)? → Transfer. Book once.
3. **Outbound to 0669 / Brendan?** $536.25 at month-end → Home office reimbursement. Memo says reimbursement for itemized business costs → those expense accounts, receipts required (check Delta Intake). Anything else → Shareholder distributions.
4. **IRS / FTB (EFTPS, "IRS USATAXPYMT", "FRANCHISE TAX BD")?** Personal estimated tax for Brendan → Shareholder distributions. CA LLC/S-corp franchise tax ($800 min / 1.5%) → Taxes & licenses (business). If unclear which, ask.
5. **Inbound to Tax Reserve from personal 0669** → Owner contributions.
6. **Known vendor** (see §6) → mapped category.
7. **Unknown vendor** → best guess + flag as a misfit. Never leave Uncategorized.
8. **Looks personal** (loans, groceries, gas, personal bills) → stop and flag; likely Shareholder distributions or a mistake to raise.
9. **Always match to an existing booked entry before creating a new one.**

## 6. Vendor map
| Bank text pattern | Category | Notes |
|---|---|---|
| ARROW FUND ~$8,000 | Consulting income | monthly, ~28th |
| ARROW FUND other amounts | offset the fronted expense | Rule 2 |
| INTUIT *QBooks | Software | $38, ~2nd |
| NordVPN / al.Nord*Products | Software | $11.99, ~17th |
| Supabase | Software | ~14th, $25–35 |
| OpenAI | Software | $20, ~10th |
| GOOGLE Workspace | Software | $16.80, ~1st |
| FACEBK* / Facebook | Advertising | if for Arrow, expect reimbursement |
| LinkedIn Sales Navigator | Dues and subscriptions | Arrow historically reimburses |
| VERIZON / VZWRLSS | Utilities: Phone service | 90/10 |
| Zelle to Ben Thauvin $45 | Utilities: Internet & TV | 90/10 |
| Online Transfer to CHK…0669, $536.25 month-end | Home office reimbursement | |
| Online Transfer to CHK…0669, other | Shareholder distributions | |
| Transfer to/from savings 8603 | Transfer | |
| Restaurants (Shake Shack, Cava, TST*, Border Grill…) | Meals | receipt if >$75; note who/why |
| Uber / Lyft / taxis | Travel: Taxis or shared rides | |
| Airlines | Travel: Airfare | receipt always |
| Hotels / Airbnb | Travel: Hotels | receipt always |
| U-Haul / Hertz / car rental | Travel: Vehicle rental | business % only |
| Parking / tolls | Travel: Parking & tolls | |
| Home Depot / Amazon (office-related) | Office supplies | |
| Arco / Chevron / Shell (fuel) | Shareholder distributions | Rule 8 |
| Venmo/Zelle to individuals for work | Contract labor | 1099 threshold |
| Card cashback / rewards | Other income: Credit card rewards | |
| Savings interest | Other income: Interest earned | |
| Micro-deposits < $1 | Other income | |
| GOLDMAN/MARCUS, SALLIE MAE, ECSI, APPLECARD, CITI, DISCOVER, DMV | **PERSONAL — must never be in Delta's books** | flag immediately |

## 7. Monthly rhythm (after Arrow's ~28th payment)
1. Match the ~$8k deposit → Consulting income.
2. $2,000 → Tax Reserve savings (Brendan clicks; book Transfer).
3. $536.25 → Brendan (Home office reimbursement).
4. ~$3,500–4,000 → Brendan (Shareholder distributions).
5. Keep ~$1,500–2,000 operating float in 0012.
6. Clear feed; match Delta Intake receipts.

## 8. Tax framework (S-corp, California)
- **Reserve rule:** ~25% of net operating income. YTD NOI at 8/4/26 was $46,049.70 → target then ≈ $11,500; full-year NOI trending ~$85–90k → blended rate may drift to ~30%. Check reserve vs. YTD NOI each session and flag shortfalls.
- **Estimated taxes:** federal + CA quarterly. Q1/Q2 2026 were missed; CPA sizing catch-up. Q3 due Sept 15, 2026; Q4 due Jan 15, 2027. Personal estimates paid from Delta → Shareholder distributions.
- **December 2026:** reasonable-salary payroll (Officer wages + Payroll taxes). Provider not chosen as of Aug 2026.
- **CA franchise tax:** $800 minimum + 1.5% of S-corp net income → Taxes & licenses (business).
- **Accountable plan:** home office + mileage reimbursements; formal document still to be written.
- Open CPA questions live in Notion Open Items.
