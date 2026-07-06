# Level 4 — Leading Edge

**Typical org profile.** An industry-leading AI security program, with an AI red team in place and DR
drills running quarterly for Tier 1 systems. The organization operates advanced and emerging AI
architectures — persistent agent memory, self-hosted models, multi-agent chains — and holds the
mature incident-response capability to contain them when they fail.

Navigation: [← Level 3](level-3-advanced.md) · **Level 4** · [Level 1](level-1-basic.md) ·
[Level 2](level-2-intermediate.md) · [Full catalog](../maturity-model.md#full-catalog)

---

## The coherent posture at Level 4

An organization at Level 4 can affirmatively say everything from [Level 3](level-3-advanced.md),
plus:

- **Persistent agent memory is safe by construction.** Where agents carry memory across sessions, one
  user's or task's stored context cannot leak into another's (a confidentiality property), and what an
  agent remembers cannot be poisoned to shape its future behavior (an integrity property, backed by
  write-time validation and bounded retention of memories derived from untrusted content).
- **The model artifacts in use are verified.** For self-hosted weights, integrity (checksums),
  authenticity (signed model cards), and safe formats (safetensors over pickle) are verified so that a
  tampered or code-executing model cannot be loaded. (Not applicable to pure API consumers — the
  provider handles this.)
- **A tested, AI-specific incident-response capability is in place.** This is not a document but an
  exercised program covering rogue-agent containment (terminate, revoke, assess reversibility,
  restore), model rollback, an AI system inventory, AI-specific DR drills, and pre-drafted incident
  communications.
- **Agents in a chain are not implicitly trusted.** In multi-agent workflows, each sub-agent's identity
  is verified and its authorization re-validated at every hop — never inherited from the caller — and
  each sub-agent holds only the privilege its subtask requires.

L4 is not more controls for everyone. Each control here is triggered by a specific architecture:
persistent memory, self-hosting, or agent-to-agent chains. An organization that consumes a hosted
model and runs no persistent memory or sub-agent chains may legitimately have no L4 work in those
categories — while still needing L4-grade incident response. This is the applicability convention at
its most pronounced: a control's level states *when it should appear if it is relevant*, and several
L4 controls are relevant only to the architectures they defend.

A closing caution, carried from the model's scope statement: implementing every L4 control does not
make an organization "secure." It makes it well-postured against the risks this model enumerates.
No control set is complete, adversaries adapt, and several controls throughout the model are
explicitly probabilistic rather than boundaries. L4 is the top of *this* model, not the end of the
work.

---

## Controls that first appear at Level 4

Five controls, grouped by category. Each links to its detail page.

### [Memory & State Controls](../controls/memory-state/README.md)

- [Per-user / per-task memory isolation](../controls/memory-state/memory-isolation.md) — *Agentic · Low* — partition persistent memory so it cannot bleed across users or tasks.
- [Memory poisoning prevention](../controls/memory-state/memory-poisoning-prevention.md) — *Agentic · Low* — validate at write time; bound how long poisoned entries can persist.

### [Supply Chain & Model Integrity](../controls/supply-chain/README.md)

- [Model provenance verification](../controls/supply-chain/model-provenance-verification.md) — *Both · Low* — integrity, authenticity, and safe formats for self-hosted weights (N/A for pure API consumers).

### [Resilience & Recovery](../controls/resilience-recovery/README.md)

- [AI incident response](../controls/resilience-recovery/ai-incident-response.md) — *Both · Low* — a tested, AI-specific IR capability (rogue-agent containment, model rollback, drills).

### [Multi-Agent Trust](../controls/multi-agent-trust/README.md)

- [Sub-agent trust](../controls/multi-agent-trust/sub-agent-trust.md) — *Agentic · Low* — verify identity and re-validate authorization at each hop; never inherit trust.

---

## Beyond Level 4

There is no Level 5. Level 4 is the leading edge as this model draws it, but the model is a snapshot
of a fast-moving field, and the controls here — memory safety, agent-to-agent trust, model
provenance — sit on the areas moving fastest. Reaching Level 4 should be read as being well-postured
against the risks enumerated as of mid-2026, and the specific systems in a deployment should still
be threat-modeled for the risks this catalog does not yet name. Revisit the model as the field, the
standards, and the threats move.
