# Toxicity and harmful-content filters

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: Medium]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Before delivery, block output that is *universally* harmful regardless of deployment purpose: abusive, harassing, hateful, or obscene content, and dangerous or violent recommendations. It is applied at both the provider level, through built-in safety filters, and the application level, for defense in depth.

## Universal harm, not deployment-specific policy

This L1 control covers *universal* harm only. It deliberately excludes *deployment-specific* policy, which is a different thing and lives in the L2 [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md) control. Folding "policy-violating" content into L1 would overlap with L2 and blur the crawl/walk progression, so this control is scoped to universal harmful-content categories only.

## Why it sits at this level

Universal harmful-content categories are the ones no deployment should ever emit, and they are largely available out-of-the-box from model providers, so filtering them is a genuine day-one floor rather than a custom build. That out-of-the-box availability is precisely what distinguishes this control from the L2 scope control, which requires an organization to define custom policy.

## Crawl/walk/run

This control is the home of the category progression table. The category advances along a single axis — *what kind of boundary is being enforced on outputs* — moving from universal harm at L1 to deployment-specific correctness at L2.

| Level | Controls | Boundary being enforced |
|-------|----------|-------------------------|
| L1 (crawl) | PII/secret scrubbing · **Toxicity/harmful-content filtering** | *Universal* — don't leak secrets; don't emit content harmful regardless of purpose. Largely provider-default. |
| L2 (walk) | Topic/scope enforcement · Grounding/hallucination checks | *Deployment-specific* — enforce *your* operational lane and *your* accuracy bar. Requires custom policy. |

## What implementing it looks like

- Enable provider-level safety filters.
- Add an application-level moderation pass for defense in depth.
- Treat the categories — harassment, hate, sexual, violence, self-harm, dangerous advice — as configurable thresholds.

Tooling (not sources): provider safety endpoints; open-source toxicity classifiers.

## Source grounding

- **Primary — NIST AI 600-1 (Generative AI Profile),** the companion to the AI RMF. 600-1 enumerates GenAI risk categories that this control mitigates — "Obscene, Degrading, and/or Abusive Content" and "Dangerous or Violent Recommendations" — and ties them to the **"Safe"** trustworthy-AI characteristic (AI should not endanger human life, health, property, or the environment). This is a more specific primary than the category-default NIST AI RMF Measure 2.5.
- **Supporting — NIST AI RMF 1.0** "Safe" characteristic and **Measure/Manage** functions (content-safety testing and incident handling for unsafe outputs).

For the full reference list, see [sources](../../reference/sources.md).

## Honest limits and trade-offs

- **Know where the line sits between universal harm and deployment-specific policy.** This control covers only *universal* harmful-content categories: content that is harmful regardless of what the deployment is for, and that model providers largely block by default. Content that violates *your* particular policy or falls outside *your* operational lane is a separate, L2 concern handled by [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md). That split — universal harm (L1, provider-default) against deployment-specific policy (L2, custom policy) — is what keeps the category crawl/walk clean, and it is why policy-violating content sits at L2 rather than in this control.
- **Don't confuse this with OWASP LLM05.** Keep it distinct from **OWASP LLM05 (Improper Output Handling)**, which concerns downstream injection and execution risk from unsanitized output, not content safety.

## Related controls

- [PII / sensitive data scrubbing](pii-sensitive-data-scrubbing.md) — the other L1 universal-harm output control.
- [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md) — the L2 deployment-specific counterpart (universal harm here versus your particular lane there).
- [Grounding & hallucination mitigation](grounding-hallucination-mitigation.md) — the other L2 deployment-specific walk-tier control.
