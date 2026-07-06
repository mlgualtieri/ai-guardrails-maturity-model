# Behavioral anomaly detection

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: High]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Baseline the agent's normal behavior and alert on deviations from it — unusual tool sequences, spikes in data retrieval or tool-call volume, out-of-scope access attempts, activity outside normal patterns, or output and behavior drift.

## Why it sits at this level

This is the "detect" tier that sits on top of the captured record: it consumes agent traces, cost data, and rate signals rather than operating as a standalone silo. It is L3 because effective baselining and detection is a mature capability built on the L1/L3 logging substrate, the same capture-then-detect pattern used in cost governance (visibility then anomaly) and elsewhere.

## AI-specific slice (honest scoping)

Anomaly detection is a general security discipline, and this control does not restate generic SIEM anomaly detection. What is AI-specific is *behavioral baselining of agent activity* — tool-use patterns, semantic/output drift, and agent-loop signatures — which traditional UEBA does not model. The control is the AI-behavior application of anomaly detection, not generic SIEM anomaly detection restated. Alert fatigue is High, because baselining noise is real and needs tuning.

## What implementing it looks like

Establish behavioral baselines per agent or workflow; alert on deviation (tool-sequence anomalies, volume spikes, off-hours or out-of-scope access, output drift); and route alerts to response, which ties to the kill switch. It consumes the agent-trace, cost, and rate-limiting signals rather than gathering its own.

## Source grounding

- **Primary — NIST SP 800-53 SI-4 (System Monitoring)** and **AU-6 (Audit Record Review, Analysis, and Reporting)** applied to agent behavior.
- **Supporting — NIST AI RMF** (Measure / Manage — continuous monitoring).
- **Supporting — OWASP Agentic** (behavioral monitoring for excessive agency).
- **Category anchor — NIST SP 800-53 SI-4 / NIST AI RMF.**

See the [sources reference](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

- **This is not generic SIEM/UEBA anomaly detection restated — do not treat your existing UEBA as covering it.** Anomaly detection is a general discipline; the AI-specific slice is behavioral baselining of agent activity (tool-use patterns, semantic/output drift, agent-loop signatures) that traditional UEBA does not model. The control is the AI-behavior application, not the generic capability under a new name.
- **Expect real alert fatigue here — the High tag is not a formality.** Baselining noise is real, and the detection requires tuning before it is useful. That is why it is L3, a mature capability, rather than a floor control, and why it consumes existing traces, cost, and rate signals rather than adding a new noisy sensor. Plan for the tuning effort before you rely on the alerts.

## Related controls

- [Agent trace logging](agent-trace-logging.md) — the primary signal this detection layer consumes.
- [Decision & debug logging](decision-debug-logging.md) — the L1 capture floor this detection tier is built on.
- [Cost visibility & tracking](../cogs-cost-governance/cost-visibility-tracking.md) — cost data is a consumed signal; this mirrors the visibility-then-anomaly pattern of cost governance.
- [Rate limiting tool calls](../tool-command-restrictions/rate-limiting-tool-calls.md) — rate signals are a consumed input.
- [Kill switch](../hitl/kill-switch.md) — anomaly detection triggers the kill switch / HITL escalation.
