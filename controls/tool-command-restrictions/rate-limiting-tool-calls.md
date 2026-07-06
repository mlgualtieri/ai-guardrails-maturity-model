# Rate limiting on tool call volume

`[Applies To: Agentic / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Bound how many tool calls — and how much downstream resource — an agent can consume, so that a runaway or manipulated agent cannot spiral and overwhelm the systems it calls. In practice this means per-task iteration caps (max steps in an agent loop), maximum recursion depth, per-minute call-rate limits, and circuit-breakers that halt the agent when thresholds trip.

## What this control actually protects — the downstream infrastructure

The primary risk here is *availability of the infrastructure the agent depends on* — the tools, APIs, databases, file stores, and microservices the agent calls. Crucially, **those downstream systems are often not AI at all.** An agent stuck in a loop hammering a database or an internal API can take that service down through classic resource exhaustion — a denial-of-service against downstream infrastructure — even when the LLM-token cost is negligible. This is why the control is *not* a cost control: its victim is downstream infrastructure, not the budget.


## Distinct from Denial-of-Wallet / COGS (two different victims)

These address different failure modes, which is why they live in different categories:

- **Rate limiting (this control, Tool & Command):** victim = the *downstream infrastructure* the agent calls (AI or not). Classic DoS / resource-protection applied to agent tool calls.
- **Denial-of-Wallet ([COGS, category 11](../cogs-cost-governance/cost-control-abuse-defense.md)):** victim = the *budget* (LLM token and API spend).

An agent can trigger either or both, but they are separate risks with separate victims. Cross-reference them rather than merging them.

## Why it sits at this level (L3, not L2)

A basic iteration cap is nearly an L2 `max_steps` parameter. What makes the *control* L3 is doing it well: **token-/resource-aware limits** rather than blunt request-counting, which cannot tell a productive agent from a runaway loop, together with **loop detection** and **circuit-breakers**. It is kept as one L3 control rather than split into a naive L2 cap, because a request-counter that misses the actual failure mode is not worth leveling separately. Alert fatigue is Low.

## What implementing it looks like

- Per-task iteration/step caps and max recursion depth in the agent framework.
- Per-minute / per-tool call-rate limits protecting each downstream dependency.
- Circuit-breakers that halt the agent when a threshold trips, together with loop and anomaly detection.
- The circuit-breaker ties to the [kill switch (7.2)](../hitl/kill-switch.md): an automated circuit-breaker is a threshold-triggered kill for a single run.

## Source grounding

- **Primary — NIST SP 800-53 SC-5 (Denial-of-Service Protection).** The classic resource-protection control family; rate limiting agent tool calls is SC-5 applied to protect whatever the agent calls (AI or not), matching the infrastructure-protection framing above.
- **Supporting (AI-specific case) — OWASP LLM10:2025 (Unbounded Consumption).** Names recursive tool calls and infinite agent loops as a risk; mitigations are iteration caps, recursion-depth limits, session termination on threshold, and circuit-breakers. Maps to **CWE-770 (Allocation of Resources Without Limits)** and **CWE-834 (Excessive Iteration)**. **OWASP LLM06 (Excessive Agency)** supplies the bound-autonomy principle.
- **Category anchor — Anthropic, Building Effective Agents.**

## Honest limits and trade-offs

You might expect this to be folded into Denial-of-Wallet / cost controls, and the reason it is not comes down to **two different victims.** This control protects *downstream infrastructure* — services that are frequently not AI, and where a loop causes classic resource exhaustion regardless of token spend. Denial-of-Wallet protects the *budget*. A single runaway agent can cause either or both, but the failure mode, the defended asset, and the mitigation are distinct, so treat them as separate controls in separate categories and cross-reference rather than combine them. Watch for the trap of assuming a token-cost limit also protects a database an agent is hammering — it does not.

## Related controls

- [COGS — cost control & abuse defense](../cogs-cost-governance/cost-control-abuse-defense.md) — the Denial-of-Wallet counterpart, with a different victim (the budget).
- [5.2 Sandboxed execution environments](sandboxed-execution.md) — another way to bound a misbehaving agent.
- [Kill switch (7.2)](../hitl/kill-switch.md) — a circuit-breaker is a threshold-triggered kill for a single run.
