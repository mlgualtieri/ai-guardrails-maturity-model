# Level 3 — Advanced

**Typical org profile.** A mature AI security practice, in which AI systems are explicitly covered in
disaster-recovery and business-continuity plans and FinOps discipline is applied to AI. The controls
are no longer merely configured — they are continuously operated: detection runs on captured
telemetry, access is granted and revoked dynamically, and recovery is planned and tested rather than
assumed.

Navigation: [← Level 2](level-2-intermediate.md) · **Level 3** ·
[Level 4 →](level-4-leading-edge.md) · [Level 1](level-1-basic.md) ·
[Full catalog](../maturity-model.md#full-catalog)

---

## The coherent posture at Level 3

An organization at Level 3 can affirmatively say everything from [Level 2](level-2-intermediate.md),
plus:

- **Elevated access is granted just in time and revoked when the task ends.** Agents run at a low
  baseline and receive time-boxed, task-scoped authorization only for the operations that require it,
  with reliable automatic expiry — least privilege across the time dimension, not just scope.
- **Everything the agent fetches is treated as untrusted, not only what the user types.** Retrieved
  documents, tool outputs, memory retrievals, and fetched pages are scanned and segregated so that
  environment-borne (indirect) injection cannot silently redirect the agent.
- **A runaway or hijacked agent cannot overwhelm the systems it calls, spend catastrophically, or
  exfiltrate data.** Tool-call volume is bounded to protect downstream infrastructure, cost-abuse
  defenses — hard caps, anomaly alerting, attribution — defend the budget against Denial-of-Wallet,
  and egress is controlled so a hijacked agent cannot ship data to an arbitrary endpoint.
- **Human oversight is proportional to risk.** Approval graduates from a binary gate to a risk-tiered
  model in which the oversight intensity is set by the action's consequence and the model's confidence.
- **What agents actually do can be seen and analyzed.** Full agent execution traces are captured, and
  behavioral anomaly detection baselines normal agent behavior and alerts on deviation, consuming the
  trace, cost, and rate signals rather than sitting as a standalone silo.
- **There is a real recovery posture for AI.** RTO/RPO targets are set and tested, differentiated by
  asset type, alongside a tested fallback for model, provider, or region failure, reversibility for the
  actions agents take, and dedicated logging of model-change evaluations.

L3 is where the model's recurring pattern completes for several categories: *capture at the floor →
detect and defend higher up*. Logging (L1) becomes anomaly detection (L3); cost visibility (L2)
becomes cost-abuse defense (L3); a binary approval gate (L2) becomes risk-tiered approval (L3).

---

## Controls that first appear at Level 3

Eleven controls, grouped by category. Each links to its detail page. (Two L2 controls also reach
their mature tier here: [risk-tiered approval](../controls/hitl/human-approval-consequential-actions.md)
and [systematic A/B evaluation](../controls/resilience-recovery/ab-model-change-validation.md).)

### [Identity & Access Controls](../controls/identity-access/README.md)

- [JIT authorization](../controls/identity-access/jit-authorization.md) — *Both · Low* — access authorized at the moment of need, auto-revoked on completion.

### [Prompt & Input Filtering](../controls/prompt-input-filtering/README.md)

- [Indirect prompt injection protection](../controls/prompt-input-filtering/indirect-prompt-injection-protection.md) — *Agentic · High* — treat all external / retrieved content as untrusted.

### [Tool & Command Restrictions](../controls/tool-command-restrictions/README.md)

- [Rate limiting on tool call volume](../controls/tool-command-restrictions/rate-limiting-tool-calls.md) — *Agentic · Low* — bound tool-call volume to protect downstream infrastructure.

### [Audit Logging & Observability](../controls/audit-logging/README.md)

- [Agent trace logging](../controls/audit-logging/agent-trace-logging.md) — *Agentic · Medium* — capture the full multi-step agent execution trace.
- [Behavioral anomaly detection](../controls/audit-logging/behavioral-anomaly-detection.md) — *Both · High* — baseline agent behavior and alert on deviation.

### [Network & Egress Controls](../controls/network-egress/README.md)

- [Egress control for AI agents](../controls/network-egress/egress-control.md) — *Agentic · Medium* — constrain where an agent can send data and connect.

### [COGS & Cost Governance](../controls/cogs-cost-governance/README.md)

- [Cost control & abuse defense (Denial-of-Wallet)](../controls/cogs-cost-governance/cost-control-abuse-defense.md) — *Both · Medium* — hard caps, anomaly alerting, and attribution against cost abuse.

### [Resilience & Recovery](../controls/resilience-recovery/README.md)

- [RTO/RPO targets, controls & testing](../controls/resilience-recovery/rto-rpo-targets-testing.md) — *Both · Low* — set, build for, and test recovery targets.
- [Tested backup failover (model / provider / region)](../controls/resilience-recovery/tested-backup-failover.md) — *Both · Low* — a defined, tested fallback for primary failure.
- [Reversible actions](../controls/resilience-recovery/reversible-actions.md) — *Agentic · Low* — actions an agent takes should be undoable.
- [Dedicated evaluation logging for model testing](../controls/resilience-recovery/evaluation-logging-model-testing.md) — *Both · Low* — capture model-test runs in their own auditable record.

---

## Key gap to Level 4

Level 3 is a continuously operated practice, but it stops short of the leading-edge controls that
only certain architectures trigger and only mature programs exercise. The move to
[Level 4](level-4-leading-edge.md) adds the controls for advanced and emerging deployment patterns:
per-user and per-task memory isolation and memory-poisoning prevention (for systems with persistent
memory), model provenance verification (for self-hosted weights), a tested AI-specific
incident-response capability (rogue-agent containment, model rollback, drills), and sub-agent trust
(for multi-agent chains). These are not more of the same. Each enters only once the deployment has
the architecture it defends, which is why they sit at the top.
