# 2. Data Access & Document Protections

**Source.** Per-control primaries are the NIST SP 800-53 AC family (AC-3, AC-4, AC-6) plus OWASP LLM08:2025 (Vector and Embedding Weaknesses). Category anchor: CISA/NCSC secure AI guidance. Microsoft 365 Copilot — Data & Privacy is a vendor source and is demoted to illustrative (used as an example, not as an authority).

## What this category defends against

This category governs what data an AI system can reach and whose authorization is enforced when it reaches it. The risks are an over-broad agent that becomes a permissions-bypass path (surfacing data the requesting user could not otherwise access), a large blast radius if the agent is compromised, and a shared agent leaking one user's data to another because per-requester entitlements were never enforced at retrieval. It applies regardless of how the agent reaches data — RAG/vector retrieval, direct database connection, API, or file mount.

## Progression: L1 scopes the agent → L2 scopes the requester

This category does not decompose across the four maturity levels as a single clean crawl/walk/run table, but it has a clear progression across two levels:

| Level | What is bounded | Nature of the check |
| --- | --- | --- |
| **L1** (Basic) | What the *agent* can reach at all — bound to exactly the data required, no more, no less | Static, design-time boundary, identical for every request |
| **L2** (Intermediate) | Retrieval to the *requester* and their authorization — including classification clearance and per-record entitlement | Dynamic, per-request check applied at the retrieval boundary |

The key point for the practitioner is that satisfying L1 does not satisfy L2. An agent can be correctly scoped to the right data set and still leak, because it returns one user's records to another user when per-user entitlement was not enforced at retrieval. **L1 scopes the agent; L2 scopes the user.**

## Controls in this category

- [Data access scoping](data-access-scoping.md) — `[Both / L1 / Low]` — the agent is scoped to exactly the data required, no more, no less, regardless of retrieval mechanism.
- [Classification-aware access enforcement](classification-aware-access-enforcement.md) — `[Both / L2 / Low]` — the AI honors the organization's data classification at its retrieval boundary, refusing content above the requester's clearance.
- [Retrieval-time authorization checks](retrieval-time-authorization.md) — `[Both / L2 / Low]` — every retrieval is authorized against the requesting user's own entitlements at the moment of retrieval.
