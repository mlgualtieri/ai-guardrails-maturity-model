# Data access scoping

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: Low]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

The agent is scoped to exactly the data required for its purpose — **no more, no less** — regardless of how it reaches that data (RAG/vector retrieval, direct database connection, API, or file mount). Access is bounded to the authorized data stores, and the agent cannot surface data that the requesting user is not themselves authorized to access.

## Why it sits at this level

This is the mechanism-agnostic floor for data protection, and it rests on three reasons that are captured together because together they are stronger than any one alone:

1. **Effectiveness.** More data is not better; it degrades the agent. Over-broad reach introduces noise, worse retrieval relevance, and weaker grounding. Scoping is therefore a *capability* control, not only a security one. *(This reason is the model's own contribution, layered on the access-control grounding below, and it is not attributed to the standards.)*
2. **Breach blast radius.** Whatever the agent can reach is exposed if the agent is compromised. Its data reach *is* its exposure surface, so minimizing it is blast-radius reduction, the least-privilege principle from the Identity cluster applied to data.
3. **Access-control correctness.** The AI must not become a permissions-bypass path that surfaces data the requesting user could not otherwise access.

"No more, no less" captures all three at once: *no more* serves effectiveness and blast-radius reduction, and *no less* serves the agent actually being able to do its job.

This is the mechanism-agnostic principle. Its RAG-specific enforcement — authorization at the retrieval/vector layer rather than only the application layer, tenant isolation, and embedding controls — is a more advanced instance at L2, connecting to [retrieval-time authorization checks](retrieval-time-authorization.md).

## What implementing it looks like

The agent's reachable stores are bounded to the authorized set, and retrieval is enforced so that it cannot return data above the requesting user's authorization. Scope is reviewed for over-provisioning (the "no more" test) and for under-provisioning (the "no less" test). This discipline applies regardless of the retrieval mechanism.

## Source grounding

- **Primary — NIST SP 800-53 AC-3 (Access Enforcement):** enforces approved authorizations for logical access to information and resources at the system, application, and service level, so that every layer that makes an access decision enforces approved authorization rather than defaulting to implicit trust. This directly grounds both the general principle and the reason L2 retrieval-layer enforcement is a valid specialization.
- **Primary — NIST SP 800-53 AC-6 (Least Privilege):** applies to processes acting on behalf of users, with access no higher than necessary. This grounds "no more than needed" and the blast-radius reasoning.
- **Supporting (AI-specific) — OWASP LLM08:2025 (Vector and Embedding Weaknesses):** vector/retrieval stores are data stores that require the same access control as any database, and the canonical failure is a RAG store surfacing another user's records when per-user authorization is not enforced. This is the primary grounding for the L2 specialization and is supporting here for the correctness reason.
- **Category anchor:** CISA/NCSC secure AI guidance.

See [reference/sources.md](../../reference/sources.md) for full source details.

## Honest limits and trade-offs

- **This control is mechanism-agnostic, by design.** It states the scoping principle so that you get a real control at the floor no matter your architecture — RAG, direct database connection, API, or file mount. RAG-specific enforcement at the retrieval/vector layer is a more advanced instance that sits at L2, not a substitute for the floor.
- **Don't read the effectiveness reason as an access-control claim.** It is not one, and it is not presented as one. The effectiveness reason — that over-scoping degrades the agent — is the model's own contribution and is labeled as such. It is layered on the access-control grounding (AC-3/AC-6) rather than attributed to the standards.

## Related controls

- [Retrieval-time authorization checks](retrieval-time-authorization.md) — matures this control from agent-scope (L1) to requester-authorization (L2).
- [Classification-aware access enforcement](classification-aware-access-enforcement.md) — a sibling L2 control that adds a classification dimension within reachable data.
- Identity: [scoped service accounts](../identity-access/scoped-service-accounts.md) and the broader [Identity & Access](../identity-access/README.md) cluster — the least-privilege principle applied to identity, of which this control is the data-reach counterpart.
