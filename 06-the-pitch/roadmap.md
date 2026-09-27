# Three-Horizon Roadmap & Board Pitch

## Roadmap

### Horizon 1 — Now (0-3 months)
*Quick wins. Ship with existing capabilities.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| **P1 validation** — corpus coverage inventory by site, task class, date range (week 3) | Coverage spans ≥3 sites and ≥3 task classes, or Data Advantage reverts to 2 | H |
| **P2 validation** — attempt the clinical join on a 5,000-event sample (week 4) | ≥60% join rate, or the moat argument reverts to the pre-premise version | M |
| **P3 validation** — legal review of owned-site, partner-network and vendor telemetry terms (week 6) | Commercial use cleared, or the product is internal-only | M |
| Unified queue + qualified assignment live on one med-surg unit, three task classes, seeded with inherited rules (weeks 0-6) | ≥55% of eligible errands entering the queue by week 8 | H |
| Override capture with four reason codes shipped in v0 (weeks 0-3) | ≥200 new labeled overrides in the first 8 weeks | H |
| **Weekly rule-promotion ritual running** (weeks 4-12) | ≥1 rule promoted through the eval gate per week, sustained 6 weeks | M |
| 120-row golden set sampled from the corpus, gating CI (weeks 2-6) | Every model or adapter change gated, zero exceptions | H |
| Open-RMF fleet adapter proven against the pilot embodiment (weeks 1-4) | One task round-trip through a non-proprietary adapter | M |

### Horizon 2 — Next (3-9 months)
*Bets. Requires new capabilities or integrations.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| **Loop A** — conformance obligation and telemetry rights into the next two fleet agreements; first cross-embodiment benchmark published to contributors | ≥2 vendors under the capability contract; benchmark cited in one procurement decision | M |
| **Loop B** — live clinical event stream feeding demand prediction on the pilot unit | Prediction precision ≥70%; median wait time down ≥25% | L |
| SMART on FHIR inbound adapter — transport orders arrive as requests, verified completions write back | EHR-originated requests ≥30% of queue volume | M |
| Second embodiment from a second vendor through the same capability contract | Cross-vendor assignment with no per-vendor dispatch code | M |
| Cross-domain transfer into sterile processing case-cart flow | ≥40% of med-surg rules reused without rewrite | M |
| Per-site integration productized — reusable adapter set plus inherited rules | Site 3 implementation cost ≤50% of site 1 | L |

**Kill line for H2:** if no fleet vendor accepts the conformance obligation by month 6, Loop A does not exist and the Network score reverts to 1. At that point the commercial thesis fails even though the internal product works, and we should say so rather than continue.

### Horizon 3 — Bet (9-18 months)
*Moonshots. High uncertainty, high potential.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Qualification exchange opened to an external multi-site design partner, with the benchmark as the contributor incentive | A second system reaches ≥90% qualification accuracy in under 30 days from inherited rules | L |
| Incident and near-miss corpus offered as an underwriting base; qualification acquires insurable value | One captive or carrier prices a differential on qualified versus unqualified operation | L |

## AI Evaluation
Composite 10/15, flywheel 13/20 conditional and 5/20 unconditional, 7% margin at five units, no signed counterparties; biggest risk is that the moat depends entirely on signatures nobody has given (mitigation: one LOI and one conformance clause by month 6)

## Board Pitch

**Thesis (1 sentence):**
Nurses on med-surg units spend roughly 11 hours a day per unit walking errands that don't require a nurse, and we can give most of that time back by running one queue that assigns every errand to whichever robot or person is actually qualified for it.

**The case:**

1. Why now: Three things changed inside our own multi-site hospital operations. First, we already run mixed robot and human capacity across sites, and vendors ship new embodiments quarterly, so the fleet is heterogeneous whether or not we manage it deliberately. Second, the Shadow AI audit found five tools, and two of them are this product being hand-run: unit spreadsheets tracking which robot actually works for which errand, and supervisor census sheets forecasting tomorrow's demand. Staff built the qualification corpus and the demand model manually because nothing else exists. Third, two enterprise pilots convert in the coming quarter, which sets the window for negotiating telemetry and conformance rights into the next fleet agreements. Honest caveat for the room: the external market timing case is not in the strategy. The why-now here is internal evidence, strong enough to fund a 12-week test, not strong enough to claim a market window.

2. What's defensible: The position is workflow depth crossed with trust and compliance, and that quadrant is empty. Epic is deep and compliant but robot-blind. Aethon owns hardware integration with no clinical adjudication. A large customer building in-house has scale but no cross-vendor authority. What we own is the assignment decision and the completion proof, conceding the request surface to the EHR. The asset underneath is the vendor qualification corpus: cross-embodiment comparison under identical clinical conditions, which only a multi-site operator running mixed fleets can assemble. The defense against our largest customer going in-house is that the cross-site benchmark is contributor-only, so staying inside the standard beats leaving it. Stated plainly: the flywheel scores 13/20 conditional, 5/20 unconditional, and the weakest loop is Network at 3/5, gated on contract language rather than engineering. The moat depends on signatures nobody has given us.

