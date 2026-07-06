# RTO/RPO targets, controls & testing

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Set recovery targets, build the technical controls to meet them, and *test* them. Decide how fast the system must recover (Recovery Time Objective, RTO) and how much data the organization can afford to lose (Recovery Point Objective, RPO), and differentiate those targets rather than applying one number everywhere. Differentiate by **component tier** — customer-facing inference may need to recover in minutes, while internal analytics may tolerate hours — and by **AI asset type**, since training data, model state, feature stores, and inference logs each carry different recovery requirements.

## Why it sits at this level

This is L3 because it is more than a plan on paper: it requires building the technical controls that actually meet defined targets and then testing that they work under realistic conditions. Setting targets is the crawl step, while establishing and *verifying* the recovery machinery is what puts this control at the advanced level.

## What implementing it looks like

Start from a business-impact analysis that drives the RTO/RPO numbers and documents system dependencies, then set differentiated targets by component tier and by AI asset type. Establish the technical controls that meet them — verified clean recovery points for the data and model state that matter, with mechanisms such as model-checkpoint persistence or cross-region replication of feature stores and vector databases serving as examples of how a target is met where relevant. Maintain a disaster-recovery plan; it is something an organization should have, not a separate control.

**Restore-after-corruption is an RPO problem that lands here.** Failover to a standby replicates corruption faithfully — if an agent, an attacker, or a bad update corrupts the data or system, a warm standby simply mirrors the corrupted state. Restoring the data or system to a known-good point after corruption is therefore a recovery-point problem rather than a failover one, and it needs *verified clean* recovery points to restore from.

**Testing is the point.** Untested plans fail. A large share of outages stem from procedural failures, and a recovery target "exists only on paper if it is never executed under realistic conditions." Test recovery, validate that the recovery points are clean, and account for the security wrinkle: a compromised recovery — a poisoned backup or compromised credentials — extends the effective RTO, because the team must first verify it is restoring from a clean point before it can trust the restore.

## Source grounding

- **Primary —** **NIST SP 800-34** — the business-impact analysis drives RTO/RPO and documents system dependencies. **NIST SP 800-53 CP-2 / CP-4** — the contingency plan and its testing, scaled by impact level.
- **Supporting —** **ISO 22301** — business-continuity management.
- *AI-specific slice:* differentiated RPO across AI asset types, and restore-after-agent-corruption.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Tiered RTO/RPO, differentiated RPO, and restore-after-corruption are one control here, not three — set targets, build controls to meet them, and test them, with the granular items as examples of differentiating those targets. Watch the restore-after-corruption case in particular: it is called out explicitly rather than left implicit, because failover replicates corruption faithfully and this recovery-point problem would otherwise have no home in your plan.

You will not find separate controls for model-checkpoint persistence or cross-region feature-store/vector-DB replication — here is why, and where they live instead. Checkpoint persistence matters only if you train or fine-tune your own models; it is irrelevant if you consume an API, so it stays as a one-line example of meeting a target rather than a control you must satisfy. Cross-region replication of feature stores and vector databases is general database disaster recovery applied to AI data stores — good engineering, but nothing AI-specific — so it too appears only as an example of how a target is met. If either applies to your deployment, implement it as a mechanism under this control, not as a checklist item of its own.

## Related controls

- [12.2 Tested backup failover](tested-backup-failover.md) — failover addresses provider/model/region outage; this control addresses corruption and data-loss recovery, which failover cannot, because a standby mirrors corrupted state.
- [12.4 AI incident response](ai-incident-response.md) — restoring from a known-good state is a step in rogue-agent containment, and the RTO/RPO tiers feed the AI system inventory used there.
- [Category overview](README.md).
