# Cost control & abuse defense (Denial-of-Wallet)

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: Medium]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Defend against cost *abuse* — an attacker or a runaway agent driving catastrophic spend (Denial-of-Wallet). Where [cost visibility / tracking](cost-visibility-tracking.md) provides visibility, this control **acts** on it through hard budget caps that stop spend, anomaly alerting on spikes, and attribution fine enough to localize the abuse.

## Why one control (collapse)

Budget caps, anomaly alerting, Denial-of-Wallet protection, and per-inference attribution are facets of one guardrail — the guardrail that detects and stops cost abuse:

- **Budget caps** are circuit-breakers that *stop* spend at a threshold, not merely alerts.
- **Anomaly alerting** *detects* spikes: a runaway agent, injection-driven tool storms, context inflation.
- **Per-inference attribution** is the finer-grained visibility, extending 11.1, that lets alerting localize the source and enables chargeback.
- **Denial-of-Wallet** is the named threat they jointly defend against.

Semantic caching, model routing, and prompt compression fold in here as **efficiency techniques** that shrink baseline spend and the blast radius of a cost attack; they are levers, not controls.

## Why it sits at this level

Denial-of-Wallet is an **AI-native attack**: crafted inputs make an agent loop or emit expensive reasoning chains, draining budgets in minutes. This control sits at L3 because it needs the enforcement machinery — gateway-level caps, anomaly baselining, attribution — built on the L2 visibility floor. That is the see→act progression, since an organization cannot cap or baseline what it has not first instrumented. Alert-fatigue risk is Medium: anomaly alerting fires and demands triage, whereas hard caps act automatically rather than adding to the queue.

## What implementing it looks like

Set hard per-agent, per-team, or per-customer spend caps at the gateway — circuit-breakers that stop spend rather than merely alert. Add cost anomaly alerting on token and spend spikes. Key per-inference cost attribution to user / team / feature so that alerting can localize the source. Layer in Denial-of-Wallet-specific defenses — iteration caps, per-task token budgets, and loop detection — which are shared with rate limiting.

Tooling in this space includes LLM gateways with budget enforcement and cost-observability platforms.

## Source grounding

- **Primary: OWASP LLM10 (Unbounded Consumption).** The Denial-of-Wallet threat and its countermeasures — budget caps, token limits, session termination.
- **Supporting: NIST SP 800-53 SC-5 (Denial-of-Service protection).** Extends DoS thinking to the budget dimension, treating spend exhaustion as a resource-exhaustion attack.
- **Supporting: FinOps Foundation.** Cost governance and anomaly practices for production AI.
- **Category anchor:** OWASP GenAI Security Project / FinOps Foundation.

## Honest limits and trade-offs

- **You might expect budget caps, anomaly alerting, attribution, and DoW protection to be four separate controls — they are one, and you implement them together.** They are facets of a single guardrail that detects and stops cost abuse. Caps are the circuit-breakers that stop spend; alerting detects the spikes; attribution is the finer visibility that localizes the source; Denial-of-Wallet is the named threat they jointly defend against. Treating them as separate leaves gaps between them — a cap with no alerting fails silently, alerting with no attribution cannot tell you where to look. (Caching, routing, and compression fold in as efficiency techniques, not controls — do not count them as the defense.)
- **Do not assume rate limiting already covers this — the victim is different.** This control defends the **budget**, while [rate limiting](../tool-command-restrictions/rate-limiting-tool-calls.md) defends **downstream infrastructure**. They share loop and iteration defenses, yet they protect different things: an attack can exhaust a budget without stressing infrastructure, and the reverse holds as well. If you have rate limiting but no spend caps, your budget is still exposed.

## Related controls

- [Cost visibility / tracking](cost-visibility-tracking.md) — the L2 floor this control acts on (see→act).
- [Rate limiting on tool calls](../tool-command-restrictions/rate-limiting-tool-calls.md) — shares loop / iteration defenses, but a different victim (budget vs. infra).
- [Kill switch](../hitl/kill-switch.md) — a breached cap can trigger a hard stop.
- [COGS & Cost Governance overview](README.md)
- [Reference: sources](../../reference/sources.md)
