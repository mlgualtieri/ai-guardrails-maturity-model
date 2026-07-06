# 12. Resilience & Recovery

**Source.** NIST SP 800-34 (Contingency Planning) + the NIST SP 800-53 CP (Contingency Planning) family. Supporting: ISO 22301 (business continuity); OWASP GenAI Incident Response.

## What this category defends against

AI systems fail and get attacked like any other system, but they change the recovery picture in specific ways. They depend on external model providers an organization does not control, so a provider outage can take an AI service dark. They give autonomous agents the ability to corrupt data or take damaging actions at machine speed, so a wrong or hijacked agent can do harm before a human notices. A model update can also silently degrade or distort behavior in ways no deterministic test catches. This category is about recovering from those failures: setting and testing recovery targets, keeping a tested fallback for provider or model failure, making agent actions reversible, running a tested AI-specific incident-response capability, and validating and logging model changes before they ship.

## Category note — scope

This category covers AI-specific resilience and recovery: recovery targets and testing, tested failover, reversibility of agent actions, incident response, and change validation. The guiding scope line: **general business-continuity and disaster-recovery practice that is not AI-specific is out of scope**; what remains is where AI changes the recovery picture (external AI-provider dependency, agent-caused corruption, rogue agents, model rollback). Each control below calls out the mechanisms and cases it covers.

## Progression across the category

This category does not advance along a single clean axis, so there is no one crawl/walk/run gradient for it. The controls span three levels. **A/B model change validation (12.5)** is the earliest, decomposing from a basic gated comparison at L2 to systematic evaluation at L3. Most of the category sits at L3 — **RTO/RPO targets (12.1)**, **tested backup failover (12.2)**, **reversible actions (12.3)**, and **dedicated evaluation logging (12.6)** — the point at which you build and test real recovery machinery rather than only planning for it. **AI incident response (12.4)** sits at L4, because a mature, exercised incident-response program (not a document) is a leading-edge capability that ties the other controls together.

## Controls in this category

- **[12.1 RTO/RPO targets, controls & testing](rto-rpo-targets-testing.md)** — `[Both / L3 / Low]`. Set recovery targets, build the technical controls to meet them, and test them — differentiated by component tier and AI asset type, with verified clean recovery points for restore-after-corruption.
- **[12.2 Tested backup failover (model / provider / region)](tested-backup-failover.md)** — `[Both / L3 / Low]`. A defined, tested fallback for when the primary model, provider, or region fails, with a fail-closed vs. fail-over decision made in advance.
- **[12.3 Reversible actions](reversible-actions.md)** — `[Agentic / L3 / Low]`. Actions an agent takes should be reversible, so you can recover from agent error or compromise — soft-delete and versioning at the simple end, atomic action patterns and undo stacks at the sophisticated end.
- **[12.4 AI incident response](ai-incident-response.md)** — `[Both / L4 / Low]`. A tested, AI-specific incident-response capability — rogue-agent containment and model rollback as the AI-specific core, with inventory, drills, and comms templates as plan components.
- **[12.5 A/B model change validation (gated before rollout)](ab-model-change-validation.md)** — `[Both / L2→L3 / Low]`. Before a model or agent change ships, validate the candidate against the incumbent on representative traffic and gate the rollout on it passing.
- **[12.6 Dedicated evaluation logging for model testing](evaluation-logging-model-testing.md)** — `[Both / L3 / Low]`. Capture A/B and model-test runs in their own logged record, separate from production decision-logging, so change-promotion decisions are auditable and comparable over time.
