# Scoped service accounts (NHIs)

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: Low]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Every AI agent is provisioned with its own dedicated non-human identity (NHI), carrying only the permissions its specific task requires. The control forecloses two distinct failure modes. The first is reuse of an existing privileged account, which causes the agent to inherit far more access than it needs. The second is the sharing of a single identity across multiple agents, which makes actions impossible to attribute and turns any single compromise into total compromise.

## Why it sits at this level

This is at once a security floor and a *structural prerequisite* for the rest of the category. Read cannot be separated from write identities (L2) without a distinct identity to decompose, just-in-time permissions cannot be granted (L3) without a scoped identity to grant them to, and audit attribution depends on a unique identity per actor. It earns L1 on both grounds, in that it is the baseline and everything above it depends on it existing.

## Applicability (`Both`) — resolved

Tagged **`Both`**. A conversational-only bot with no tool access has no NHI in the agentic sense; its "identity" is simply the application's API key to the model provider. For those deployments the control reduces to a single rule: do not run the application under a privileged shared account, and instead give it a dedicated, least-privilege service identity. For agentic deployments it becomes the full task-scoped-identity discipline. The control applies to both, and only its depth scales with the architecture, consistent with the model's applicability convention that a control's level indicates when it should appear *if relevant*, with depth following the deployment.

## What implementing it looks like

- A dedicated workload or service identity is provisioned for each agent.
- Permissions are scoped to the task rather than to the team or the environment.
- An agent is never attached to a pre-existing privileged account.
- One identity is maintained per agent, so that logs attribute actions cleanly to a specific actor.

For context on the scale of the problem, CyberArk's 2025 Identity Security Landscape reports that machine identities outnumber humans by more than 80 to 1, and that 68% of organizations lack identity security controls for their AI systems. (Vendor report, cited for framing only.)

## Source grounding

- **Primary — NIST SP 800-53 Rev 5, AC-6 (Least Privilege).** Verified: the control text applies least privilege to *system processes*, requiring that they operate at privilege levels no higher than necessary, and directs organizations to create additional processes, roles, and accounts as needed to achieve least privilege. This is the authoritative basis for a dedicated, minimally scoped identity per agent. Supported by **AC-2 (Account Management)**.
- **Author's mapping note.** NIST SP 800-53 predates AI agents, and it governs "processes acting on behalf of users." This model applies that provision to AI agents. The mapping represents this model's reasoning rather than a NIST pronouncement about AI agents.

See the [source library](../../reference/sources.md) for full citations.

## Related controls

- [No standing admin access](no-standing-admin-access.md) — the companion L1 control; together they form the L1 identity posture.
- [Separation of read vs. write identities](separation-read-write-identities.md) and [JIT authorization](jit-authorization.md) — each depends on a distinct scoped identity to decompose or to grant.
- Clean per-actor identity is what enables attribution in [audit logging](../audit-logging/decision-debug-logging.md).
