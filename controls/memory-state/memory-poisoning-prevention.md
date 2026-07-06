# Memory poisoning prevention

`[Applies To: Agentic / Maturity Level: L4 / Alert Fatigue Risk: Low]`

**Level:** [Level 4 — Leading Edge](../../levels/level-4-leading-edge.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

An attacker who influences what an agent *remembers* can shape its future behavior persistently. Unlike a single-turn prompt injection, which is bounded to one exchange, a poisoned memory persists across sessions and continues to steer the agent. This is an *integrity* failure. The defense is to apply validation at write time and to treat memory stores as sensitive, attackable assets rather than as trusted scratch space.

## Why it sits at this level

This is one of the more distinctly agentic threats — prompt injection with persistence — and it only exists once an agent has durable memory to poison, which is why it is placed at L4 and treated as AI-specific.

## What implementing it looks like

Validate entries semantically at the point they are written to memory, especially entries derived from user-supplied or external content, rather than accepting anything the agent proposes to store. Treat the memory store as a sensitive asset, with the access and integrity controls that this implies.

TTL-based memory expiration and pruning is folded in here as a *mitigation technique* rather than a separate control. Time-to-live limits on stored memories — again, especially those derived from user-supplied or external content — bound how long any poisoned entry can persist and do damage. This works alongside write-time semantic validation and the practice of treating memory as a sensitive store.

## Source grounding

- **Primary — OWASP LLM01 (Prompt Injection), persistent form.** Grounds memory poisoning as the persistent variant of prompt injection.
- **Threat reference — Microsoft AI Red Team, Taxonomy of Failure Modes in AI Agents.** Names memory poisoning as an agent failure mode.
- **Supporting — NIST SP 800-53 SI-family (system and information integrity).** Grounds the integrity framing and write-time validation.
- *AI-specific slice: persistent-memory poisoning.*

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Do not treat TTL as a control in its own right. Time-to-live and expiration are folded into this control as one mitigation technique, not carried separately, because they defend the same asset — the memory store — as write-time validation. What this means for you: TTL bounds how long a poisoned entry can persist and do damage, but it does not stop the entry from being written or from causing harm within its lifetime. Rely on it as one layer alongside write-time semantic validation and treating memory as a sensitive store, not as a standalone guardrail that makes poisoning prevention optional.

## Related controls

- [8.1 Per-user / per-task memory isolation](memory-isolation.md) — the confidentiality counterpart over the same persistent-memory surface.
- [3.3 Indirect prompt injection protection](../prompt-input-filtering/indirect-prompt-injection-protection.md) — poisoned external content is often what gets stored into memory in the first place.
- [6.5 Behavioral anomaly detection](../audit-logging/behavioral-anomaly-detection.md) — may surface the downstream behavioral effects of a poisoned memory.
- [Category overview](README.md).
