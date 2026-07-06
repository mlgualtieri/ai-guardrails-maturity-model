# Kill switch (rapid capability / deployment disable)

`[Applies To: Agentic / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

The ability to *rapidly disable* a tool, capability, or an entire agent deployment — a system-scoped emergency stop. When something is wrong at scale, whether a misbehaving agent, a compromised tool, or a bad model update affecting all runs, an operator can take the agent's ability to act offline quickly and across the board for everyone: revoke the integration, cut the capability, pull the deployment. It is not scoped to a single run; it is scoped to the whole capability or fleet.

## Why it sits at this level

An organization should not field an autonomous, action-taking agent that it cannot rapidly turn off. That makes a kill switch baseline safety for any agentic deployment, an L2 floor requirement rather than an advanced capability. The *principle* — that you must be able to rapidly disable the capability — is the L2 bar. A fully engineered version, meaning instant, fleet-wide, tested revocation with clear ownership, is the mature realization, but the existence of the capability is required from the start.

## The three-layer "human maintains control" story

HITL keeps a human in ultimate control of an autonomous system at two ends. [7.1 approval](human-approval-consequential-actions.md) *prevents* bad actions before they start, through the approval gate. This control *kills* the capability in an emergency. Prevention and kill are the two ends of human control over an agent that can act on its own.

## What implementing it looks like

A robust implementation provides a fast, reliable path to revoke an agent's credentials or disable its tool access without requiring the agent's cooperation; a documented, owned procedure that names who can trigger it and how, so that it works under pressure; scoping by both capability and deployment, so an operator can disable one tool or the whole agent; and, above all, regular testing. An untested kill switch is not a kill switch. Per-run pause or stop may exist as example tooling under this control, but it is not the guardrail itself. This ties to Identity, which revokes the agent's scoped credentials, and to the incident-response controls at L4, where rogue-agent containment builds on this capability.

## Why this replaces the former interruption / termination control

You might look for a separate "mid-task interruption / termination" control that pauses, rolls back, or kills a *single running task*. There is not one, and here is why that does not leave you exposed. The parts of that idea you actually need are already covered by other guardrails: confirmation gates ([7.1](human-approval-consequential-actions.md)) stop a bad run from taking irreversible actions without approval, [sandboxing](../tool-command-restrictions/sandboxed-execution.md) and [rate limiting](../tool-command-restrictions/rate-limiting-tool-calls.md) bound a runaway, and where you genuinely need to stop the agent, this kill switch disables it. Pausing one individual run is operational agent-ops convenience rather than a distinct AI security guardrail — so if that is what you want, build it as tooling, but do not mistake it for the containment control. The containment control is this system-scoped kill switch.

## Source grounding

- **Primary —** **OWASP LLM06:2025 (Excessive Agency)** — an effective defense against excessive agency requires the ability to constrain and shut down agent action; kill switches are named among the required architectural defenses alongside privilege separation, rate limits, and budget caps. **NIST AI RMF (Manage 2.x / 4.x)** — the human ability to intervene in and disable AI systems as a risk-management response.
- **Supporting —** **NIST SP 800-53 AC-2 / IR family** — account and credential revocation and incident response as the underlying mechanisms. **CISA/NCSC Secure AI Development Guidelines** — the ability to respond to and contain AI incidents.
- **Category anchor —** NIST AI RMF Govern/Manage.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

The gap to watch for is scope. This kill switch is a system-scoped emergency stop — it takes a tool, capability, or whole deployment offline for everyone. It is not a way to pause one misbehaving run, and you should not expect it to be. For that finer-grained "stop this run" need, look to [7.1](human-approval-consequential-actions.md) approval gates, which stop unapproved irreversible actions, and to sandboxing and rate limiting, which bound runaways. The one hard requirement: an untested kill switch is not a kill switch. If you cannot demonstrate, on a schedule, that the disable path actually revokes access under pressure, you do not yet have this control no matter what the runbook says.

## Related controls

- [7.1 Human-in-the-loop approval](human-approval-consequential-actions.md) — the preventive counterpart. 7.1 prevents bad actions before they start, whereas this control kills the capability in an emergency.
- [Identity & Access](../identity-access/README.md) — the kill switch depends on Identity for credential revocation. See [JIT authorization](../identity-access/jit-authorization.md) for the scoped credentials that get revoked.
- [AI incident response](../resilience-recovery/ai-incident-response.md) — this control is the foundation for L4 rogue-agent containment (12.4), which builds on it.
- [Sandboxed execution](../tool-command-restrictions/sandboxed-execution.md) and [rate limiting on tool calls](../tool-command-restrictions/rate-limiting-tool-calls.md) — complementary ways to bound a misbehaving agent.
