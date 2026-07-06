# JIT authorization

`[Applies To: Both / Maturity Level: L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Rather than the agent holding elevated permissions continuously, access is authorized *at the moment of need*, scoped to the specific task, and expired or revoked the instant the task completes. The agent runs at a low baseline and receives time-boxed authorization only for the operations that require it, so that elevated access exists only during the narrow window it is actually in use.

## Why "JIT authorization"

This control is deliberately named "JIT authorization" rather than any variant built on "privilege escalation." "Privilege escalation" is the name of an *attack* technique — an adversary illicitly gaining elevated rights — so applying it to a defensive control is confusing. "JIT authorization" is the accurate, general framing, without the attack-term baggage.

## Why it sits at this level

JIT is the L3 endpoint of the Identity progression, on the *duration* axis. It completes the two-axis story: L1 (1.1/1.2) scoped identity and no standing admin bound *what*, L2 (1.3) read/write separation decomposes by *function*, and L3 (1.4) JIT bounds authorization in *time*. The principle is least privilege for the least time. It sits at L3 because it requires real machinery beyond the L1/L2 baseline, namely runtime policy evaluation, ephemeral-credential issuance, and reliable automatic revocation.

## JIT vs. Zero Standing Privilege (target vs. ideal)

**JIT** grants elevated access for a defined period, after which it expires. **Zero Standing Privilege (ZSP)** is stricter, in that the identity holds *no* standing entitlement at rest, and privilege exists only when policy grants it per request. JIT is the achievable L3 target, and ZSP is the stricter ideal. The sources are candid that there is no single perfect implementation yet, and that true ZSP is not always attainable (legacy systems, CI/CD), so this control targets JIT and treats ZSP as the aspirational end-state.

## AI-specific angle

Least privilege for agents should be enforced *at the moment of action, not just at onboarding*, because agents perform **chained access**: a sequence that is harmless in isolation (search, then retrieve, then file access, then external messaging) becomes dangerous when combined. Standing privilege on an agent is especially dangerous, because a hijacked agent — via prompt injection — can use *all* of it at once. JIT ensures that a compromised agent holds only whatever narrow authorization it has *right now*, which makes it a direct blast-radius control against the injection threat the rest of the model addresses. The identity primitive should be the *workload*, receiving short-lived access only for its current task.

## What implementing it looks like

- A zero or minimal standing-privilege baseline is maintained.
- Policy is evaluated at runtime, at the moment of the request.
- Ephemeral, task-scoped credentials are issued with tight TTLs, bound to the workload identity rather than issued as a reusable secret.
- Revocation is automatic and reliable on completion or expiry.
- An emergency-access fallback is provided that is itself time-bound, logged, and separately approved.
- JIT events are streamed to monitoring.

A distinct **agent-identity** product category now exists, including Okta for AI Agents (GA April 2026, IdP-neutral), Microsoft Entra Agent ID (Microsoft-ecosystem-native), and AWS Bedrock AgentCore identity (AWS-native), alongside general JIT/PAM platforms such as CyberArk, Delinea, and Aembit, and cloud-native IAM STS / short-lived credentials.

## Source grounding

- **Primary — NIST SP 800-53 AC-6 (Least Privilege)** + **AC-2 (Account Management).** JIT is least privilege applied across the *time* dimension, with automated provisioning and deprovisioning. **NIST SP 800-207 (Zero Trust Architecture)** — verified: dynamic, per-request, real-time-context access decisions, of which JIT is a core ZTA enabler.
- **Supporting — OWASP Non-Human Identity Top 10** (verified: standing access as a liability for agents and NHIs, with a recommendation to bind to workload identity and use short-lived credentials) and **NIST AI RMF** (governance, accountability, and revocation for agent access, enforced at the moment of action).
- **Category anchor — CISA/NCSC secure-AI and zero-trust guidance.**

See the [source library](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

Before you rely on JIT, weigh the real tradeoffs that come with it.

- **Throughput and latency cost.** Routing every access through runtime policy evaluation and ephemeral-credential issuance adds latency and identity-system overhead on the path to the backend. For high-throughput agents making many rapid tool calls, that per-request identity round-trip can become a measurable bottleneck. It is worth the cost where access is consequential, and less so on hot paths where the overhead outweighs the benefit.
- **Revocation must be reliable, or JIT is an illusion.** The security value depends entirely on access actually *going away* when the task ends. If revocation fails — a missing TTL, a failed teardown call, a bug — the "temporary" credential lingers. Over repeated runs the agent silently accumulates access that was supposed to be ephemeral but has effectively become permanent, which is standing privilege in disguise. What you are actually deploying is therefore "JIT *with reliable automatic expiry/revocation*," and without that guarantee you have not actually reduced standing privilege.
- **Auditing wrinkle.** Be aware that dynamic, transactional access generates high log volume, which can make anomaly detection harder, and this ties back to the audit-logging controls.

## Related controls

- Duration-axis endpoint of the Identity progression: [scoped service accounts](scoped-service-accounts.md) and [no standing admin access](no-standing-admin-access.md) (1.1/1.2) → [separation of read vs. write identities](separation-read-write-identities.md) (1.3) → JIT (1.4).
- Reinforces the same blast-radius goal as [tool & command restrictions](../tool-command-restrictions/README.md) and the [kill switch](../hitl/kill-switch.md).
- High JIT log volume ties back to [audit logging](../audit-logging/decision-debug-logging.md).
