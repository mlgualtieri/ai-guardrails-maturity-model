# Reference Library

This library is organized by authority tier to make the sourcing hierarchy explicit. **Standards
bodies and neutral authorities are the primary grounding throughout the model.** Vendor and
practitioner sources appear only as *supporting / illustrative* — real-world implementations of
principles the standards define — and never as the sole authority for a control.

This reflects the source-hierarchy rule the model follows: a control is grounded in a standards body
or neutral authority wherever one supports it, and a vendor reference is used only to illustrate that
principle in practice, never as the control's authority. Every URL should be verified live, as some
are recent and may move.

A note on tools versus sources: the implementations named on the control pages (Presidio, Lakera,
Rebuff, E2B, Daytona, LangSmith, Socket, and so on) are **not** citations. They appear only in the
"What implementing it looks like" sections of control pages, never here.

---

## Primary authorities (standards bodies & neutral references)

| Resource | Organization | URL |
|----------|--------------|-----|
| NIST AI Risk Management Framework (AI RMF 1.0) + Generative AI Profile (NIST AI 600-1) | NIST | https://airc.nist.gov/Home |
| NIST SP 800-53 Rev. 5 (Security & Privacy Controls) | NIST | https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final |
| NIST SP 800-207 (Zero Trust Architecture) | NIST | https://csrc.nist.gov/pubs/sp/800/207/final |
| NIST SP 800-34 (Contingency Planning) | NIST | https://csrc.nist.gov/pubs/sp/800/34/r1/final |
| NIST SP 800-61 (Computer Security Incident Handling) | NIST | https://csrc.nist.gov/pubs/sp/800/61/r2/final |
| NIST SP 800-161 (Supply Chain Risk Management) | NIST | https://csrc.nist.gov/pubs/sp/800/161/r1/final |
| NIST SP 800-218 (Secure Software Development Framework / SSDF) | NIST | https://csrc.nist.gov/pubs/sp/800/218/final |
| NIST SP 800-190 (Application Container Security) | NIST | https://csrc.nist.gov/pubs/sp/800/190/final |
| OWASP Top 10 for LLM Applications (2025) | OWASP | https://owasp.org/www-project-top-10-for-large-language-model-applications/ |
| OWASP MCP Top 10 (2025) | OWASP | https://owasp.org/www-project-mcp-top-10/ |
| OWASP LLM Prompt Injection Prevention Cheat Sheet | OWASP | https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html |
| OWASP Non-Human Identity Top 10 | OWASP | https://owasp.org/www-project-non-human-identities-top-10/ |
| OWASP GenAI Incident Response Guide | OWASP | https://genai.owasp.org/resource/genai-incident-response-guide- |
| OWASP Docker Security Cheat Sheet | OWASP | https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html |
| MITRE ATLAS (Adversarial Threat Landscape for AI Systems) | MITRE | https://atlas.mitre.org/ |
| MITRE ATT&CK — T1611 (Escape to Host) | MITRE | https://attack.mitre.org/techniques/T1611/ |
| Guidelines for Secure AI System Development | CISA / NCSC | https://www.cisa.gov/resources-tools/resources/guidelines-secure-ai-system-development |
| MAESTRO (Agentic AI Threat Modeling Framework) | Cloud Security Alliance (CSA) | https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro |
| ISO/IEC 27001:2022 (Information Security Management) | ISO/IEC | https://www.iso.org/standard/27001 |
| ISO 22301 (Business Continuity Management) | ISO | https://www.iso.org/standard/75106.html |
| FinOps for AI Overview / Tokenomics | FinOps Foundation | https://www.finops.org/framework/frameworks-for-ai/ |
| OpenTelemetry Semantic Conventions for GenAI | OpenTelemetry / CNCF | https://opentelemetry.io/docs/specs/semconv/gen-ai/ |

Additional NIST controls and profiles referenced throughout (within the publications above): the
SP 800-53 AC family (AC-2, AC-3, AC-4, AC-5, AC-6), the AU family (AU-2, AU-3, AU-3(3), AU-6, AU-9,
AU-11, AU-12), the CP family (CP-2, CP-4, CP-7, CP-10), the SC family (SC-4, SC-5, SC-7, SC-39), CM-7,
SI-2, SI-4, and RA-2 / RA-5; FIPS 199 and NIST SP 800-60 for security categorization; and NIST SP
800-92 for log management.

---

## Peer-reviewed / research citations (for specific claims)

| Resource | Venue | URL |
|----------|-------|-----|
| Bypassing LLM Guardrails: Empirical Analysis of Evasion Attacks (up-to-100% detector evasion) | arXiv 2504.11168 | https://arxiv.org/abs/2504.11168 |
| RouteLLM (model-routing cost / quality benchmarks) | arXiv 2406.18665 | https://arxiv.org/abs/2406.18665 |

Also drawn on for specific claims: NSA / CISA / FBI 2025 joint guidance on AI Data Security (for
immutable logging and log-all-inference-activity); and published faithfulness-vs-factuality
research in RAG fact-checking (for the grounding control's honest framing).

---

## Supporting / illustrative sources (vendor & practitioner)

These vendor and practitioner references serve as supporting material — real-world implementations,
product references, and threat write-ups — rather than as primary authority. Each illustrates a
principle that a standards body or neutral authority grounds elsewhere in the model.

| Resource | Organization | Role in the final model |
|----------|--------------|-------------------------|
| Building Effective Agents | Anthropic | Agent tool-design practice (Tool & Command category) |
| Model Context Protocol (MCP) | Anthropic / open standard | Protocol reference (multi-agent, supply chain) |
| Taxonomy of Failure Modes in AI Agents | Microsoft AI Red Team | Threat reference (Memory & State) |
| Microsoft 365 Copilot — Data, Privacy & Security | Microsoft | Illustrative implementation (Data Access) — demoted |
| Securing AI Agent Identities | CyberArk | Illustrative implementation (Identity) — demoted |
| DR for AI Pipelines: RTO/RPO Guide | Axrail | Illustrative (Resilience) — demoted; primary is NIST SP 800-34 |
| 2025 State of AI Cost Governance | Mavvrik | Illustrative (COGS) — demoted; primary is FinOps Foundation / OWASP LLM10 |
| AI Incident Response Plan | Pivot Point Security | Illustrative (Resilience / IR) — demoted; primary is NIST SP 800-61 |
| Lessons from 2025 on Agents and Trust | Google Cloud OCTO | Illustrative (Resilience) — demoted |
| CVE-2025-68664 "LangGrinch" (LangChain serialization) | NVD / GitHub Advisory | Evidence for the supply-chain threat note (9.1) |
| Okta for AI Agents / Microsoft Entra Agent ID | Okta / Microsoft | Agent-identity implementations (JIT, 1.4) |

---

## How sources are used per control

Every control's detail page carries a **Source grounding** section naming its own primary source
(verified against the actual source content) and any supporting sources, ordered primary-first.
Where several recognized authorities independently specify a similar control, the page cites more
than one and notes what each adds, since convergence across bodies is a strength rather than
padding. Claims that assert a number, a named framework, or a specific attack technique carry a
claim-specific citation to the source of *that* claim.

Return to the [main guide](../maturity-model.md) or the [README](../README.md).
