# Cost visibility / tracking

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Instrument AI usage so that spend is *observable* at a useful granularity — per request, per workflow, per feature, per agent step — rather than discovered on the monthly invoice. An organization should be able to see what the AI spends, where that spend originates, and when it changes. This is the floor of cost governance, because you cannot govern, cap, or defend what you cannot see.

## Why it is a guardrail, not just FinOps

Cost visibility is the **precondition for detecting cost abuse.** A Denial-of-Wallet attack, or a runaway agent burning thousands in minutes, remains invisible until this instrumentation exists, and without request-level tracking an organization sees the bill only after the damage is done. Visibility is therefore not the cost *defense* itself; it is the **sensor** the defense depends on.

This is the same logic that makes decision-logging a real control rather than housekeeping: it is the observability substrate on which anomaly detection is built. See the spend first, here at L2, then act on it and defend against abuse ([cost control & abuse defense](cost-control-abuse-defense.md), L3).

## The agentic amplifier

Agents multiply cost opacity. A single user interaction can trigger 5–50 model calls as the agent loops (reason–act–observe), each accumulating context overhead. The billing dashboard shows only the aggregate, and without tracing there is no way to connect that aggregate back to a specific agent run. Per-step cost tracing is what makes runaway-loop cost even *diagnosable*. This ties directly to [rate limiting](../tool-command-restrictions/rate-limiting-tool-calls.md): visibility detects the runaway, and rate limiting bounds it.

## Why it sits at this level

This is the **floor of the cost progression** — the baseline observability an organization wants before running AI in production at scale, rather than an advanced capability. Alert-fatigue risk is Low because this is instrumentation (dashboards, traces, per-request cost) and not an alerting filter that fires and demands triage. It sits at L2 rather than L1 because it is deployment-specific instrumentation that matters once real workloads run.

## What implementing it looks like

Capture per-request token counts (input / output, cached vs. uncached) and cost, tagged so that spend can be attributed to a feature, team, workflow, or agent run. Prefer a gateway/proxy or OpenTelemetry-based instrumentation so that this tagging happens automatically rather than by hand. Surface the result in dashboards that engineers actually see. For agents, trace cost per run and per step rather than only per API call. This is the data layer that the L3 budget and anomaly controls consume.

Tooling in this space includes LLM gateways such as LiteLLM or Portkey, observability platforms such as Langfuse, Helicone, or Datadog LLM Observability, and instrumentation following the OpenTelemetry GenAI semantic conventions.

## Source grounding

- **Primary: FinOps Foundation (FinOps for AI / Tokenomics).** Establishes visibility, attribution, and accountability as foundational, and treats observability as a non-negotiable component of production AI rather than an optional extra.
- **Primary: NIST AI RMF (Measure/Manage).** An organization measures what it must manage — cost is a dimension it cannot manage without first measuring it.
- **Supporting: OWASP LLM10 (Unbounded Consumption).** Frames visibility as the detection precondition for the Denial-of-Wallet threat that is defended at L3.
- **Supporting: OpenTelemetry GenAI semantic conventions.** The emerging telemetry standard for instrumenting AI usage in a portable, tool-neutral way.
- **Category anchor:** FinOps Foundation / OWASP GenAI Security Project.

## Honest limits and trade-offs

- **This is a sensor, not a defense — do not mistake it for one.** Cost belongs in a security model **only where it is a guardrail** — cost abuse / Denial-of-Wallet — and not where it is optimization. Cost visibility earns its place because it is the **sensor the abuse defense depends on**: a DoW attack or runaway spend cannot be detected if spend is invisible. It follows the same logging → anomaly-detection pattern you have already met — the observability floor is a control precisely because detection is built on it. What it will not do on its own is stop a single dollar of abusive spend; that is the L3 control's job, and instrumenting spend without wiring it to caps and alerting leaves you watching the meter run.
- **Semantic caching and model routing are not controls here — they are techniques.** Caching, model routing / right-sizing, and prompt compression reduce baseline spend and shrink the blast radius of a cost attack, yet they defend against nothing on their own. They fold into the L3 cost-abuse control as efficiency levers rather than standing as guardrails in their own right, so do not count them toward your cost-defense posture.

## Related controls

- [Cost control & abuse defense (Denial-of-Wallet)](cost-control-abuse-defense.md) — the L3 control that consumes this visibility; the see→act progression.
- [Rate limiting on tool calls](../tool-command-restrictions/rate-limiting-tool-calls.md) — the detection counterpart for runaway agents: visibility detects the runaway, rate limiting bounds it.
- [Behavioral anomaly detection](../audit-logging/behavioral-anomaly-detection.md) — a downstream detection tier that consumes cost signals alongside trace and rate signals.
- [COGS & Cost Governance overview](README.md)
- [Reference: sources](../../reference/sources.md)
