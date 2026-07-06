# Classification-aware access enforcement

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

The AI enforces the organization's data classification at its retrieval boundary, and it will not retrieve or surface content whose classification level exceeds the requesting user's clearance. If data is labeled Confidential or Restricted, a user cleared only for Internal cannot obtain that content through the AI, because the AI honors the classification the same way a properly-permissioned human would.

## The precondition — explicitly NOT part of the control

This control assumes that the organization *has* a data classification scheme, meaning labels such as Public / Internal / Confidential / Restricted assigned to data. Building that scheme is general infosec governance (NIST RA-2, FIPS 199, ISO/IEC 27001) rather than an AI guardrail, and the model treats it as a prerequisite the organization brings rather than as leveled AI-guardrail work. **The guardrail is that the AI *respects* the scheme — not that the org creates one.** This keeps the model focused on AI guardrails rather than absorbing an enterprise data-governance program.

## Why it sits at this level

[Data access scoping](data-access-scoping.md) (L1) bounds what the agent can reach at all. This control adds a classification dimension, in that even within reachable data it enforces clearance levels per requester. It requires classification metadata to be present and wired into the retrieval path, which is deployment-specific work beyond the day-one floor, and this is why it sits at L2 rather than L1.

## What implementing it looks like

Classification can be enforced through metadata filtering in the retrieval path, or through platform sensitivity-label integration. Microsoft Purview / Copilot and Google Workspace labels are the prominent managed implementations, in which the platform already carries labels and the AI is configured to honor them. Labels are one mechanism rather than a separate control, and an organization without Purview or Workspace enforces the same boundary through its own classification metadata.

## Source grounding

- **Primary (the AI-enforcement guardrail) — NIST SP 800-53 AC-3 (Access Enforcement) + AC-4 (Information Flow Enforcement):** the AI is an access-decision layer and must enforce classification-based flow control. This is the same enforcement backbone as [retrieval-time authorization checks](retrieval-time-authorization.md), applied to classification rather than to identity.
- **Supporting (the classification scheme this depends on — the precondition):** **NIST RA-2 (Security Categorization)**, **FIPS 199** (low/moderate/high impact levels), **NIST SP 800-60** (information-type → impact mapping), **ISO/IEC 27001:2022 5.12** (information classification), and **CIS Control 3.7** (classification scheme with labels Public/Sensitive/Confidential). RA-2 governs the categorization of information by impact, and these define the scheme the AI enforces without being themselves AI controls.
- **Category anchor:** CISA/NCSC secure AI guidance.

See [reference/sources.md](../../reference/sources.md) for full source details.

## Honest limits and trade-offs

- **Don't expect "sensitivity label integration" as a separate control.** The former standalone "Sensitivity label integration" (Purview/Workspace) was a vendor mechanism for this control rather than a distinct guardrail, following the same reasoning that generalizes RAG-specific controls. It is merged here as one implementation mechanism among several, so look for it as an option under this control rather than on its own.
- **Note that the hard part — classifying the data — sits outside the model, deliberately.** Building the classification scheme is general infosec governance (RA-2/FIPS 199/ISO 27001), an explicitly-excluded precondition. The guardrail is that the AI respects an existing scheme; importing the enterprise data-governance program into the model would blur what an AI guardrail is. If you do not already have a scheme, that is prerequisite work you bring before this control applies.
- **Don't conflate this with retrieval-time authorization — they are distinct enforcement models.** Both enforce a data boundary at retrieval, but they are keyed differently: [retrieval-time authorization checks](retrieval-time-authorization.md) enforce *identity-based* access (is this user on the ACL for this record?), while this control enforces *classification-based* access (is this user cleared for this classification level?). You may need both, meaning per-record ACLs *and* classification clearance, which is why they are kept as two sibling controls rather than merged.

## Related controls

- [Retrieval-time authorization checks](retrieval-time-authorization.md) — sibling L2 control; identity-based enforcement versus this control's classification-based enforcement.
- [Data access scoping](data-access-scoping.md) — the L1 control this builds on.
- The external classification scheme (NIST RA-2 and related standards) is a precondition this control depends on, not an AI control in the model.
