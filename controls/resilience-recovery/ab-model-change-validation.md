# A/B model change validation (gated before rollout)

`[Applies To: Both / Maturity Level: L2→L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) → [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Before a model or agent change ships — a new model version, a provider swap, a prompt change, or a fine-tune — validate it by comparing the candidate against the incumbent on representative traffic, and **gate** the rollout on that comparison passing. Unlike deterministic software, an AI model change can silently shift behavior that no unit test catches, so "do not change the model powering production without validating the change" is a genuinely AI-specific control.

## Why it sits at this level

This control decomposes across two levels. At **L2** the bar is a basic gated comparison — the candidate is compared against the incumbent and the rollout is held on the result. At **L3** it matures into systematic evaluation — a representative evaluation set, defined pass criteria, and a repeatable process. It is AI-specific from the start because the failure mode it addresses, a change that quietly alters behavior, is unique to non-deterministic models.

## Crawl/walk/run

| Level | Stage | What it looks like |
|-------|-------|--------------------|
| **L2** | Basic gated comparison | Compare the candidate change against the incumbent on representative traffic and gate the rollout on the comparison passing. |
| **L3** | Systematic evaluation | A representative evaluation set with defined pass criteria and a repeatable, documented evaluation process behind the gate. |

## What implementing it looks like

Assemble traffic representative of what the change will actually serve, run the candidate and the incumbent against it, and compare the results against criteria that reflect the required quality and behavior. Then wire the gate: the change does not roll out unless the comparison passes. This is also **the "tested" mechanism behind the tested-failover control (12.2)** — validating that a fallback model or provider behaves acceptably is the same candidate-vs-incumbent comparison applied to a failover path.

This is distinct from L4 continuous red-teaming: red-teaming probes for adversarial failure, whereas this control is regression and quality validation of a specific change about to ship.

## Source grounding

- **Primary —** **NIST AI RMF (Measure — pre-deployment evaluation)** — evaluating an AI system before deployment.
- **Supporting —** MLOps validation practice — comparing a candidate model against the incumbent before promotion.
- *AI-specific slice:* validation of a non-deterministic model change.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Do not treat this as ordinary CI for models — the failure mode it catches is AI-specific. Unlike deterministic software, a model change — a new version, provider swap, prompt tweak, or fine-tune — can silently shift behavior that no unit test catches, so validating candidate against incumbent on representative traffic and gating on the result is a distinct AI need. Your existing test suite will pass and the behavior can still degrade.

Two boundaries worth keeping straight so you build the right thing. This is not continuous red-teaming (L4): red-teaming probes adversarial failure, whereas this is regression and quality validation of a change about to ship. It is also not the same as dedicated evaluation logging (12.6) — you can log evaluation runs without enforcing a gate, which is the immature case, but the gate here depends on that logging existing. Logging alone does not block a bad rollout; the gate is what does.

## Related controls

- [12.6 Dedicated evaluation logging for model testing](evaluation-logging-model-testing.md) — records the evaluation runs this gate depends on; you can log without gating, but you cannot gate without logging.
- [12.2 Tested backup failover](tested-backup-failover.md) — this validation is the mechanism that makes a failover "tested."
- [12.4 AI incident response](ai-incident-response.md) — the model-rollback procedures there rely on this validation discipline.
- [Category overview](README.md).
