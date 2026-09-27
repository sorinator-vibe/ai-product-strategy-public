# My AI Product Strategy

> A living strategy built across 6 sessions. Each module adds one component. By Module 6, this repo IS your strategy — version-controlled, board-ready, portable.

---

## Premise

This strategy is scored under three stated assumptions. Each is validated or falsified in Horizon 1. Scores carry both values: conditional, and unconditional
in parentheses.

**P1 · Coverage.** An operational corpus of physical-work task events exists across multiple owned sites and several task classes, over a period long enough
to characterize exception behavior rather than sample it. 
*Validation:* week 3 — inventory task-event coverage by site, task class and date range. 
*Kill line:* if coverage is confined to one campus or one task class, nothing generalizes and Data Advantage reverts to 2/5.

**P2 · Joinability.** Each operational event is linked to clinical context: payload class, precaution status, clinician acceptance or rejection, and downstream outcome.
*Validation:* week 4 — attempt the join on a 5,000-event sample and report the match rate. 
*Kill line:* below 60% joinable, the corpus is logistics telemetry and is not materially different from what fleet vendors already hold. Data
Advantage reverts to 2/5 and the entire moat argument reverts to v1.

**P3 · Rights.** The corpus can lawfully be used to build and commercialize a product, including partner-site contributions and vendor-generated telemetry.
*Validation:* week 6 — legal review of owned-site data use, partner-network participation terms, and the telemetry clauses in every active fleet agreement.
*Kill line:* if vendor agreements claim exclusive telemetry rights over a material share of the corpus, the commercial product is blocked and the internal product proceeds alone.

**Known weakest leg:** P3. Network members are independently owned, and fleet vendor agreements commonly claim operational telemetry for their own model training. Any corpus assembled before that clause was negotiated may be encumbered. This is the single assumption most likely to be false and it is named here rather than buried.

---

## Strategy at a Glance

| Component | Module | Status | Key Artifact |
|-----------|--------|--------|-------------|
| **The Bet** | M1 | [x] | `01-the-bet/` |
| **The Moat** | M2 | [x] | `02-the-moat/` |
| **The Margin** | M3 | [x] | `03-the-margin/` |
| **The Contract** | M4 | [x] | `04-the-contract/` |
| **The Guardrails** | M5 | [x] | `05-the-guardrails/` |
| **The Pitch** | M6 | [x] | `06-the-pitch/` |

---

## The Bet (M1)

**What we're building, for whom, why now.**

