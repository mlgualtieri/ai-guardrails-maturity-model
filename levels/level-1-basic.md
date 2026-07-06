# Level 1 — Basic

**Typical org profile.** AI deployment is early, and the security team is only beginning to engage
with AI use cases. Controls are reactive rather than systematic. A chatbot or a first internal
assistant may be in use, but agentic, tool-using systems are not yet the norm. The objective at
this level is a coherent *floor* — the small set of controls below which an AI deployment should not
be running in the first place.

Navigation: **Level 1** · [Level 2 →](level-2-intermediate.md) · [Level 3](level-3-advanced.md) ·
[Level 4](level-4-leading-edge.md) · [Full catalog](../maturity-model.md#full-catalog)

---

## The coherent posture at Level 1

An organization at Level 1 can affirmatively say:

- **Every AI agent runs under its own bounded identity, and that identity carries no idle elevated
  privilege.** Agents are not attached to pre-existing privileged accounts, and a single identity is
  never shared across agents. Any elevated rights are task-scoped rather than standing.
- **Each agent can reach only the data its purpose requires — no more, no less** — regardless of how
  it retrieves that data, and it cannot surface data the requesting user is not themselves authorized
  to see.
- **The model cannot emit secrets, PII, or universally harmful content in its responses.** Output is
  scanned for sensitive data before it leaves, and universally harmful content — abuse, harassment,
  hate, obscene material, dangerous or violent recommendations — is filtered at both the provider and
  the application level.
- **What the AI did can be reconstructed and debugged.** The deployment captures enough structured
  metadata about inputs, outputs, tool calls, and retrieved context to reconstruct every decision,
  with sensitive data scrubbed by default and verbatim retention treated as a conscious, justified
  exception. Those logs are held in tamper-evident storage, separated from the system that generates
  them, and retained for as long as the applicable compliance frameworks require.

This is the floor: bound the identity, bound the data, bound what can leak, and keep a trustworthy
record. The L1 cluster interlocks — identity establishes *who*, logging establishes *what they did*,
and immutable storage is what makes that record dependable. Read access to the log store is itself a
least-privilege concern, which is why the identity and logging controls reinforce each other.

---

## Controls that first appear at Level 1

Seven controls, grouped by category. Each links to its detail page.

### [Identity & Access Controls](../controls/identity-access/README.md)

- [Scoped service accounts (NHIs)](../controls/identity-access/scoped-service-accounts.md) — *Both · Low* — every agent gets its own least-privilege non-human identity.
- [No standing admin access](../controls/identity-access/no-standing-admin-access.md) — *Both · Low* — no permanent, always-on elevated privilege on agent identities.

### [Data Access & Document Protections](../controls/data-access/README.md)

- [Data access scoping](../controls/data-access/data-access-scoping.md) — *Both · Low* — the agent is scoped to exactly the data it needs, regardless of retrieval mechanism.

### [Output Filtering & Content Controls](../controls/output-filtering/README.md)

- [PII / sensitive data scrubbing](../controls/output-filtering/pii-sensitive-data-scrubbing.md) — *Both · High* — the output-side guard against leaking secrets and PII.
- [Toxicity and harmful-content filters](../controls/output-filtering/toxicity-harmful-content-filters.md) — *Both · Medium* — block universally harmful output, largely provider-default.

### [Audit Logging & Observability](../controls/audit-logging/README.md)

- [Decision & debug logging (scrub-by-default)](../controls/audit-logging/decision-debug-logging.md) — *Both · High* — sufficient metadata to reconstruct and debug decisions.
- [Immutable log storage & retention](../controls/audit-logging/immutable-log-storage.md) — *Both · Low · Process* — append-only, tamper-evident, separated storage, kept as long as required and no longer.

---

## Key gap to Level 2

At Level 1 the identity is bounded and the data is scoped for the *agent*, but nothing yet bounds
retrieval to the *individual requester*, screens *inputs* for attacks or policy violations, or
governs the *tools* an agent can use. The move to [Level 2](level-2-intermediate.md) is the shift
from having a floor to running AI in production with controls that match a real agentic workflow:
per-requester authorization at retrieval, prompt-injection detection, tool allowlisting and
sandboxing, a kill switch, human approval before consequential actions, and cost visibility. The
single biggest gap is that a correctly scoped agent at L1 can still leak one user's data to another,
because per-user entitlement is not yet enforced at retrieval time. That is L2 work.
