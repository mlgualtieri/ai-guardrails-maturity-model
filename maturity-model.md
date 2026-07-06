# AI Guardrails Maturity Model — Main Guide

This is a practitioner reference for assessing and advancing an enterprise AI security posture, and
it covers both conversational and agentic AI deployments. It defines four progressive maturity
levels and assigns each guardrail sub-control to the level at which that control should first
appear.

This is the main guide, and it carries the whole model on one page: the four levels, the design
principle behind the leveling, the applicability convention, and the full catalog of all 36
controls with links to every detail page. To assess an organization, start with the
[level pages](#how-to-use-this-model). To look up a specific control, use the
[full catalog](#full-catalog) below.

- New to the model: start with [What this model contributes vs. what it cites](README.md#what-this-model-contributes-vs-what-it-cites).
- For the underlying sources: see the [Reference Library](reference/sources.md).
- For the release history of the model: see the [Changelog](CHANGELOG.md).

---

## Maturity levels at a glance

| Level | Label | Typical org profile |
|-------|-------|---------------------|
| [Level 1](levels/level-1-basic.md) | Basic | Early AI deployment. Security team beginning to engage with AI use cases. Controls are reactive. |
| [Level 2](levels/level-2-intermediate.md) | Intermediate | AI in production with some security controls. At least one agentic workflow in use. Some cost visibility. |
| [Level 3](levels/level-3-advanced.md) | Advanced | Mature AI security practice. AI systems explicitly covered in DR/BCP plans. FinOps discipline applied to AI. |
| [Level 4](levels/level-4-leading-edge.md) | Leading Edge | Industry-leading AI security program. AI red team in place. DR drills run quarterly for Tier 1 systems. |

The model contains **36 controls across 13 categories and 4 levels** (L1: 7, L2: 13, L3: 11,
L4: 5).

---

## The design principle: progression within a category, not by category

Each guardrail category is sequenced so that the *floor* control — the minimum coherent posture —
appears at a lower level, while the more *sophisticated* controls in that same category appear
higher up. A category is never assigned a single level as a whole. Identity & Access, for example,
spans L1 (scoped accounts) through L3 (JIT authorization), and Audit Logging spans L1 (decision
logging) through L3 (behavioral anomaly detection).

**How controls were assigned to levels.** A control's level is the answer to one question: *at what
stage of AI-security maturity does this control first become a reasonable expectation?* Four
criteria drive each placement:

1. **Prerequisite ordering.** A control that another one structurally depends on appears at or
   below it. Scoped identity precedes read/write separation, which precedes JIT; log capture
   precedes log immutability, which precedes anomaly detection on those logs.
2. **Floor vs. sophistication within a category.** Each category's minimum coherent posture sits at
   the lowest level, and the more engineered versions of that same defense sit higher.
3. **Implementation burden and organizational readiness.** Controls that need only configuration or
   a design choice land lower, and controls that need dedicated tooling, continuous operation, or
   cross-team process land higher.
4. **Architectural trigger.** A control only enters the picture once the deployment has the
   architecture it defends — memory controls require persistent memory, and multi-agent trust
   requires agent chains — which pushes architecture-dependent controls toward L3 and L4.

Where these criteria conflict, prerequisite ordering wins first, and floor-versus-sophistication
wins second. This leveling is an engineering judgment offered to the wider community to test and to
challenge, and it is not a certification standard.

**The levels are cumulative, and the basics are the foundation.** A higher level assumes every
applicable control at the levels beneath it is already in place. This is by construction, because the
advanced controls depend on the basic ones: behavioral anomaly detection (L3) is only as trustworthy
as the decision logging (L1) it reads; just-in-time access (L3) assumes a scoped identity (L1) to
grant access to; rogue-agent containment (L4) assumes a kill switch (L2) to invoke. Reaching a
higher level does not retire the Basic controls — it stands on them. An organization does not
graduate past least privilege or audit logging, and a single missing L1 control means L1 work
remains no matter how many advanced controls are already running.

---

## The applicability convention: `Both` vs `Agentic`

Not every control applies to every deployment, so each one is tagged:

- **`Both`** — applies to conversational and agentic AI alike.
- **`Agentic`** — applies to autonomous, tool-using systems only.

A control that does not apply to a given system is simply not used for that system. Its level
indicates *when it should appear if it is relevant*, and it does not mean that every L1 system must
implement every L1 control regardless of architecture. Where a control applies to both but its
depth scales with the architecture — scoped identities are one example — the detail page says so.

This is the point to be clear about: **the catalog is not a bingo card, and completing all 36
controls is not the goal.** Many controls will be irrelevant to a given system, and that is expected
rather than a shortfall. Scope the model down to what genuinely applies to your architecture first,
then assess against that subset. A system that never reaches for a tool has no agentic controls to
implement, and it is no less mature for it.

Each control also carries an **alert-fatigue risk** tag of Low, Medium, or High, which is an honest
signal of how noisy the control tends to be in practice, along with a `Process` tag where the
control is procedural rather than technical.

---

## Full catalog

All 36 controls, organized by category. Every control name links to its detail page, and every
level links to its level page. Alert-fatigue risk reflects the control's tendency toward false
positives and operational noise.

### 1. [Identity & Access Controls](controls/identity-access/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Scoped service accounts (NHIs)](controls/identity-access/scoped-service-accounts.md) | Both | [L1](levels/level-1-basic.md) | Low |
| [No standing admin access](controls/identity-access/no-standing-admin-access.md) | Both | [L1](levels/level-1-basic.md) | Low |
| [Separation of read vs. write identities](controls/identity-access/separation-read-write-identities.md) | Both | [L2](levels/level-2-intermediate.md) | Low |
| [JIT authorization](controls/identity-access/jit-authorization.md) | Both | [L3](levels/level-3-advanced.md) | Low |

### 2. [Data Access & Document Protections](controls/data-access/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Data access scoping](controls/data-access/data-access-scoping.md) | Both | [L1](levels/level-1-basic.md) | Low |
| [Classification-aware access enforcement](controls/data-access/classification-aware-access-enforcement.md) | Both | [L2](levels/level-2-intermediate.md) | Low |
| [Retrieval-time authorization checks](controls/data-access/retrieval-time-authorization.md) | Both | [L2](levels/level-2-intermediate.md) | Low |

### 3. [Prompt & Input Filtering](controls/prompt-input-filtering/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Prompt injection detection](controls/prompt-input-filtering/prompt-injection-detection.md) | Both | [L2](levels/level-2-intermediate.md) | High |
| [Indirect prompt injection protection](controls/prompt-input-filtering/indirect-prompt-injection-protection.md) | Agentic | [L3](levels/level-3-advanced.md) | High |

### 4. [Output Filtering & Content Controls](controls/output-filtering/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [PII / sensitive data scrubbing](controls/output-filtering/pii-sensitive-data-scrubbing.md) | Both | [L1](levels/level-1-basic.md) | High |
| [Toxicity and harmful-content filters](controls/output-filtering/toxicity-harmful-content-filters.md) | Both | [L1](levels/level-1-basic.md) | Medium |
| [Grounding & hallucination mitigation](controls/output-filtering/grounding-hallucination-mitigation.md) | Both | [L2](levels/level-2-intermediate.md) | Medium |
| [Acceptable-use & scope enforcement](controls/output-filtering/acceptable-use-scope-enforcement.md) | Both | [L2](levels/level-2-intermediate.md) | High |

### 5. [Tool & Command Restrictions](controls/tool-command-restrictions/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Tool & operation allowlisting (default-deny)](controls/tool-command-restrictions/tool-allowlisting.md) | Agentic | [L2](levels/level-2-intermediate.md) | Medium |
| [Sandboxed execution environments](controls/tool-command-restrictions/sandboxed-execution.md) | Agentic | [L2](levels/level-2-intermediate.md) | Low |
| [Rate limiting on tool call volume](controls/tool-command-restrictions/rate-limiting-tool-calls.md) | Agentic | [L3](levels/level-3-advanced.md) | Low |

### 6. [Audit Logging & Observability](controls/audit-logging/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Decision & debug logging (scrub-by-default)](controls/audit-logging/decision-debug-logging.md) | Both | [L1](levels/level-1-basic.md) | High |
| [Immutable log storage & retention](controls/audit-logging/immutable-log-storage.md) | Both | [L1](levels/level-1-basic.md) | Low · Process |
| [Agent trace logging](controls/audit-logging/agent-trace-logging.md) | Agentic | [L3](levels/level-3-advanced.md) | Medium |
| [Behavioral anomaly detection](controls/audit-logging/behavioral-anomaly-detection.md) | Both | [L3](levels/level-3-advanced.md) | High |

### 7. [Human-in-the-Loop (HITL) Controls](controls/hitl/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Human-in-the-loop approval for consequential actions](controls/hitl/human-approval-consequential-actions.md) | Agentic | [L2](levels/level-2-intermediate.md) → [L3](levels/level-3-advanced.md) | Low |
| [Kill switch (rapid capability / deployment disable)](controls/hitl/kill-switch.md) | Agentic | [L2](levels/level-2-intermediate.md) | Low |

### 8. [Memory & State Controls](controls/memory-state/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Per-user / per-task memory isolation](controls/memory-state/memory-isolation.md) | Agentic | [L4](levels/level-4-leading-edge.md) | Low |
| [Memory poisoning prevention](controls/memory-state/memory-poisoning-prevention.md) | Agentic | [L4](levels/level-4-leading-edge.md) | Low |

### 9. [Supply Chain & Model Integrity](controls/supply-chain/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Pre-integration vetting of agent resources](controls/supply-chain/pre-integration-vetting-agent-resources.md) | Agentic | [L2](levels/level-2-intermediate.md) | Low |
| [Model provenance verification](controls/supply-chain/model-provenance-verification.md) | Both | [L4](levels/level-4-leading-edge.md) | Low |

### 10. [Network & Egress Controls](controls/network-egress/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Egress control for AI agents](controls/network-egress/egress-control.md) | Agentic | [L3](levels/level-3-advanced.md) | Medium |

### 11. [COGS & Cost Governance](controls/cogs-cost-governance/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Cost visibility / tracking](controls/cogs-cost-governance/cost-visibility-tracking.md) | Both | [L2](levels/level-2-intermediate.md) | Low |
| [Cost control & abuse defense (Denial-of-Wallet)](controls/cogs-cost-governance/cost-control-abuse-defense.md) | Both | [L3](levels/level-3-advanced.md) | Medium |

### 12. [Resilience & Recovery](controls/resilience-recovery/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [RTO/RPO targets, controls & testing](controls/resilience-recovery/rto-rpo-targets-testing.md) | Both | [L3](levels/level-3-advanced.md) | Low |
| [Tested backup failover (model / provider / region)](controls/resilience-recovery/tested-backup-failover.md) | Both | [L3](levels/level-3-advanced.md) | Low |
| [Reversible actions](controls/resilience-recovery/reversible-actions.md) | Agentic | [L3](levels/level-3-advanced.md) | Low |
| [AI incident response](controls/resilience-recovery/ai-incident-response.md) | Both | [L4](levels/level-4-leading-edge.md) | Low |
| [A/B model change validation (gated)](controls/resilience-recovery/ab-model-change-validation.md) | Both | [L2](levels/level-2-intermediate.md) → [L3](levels/level-3-advanced.md) | Low |
| [Dedicated evaluation logging for model testing](controls/resilience-recovery/evaluation-logging-model-testing.md) | Both | [L3](levels/level-3-advanced.md) | Low |

### 13. [Multi-Agent Trust](controls/multi-agent-trust/README.md)

| Control | Applies to | Level | Alert fatigue |
|---------|-----------|-------|---------------|
| [Sub-agent trust](controls/multi-agent-trust/sub-agent-trust.md) | Agentic | [L4](levels/level-4-leading-edge.md) | Low |

---

## How to use this model

The model has two entry points, and it is worth picking the one that matches what you are doing.

**To assess your organization,** work the levels in order:

1. **Start at [Level 1](levels/level-1-basic.md).** Read the coherent-posture narrative, which is
   the set of things an organization at that level can affirmatively say about its AI security. For
   each control that first appears at the level, decide whether it applies to your deployment by
   checking the `Both` or `Agentic` tag, and, where it does apply, whether you actually have it.
2. **Find your floor.** You sit at the highest level for which every applicable control at that
   level, *and at every level beneath it*, is genuinely in place. A single missing L1 control means
   L1 work remains, no matter how many L3 controls you have already built.
3. **Read the "key gap to next level"** on your level page. That is the shortest honest path up.
4. **Move up one level at a time.** The leveling is prerequisite-ordered, and skipping tends to
   leave a control standing on a foundation that is not there yet.

**To look up a control,** use the [full catalog](#full-catalog) above, or navigate in from the
category overview pages linked in each catalog section. Each control detail page states what the
control is, why it sits at its level, what implementing it looks like, its sources, its honest
limits and trade-offs, and its related controls.

**A caution on what a level means.** Reaching a level is a self-assessment aid. It is not an
attestation, and it is not a guarantee of security. An organization that implements every L4
control is not "secure" — it is well-postured against the risks this model enumerates. Several
controls, prompt injection and scope detection among them, are explicitly probabilistic rather than
hard boundaries. The model does not replace a threat model for your specific system. See the
[scope and limitations](README.md#scope--limitations) in the README.