- **Product:** Motion Layer — vendor-neutral control plane arbitrating physical work across mixed robot and human capacity on inpatient units
- **AI Value Archetype:** Orchestrator (dominant) / Oracle (secondary)
- **Vulnerability Scores:** Moat 3/5 · Data 4/5 · Platform 3/5 (composite 10/15; 6/15 absent premises P1-P3)
- **Top Risk:** the corpus is a stock and the flywheel is a claim — without the weekly promotion ritual, a competitor with more raw telemetry overtakes us
- **Confidence:** H, conditional on P1-P3 clearing Horizon 1 gates
- **Prototype:** [Dispatch console — 32-bed med-surg unit](https://claude.ai/artifact/S4G8JEmjRB3c84dkFbAiKv)
- **Kill Criteria:** <55% queue adoption · <90% verified completion · <12 nurse-minutes returned per shift · <200 new overrides or zero rules promoted, all at 8 weeks. P2 join rate below 60% stops the bet regardless.

→ Details: [`01-the-bet/`](01-the-bet/)

---

## The Moat (M2)

**Why this won't get copied in 6 months.**

- **Data Flywheel Score:** 13/20 (5/20 unconditional)
- **Weakest Loop:** Network (3/5) — gated on contract terms rather than engineering, and it decides whether the commercial product exists
- **Core loops:** (A) vendor qualification exchange - cross-embodiment comparison under identical conditions, assemblable only by a multi-site operator running mixed fleets; (B) clinical demand prediction — physical work forecast from EHR events, requiring both streams in one system
- **Competitive Position:** Workflow Depth x Trust/Compliance. Epic is deep and compliant but robot-blind; Aethon owns hardware integration without clinical adjudication; a large customer building in-house has scale without authority. The high/high quadrant is empty.
- **Encroachment Defense:** concede the request surface to the EHR, integrate via SMART on FHIR, hold the assignment decision and the completion proof, and make the cross-site benchmark contributor-only so the largest customer prefers to stay inside the standard
- **Vendor Portability:** Partial — models Ready in two weeks, fleets Locked until the first Open-RMF adapter ships, corpus rights Unknown until P3 completes

→ Details: [`02-the-moat/`](02-the-moat/)

---

## The Margin (M3)

**Will this make money or bleed it?**

- **Gross Margin (current):** not applicable, 0-to-1. The baseline is an unmeasured cost absorbed invisibly into nursing hours — roughly 11.2 nurse-hours per unit per day spent on off-unit errands.
- **Gross Margin (AI-adjusted):** ~87% against AI COGS alone — $153 COGS against $1,152 of usage revenue per unit per month. (The ~$8,850 of monthly value delivered is the customer's benefit, not our revenue, and does not set margin.) Blended margin, once per-hospital implementation is amortized over 36 months at a $300k planning figure, is set by density rather than by inference — see below. The finding is unchanged: the margin problem is integration labor.
- **Pricing Model:** two SKUs. *Runtime* — $4,000 per hospital per month base plus $0.40 per verified completion, so the unit of charge is a closed loop rather than a dispatch. *Qualification exchange* — annual subscription priced on bed count, granting the inherited rule library, the cross-site benchmark and vendor conformance reporting, available to contributors only. That contributor condition is the pricing expression of the Network loop and what makes Loop A commercially self-enforcing.
- **Cascading Strategy:** a deterministic constraint solver owns performer assignment — it is a scheduling problem and an LLM is the wrong instrument. Models handle intake parsing, exception explanation and demand prediction only. Expected split ~92% deterministic or small-model, ~8% frontier.
- **Break-even at:** ~5 instrumented units per hospital. Monthly contribution is $4,000 base plus ~$999 per unit against ~$8,333 of amortized implementation. Below five units a site is a services engagement rather than a software deployment, which is the argument for productizing integration before site three.
- **Margin by density:** ~7% at 5 units per hospital, ~36% at 10, ~58% at 20, ~66% at 30. Implementation lands per hospital, so units per hospital — not total unit count — is the margin variable, and the 30-unit case is a ~960-bed facility rather than the representative one. Worth reading against the line above: break-even clears implementation, but a healthy margin sits ~25 units beyond it.
- **Value capture:** ~14.5% of the labor value returned i.e. ~$463k annual runtime revenue against ~$3.2M returned across 30 units, deliberately leaving enough on the table that the buyer's business case survives a skeptical CFO.

→ Details: [`03-the-margin/`](03-the-margin/)

---

## The Contract (M4)

**Why users will trust a probabilistic system.**

- **Reliability Target:** 99.0% qualification accuracy · <0.1% unqualified-dispatch rate · ≥97% verified-completion rate · p95 dispatch latency <2s · drift velocity <0.5%/week · ≥70% prediction precision on Loop B. Targets are derived from observed corpus distributions rather than aspiration, which is what makes them warrantable to a buyer.
- **Golden Dataset:** ~120 rows, ~30 adversarial, sampled from real adjudications rather than authored. Adversarial coverage centers on the cases carrying patient risk: STAT specimen with the elevator bank down, precaution-room entry by an unqualified embodiment, controlled substance entered as a generic supply pull, completion reported without a proof event, and a prediction that fires and never materializes.
- **Confidence UX:** three tiers, each naming the required proof. High confidence auto-releases with one quiet provenance line stating how many prior adjudications support it. Medium holds for release, shows the two runners-up and names the factor that lowered confidence. Low or disqualified blocks and quotes the failed qualification rule in the hospital's own policy language. A four-code override sits at every tier including high — buried, the correction loop does not compound.
- **HITL Architecture:** six escalation triggers — confidence below 70%, restricted payload class, destination under precautions no embodiment is qualified to enter, completion without valid proof, a clinician request, or an embodiment failing two consecutive verifications. The sixth closes the red-team finding on silent qualification downgrade: the contract watches decisions, so a degrading performer needs its own trigger. Path runs charge nurse → logistics supervisor → on-call PM. Starting rate ~12% under inherited rules, target below 6% by month twelve. A flat rate at scale is the signal that corrections are not reaching the rules.
- **Failure Mode Coverage:** the governing one is motion reported as completion. Nothing is marked verified without a valid proof event, under any condition, regardless of what the robot reports. Above a 0.25% unqualified-dispatch rate, autonomous dispatch halts for all restricted payload classes.

→ Details: [`04-the-contract/`](04-the-contract/)

---

## The Guardrails (M5)

**What breaks when this scales — and what compounds.**

- **Compounding System:** five loops. *Recursive learning* - broken; signal is captured under P1-P2 but promotion is not automated, and the fix is a weekly ritual rather than a model. *Loop A, vendor qualification exchange* - missing; it is a contracting task, and the conformance obligation goes into the next two fleet agreements or the loop never starts. *Loop B, clinical demand prediction*
  — missing; the only loop needing new engineering, and it needs the clinical event stream live rather than historical. *Cross-domain transfer* — active on a shared payload taxonomy. *Network intelligence* — broken; data is collected, value is not yet redistributed to contributors.
- **Freeze test:** frozen three months, the answer is mixed and the mix is the finding. Qualification rules stay accurate because clinical policy changes slowly, so the product keeps returning nurse-minutes. Fleet coverage decays, because new embodiments ship quarterly and an unqualified robot is an idle one. Prediction decays as census and case mix shift. The product compounds on rules and decays on coverage, which is why the qualification exchange is load-bearing rather than an extra.
- **Governance Posture:** autonomy bounded by payload class, not by confidence alone. Proposing is always automatic. Releasing to a qualified robot for an unrestricted class above 90% confidence is automatic. Releasing for any restricted class requires human approval regardless of confidence. Entering a patient room under precautions is never automatic. Marking a task verified without proof is never permitted. Audit runs weekly against the golden set, monthly over 50 sampled decisions with nursing operations and infection prevention, quarterly with security, legal and clinical practice under CNO sign-off.
- **Shadow AI Status:** 5 tools found, 5 triaged — 3 build, 1 partner, 1 govern. Two are the product being hand-run: unit spreadsheets tracking which robot actually works for which errand (the qualification corpus, maintained by hand) and supervisor census spreadsheets forecasting demand (Loop B, maintained by hand). That both exist is evidence of demand; that both are manual is the opportunity.
- **Agent Boundaries:** five agents with explicit limits. Intake parses and clarifies, and never infers payload class from ambiguous text. Prediction pre-positions, and never displaces a real queued request or holds a performer past 15 minutes. Arbitration proposes and releases for unrestricted classes, and never modifies the rule set. Verification marks verified or unverified, and never closes a disputed task. Rule-promotion drafts a proposed rule change and never promotes it — a named human owner promotes weekly after the eval gate passes.
- **Regulatory Exposure:** EU AI Act — limited risk at current scope; extending into patient transport would likely move it toward high-risk as a safety component, named here as a deliberate boundary rather than a roadmap accident. HIPAA — task metadata linking a specimen to an encounter is PHI, and the prediction service consumes clinical events directly, so both sit inside the covered environment with rules and outcomes crossing to the cross-site library while identifiers never do. ISO 13482 — applies to the embodiments, and the capability contract requires conformance evidence as a condition of qualification. SOC 2 — log retention. Premise P3 governs whether any of it can be commercialized.

→ Details: [`05-the-guardrails/`](05-the-guardrails/)

---

## The Pitch (M6)

**How you get this funded, shipped, and adopted.**

- **Horizon 1 (Now, 0-3 months):** validate P1, P2 and P3 at weeks 3, 4 and 6. Stand up the unified queue and qualified assignment on one med-surg unit, seeded with inherited rules. Ship override capture with four reason codes in v0. Get the weekly rule-promotion ritual running and sustained six weeks. Gate CI on a 120-row golden set sampled from the corpus. Prove one task round-trip through an Open-RMF adapter.
- **Horizon 2 (Next, 3-9 months):** Loop A — conformance obligation and telemetry rights into the next two fleet agreements, first cross-embodiment benchmark published to contributors. Loop B — live clinical event stream feeding demand prediction. SMART on FHIR inbound adapter with completion write-back. Second vendor embodiment through the same capability contract. Cross-domain transfer into sterile processing case-cart flow. *Kill line:* if no fleet vendor accepts the conformance obligation by month 6, Loop A does not exist, Network reverts to 1, and the commercial thesis fails even where the internal product works.
- **Horizon 3 (Bet, 9-18 months):** open the qualification exchange to an external multi-site design partner, with the benchmark as the contributor incentive and a second system reaching ≥90% qualification accuracy in under 30 days from inherited rules. Offer the incident and near-miss corpus as an underwriting base, so qualification acquires insurable value and the buyer shifts from the CNO to the CFO.
- **Board Narrative:** hospitals are buying their second and third robot fleet before anyone owns the decision of which performer is qualified for which job and what proves it finished, and an operator holding a clinically joined record of that work is the only kind of organization that can write the standard and be believed.
- **Key Metric:** nurse-minutes returned per shift per unit, target ≥12 at week 8. The leading indicator behind it is rules promoted through the eval gate per week, which is the number that separates a corpus from a flywheel and the one that would tell us the Data Advantage score was wrong.
- **Leadership pitch deck:** [view the deck](https://sorinator-vibe.github.io/ai-product-strategy-public/leadership-pitch-deck.html) — eight slides, arrow keys or scroll to navigate.

→ Details: [`06-the-pitch/`](06-the-pitch/)
