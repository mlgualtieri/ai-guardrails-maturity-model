# 9. Supply Chain & Model Integrity

**Source.** OWASP MCP Top 10 (MCP04, Software Supply Chain Attacks & Dependency Tampering) and MITRE ATLAS (ML Supply Chain Compromise). Supporting: NIST SP 800-161 (Supply Chain Risk Management) and NIST SSDF (SP 800-218); NCSC supply-chain guidance.

## What this category defends against

An AI system is only as trustworthy as what it pulls in. Two distinct things enter the trust boundary: the third-party resources an agent connects to at runtime (tools, MCP servers, plugins, libraries), and the model artifact itself (weights and adapters). This category defends against attackers who compromise either surface, whether by poisoning or tampering with a dependency the agent relies on, or by substituting or altering the weights an organization self-hosts.

This category is deliberately **supply-chain security** — establishing the provenance and trustworthiness of what you bring in — and **not** general **vulnerability management** (the scan-and-patch cycle for known CVEs). Vulnerability management is universal security practice you should already apply to AI frameworks like any other dependency, so it is not carried here as an AI-specific guardrail. That is why you will not find a standalone "vulnerability monitoring for AI frameworks" control — its one AI-specific wrinkle survives as a note under 9.1 — and why there is no separate "fine-tuning data integrity" control, which is niche to self-training organizations and covered elsewhere.

## Crawl/walk/run

Two distinct supply-chain surfaces at increasing sophistication. Each covers a surface the other does not: vetting what you connect does nothing for a tampered weights file, and verifying the model artifact does nothing for a poisoned dependency.

| Level | Control | Progression logic |
|-------|---------|-------------------|
| L2 | 9.1 Pre-integration vetting of agent resources | Vet what you connect — tools, MCP servers, plugins, libraries. Baseline hygiene at the point of integration, once an agent connects external resources. |
| L4 | 9.2 Model provenance verification | Verify the model artifact itself — the weights. Needed only by the self-hosting subset, hence L4. |

## Controls in this category

- [9.1 Pre-integration vetting of agent resources](pre-integration-vetting-agent-resources.md) — `[Agentic / L2 / Low]`
- [9.2 Model provenance verification](model-provenance-verification.md) — `[Both / L4 / Low]`
