# Sub-agent trust

`[Applies To: Agentic / Maturity Level: L4 / Alert Fatigue Risk: Low]`

**Level:** [Level 4 — Leading Edge](../../levels/level-4-leading-edge.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

In a chain of agents (A calls B, which calls C), do not implicitly trust a sub-agent. Verify *who it is* and re-validate *what it is allowed to do* at each hop, and give each sub-agent only the privilege its subtask needs. A compromised or spoofed sub-agent should be contained by identity, per-hop authorization, and least privilege, rather than trusted because it happens to be in the chain.

This is a single control covering four related concerns as one discipline. Rather than restating principles the model establishes elsewhere, it applies them to agent-to-agent relationships and cross-references their home categories.

## What this control preserves

- **Per-hop authorization re-validation and sub-agent identity verification** are the two halves of one idea: verify identity, re-check authorization at each hop, and never inherit trust from the calling agent. Authentication between agents is still maturing, so use signed requests, shared-secret validation, or platform-native agent identity tokens where available.
- **Minimal footprint per agent** is least privilege (see [Identity & Access](../identity-access/README.md), and specifically [scoped service accounts](../identity-access/scoped-service-accounts.md)) applied to sub-agents. It is folded in here as the least-privilege dimension rather than restated as a separate control.
- **MCP-based tool/resource authorization** is a named mechanism — an emerging standard for performing per-hop authorization — rather than a separate control, in the same way sensitivity labels are a technology rather than a control.

### Where the fourth idea went

"Don't blindly trust a sub-agent's *output*" is handled in category 3, [indirect prompt injection protection](../prompt-input-filtering/indirect-prompt-injection-protection.md): a sub-agent's output is simply another untrusted external input to the calling agent. It is not duplicated here.

## Why it sits at this level

Multi-agent chains are an emerging, leading-edge deployment pattern, and trust between autonomous agents is a genuinely new problem that CSA MAESTRO models explicitly. The control is AI-specific because agent-to-agent trust is new, yet the mechanisms it relies on are the same authorization and least-privilege principles the model establishes for identity and data access. That is why it is expressed as one control that points back to those categories rather than re-deriving them.

## What implementing it looks like

- Give each sub-agent a distinct, verifiable identity and confirm it at every hop, and do not accept a sub-agent's claimed identity on the strength of the caller's.
- Re-run the authorization check for each requested tool or resource at each hop, scoped to the current subtask, rather than letting a downstream agent inherit the caller's permissions.
- Scope each sub-agent's privileges to only what its subtask requires, following the same least-privilege discipline as scoped service accounts.
- Where the platform supports it, use signed requests, shared-secret validation, or platform-native agent identity tokens, and treat MCP-based tool/resource authorization as the emerging standard mechanism for expressing and enforcing these checks.

## Source grounding

- **Primary — OWASP MCP Top 10** — agent/tool authorization; the authoritative grounding (and category anchor) for trust in agent and tool chains.
- **Supporting — NIST SP 800-53 AC-3 / AC-4 / AC-6** — authorization and least privilege, applied per hop rather than once at the top of the chain.
- **Supporting — NIST SP 800-207** — per-request verification, the zero-trust basis for re-validating rather than inheriting trust.
- **Supporting — CSA MAESTRO** — multi-agent threat modeling; frames trust between autonomous agents as a distinct problem.

## Honest limits and trade-offs

You might expect this to be several separate controls. It is deliberately one, because it applies principles you have already met elsewhere to agent-to-agent chains, and knowing that changes how you implement it. Per-hop re-authorization is the same authorization principle you build for retrieval-time authorization and Identity, now applied so that authorization is re-validated at each hop and never inherited; if you have that principle in place already, extend it to your agent chains rather than standing up something new. Minimal footprint is least privilege applied to sub-agents — the same discipline as scoped service accounts under Identity. MCP-based authorization is a named mechanism for expressing those checks, not a separate discipline to invent. And the one genuinely distinct idea — that even an authorized sub-agent's *output* should be treated as untrusted — is the same problem as untrusted tool-output and indirect injection, so you handle it in category 3 (3.3), not here. The practical takeaway: do not build this control from scratch. Wire your existing identity, least-privilege, and injection defenses across each hop of the chain, and the four concerns come together as one coherent job.

## Related controls

- [Identity & Access](../identity-access/README.md), and specifically [scoped service accounts](../identity-access/scoped-service-accounts.md) — least privilege applied to sub-agents.
- [Retrieval-time authorization](../data-access/retrieval-time-authorization.md) — the per-request authorization principle this control applies to agent-to-agent chains.
- [Indirect prompt injection protection](../prompt-input-filtering/indirect-prompt-injection-protection.md) — handles sub-agent *output* as untrusted external input.
- [Pre-integration vetting of agents and resources](../supply-chain/pre-integration-vetting-agent-resources.md) — MCP authorization also relates to MCP vetting.
