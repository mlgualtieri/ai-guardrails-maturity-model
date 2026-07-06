# Per-user / per-task memory isolation

`[Applies To: Agentic / Maturity Level: L4 / Alert Fatigue Risk: Low]`

**Level:** [Level 4 — Leading Edge](../../levels/level-4-leading-edge.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Persistent agent memory must be partitioned so that one user's or task's stored context cannot leak into another's session. In multi-tenant deployments that use a shared memory store, cross-user memory bleed is a documented risk — one tenant's stored context surfacing in another tenant's session. This is a *confidentiality* failure.

## Why it sits at this level

Persistent agent memory is a genuinely new construct, and this control is the familiar multi-tenancy and isolation problem applied to that new surface, which is why it is treated as AI-specific and placed at L4.

Level note: the L4 placement reflects when most organizations reach persistent agent memory, not when the control becomes urgent. If you already run multi-user persistent memory, do not wait for a formal L4 push — for that architecture, isolation is baseline hygiene you should have in place now. The control is filed at L4 only because the isolation requirement travels with persistent agent memory, which is itself an advanced capability most deployments have not yet adopted.

## What implementing it looks like

Partition stored memory by tenant and by task, and enforce that boundary on both the read and write paths so that retrieval for one principal can never return another principal's entries. Scope memory keys to an authenticated identity rather than to a shared namespace, and confirm that the isolation holds where memory is co-located in a shared backing store.

## Source grounding

- **Primary — NIST SP 800-53 AC-4 (information flow enforcement) + SC-4 (information in shared resources).** AC-4 grounds the requirement that context does not flow across the tenant/task boundary; SC-4 grounds preventing residual information exposure where a memory store is shared.
- **Threat reference — Microsoft AI Red Team, Taxonomy of Failure Modes in AI Agents.** Names cross-user memory bleed as a failure mode for shared-memory multi-tenant deployments.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

The level label can mislead you on timing. This control is filed at L4, but if you already run multi-user persistent memory, treat it as baseline hygiene to implement now rather than a leading-edge item to defer. The L4 placement is about adoption, not urgency: the control only becomes relevant once you have persistent agent memory, and few deployments have reached that point, so it lands with the advanced-memory concerns. For the subset that is already there, the isolation requirement is immediate.

## Related controls

- [8.2 Memory poisoning prevention](memory-poisoning-prevention.md) — the integrity counterpart to this confidentiality control, over the same persistent-memory surface.
- [Category overview](README.md).
