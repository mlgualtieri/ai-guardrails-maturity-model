# Dedicated evaluation logging for model testing

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Capture A/B and model-test runs in their own logged record, kept separate from production decision-logging ([6.1](../audit-logging/decision-debug-logging.md)), so that change-promotion decisions are evidence-based, auditable, and comparable over time. When the team decides to promote or reject a model change, the evaluation that justified the decision is recorded as durable evidence rather than lost.

## Why it sits at this level

This is L3 because it extends audit-logging discipline to a new surface — the model-testing process — as deliberate, maintained infrastructure rather than an ad-hoc artifact. Keeping evaluation runs in a dedicated record so that promotion decisions can be reviewed and compared across time is an advanced-maturity practice.

## What implementing it looks like

Log each evaluation and model-test run — what was compared, on what traffic, against what criteria, and with what result — in a record dedicated to evaluations, distinct from the production decision logs that capture live inference. Retain those records so that successive change-promotion decisions can be compared over time, and so that any single decision can be audited after the fact.

This is distinct from A/B change validation (12.5): the gate in 12.5 *depends on* this logging existing, but an organization can log evaluations without enforcing a gate. That is the immature case, in which the evidence trail exists but rollout is not yet blocked on it.

## Source grounding

- **Primary —** **NIST AI RMF (Measure — document evaluation)** — documenting the evaluation of an AI system.
- **Supporting —** the **NIST SP 800-53 AU (Audit and Accountability) family**, applied to evaluation runs.
- *AI-specific slice:* evaluation evidence for non-deterministic model changes.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

This is a separate control from A/B change validation (12.5), and the distinction matters for what "done" looks like. The two are genuinely different: you can log evaluation runs without enforcing a gate — the immature case — while the gate in 12.5 depends on this logging existing. So having this control does not mean rollout is blocked on it; capturing the evidence and enforcing the gate are two steps, and logging without the gate leaves the evidence trail in place but the door open.

Do not try to fold this into your production decision logs. Evaluation runs are a different surface with a different purpose — justifying change-promotion decisions and comparing them over time — and mixing them into production decision-logging (6.1) blurs the record that makes promotion decisions auditable. Keep them in a dedicated record so a single promotion decision can still be reconstructed after the fact.

## Related controls

- [12.5 A/B model change validation](ab-model-change-validation.md) — the gate that depends on this logging; logging without a gate is the immature case.
- [6.1 Decision & debug logging](../audit-logging/decision-debug-logging.md) — the production decision-logging this is deliberately kept separate from; this control extends the same audit discipline to the model-testing surface.
- [Category overview](README.md).
