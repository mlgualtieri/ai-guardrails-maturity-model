# Level 2 — Intermediate

**Typical org profile.** AI is in production with some security controls in place, at least one
agentic, tool-using workflow is running, and there is some cost visibility. The organization has
moved past a first experiment and is operating AI as a real system, which means the agentic attack
surface — tools, inputs, actions — is now live and has to be governed, not just the model's output.

Navigation: [← Level 1](level-1-basic.md) · **Level 2** · [Level 3 →](level-3-advanced.md) ·
[Level 4](level-4-leading-edge.md) · [Full catalog](../maturity-model.md#full-catalog)

---

## The coherent posture at Level 2

An organization at Level 2 can affirmatively say everything from [Level 1](level-1-basic.md), plus:

- **Retrieval is bounded to the individual requester, not just the agent.** Within the data the
  agent may reach, each user receives only what their own entitlements allow, and the AI honors the
  organization's data classification — a user cleared only for Internal cannot obtain Confidential
  content through the AI.
- **Inputs are screened for attacks and for policy violations.** A detection layer flags
  prompt-injection attempts, and the acceptable-use policy and operational scope are enforced on both
  the way in and the way out, with the understanding that both are probabilistic layers rather than
  hard boundaries.
- **Agents can use only approved tools, those tools can perform only approved operations, and code
  execution is sandboxed.** Tool access is default-deny, operations are allowlist-preferred, and
  anything the agent executes runs in an isolated, ephemeral environment with no path to the host or
  production.
- **A human approves consequential actions, and the whole thing can be turned off quickly.**
  Irreversible or high-impact actions require explicit human confirmation, backed by a tested,
  system-scoped kill switch that rapidly disables a tool, capability, or deployment.
- **What is connected is vetted before it is connected,** and **AI spend is visible.** Third-party
  tools, MCP servers, plugins, and libraries are treated as trust boundaries and vetted at
  integration time, and AI spend is observable per request, workflow, or agent rather than discovered
  on the monthly invoice.
- **The model powering production is never changed without validating the change** against the
  incumbent on representative traffic.

The theme of L2 is that the agent now *acts*, so the controls move from bounding what it can see and
say to bounding what it can do. The containment for prompt injection is architectural and lives
here: tool least-privilege, sandboxing, and human approval are what actually contain a hijacked
agent, because the input-detection layer alone cannot.

---

## Controls that first appear at Level 2

Thirteen controls, grouped by category. Each links to its detail page.

### [Identity & Access Controls](../controls/identity-access/README.md)

- [Separation of read vs. write identities](../controls/identity-access/separation-read-write-identities.md) — *Both · Low* — read and write/delete operate under different identities.

### [Data Access & Document Protections](../controls/data-access/README.md)

- [Classification-aware access enforcement](../controls/data-access/classification-aware-access-enforcement.md) — *Both · Low* — the AI enforces data classification at its retrieval boundary.
- [Retrieval-time authorization checks](../controls/data-access/retrieval-time-authorization.md) — *Both · Low* — every retrieval is authorized against the requesting user's own entitlements.

### [Prompt & Input Filtering](../controls/prompt-input-filtering/README.md)

- [Prompt injection detection](../controls/prompt-input-filtering/prompt-injection-detection.md) — *Both · High* — a probabilistic detection layer for injection / override attempts.

### [Output Filtering & Content Controls](../controls/output-filtering/README.md)

- [Grounding & hallucination mitigation](../controls/output-filtering/grounding-hallucination-mitigation.md) — *Both · Medium* — design-first practices to keep high-stakes outputs grounded.
- [Acceptable-use & scope enforcement](../controls/output-filtering/acceptable-use-scope-enforcement.md) — *Both · High* — keep the agent in its sanctioned lane, enforced on input and output.

### [Tool & Command Restrictions](../controls/tool-command-restrictions/README.md)

- [Tool & operation allowlisting (default-deny)](../controls/tool-command-restrictions/tool-allowlisting.md) — *Agentic · Medium* — only pre-approved tools are callable, and only their approved operations.
- [Sandboxed execution environments](../controls/tool-command-restrictions/sandboxed-execution.md) — *Agentic · Low* — isolated, ephemeral execution with no host access.

### [Human-in-the-Loop (HITL) Controls](../controls/hitl/README.md)

- [Human-in-the-loop approval for consequential actions](../controls/hitl/human-approval-consequential-actions.md) — *Agentic · Low* — first appears here as a binary confirmation gate (matures to risk-tiered approval at [L3](level-3-advanced.md)).
- [Kill switch (rapid capability / deployment disable)](../controls/hitl/kill-switch.md) — *Agentic · Low* — rapid, system-scoped emergency disable.

### [Supply Chain & Model Integrity](../controls/supply-chain/README.md)

- [Pre-integration vetting of agent resources](../controls/supply-chain/pre-integration-vetting-agent-resources.md) — *Agentic · Low* — vet tools / MCP servers / plugins / libraries at connection time.

### [COGS & Cost Governance](../controls/cogs-cost-governance/README.md)

- [Cost visibility / tracking](../controls/cogs-cost-governance/cost-visibility-tracking.md) — *Both · Low* — make AI spend observable at a useful granularity.

### [Resilience & Recovery](../controls/resilience-recovery/README.md)

- [A/B model change validation (gated)](../controls/resilience-recovery/ab-model-change-validation.md) — *Both · Low* — first appears here as basic gated comparison (matures to systematic evaluation at [L3](level-3-advanced.md)).

---

## Key gap to Level 3

Level 2 supplies the controls that match a production agentic workflow, but several of them remain
at their *crawl* tier: approval is a binary gate rather than a risk-tiered model, and change
validation is a basic comparison rather than a systematic evaluation practice. More importantly, L2
is missing the controls that require continuous operation on top of the L1/L2 substrate: temporal
least-privilege (JIT), indirect-injection protection across every external source the agent touches,
rate limiting to protect downstream infrastructure, egress control to stop exfiltration, cost-abuse
defense, agent-trace logging, behavioral anomaly detection, and a real recovery posture (RTO/RPO,
tested failover, reversibility). The move to [Level 3](level-3-advanced.md) is the shift from having
controls in place to running a mature, continuously operated AI security practice.
