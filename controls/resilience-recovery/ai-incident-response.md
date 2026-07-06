# AI incident response

`[Applies To: Both / Maturity Level: L4 / Alert Fatigue Risk: Low]`

**Level:** [Level 4 — Leading Edge](../../levels/level-4-leading-edge.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

A tested, AI-specific incident-response capability for when an AI system fails or an agent misbehaves. It is not a document; it is an exercised program that a team has rehearsed and can execute under pressure.

## Why it sits at this level

This is L4 (leading-edge) precisely because the bar is a mature, *exercised* program rather than a written plan. Having an incident-response document is table stakes, whereas having a tested AI-IR capability that a team has drilled — including the AI-specific moves that generic IR does not cover — is a leading-edge maturity marker.

## What implementing it looks like

The AI-specific core is kept prominent, while general IR hygiene folds in as components of the plan rather than as standalone controls.

- **Rogue-agent containment playbook** (AI-specific — the prominent piece): the steps for unintended autonomous behavior are to terminate or pause the agent (ties to **[the kill switch, 7.2](../hitl/kill-switch.md)**), revoke its credentials (ties to **[JIT authorization / Identity, 1.4](../identity-access/jit-authorization.md)**), assess the reversibility of the actions it already took (ties to **[reversible actions, 12.3](reversible-actions.md)**), and restore from a known-good state.
- **Model rollback procedures** (AI-specific — also kept prominent): if a model update or fine-tune introduces degraded or harmful behavior, roll back to a previous registry version and test that rollback in drills. This ties to **[A/B change validation, 12.5](ab-model-change-validation.md)**.
- **Plan components** (general IR hygiene given an AI flavor): an **AI system inventory** with dependencies and RTO/RPO tier; **AI-specific DR drills** exercising failover, rollback, agent termination, and data recovery (annual at minimum, quarterly for Tier 1 systems); and **pre-drafted incident-communications templates** for data exposure, unauthorized autonomous actions, and regulatory notification. These are the plan's supporting components rather than separate controls.

## Source grounding

- **Primary —** **NIST SP 800-61** (Computer Security Incident Handling) + the **NIST SP 800-53 IR family**.
- **Supporting —** **OWASP GenAI Incident Response Guide**; **NIST AI RMF (Manage — incident response)**.
- *AI-specific slice:* rogue-agent containment and model rollback.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

You might expect this to be five controls; it is one, and the shape tells you where to focus. The AI-specific parts — containing a rogue agent and rolling back a model — are what distinguish this from generic incident response, so those are where your effort goes. The general parts — inventory, drills, and comms templates — are supporting components of the plan, not distinct guardrails, so implement them as scaffolding rather than as separate programs. Bundling them as "have a tested AI-IR plan" keeps the AI-specific detail prominent instead of burying it under process boilerplate.

A plan on its own will not save you here — the level bar is deliberately a *tested*, exercised capability. A plan that has never been drilled tends to fail when it is actually needed, so treat the drill cadence and the rehearsed containment and rollback steps as part of the control, not optional extras. If your only artifact is a document, you have not yet met this control.

## Related controls

- [7.2 Kill switch](../hitl/kill-switch.md) — the rapid-disable capability the containment playbook invokes to terminate or pause a rogue agent.
- [1.4 JIT authorization](../identity-access/jit-authorization.md) — the scoped credentials that containment revokes.
- [12.3 Reversible actions](reversible-actions.md) — what containment relies on when it assesses and undoes the actions a rogue agent took.
- [12.5 A/B model change validation](ab-model-change-validation.md) — the validation discipline behind the model-rollback procedures.
- [12.1 RTO/RPO targets, controls & testing](rto-rpo-targets-testing.md) — supplies the RTO/RPO tiers recorded in the AI system inventory and the known-good recovery points restored during containment.
- [Category overview](README.md).
