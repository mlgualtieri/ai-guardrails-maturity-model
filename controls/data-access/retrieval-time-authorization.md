# Retrieval-time authorization checks

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Every data retrieval is authorized against the *requesting user's* own entitlements, at the moment of retrieval, and not merely against the agent's overall scope. Within the data an agent is permitted to reach, individual users hold different permissions, and the retrieval path must enforce each requester's authorization so that a shared agent never returns data the specific requester is not entitled to see.

## Why it sits at this level — L1 scopes the agent; L2 scopes the user

[Data access scoping](data-access-scoping.md) (L1) bounds what the *agent* can reach, a static, design-time boundary that is identical for every request. This L2 control bounds retrieval to the *requester and their authorization*, a dynamic, per-request check. They are not the same, and **satisfying L1 does not satisfy L2** — an agent can be perfectly scoped to the right data set and still leak, if it returns one user's records to another user because per-user entitlement was not enforced at retrieval. **L1 scopes the agent; L2 scopes the user.**

Per-user, per-request authorization enforcement requires real machinery, in that the retrieval path must know the requester's identity and entitlements and apply them before returning results. That is deployment-specific engineering beyond the day-one floor of simply bounding the agent's reach, which is why it sits at L2.

## Where it bites — across common architectures (not just RAG)

The unifying principle is that whenever an agent retrieves data on behalf of a user, the retrieval must be filtered by that specific user's authorization, regardless of storage or retrieval technology. Examples that developers actually encounter include the following:

- **Direct database queries** — results are scoped to the requester's rows: `SELECT ... WHERE user_id = :requester`, not `SELECT *`. This is the oldest access-control bug, now with an AI issuing the query.
- **API / microservice calls** — the requester's identity is passed so that the downstream API applies *that user's* authorization, rather than calling with a broad service account that returns everything.
- **File / document stores** — per-user file ACLs are honored (SharePoint, Drive, shared file systems), rather than relying on a blanket service identity that can read all files.
- **Multi-tenant SaaS** — tenant isolation is enforced at retrieval so that tenant A's query can never reach tenant B's data.
- **RAG / vector search (one case among these)** — the vector index must apply the requester's authorization, and because a vector store frequently ends up with broader access than the source it was built from, ACL inheritance must be deliberately enforced.

## What implementing it looks like

Requester identity and entitlements are passed into the retrieval call, and authorization is enforced *before* results are returned (for vector search, before or during ranking, not as a leaky post-filter). The retrieval layer's ACLs are kept synchronized with the source system, and retrieval is isolated by tenant in multi-tenant cases. As a practitioner awareness note, a retrieval index (vector or otherwise) does not automatically inherit the permissions of the data it was built from, and that inheritance must be enforced deliberately.

## Source grounding

- **Primary — NIST SP 800-53 AC-3 (Access Enforcement):** enforces approved authorizations at *every layer that makes an access decision*, and the retrieval layer is such a layer, so this control is AC-3 applied there. **AC-4 (Information Flow Enforcement)** governs what data flows into the response context. Grounding in AC-3 rather than an AI-specific source is the point, since this is broken-access-control, decades old, applied to AI retrieval.
- **Supporting (AI/RAG-specific example) — OWASP LLM08:2025 (Vector and Embedding Weaknesses):** vector stores require the same access control as any data store, and per-user authorization must be enforced at the retrieval layer, not only the application layer.
- **Category anchor:** CISA/NCSC secure AI guidance.

See [reference/sources.md](../../reference/sources.md) for full source details.

## Honest limits and trade-offs

- **You might expect this to be the same control as data access scoping — here is why it is separate, and what that means for you.** The two are materially distinct. L1 (2.1) bounds what the *agent* can reach (static, design-time, the same for every request), while this L2 control bounds retrieval to the *requester* and their authorization (dynamic, per-request). Satisfying L1 does not satisfy L2, because a correctly-scoped agent still leaks if it returns user A's data to user B. The distinction collapses only in single-user or uniform-entitlement deployments, and it earns its keep whenever an agent is a shared front-end over data with per-user ACLs. Treat them as two controls to satisfy separately: the RAG enforcement carried up from L1 scoping is absorbed here rather than spun out as a separate one.
- **Don't assume a retrieval index inherits the source system's permissions.** It does not, and this is the load-bearing awareness note. A retrieval index (vector or otherwise) does not automatically inherit the permissions of the data it was built from, and a vector store frequently ends up with broader access than its source. You must enforce that inheritance deliberately.

## Related controls

- [Data access scoping](data-access-scoping.md) — this control matures it from agent-scope (L1) to requester-authorization (L2).
- [Classification-aware access enforcement](classification-aware-access-enforcement.md) — sibling L2 control; classification-based enforcement versus this control's identity-based enforcement.
- [Identity & Access](../identity-access/README.md) (controls 1.1–1.3) — the requester and agent identities this check depends on.
