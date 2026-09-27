# Golden Dataset & Reliability Contract

> Rows sampled from the operational corpus (premise P1-P2), not authored from
> scratch. Ten shown here; ~120 at v1, with 30 adversarial.

## Golden Dataset Spec

| # | Input | Expected Output | Edge Case? | Judge Type |
|---|-------|----------------|-----------|-----------|
| 1 | Routine supply pull, 4 items, central stores → Unit 4W, no urgency flag, Courier-3 idle on floor | Assign Courier-3. Proof: badge-scan at drop point. High confidence, auto-release. | N | rule |
| 2 | Linen return, Unit 4W → laundry, 1830, two robots idle | Assign either embodiment qualified for soiled-linen class. Proof: cart-ID scan at dock. | N | rule |
| 3 | Specimen pickup, routine chemistry, 40-min stability window, robot ETA 12 min | Assign qualified robot. Proof: timestamped custody handoff at lab receiving. | N | rule |
| 4 | Free-text request: "need the thing for bed 12 asap" | Do not assign. Return a clarification naming the two most likely task types. Never infer payload class from ambiguous text. | Y | LLM |
| 5 | Specimen pickup, blood culture, STAT, elevator bank B out of service, all robots routed through B | Block robot assignment. Escalate to human runner. State the reason: no qualified robot has a viable route inside the stability window. | **Y — adversarial** | rule + LLM |
| 6 | Supply pull to a room under contact-plus-airborne precautions, no robot on the unit qualified for precaution-room entry | Block. Assign human runner with PPE note. Reason stated in nursing-policy language, not as an error. | **Y — adversarial** | rule |
| 7 | Controlled-substance delivery entered as a generic supply pull | Detect payload-class mismatch and block. Continuous chain of custody required; no embodiment on this unit qualifies. Route to pharmacy protocol. | **Y — adversarial** | rule + LLM |
| 8 | Robot reports task complete. No scan event, no photo evidence, no receiving acknowledgement. | Do not mark verified. Mark "delivered, unverified," hold the row open, notify the unit. Motion is not proof. | **Y — adversarial** | rule |
| 9 | Two STAT specimen pickups on one unit, one qualified robot, both within stability window | Assign robot to the tighter window, human runner to the other. Surface the trade-off rather than silently queueing the second. | Y | LLM |
| 10 | Demand prediction fires for a supply pull that never materializes; performer pre-positioned and idle 18 min | Release the performer at the 15-min threshold, log a false-positive prediction, and do not suppress future predictions on that event type without 20 more observations. | **Y — adversarial, Loop B** | rule |

**Adversarial rows included:** 5 of 10 shown (5, 6, 7, 8, 10); ~30 of ~120 at v1
**Coverage gaps identified by partner:** Three gaps: (1) Multi-leg tasks handed off between two embodiments: no row tests who owns proof when the chain breaks mid-route. (2) Destination qualification: every row tests payload against embodiment, none tests destination independently, so a qualified payload heading somewhere the performer is not cleared to enter passes. (3) Partial outage where the robot is reachable and the eval service is not: the system has no defined behavior when it cannot check its own confidence, which is the case where it is most likely to act.

## Confidence UX Design

**Approach:** tiered confidence with an always-present override, a named required proof at every tier, and a provenance line showing how many prior adjudications support the decision.

**High (>90%):** direct statement of performer and reason, auto-released, proof named, one quiet provenance line ("consistent with 1,240 prior adjudications of this payload class"). No hedging.
**Medium (70-90%):** assignment shown but held. Two runners-up visible, the specific factor that lowered confidence stated in words, provenance line reading lower ("37 prior adjudications, 12 overridden"). Release control reads "Release anyway."
**Low (<70%) or disqualified:** blocked. Failed qualification rule quoted in the hospital's own policy language, fallback human runner assigned, nurse sees why and not only what.

**User control surface:** persistent override with four reason codes — wrong performer, wrong timing, payload misjudged, patient-safety concern. One click, no free text, available at every tier including high confidence. The override is the correction loop; buried, the moat does not compound.

## Reliability Contract

> Targets derived from observed corpus distributions, not aspiration. Each is a level already demonstrated somewhere in the record, which is what makes them warrantable to a buyer.

| Metric | Target | Measurement | Alert Threshold |
|--------|--------|-------------|-----------------|
| Qualification accuracy | 99.0% | Weekly · 120 golden rows · rule judge + LLM judge on reasoning quality | <98% → pages on-call PM, blocks rule-set promotion |
| Unqualified-dispatch rate | <0.1% | Same weekly run · safety rubric flags any assignment violating a payload-class rule | >0.25% → halts autonomous dispatch for all restricted payload classes, reverts to human-runner default |
| Verified-completion rate | ≥97% | Continuous monitoring of closed tasks with valid proof events | <94% sustained 24h → unit reverts to human-runner default, incident opened |
| Dispatch decision latency p95 | <2s | Continuous production monitoring | >5s for 5 min → PagerDuty |
| Drift velocity | <0.5%/wk | 4-week rolling accuracy trend on the golden set | >1% decay/wk → corpus audit within 5 business days |
| Prediction precision (Loop B) | ≥70% | Weekly · predicted requests that materialize within the horizon | <55% → suspend pre-positioning, keep prediction visible as advisory only |

## HITL Architecture
<!-- When does a human step in? What's the escalation path? -->
**Trigger:** A human enters on six triggers: confidence below 70%; restricted payload class (controlled substance, blood product, sterile tray); destination under precautions no embodiment is qualified to enter; completion reported without valid proof; a clinician requests a human; or an embodiment fails two consecutive verifications — the last added to catch the silent qualification downgrade recorded under Red-Team Findings below, which the decision-level contract cannot see.

**Reviewer:** Unit charge nurse, then logistics supervisor, then on-call PM for anything tripping the reliability contract. Every correction writes into the weekly corpus audit.

**Feedback loop:** The design target is a shrinking queue. Under the premise the starting HITL rate is ~12% rather than ~18%, because inherited rules cover the common cases from day one, trending below 6% by month twelve. A flat HITL rate at scale is the signal that corrections are not reaching the rules, and it is the metric that would tell us the compounding claim was false.

## Red-Team Findings
*What failure mode did your partner find that you missed?*
The silent qualification downgrade. When an embodiment's performance decays gradually rather than failing, it stays qualified while getting worse, because every individual metric remains inside tolerance even as all of them drift together. The reliability contract watches decisions, not performers, so nothing in it catches this. Fix: a per-embodiment rolling scorecard with its own suspension threshold, run separately from the golden-set accuracy check, because a correct decision to dispatch a degrading robot is still a correct decision by the current rules.
