# 8. Memory & State Controls

**Source.** Microsoft AI Red Team — Taxonomy of Failure Modes in AI Agents; OWASP LLM Top 10. Supporting: NIST SP 800-53 AC-4/SC-4. (The Microsoft AI Red Team taxonomy is used here as a named threat-reference authority for agent failure modes, not as a product or vendor tool.)

## What this category defends against

This category defends the persistent memory that modern agents accumulate across sessions. Persistent agent memory is a genuinely new construct, and it introduces two distinct failure modes that the model frames as two security properties:

- **Confidentiality (8.1).** In multi-tenant deployments that share a memory store, one user's or task's stored context can leak into another's session — documented as cross-user memory bleed.
- **Integrity (8.2).** An attacker who influences what an agent *remembers* can shape its future behavior persistently. Unlike a single-turn prompt injection, a poisoned memory persists across sessions.

Both controls are Agentic and sit at Level 4. They are two parallel L4 concerns rather than a single control decomposed across levels, so this category has no crawl/walk/run progression — the framing is simply the two properties above. Time-to-live (TTL) / expiration was not kept as a separate control; it is folded into 8.2 as a mitigation technique that bounds how long a poisoned entry can persist.

## Controls in this category

- [8.1 Per-user / per-task memory isolation](memory-isolation.md) — `[Agentic / L4 / Low]` — confidentiality; prevent cross-user memory bleed.
- [8.2 Memory poisoning prevention](memory-poisoning-prevention.md) — `[Agentic / L4 / Low]` — integrity; validate at write time, treat memory as a sensitive attackable asset, TTL folded in.
