# Reversible actions

`[Applies To: Agentic / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Actions an agent takes should be reversible, so that what the agent did can be undone. This is the principle, and specific mechanisms are examples of it. When an agent can write, modify, or delete, the ability to walk that action back is what allows recovery after the agent gets something wrong or is hijacked.

## Why it sits at this level

This is L3 because reversibility is more than a default setting: meeting it well requires designing resources and agent workflows so that their effects can be undone, up to and including coordinated rollback of multi-step actions. It applies specifically to agentic systems, where autonomous action at machine speed makes recover-after capability a real safeguard rather than a nicety.

## What implementing it looks like

- *Soft-delete and versioning on AI-accessible resources* are the primary examples — S3 Object Lock, Azure Blob soft-delete, database point-in-time recovery. Any resource an agent can write to or delete from should have a mechanism like this, so that a destructive action can be reversed.
- *Atomic action patterns and agent undo stacks* are the more sophisticated agentic form of the same reversibility idea. Agent frameworks tend to model multi-step actions without a transaction coordinator, which creates non-atomic failure modes: a chain fails partway and leaves the system in an inconsistent state. Undo stacks and action coordinators address this by triggering a safe rollback rather than leaving that inconsistent state in place.

**Why this is AI-specific.** Agents take autonomous, chained actions at machine speed, so a wrong or hijacked action can do damage before a human notices. Reversibility is the safety net that allows recovery from agent error or compromise. It pairs with the two other ends of agent control: **[HITL approval (7.1)](../hitl/human-approval-consequential-actions.md)** approves *before* an action, **[the kill switch (7.2)](../hitl/kill-switch.md)** stops the agent *mid-flight*, and reversibility is *recover-after*.

## Source grounding

- **Primary —** **NIST SP 800-53 CP-10** — system recovery and reconstitution — together with the **SI (System and Information Integrity) family**.
- **Supporting —** **OWASP LLM06:2025 (Excessive Agency)** — containing the consequences of excessive agency, of which reversibility is one form.
- *AI-specific slice:* reversibility of autonomous agent actions.
- **Category anchor —** NIST SP 800-34 + NIST SP 800-53 CP family.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Do not read this as merely "turn on soft-delete." The control is the principle — every action the agent takes should be reversible — and soft-delete and versioning are examples of it, not the whole of it. A resource that has soft-delete but sits behind an agent that can also mutate it in ways soft-delete does not capture is still exposed, so treat versioning and point-in-time recovery as a floor and design each writable resource for reversibility.

Reversibility also has to cover multi-step chains, which is why atomic action patterns and undo stacks live here alongside single-resource reversibility. Agent frameworks tend to model chained actions without a transaction coordinator, so a chain that fails partway leaves the system inconsistent, and single-resource soft-delete alone will not walk that back. Keeping the principle and its most advanced mechanism in one control is deliberate: undoing an agent's actions is one job, whether the action is a single write or a coordinated sequence.

## Related controls

- [7.1 Human-in-the-loop approval](../hitl/human-approval-consequential-actions.md) — the approve-before counterpart; reversibility handles what approval did not gate.
- [7.2 Kill switch](../hitl/kill-switch.md) — the stop-mid-flight counterpart; after the stop, reversibility undoes what the agent already did.
- [12.4 AI incident response](ai-incident-response.md) — assessing the reversibility of actions a rogue agent has taken is a step in the containment playbook.
- [Category overview](README.md).
