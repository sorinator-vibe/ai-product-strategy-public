# Three-Axis Vulnerability Diagnostic

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

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Motion Layer — a vendor-neutral control plane that arbitrates physical work requests across mixed robot and human capacity on inpatient units, and verifies completion against a clinical qualification standard.
**Your Role: Product Leader**

*Scored under premises P1-P3 (see README). Unconditional scores in parentheses.*

---

### Contextual Moat — 3/5 (2/5 absent P1)
*Workflow depth x switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:** Under P1, operations are already instrumented across sites, which means live feeds, running integrations and a cross-site benchmark a unit gives up by leaving. That is real switching cost and it is why the score is not a 2. It is capped at 3 because the request surface still belongs to someone else: Epic Grand Central runs transport coordination, bed planning and housekeeping dispatch, and third parties already embed dispatch through SMART on FHIR. Workflow depth sits at "embedded feature," one rung below "workflow layer." The layer reaches depth when nursing policy points at the qualification standard, which is a twelve-month move.

**Named attacker:** Epic Systems (Grand Central).
https://www.epic.com/software/patient-flow/ - owns the transport order, the location record and the ADT event stream. Adding robot performers to an existing queue is a module extension for them and a platform build for us.

---

### Data Advantage — 4/5 (2/5 absent P1-P3)
*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:** The asset is the P2 join, not the volume. Fleet vendors hold motion: where a robot went, how long it took, whether it errored. The corpus holds motion joined to clinical judgment — payload class, precaution status, whether the clinician accepted the handoff, what happened next. That join cannot be backfilled, because the clinical context is not recoverable from telemetry after the fact. Two loops compound on it and neither is assemblable by a competitor: the vendor qualification exchange (cross-embodiment comparison under identical conditions, which only a multi-site operator running mixed fleets can produce) and clinical demand prediction (physical work forecast from EHR events, which requires both streams in one system).

5 means automated retraining, and a stock of history is not a retraining loop until the weekly promotion ritual runs. The corpus is a stock today; the flywheel is the claim being tested.

**Named attacker:** ST Engineering Aethon. https://aethon.com/ — TUG robots in 500+ hospitals generate more trip telemetry every week than any archive holds. Their gap is the clinical join, not volume, and they could close it by partnering with any large system. The honest framing is that our data decays more slowly than theirs grows, not that we have more of it.

---

### Platform Exposure — 3/5 (2/5 absent P1)
*Encroachment risk x pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:** The routing half is being commoditized in public. Open-RMF was built to coordinate multi-vendor hospital fleets with elevators and doors and is free. VDA 5050 standardizes the fleet interface. MassRobotics covers status exchange. NVIDIA's Project Rheo is assembling the simulation, policy and partner stack for hospital automation, and a free reference dispatcher from that direction removes our routing differentiation in a quarter.

The premise raises the score from 2 to 3 for a specific reason. Rheo's stated bottleneck is the data gap in healthcare robotics, which is why Apian is building NHS hospital digital twins on it. Holding characterized real environments and real exception scenarios makes us a supplier into that stack rather than only a target of it. Pivot speed helps too: the qualification layer is policy, so it survives the runtime being replaced.

**Named attacker:** NVIDIA, Project Rheo within Isaac for Healthcare. https://developer.nvidia.com/isaac/healthcare

---

## Top Vulnerability
<!-- One line: what's the single biggest strategic risk? -->
The corpus is a stock and the flywheel is a claim - if the weekly promotion ritual that turns exceptions into rules does not ship, a competitor with more raw telemetry overtakes us on the only axis where we currently lead.

## Confidence Level
<!-- H / M / L — how confident are you in this bet after the diagnostic? -->

H, conditional on P1-P3 clearing their Horizon 1 validation gates. Reverts to M if P2 fails, and to L if P3 blocks commercial use, because at that point the strategy is an internal operations program rather than a product.
