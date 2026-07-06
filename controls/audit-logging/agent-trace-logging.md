# Agent trace logging

`[Applies To: Agentic / Maturity Level: L3 / Alert Fatigue Risk: Medium]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

For agentic systems, capture the agent's full execution trace — the chain of reasoning steps, tool invocations, inputs and outputs at each hop, and intermediate decisions — rather than only the final request/response. This is the record that lets you reconstruct *why* an agent did what it did across a multi-step task.

## Why it sits at this level

The L1 floor (decision & debug logging) logs decision/debug metadata for a single interaction, while agent trace logging extends that to the multi-step agentic chain, which requires structured tracing infrastructure — spans and causal links between steps — beyond basic logging. This is AI-specific: the multi-step agent trace is a genuinely new artifact, because traditional applications do not have a "reasoning plus tool chain" to capture.

**Scope discipline (inherits the L1 rules).** Agent trace logging carries the same scrub-by-default posture as decision & debug logging: capture enough to reconstruct and debug the chain without indiscriminately retaining PII or secrets, and verbatim retention remains a justified exception, including for forensic value. It is tagged Medium rather than High because trace volume is large but it is instrumentation, not an alerting filter.

## What implementing it looks like

OpenTelemetry-based tracing (GenAI span conventions) capturing per-step tool calls, arguments, and outcomes with causal links, correlated to the cost traces and the decision log. The resulting traces feed behavioral anomaly detection and post-incident forensics.

## Source grounding

- **Primary — NIST SP 800-53 AU-2 / AU-3 / AU-12** (event logging, content of audit records, and audit record generation) applied to the agent chain.
- **Supporting — OpenTelemetry GenAI semantic conventions**, the telemetry standard for capturing GenAI spans.
- **Supporting — OWASP Agentic** (observability of agent action).
- **Category anchor — NIST SP 800-53 AU family.**

See the [sources reference](../../reference/sources.md) for full citations.

## Related controls

- [Decision & debug logging](decision-debug-logging.md) — this control extends decision logging to the agent chain and inherits its scrub-by-default posture.
- [Behavioral anomaly detection](behavioral-anomaly-detection.md) — consumes these traces as a detection signal.
- [Cost visibility & tracking](../cogs-cost-governance/cost-visibility-tracking.md) — trace logging correlates with cost tracing.
- [Rate limiting tool calls](../tool-command-restrictions/rate-limiting-tool-calls.md) — traces correlate with rate signals.
