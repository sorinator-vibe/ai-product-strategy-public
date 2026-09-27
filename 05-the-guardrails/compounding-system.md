# Compounding System Design

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| Recursive learning | Clinician overrides with reason codes + failed verifications | Updated qualification rules and dispatch priors, promoted weekly through the eval gate | Y | **broken** — signal is captured (premises P1-P2), promotion is not automated |
| **A · Vendor qualification exchange** | Cross-embodiment performance under identical payload, corridor and hour; vendor conformance telemetry | Comparative benchmark for hospitals; failure-envelope knowledge that becomes the qualification standard | Y | **missing** — requires the conformance obligation in fleet contracts |
| **B · Clinical demand prediction** | EHR events: admission, discharge order, case posting, precaution change, lab order | Predicted requests with pre-positioned performers; wait-time reduction that raises queue adoption | Y | **missing** — requires the P2 join operationalized as a live stream, not a historical one |
| Cross-domain transfer | Med-surg payload taxonomy and exception corpus | Seeded qualification rules for sterile processing case-cart flow and pharmacy delivery | Y | active |
| Network intelligence | De-identified qualification adjudications across sites | Cross-site rule library; a new site inherits what an established site has settled. | Y | **broken** — data is collected, value is not redistributed to contributors |

**Broken loop identified by partner:** Network intelligence is broken more deeply than the table admits: the loop assumes contributors submit telemetry in exchange for a benchmark, but the first contributor receives nothing, because there is nobody to be benchmarked against. A network effect with no cold start is not a loop, it is a hope. Fix: seed the exchange with our own multi-site data so member one gets a real benchmark on day one rather than a promise about member two.

**Fix plan:** Recursive learning is the prerequisite for everything else and the fix is a ritual rather than a model. Overrides accumulate, a named owner reviews them against the golden set, rules that clear the eval gate promote on Fridays. Without that ritual the corpus is a log file. Loop A is a contracting task, not an engineering one: the conformance obligation goes into the next two fleet agreements or the loop never starts. Loop B is the only one needing new
engineering, and it needs the clinical event stream live rather than historical.

**Freeze test:** Frozen three months, the answer is mixed and the mix is the finding. The qualification rules stay accurate, because clinical policy and payload taxonomy change slowly, so the product keeps returning nurse-minutes. Two things degrade. Fleet coverage degrades, because new embodiments ship quarterly and an unqualified robot is an idle robot. Prediction degrades, because census and case mix shift. So the product compounds on rules and decays on coverage, which means the qualification exchange is not a nice-to-have — it is the mechanism that keeps the frozen product from aging.

## Context Connectivity
<!-- How does knowledge flow across teams and domains? Where does it silo? -->
The break sits between clinical policy and operations. Infection control writes precaution rules, pharmacy writes chain-of-custody rules, logistics builds routes, and none of the three sees the others' exceptions. A charge nurse's override reaches nobody who could change a rule. The qualification corpus is the connective tissue: one place where a clinical rule, an operational exception and a robot capability meet in the same row. Under the premise that place exists; what is missing is the path from a row to a changed rule.

## Governance Policy

**Scope:** autonomous and semi-autonomous assignment of physical work requests on inpatient medical-surgical units, covering supply, specimen and linen/waste task classes, plus the demand-prediction service that pre-positions performers. Excludes patient transport, medication and controlled-substance delivery, and all OR and sterile-processing flows, each under its own existing protocol.

**Autonomy boundaries:** Proposing an assignment — auto. Releasing to a qualified robot for an unrestricted payload class above 90% confidence — auto. Releasing for any restricted payload class — human approval required regardless of confidence. Entering a patient room under precautions — never auto. Marking a task verified without a valid proof event — never, under any condition. Pre-positioning a performer on a predicted request — auto, with a 15-minute release threshold, and never at the cost of an actual queued request.

**Escalation triggers:** (1) confidence below 70%; (2) restricted payload class; (3) destination under precautions no embodiment is qualified to enter; (4) completion reported without valid proof; (5) clinician requests a human; (6) any embodiment fails two consecutive verifications.

**Audit cadence:** Weekly — automated eval against the golden set, owner: product lead. Monthly — human review of 50 sampled dispatch decisions plus every escalation, owner: nursing operations with infection prevention. Quarterly — full policy review with security, legal and clinical practice, CNO sign-off.

**Regulatory exposure:** EU AI Act — logistics dispatch on unrestricted payload classes reads as limited risk; extension into patient transport would likely move it toward high-risk as a safety component, and that boundary should be defended deliberately rather than crossed by roadmap drift. 
HIPAA — task metadata linking a specimen to an encounter is PHI, and the demand-prediction service consumes clinical events directly, so it sits inside the covered environment with rules and outcomes crossing to the cross-site library while identifiers never do. 
Safety standards — ISO 13482 applies to the embodiments rather than to this layer, and the capability contract must require conformance evidence as a condition of qualification. SOC 2 controls apply to log retention.
Data rights — premise P3 governs whether any of the above can be commercialized.

## Agent Topology
<!-- If using agents: what can each agent do? What can't it do? Who approves what? -->
**Intake agent** — parses free-text into a structured task and asks a clarifying question. Cannot infer payload class from ambiguous text, cannot assign.
**Prediction agent** — proposes a pre-position on a predicted request. Cannot displace an actual queued request, cannot hold a performer beyond 15 minutes.
**Arbitration agent** — proposes, and for unrestricted classes at high confidence releases, an assignment. Cannot release for restricted classes, cannot override a qualification rule, cannot modify the rule set.
**Verification agent** — evaluates completion evidence and marks verified or unverified. Cannot mark verified without a proof event, cannot close a disputed task.
**Rule-promotion agent** — drafts a proposed rule change from accumulated overrides. Cannot promote it. A named human owner promotes weekly, after the eval gate passes.

Chain ownership: if verification fails on arbitration's output, the arbitration owner holds the handoff. Any agent failing its eval stops the chain for its task class rather than degrading quietly.

## Shadow AI Audit

*User-side: what are our users building with AI around the product?*

| Tool | Owner | Risk Level | Decision |
|------|-------|-----------|----------|
| Unit spreadsheets tracking which robot "actually works" for which errand | Charge nurses, informally | M | **build** — this is the qualification corpus, hand-maintained. Absorb it; it is the clearest capability-gap signal available |
| Group-chat threads coordinating runner handoffs outside any system | Unit staff | M | **build** — workflow gap. The handoff is a task class we do not model |
| Vendor fleet dashboards opened side by side to guess availability | Logistics supervisors | L | **partner** — availability should arrive through the capability contract, not through a human reading three dashboards |
| Supervisors building their own census-based staffing forecasts in a spreadsheet | Unit supervisors | M | **build** — this is Loop B being hand-run. That it exists is evidence the prediction has demand; that it is manual is the opportunity |
| Staff pasting incident narratives into consumer AI tools to draft reports | Individual staff | H | **govern** — CISO-side exposure rather than a product decision, but incident narratives contain PHI and it belongs on the register |


**Total tools found:** 5 categories at the pilot unit
**Tools after triage:** 3 absorbed, 1 partnered, 1 escalated
**Estimated hidden spend:** minimal in license cost, material in time - the spreadsheet and dashboard-watching behaviors consume roughly 20-30 supervisor minutes per shift that the product should eliminate
