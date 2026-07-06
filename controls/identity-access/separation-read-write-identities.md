# Separation of read vs. write identities

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

An agent that reads data operates under a *different* identity than one that writes or deletes it. Rather than a single agent identity holding both read and write/delete permissions, the capability is decomposed into a read-path identity that can retrieve but not modify, and a separate write-path identity for state-changing operations. A compromise or malfunction of the read path cannot cause destructive action, because that identity lacks the permission to do so.

## Why it sits at this level

This is the *functional decomposition* step in the Identity crawl/walk/run. L1 establishes a scoped identity with no standing admin, L2 splits that identity by operation so that read and write are isolated, and L3 (JIT) makes elevation temporal. It is more mature than L1 because it requires modeling the agent's operations and provisioning multiple identities, which is deployment-specific design rather than the day-one floor. It sits on the *scope* axis of the two-axis Identity progression.

## Why it matters — blast-radius reduction

The security value is blast-radius containment. When one set of credentials is compromised, the damage is bounded by least privilege, and a compromised read agent cannot be turned into a destructive one.

**AI-specific angle.** This maps directly onto how agents are built, in that the retrieval tool and the write/delete tool should authenticate as different identities. It is especially pointed for agentic systems, where an injected instruction or a reasoning error on the read path must not be able to reach a destructive operation.

## What implementing it looks like

- Separate service identities are provisioned for read versus write/delete.
- The read identity is scoped to retrieval only, and the write identity is required for any state-changing action.
- Where an agent needs both, they are kept as distinct credentials invoked for distinct operations rather than one blanket identity.
- The write path is paired with confirmation gates (see Tool & Command Restrictions).

## Source grounding

- **Primary — NIST SP 800-53 AC-5 (Separation of Duties)** + **AC-6 (Least Privilege).** Verified: AC-5 divides functions across identities to reduce the risk of abuse, while AC-6 applies least privilege to processes acting on behalf of users. This split is AC-5 applied to an agent's read versus write operations.
- **Supporting — OWASP Least Privilege Principle.** Verified: recommends decomposing a system into services that each run with unique credentials and only the permissions their task needs (the payment-system example, in which the email service cannot touch the database or card data), and frames the practice as blast-radius and attack-surface reduction.
- **Category anchor — CISA/NCSC secure AI guidance.**

See the [source library](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

Don't assume that read-only is automatically low-risk. Read access to secrets, PII, or sensitive records is itself a serious exposure. The read/write split limits *destructive* blast radius, but a compromised read identity remains a data-exposure event. Treat the split as one layer rather than a complete answer, and pair it with the data-access controls that govern what the read path can reach in the first place.

## Related controls

- Middle step of the Identity progression: [scoped service accounts](scoped-service-accounts.md) and [no standing admin access](no-standing-admin-access.md) (1.1/1.2) → this read/write split (1.3) → [JIT authorization](jit-authorization.md) (1.4).
- Reinforced by the confirmation gates in [tool & command restrictions](../tool-command-restrictions/README.md) on the write path.
- Enables cleaner attribution in [audit logging](../audit-logging/decision-debug-logging.md).
