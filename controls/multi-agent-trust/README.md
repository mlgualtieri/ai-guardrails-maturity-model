# 13. Multi-Agent Trust

**Source.** OWASP MCP Top 10 + NIST AI RMF. Supporting: CSA MAESTRO (multi-agent threat modeling).

## What this category defends against

This category defends against implicit trust inside agent chains — where agent A calls sub-agent B, which in turn calls sub-agent C. The failure mode is trusting a sub-agent simply because it is already in the chain: inheriting the caller's authorization, skipping identity verification at each hop, and handing sub-agents more privilege than their subtask requires. A compromised or spoofed sub-agent should be contained by identity, per-hop authorization, and least privilege rather than trusted by its position in the chain.

## A single control

This is a single control. Sub-agent trust covers four concerns at once — per-hop authorization re-validation, sub-agent identity verification, minimal footprint per agent, and MCP-based tool/resource authorization — because they are one discipline, not four. It does not decompose across maturity levels; it sits at Level 4 as one control that applies the authorization and least-privilege principles established elsewhere (Identity and Data Access) to agent-to-agent relationships, and points back to those categories rather than re-deriving them. One related concern — not blindly trusting a sub-agent's output — is handled in category 3 (indirect prompt injection), where a sub-agent's output is treated as another untrusted external input, so it is not duplicated here.

## Controls in this category

- [Sub-agent trust](sub-agent-trust.md) — `[Agentic / L4 / Low]`
