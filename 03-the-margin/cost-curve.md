# Cost Curve & Pricing Strategy

## Unit of account

One 32-bed medical-surgical inpatient unit. Assumptions, all stated:
- 8 RNs per shift, 2 shifts per day
- 6 off-unit errands per nurse per 12-hour shift = 96 errands/unit/day
- 7 minutes median round trip = 11.2 nurse-hours/day consumed by errands
- 40% diverted to a qualified performer with verified completion = 4.5 nurse-hours/day = 1,635 hours/year returned
- $65/hour fully loaded RN cost
- **Annual value per unit: ~$106,000. A 30-unit system: ~$3.2M/year.**

Baseline anchors, published rather than internal: med-surg nurses walk a median 3.0 miles per shift and spend 86 minutes (20.6%) of nursing practice time on care coordination against 31 minutes (7.2%) on patient assessment (Hendrich et al., Permanente Journal 2008, 767 nurses, 36 units). IFR reports staff in a typical 200-bed hospital walk 400 miles per week moving supplies, equipment and waste.

## Cost Model

~2,900 tasks per unit per month.

| Cost Category | Per-Unit/Month | Notes |
|--------------|----------------|-------|
| Inference (primary model) | $9.28 | Exception triage only. ~8% of tasks x ~$0.04 |
| Inference (cascading/triage) | $1.16 | Request intake parsing, small model, $0.0004/task |
| Inference (verification VLM) | $8.70 | Completion-evidence check, $0.003/task |
| Demand prediction (Loop B) | $6.00 | Batch inference on clinical event stream, hourly horizon |
| Eval harness (amortized) | $12.00 | Weekly golden-set runs across the footprint |
| Infrastructure | $40.00 | Edge gateway, elevator/door adapters, telemetry bus |
| Data/storage | $22.00 | Task event stream + qualification corpus retention |
| Human-in-the-loop | $54.00 | 2% of tasks x 2 min review x $28/hr (down from 3% — the corpus is what lowers the HITL rate) |
| **Total AI COGS** | **~$153** | Against ~$8,850/month of value delivered |

**The number that matters is not on this table.** Per-site implementation — elevator and door integration, badge and access provisioning, EHR interface, unit mapping, qualification-rule authoring — runs $180,000-$400,000 one-time per hospital. At a $300,000 planning figure over 36 months that is ~$8,333/month, which is 54x the monthly AI COGS of a single unit. Because that charge lands per hospital rather than per unit, blended gross margin is set by how many units a hospital instruments — not by inference cost.

**Margin by density.** Usage revenue $1,152/unit/month, AI COGS $153/unit/month, $4,000 base and $8,333 amortized implementation per hospital:

| Units per hospital | Monthly revenue | Monthly COGS | Gross margin |
|---|---|---|---|
| 5 | $9,760 | $9,098 | ~7% |
| 10 | $15,520 | $9,863 | ~36% |
| 20 | $27,040 | $11,393 | ~58% |
| 30 | $38,560 | $12,923 | ~66% |

The ~66% top of that range is the 30-units-in-one-hospital case — roughly a 960-bed facility. A 30-unit *system* spread across six hospitals at the five-unit break-even earns ~7%. Density per hospital, not total unit count, is the margin variable, and productizing integration is what shifts the whole curve left.

**What the premise changes:** rule authoring is the largest single line inside implementation, and under P1-P2 it is inherited rather than written. Site one bears it; sites two onward do not. That is the difference between a services business and a software business, and it is the strongest internal argument for funding the corpus work explicitly rather than treating it as a by-product.

## Cascading Strategy
<!-- Cheap model → frontier model routing logic -->

**Triage model:** none for assignment. A deterministic constraint solver makes the performer decision. This is the highest-leverage cost decision in the product and the easy mistake is to reach for a model because the product is called AI.
**Frontier model:** exception explanation, novel payload-class adjudication, free-text intake parsing.
**Routing rule:** the solver handles every request with a known payload class and an available qualified performer. Escalate to frontier only when the payload class is unrecognized, no performer qualifies, or a clinician disputes.
**Expected cascade ratio:** ~92% deterministic or small-model / ~8% frontier.

## Packaging: Leader, Filler, Killer

**Leader:** cross-fleet qualified assignment with verified completion.
**Filler:** unified queue view and shift-level utilization reporting. Low marginal cost, bundle it.
**Killer:** the qualification exchange — cross-site benchmark, vendor conformance reporting and inherited rule library. Heavy, high-value, and used by well under 70% of sites in year one, so price it separately per the 70% rule.

## Pricing Model

**Current pricing:** not applicable, 0-to-1.
**Proposed AI pricing:** two SKUs.
1. *Runtime* — $4,000 per hospital per month base, plus $0.40 per verified completion. The unit of charge is a verified completion, not a dispatch, so we are paid when the loop closes. Same structure as charging per resolved conversation.
2. *Qualification exchange* — annual subscription per health system, priced on bed count, granting the inherited rule library, the cross-site benchmark and vendor conformance reporting. Contributor-only: a member who stops submitting telemetry loses benchmark access at renewal.
**Model:** hybrid, tilted to outcome, with a data-contribution condition on the second SKU. That condition is the pricing expression of the Network loop and it is what makes Loop A commercially self-enforcing.

At 30 units **in a single hospital** x 96 tasks/day x 30 days = 86,400 completions/month: $34,560 usage + one $4,000 base = **$38,560/month, ~$463k/year** on the runtime, against ~$3.2M/year of returned time. Value capture ~14.5%, leaving enough on the table that the buyer's business case survives a skeptical CFO. The exchange SKU is incremental.

Spread those same 30 units across six hospitals and base revenue rises to $24,000/month while amortized implementation rises to $50,000/month: revenue improves and margin collapses to ~7%. The configuration matters more than the unit count.

## Stress Tests

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Inference costs 3x | GM 66% → 63% at 30 units/hospital. Inference is ~$25 of $153 COGS. | Absorb and document. It is the counter-argument to "AI products have unpredictable COGS." |
| Heaviest segment doubles | GM improves. Usage revenue scales with task volume under outcome pricing while COGS scales sub-linearly. | None. This is the case for outcome pricing over flat subscription; the same doubling under a flat fee is pure margin loss. |
| Model provider raises prices 50% | ~$13/unit/month. Immaterial. | None. |
| **Implementation slips 12 → 20 weeks** | The real risk. Payback moves past 24 months and the deal stops clearing internal hurdle rates. | Productize integration: a reusable elevator/door adapter set plus inherited qualification rules, so site N costs materially less than site 1. The single most important margin lever, and it is not an AI lever. |
| **P3 blocks commercial use of the corpus** | The exchange SKU disappears and implementation cost per site returns to the site-one figure at every site. GM falls toward 45%. | Internal-only product, funded as operations rather than sold. Worth modeling, because it is the scenario in which the business case changes category rather than degrading. |

## Board One-Pager
<!-- Before/After: Old SaaS revenue vs. AI usage revenue for your product -->

**Before:** physical work on an inpatient unit is coordinated by phone and on foot, robot fleets run below capacity because demand never reaches them, and none of it is measured. Cost is absorbed invisibly into nursing hours.
**After:** a metered layer charging per verified completion at ~14.5% of the labor value it returns, with a gross margin near 66% at 30 units in one hospital once implementation is amortized — falling toward break-even at the five-unit minimum — plus a second revenue line that is only available to an operator holding multi-site qualification evidence.
**Net margin shift:** from an unmeasured cost center to a metered service whose margin is set by units per hospital — ~66% at 30, ~7% at 5 — conditional on productized integration before site three and on corpus rights (P3) clearing.
