# No standing admin access

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: Low]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

An agent never holds permanent, always-on elevated privilege, and above all not write or admin access to production. Any elevated rights are task-scoped and time-limited rather than sitting idle on the account. The canonical bad example is an AI coding agent with a standing admin role: the moment it is compromised, *or simply makes a mistake*, it acts with full privilege against production.

## Why it sits at this level

This is the companion to scoped service accounts. Control 1.1 establishes that the agent has its own bounded identity; this control establishes that the identity carries no standing privilege it is not actively using. Either one alone leaves a gap, because a scoped identity that still holds standing admin is scoped in name only, and eliminating standing admin is meaningless if the agent shares a privileged account. Together they constitute the L1 identity posture.

## What implementing it looks like

- No admin or owner roles are assigned to agent identities by default.
- Elevation paths are explicit and logged rather than standing.
- Break-glass procedures are available for cases of genuine need.
- Periodic review catches privilege creep, where a "temporary" grant has quietly become permanent.

## Source grounding

- **Primary — NIST SP 800-53 Rev 5, AC-6(5) (Privileged Accounts)** and **AC-2 (Account Management).** Verified: AC-6(5) restricts privileged accounts to defined roles and keeps day-to-day users out of privileged functions, while AC-2 covers automated temporary and emergency account management and dynamic privilege management (i.e., not standing). Together these authoritatively ground the principle of no permanent standing privilege, with temporary elevation only when needed.

See the [source library](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

A common misconception is that no-standing-admin is just JIT under another name. It is not, and the distinction matters for how you implement this control and how far you have to go to satisfy it, which is why there is a three-level gap between this control and [JIT authorization](jit-authorization.md).

*No standing admin* is an **outcome**: elevated rights are not left standing. You can reach that outcome in several ways, including granting no admin at all, issuing temporary credentials manually, break-glass procedures, or manual access controls. *JIT* is a **specific class of automated technology** that delivers the same outcome, and delivers it better, through programmatic elevation bound to a task and automatically revoked. You can be fully L1-compliant using process and manual controls, with zero JIT technology, and you reach L3 only when you adopt the specialized tooling. The two are therefore not redundant; they are the same principle expressed at different levels of automation and rigor.

## Related controls

- [Scoped service accounts](scoped-service-accounts.md) — the companion L1 control; together they form the L1 identity posture.
- [JIT authorization](jit-authorization.md) — the L3 automated technology that achieves this outcome, bound to a task and revoked on completion.
