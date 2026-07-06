# Tested backup failover (model / provider / region)

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

A defined, *tested* fallback for when the primary model, provider, or region fails, so that the AI service degrades gracefully instead of going dark. The centerpiece is a decision made in advance: whether to **fail closed** (stop serving) or **fail over** to an alternative model or provider, together with proof that the chosen path works before it is actually needed.

## Why it sits at this level

This is L3 because a fallback that has been designed and *proven*, rather than merely written down, is advanced recovery machinery. The provider dependency it addresses is specific to AI: consuming an external model means the service can go dark because of an outage at a third party the organization does not control, and having a rehearsed answer to that is beyond the baseline.

## What implementing it looks like

Decide the fail-closed vs. fail-over posture per capability and review the providers' SLAs so that the decision is grounded in what those providers actually commit to. Then build and prove the failover path.

- *Active-passive endpoint deployment* — a primary endpoint plus a warm standby with automatic failover — is a mechanism for achieving this.
- *Model registry replication* — copying weights, adapters, and metadata to secondary storage — is another, since a model held in a single location is a single point of failure.
- *Third-party provider outage fallback* is the AI-specific case: the fail-closed vs. fail-over decision for when the external model provider is down, since provider dependency is unique to consuming external models.

*(Infrastructure-as-code for AI environments is out of scope here — it is general infrastructure maturity, not AI-specific.)*

**The "tested" caveat is load-bearing.** Failover to a different model or provider is not free: prompt behavior differs across models, so a prompt tuned for one model can behave differently on another, and an untested fallback tends to fail at the exact moment it is needed. That is why the control is *tested* failover, and the mechanism that validates a fallback model behaves acceptably is the A/B change-validation control (12.5).

## Source grounding

- **Primary —** **NIST SP 800-53 CP-2 / CP-7** — the contingency plan and an alternate processing site or capability.
- **Supporting —** **CISA/NCSC** secure-AI resilience guidance — the ability to withstand and recover from failures in AI systems and their dependencies.
- *AI-specific slice:* model/provider failover, including the behavior differences between models.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Tested failover is not free, and that is the trap to plan around. Failing over to a different model or provider is not like swapping in an identical replica: prompt behavior differs across models, so a prompt tuned for one can behave differently on another, and an untested fallback tends to fail at the exact moment you need it. Active-passive endpoints and model-registry replication are mechanisms for achieving a failover path, not controls to satisfy separately — what you actually have to prove is a tested fallback for a failed provider, model, or region.

Do not dismiss this as standard resilience engineering. The provider-level case is genuinely AI-specific: depending on an external model provider is unique to consuming external models, and failing over between models carries a behavior-difference risk that ordinary infrastructure failover does not. That is exactly why the "tested" qualifier and the tie to 12.5 matter — a failover path you have not validated with the A/B change-validation control is a failover path you cannot trust.

## Related controls

- [12.5 A/B model change validation](ab-model-change-validation.md) — the validation mechanism that makes a fallback "tested"; a swap to a different provider or model is itself a model change to validate.
- [12.1 RTO/RPO targets, controls & testing](rto-rpo-targets-testing.md) — the "test it" principle applied here comes from the same testing discipline; failover addresses outage while 12.1 addresses corruption and data loss.
- [12.4 AI incident response](ai-incident-response.md) — invoking a tested failover is a response step during a provider or model incident.
- [Category overview](README.md).