3. The economics: At a hospital with 30 inpatient units, a $4,000 base plus $0.40 per verified completion across 86,400 completions/month = $38,560/month, ~$463k/year on the runtime, against ~$3.2M/year of returned time. Value capture ~14.5%, leaving enough on the table that the buyer's business case survives a skeptical CFO. 
Inference stays cheap by design — a deterministic solver owns assignment and models only parse intake and predict demand, so 92% of the work never touches a frontier model and frontier pricing could triple without changing the decision. The cost that matters is integration labor. Against $300k per hospital amortized over 36 months, blended margin runs ~7% at 5 instrumented units, ~36% at 10, ~58% at 20, ~66% at 30. Break-even is 5 units, so we productize integration before site three or every new hospital is a services engagement wearing a software label.

**The risks:**

1. Trust / failure modes: The headline risk is motion reported as completion — a STAT specimen or controlled substance marked delivered that never arrived. The answer is structural, not statistical: nothing is verified without a proof event, under any condition. Restricted payloads require human approval regardless of confidence. Above 0.25% unqualified dispatch, autonomous dispatch halts. A 120-row golden set with 30 adversarial cases gates every change.

2. Scale / governance: What breaks at 10x is the correction loop, not cost. Escalation starts near 12% and must reach under 6% by month twelve; flat at scale means we hire reviewers linearly with volume. Stated plainly: promoting a correction into a rule is a weekly human ritual today, not an automated system. Autonomy is bounded by payload class, no agent can promote a rule, and patient transport stays out of scope deliberately.

3. Competitive: The kill scenario is not a faster competitor. It is no fleet vendor accepting the conformance obligation by month 6 — then the commercial thesis fails even though the internal product works. Nearer in, week 8 has hard gates: under 55% queue adoption, under 90% verified completion, under 12 nurse-minutes returned per shift, or a P2 join rate under 60%. Any one stops the bet.

**The ask:**
$400k, 12 weeks, one med-surg unit. Two senior engineers, one product lead, 0.5 clinical lead, 0.5 data engineer. Hard go/no-go at week 8 against the gates above.

What the money buys is not a platform. It buys answers to the three premises the whole strategy rests on: whether the corpus actually spans three sites and three task classes (week 3), whether the clinical join clears 60% on a 5,000-event sample (week 4), and whether legal clears commercial use of owned-site, partner-network and vendor telemetry (week 6). Alongside that: a live unified queue on one unit, 200+ labeled overrides, the golden set gating CI, and one task round-tripped through a non-proprietary fleet adapter. If P3 comes back negative, this is an internal efficiency tool and we should stop calling it a product.

What gets paused if this is funded: all of Horizon 2 waits behind the week-8 gate. No live clinical event stream for demand prediction, no EHR inbound adapter, no second vendor embodiment, no sterile-processing transfer, no integration productization. Horizon 3 does not start at all.

## M1 Baseline vs. Now
*Your 3-sentence AI strategy from Module 1 vs. what you'd say now:*

**M1 baseline:** We have spent years making clinical AI trustworthy and almost none making physical AI trustworthy, and robots are already arriving on our units on separate contracts with no shared queue and no record of what they were asked to do. Our AI strategy for physical work should be to own the layer that decides which performer - robot or person - is qualified for a given job and what proves it finished, because we are the only kind of organization that can make that call and be believed, and because the operational record we already hold is the one thing a robot vendor cannot manufacture. If we do not write that standard for ourselves in the next eighteen months, a platform vendor will write it for us and we will spend the following decade integrating to it.

**Moderator challenge, Module 1:** "How do you measure the outcome?" I could not answer it. Every number I reached for was an output — robots dispatched, queue built, integrations shipped — and none of them said whether anyone was better off. The correction runs through the whole strategy and it is the largest single change between the baseline and now.

**Now:** Our strategy is to own the layer that decides which performer is qualified for each job and what proves it finished, and the outcome we are accountable for is nurse-minutes returned to the bedside, measured as off-unit errand time per nurse per shift before the queue against after it on the same unit, with a target of at least 12 minutes per shift by week 8. Verified completions, queue adoption and fleet utilization are outputs: they tell us the machine is running, and not one of them tells us a nurse got time back, which is why the reliability contract gates on verified completion while the funding case gates on returned minutes. The measurement is also what makes a returned minute countable — it only counts when it is attributable to a task the nurse would otherwise have walked and closed with a valid proof event, which is the reason proof of completion stopped being a feature in this strategy and became the unit we charge for.

