# Kill Switch Audit

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | 48-Hour Action |
|-----------|--------------|------------|---------------|
| **Provider** | Three dependencies. Model: frontier provider via the enterprise cloud footing, used for exception triage and intake only. Fleet: each vendor's proprietary fleet manager, with elevator and door integration bought per vendor. Corpus rights: telemetry clauses in active fleet agreements may claim our operational record for vendor model training (premise P3). | M (model) / H (fleet) / H (rights) | Model: nothing needed in 48h; usage is thin and non-critical by design. Fleet: stand up an Open-RMF adapter against the pilot embodiment and prove a task round-trip by Friday. Rights: pull every active fleet agreement and extract the telemetry clause into one table. That table is the P3 validation and it is a legal task, not an engineering one. |
| **Abstraction** | Prototype code calls the fleet vendor API and the model provider SDK directly. | H | `motion_gateway.reason()` as the single entry point for every model call; the capability contract as the only interface a fleet adapter may implement. No vendor SDK import outside those two modules, enforced by a CI lint rule rather than a convention. |
| **Routing** | One path proposes everything. No tiering between deterministic assignment and model-assisted triage. | H | Split the decision. A deterministic constraint solver owns performer assignment, because it is a scheduling problem and an LLM is the wrong instrument. Models handle intake parsing, exception explanation and demand prediction only. Ship a rules-only degraded mode that keeps the queue alive with zero model calls and make it the default failover. |
| **Eval** | Golden set can now be sampled from the corpus rather than hand-written, which changes the timeline from weeks to days. | M | Freeze a 120-row golden set sampled from real adjudications, with 30 adversarial rows drawn from actual exception history. Gate every model swap, prompt change and fleet-adapter change in CI. Under the premise this ships before the second unit rather than before the second site. |

## Portability Score
<!-- Ready / Partial / Locked -->

**Partial.** Model portability reaches Ready within two weeks because the model surface is deliberately small. Fleet portability is Locked today and moves to Partial once one Open-RMF adapter is live. Corpus rights are Unknown until P3 validation completes in week 6, and Unknown is the honest entry - for the primary asset in the strategy, that is the most important cell in this table.


## If [primary vendor] doubles pricing tomorrow:
<!-- What's your 48-hour response? -->

Model provider: negligible. Inference is roughly 11% of per-unit AI COGS because assignment is deterministic. Doubling the frontier price moves total COGS about $19 per unit per month against ~$8,850 of monthly value delivered.

Fleet vendor: material and not escapable in 48 hours, because the robot is in the building and the elevator integration is theirs. The response is commercial: invoke the capability-contract clause requiring a compliant adapter and route new task volume to the qualified alternative embodiment while the contract is renegotiated. This is the argument for never signing a fleet agreement without both the adapter obligation and the telemetry-rights clause in it.

## If [primary vendor] ships a competing product:
<!-- What's defensible that they can't replicate? -->

If NVIDIA bundles a reference dispatcher into Isaac for Healthcare, routing differentiation is gone in a quarter. What survives is the qualification corpus and the authority behind it. A platform vendor can compute the optimal assignment; it cannot state that an embodiment is clinically cleared to carry a payload class on an inpatient unit, because that statement carries liability and requires an operator to make it.

Under the premise there is a second response available. Rheo's constraint is the data gap in healthcare robotics - Apian is building NHS digital twins precisely to fill it. Characterized real environments and real exception scenarios are scarce inputs, which makes a supply relationship possible on terms rather than a retreat. The pivot, if it comes, is to run the qualification and verification layer on top of their runtime and contribute environments into it, which is a worse business than owning the stack and a considerably better one than being a feature.
