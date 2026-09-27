# Data Flywheel Map

> Score each loop 1-5. Your weakest loop is where competitors attack first.
> The four loops below are the M2 starting point - adapt if your product has 2 or 6 loops instead of 4.
> Scored under premises P1-P3 (see README). Unconditional scores in parentheses.

## Flywheel Loops

| Loop | What It Measures | Score 1 | Score 5 | Score | Target @12mo |
|------|------------------|---------|---------|-------|--------------|
| **Correction** | Do users fix AI outputs? Is that signal captured and reused? | No capture | Automated retraining | 3/5 (1) | 4/5 |
| **Preference** | Does the product learn individual / team preferences over time? | Stateless | Deep personalization | 3/5 (1) | 4/5 |
| **Domain Context** | Does usage in one area improve quality in adjacent areas? | Siloed | Cross-domain transfer | 4/5 (2) | 5/5 |
| **Network** | Does each new user / team make the product better for everyone? | Isolated | Strong network effects | 3/5 (1) | 5/5 |

### Correction Loop - 3/5
**What you capture today:** Under P1-P2, clinician overrides and failed handoffs exist as labeled history joined to payload class, precaution status and outcome. The reason a robot was rejected is recoverable, which is the thing fleet vendors cannot recover from their own telemetry.

**How it compounds:** Each override is a labeled row in the qualification corpus. Not a 4, because capture is not promotion - until the weekly ritual runs and rules move through the eval gate, this is a stock rather than a loop. The fix is a ritual, not a model, and it is Horizon 1 work.

### Preference Loop - 3/5
**What you capture today:** Multi-site history carries unit-level and role-level conventions: which floors stage linen before 0600, which charge nurses never release a specimen to a robot during shift change. Those patterns are present in the record even where nobody wrote them down as rules.

**How it compounds:** Per-unit and per-role dispatch priors. A unit six months into the queue gets better assignments than a unit on day one, which is what makes removing the layer expensive.

### Domain Context Loop - 4/5
**What you capture today:** The corpus spans med-surg, sterile processing, pharmacy and lab on a shared payload taxonomy, alongside the written clinical policy - infection-control precautions, chain-of-custody requirements, specimen handling windows - that defines a correct dispatch. This is the highest loop and it is why the premise matters: the same rule set governs multiple domains.

**How it compounds:** Rules written for med-surg supply transport transfer directly into sterile processing case-cart flow and pharmacy delivery. One domain's exception corpus tightens the adjacent domain's rules. Reaching 5 needs demonstrated transfer, measured as the share of rules reused without rewrite.

### Network Loop - 3/5
**What you capture today:** Multi-site history across owned hospitals, with the partner network as a consented extension path.

**How it compounds:** Two mechanisms, and they are the core of this strategy.

**(A) Vendor qualification exchange.** Running mixed fleets across many sites produces the one dataset no vendor can assemble: how embodiment X performs against embodiment Y on the same payload class, in the same corridor, at the same hour. To be qualified, a vendor implements the capability contract and submits conformance telemetry; in return it receives a benchmark showing where it loses. Each vendor makes the benchmark more useful to hospitals; each hospital makes qualification more valuable to vendors. Two-sided, and the precedent is our existing validation program for clinical models, which does exactly this.

**(B) Clinical demand prediction.** Physical work is caused by clinical events - admission, discharge order, case posting, precaution change, lab order. With the P2 join, the request is predicted before it is made and the performer pre-positioned. Census forecasting from EHR data is established practice; nobody has connected it to robot pre-positioning, because that needs both streams in one system. The loop tightens: better prediction cuts wait time, lower wait time raises adoption, higher adoption produces more predicted-versus-actual pairs.

Not higher than 3 today because members must receive value from each other's data for network effects to exist, and that needs the federated agreement live.

**Total Flywheel Score: 13/20 (5/20 unconditional)**
**Weakest Loop:** Network. It is the loop that determines whether the commercial product works at all, and it is gated on legal terms rather than engineering.
**Fix for weakest loop:** Stand up the qualification exchange as a contractual instrument before the second site, not after the tenth. Two terms carry it: a conformance obligation in every fleet agreement, and a contributor-only benchmark that members see when they submit and lose when they stop.

---

## Encroachment Threat Assessment

### 1. Platform Encroachment
**Attacker:** NVIDIA — Project Rheo within Isaac for Healthcare
**Vector:** Ships a reference multi-fleet dispatcher alongside the hospital digital twin and GR00T policy stack. Free, bundled with tooling robot vendors already need, distributed through an existing partner network (Apian on NHS digital twins, Proximie in the OR).
**Time-to-threat:** 9-15 months
**% of value at risk:** 40% — the routing and arbitration half. Lower than the unconditional case because the corpus gives us a supply relationship into their stated data gap rather than only a defensive position.

### 2. Vertical Competitor
**Attacker:** ST Engineering Aethon
**Vector:** Opens the TUG fleet manager to third-party robots and rebrands it as neutral orchestration. They run a 24/7 fleet help desk, own elevator integration hardware and software through ReadyElevator, and have twenty years of architect relationships at schematic design.
**Time-to-threat:** 6-12 months
**% of value at risk:** 55% — they can take the whole layer wherever they are already installed, which is 500+ hospitals.

### 3. Adjacent Expansion
**Attacker:** Epic Systems
**Vector:** Extends Grand Central transport coordination to accept robot performers. Epic owns the request surface, the ADT stream and the location record, and third party dispatch already embeds via SMART on FHIR.
**Time-to-threat:** 12-24 months
**% of value at risk:** 70% — highest, because Epic does not need to win on quality. It needs to be adequate and already installed.

### 4. Customer as Competitor
**Attacker:** a large multi-site customer building in-house
**Vector:** a system on the order of 200 hospitals and a few thousand care sites, with an established hyperscaler partnership, an EHR replacement in progress, and an innovation group that has already shipped a generative-AI clinical tool. Within a year or two of going live they out-generate our operational data and can rebuild the layer in-house.
**Time-to-threat:** 18-30 months
**% of value at risk:** 60% of the commercial opportunity, none of the internal one.
**Why this vector exists only under the premise:** when the moat is a data corpus rather than a workflow, the largest data generator is the natural successor. This is the cost of the premise and it belongs in the assessment, not in a footnote.

---

## 90-Day Encroachment Plan

*Your partner played the Big Tech attacker. What was their plan to kill you?*

**Attacker:** Epic Systems
**Attack vector (target the weakest loop):** Network, at 3/5. Our cross-site value is real but not yet contractual, so there is little to lose by switching. Epic attacks by making dispatch a field in a screen clinicians already open thirty times a shift.
**Weeks 1-4 — what they ship:** A robot-performer type in Grand Central transport with a generic VDA 5050 adapter. Not good. Present.
**Weeks 5-8 — how they poach users:** No net-new vendor, no new login, no new security review, no incremental contract. The charge nurse never leaves the chart. We lose on friction before anyone evaluates quality.
**Weeks 9-12 — why users don't come back:** Transport status writes back into the patient record natively. Once the audit trail lives in Epic, moving it is a compliance project rather than a product decision.
**Your defense:** Do not fight for the request surface. Publish the qualification contract as the thing Epic dispatches *against*, and integrate so a Grand Central transport order arrives as a request into our arbitration layer and returns a verified completion. Epic keeps the front door; we keep the decision and the proof. Concretely in the same 90 days: ship the SMART on FHIR inbound adapter and completion write-back, and get the conformance obligation into the next two fleet contracts so the qualification exchange has legal teeth before Epic's version arrives.
