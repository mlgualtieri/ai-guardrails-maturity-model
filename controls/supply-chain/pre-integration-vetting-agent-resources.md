# Pre-integration vetting of agent resources

`[Applies To: Agentic / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Before connecting a tool, MCP server, plugin, or library to an agent, verify its source, integrity, and requested permissions, and keep verifying as versions change. Every third-party resource an agent connects to should be treated as a trust boundary rather than a convenience. If you have not been vetting what your agents pull in, this is the gap to close first — it covers the supply chain of everything an agent connects to at runtime.

## Why it sits at this level

Vetting applies once an agent connects external tools or resources, which makes it agentic and places it at the point of integration as baseline hygiene. It anchors the Supply Chain progression: 9.1 vets what you connect at L2, and then [9.2 verifies the model artifact](model-provenance-verification.md) at L4.

## Why it is a real and current gap

MCP changed the agent threat model. Agents now discover and pull tools at runtime, so every server they reach is a trust boundary. The attacks are live and accelerating:

- A critical remote-code-execution vulnerability (CVE-2025-6514, CVSS 9.6) hit an MCP package with 437,000+ downloads.
- The first malicious MCP server was caught in the wild in 2025.
- Coordinated npm and PyPI campaigns in 2026 pushed hundreds of malicious versions of legitimate packages, targeting the developer and CI/CD pipelines that agents depend on.

OWASP now catalogs this as MCP04 (Software Supply Chain Attacks & Dependency Tampering).

## What implementing it looks like

The guidance here is practices-first, not SBOM-first. An SBOM or AIBOM is one option rather than the anchor, because a generated-and-ignored SBOM protects nothing.

- **Maintain a list of installed packages/resources and their versions.** Keep a dependency inventory in whatever format fits the workflow. NCSC guidance is explicit that the inventory can take whatever form suits the organization, and that reducing dependency size and complexity where possible is itself a mitigation. An SBOM or AIBOM is one way to maintain this list, useful if you already produce it, but it is not the primary recommendation.
- **Use version pinning.** Pin exact versions and use lockfiles so that an agent or CI pipeline cannot silently pull a newly-published malicious version. Multiple 2026 incidents were mitigated specifically by pinning to known-clean releases.
- **Use package aging / cooldown.** Do not install a package, package version, or MCP server until it has been public for a minimum period, commonly days to roughly two weeks. Malicious versions are typically detected and pulled quickly, often within minutes to hours of publication, so waiting before adopting a new release means that most malicious versions are removed before they ever reach you. This pairs with pinning: pinning blocks silent auto-updates, and aging protects the moment you deliberately upgrade, so that you are never on the bleeding edge where fresh attacks live. It is increasingly enforceable automatically through a minimum-age or cooldown policy in supply-chain tooling.
- **Use checksum / integrity verification.** Verify that what you pulled matches the trusted release, and prefer signed releases and verified publishers where they are available.
- **Use supply-chain security tooling that watches the registry surface.** Tools that analyze package behavior — install scripts, network calls, obfuscation, new-maintainer signals — catch malicious packages that CVE-based scanners miss. The large majority of malicious packages never receive a CVE at all (industry tooling estimates put it at roughly 90–95%; treat this as a vendor-reported figure, not an authoritative statistic), which is the entire reason behavioral detection matters alongside CVE scanning. Behavioral malicious-package detection with install-time interception and minimum-age policies (for example Socket) targets exactly this case, while CVE- and vulnerability-focused scanners (Snyk, GitHub Dependabot), CI/CD hardening (StepSecurity), and open-source scanning and inventory tooling (Trivy, Grype, Syft) cover complementary ground.
- **Review requested permissions against function.** A resource requesting more scope than its job requires is a red flag. This ties to least-functionality — see [tool allowlisting](../tool-command-restrictions/tool-allowlisting.md).

**Two different problems — you need both.** Vulnerability scanning (Snyk, Dependabot, `npm audit`) checks dependencies against known CVEs, which are accidental flaws in legitimate code. Malicious-package detection, through behavioral analysis, catches intentional supply-chain attacks that never appear in CVE databases. This control is primarily about the second, the malicious or tampered case. Generic CVE scanning is more the job of [model provenance verification](model-provenance-verification.md) and of ordinary vulnerability management (see the note below).

## Honest limits and trade-offs

**Vetting cannot catch the "rug pull" — it is necessary but not sufficient.** Pre-integration vetting catches known-bad packages, typosquats, and obvious misconfigurations, but it cannot catch a resource that passes review and then turns malicious. This is a documented MCP attack: a server looked harmless when it was approved, then changed its tool description on a later run. What you approve today may not be what runs tomorrow. This control is therefore the front gate, and it must be paired with runtime containment that limits a resource which goes bad after vetting: tool restrictions ([category 5](../tool-command-restrictions/README.md)), monitoring, and least-privilege scoping. Package aging mitigates the initial poisoning window, and runtime controls handle post-approval mutation.

**Do not mistake an SBOM for the control.** SBOMs are often generate-and-ignore compliance artifacts, and one sitting in your pipeline protects nothing on its own. The load-bearing practices here are the inventory, pinning, aging, integrity verification, and behavioral tooling. If an SBOM is how you already hold the inventory, that is fine — just do not let producing it stand in for actually doing the vetting.

## Related controls

- [9.2 Model provenance verification](model-provenance-verification.md) — the next surface in the Supply Chain progression: verifying the model artifact itself.
- [Tool allowlisting](../tool-command-restrictions/tool-allowlisting.md) — the permission-versus-function review ties to least functionality (5.1).
- [Tool & Command Restrictions](../tool-command-restrictions/README.md) — the runtime containment that handles the rug-pull limit this control cannot.
- [Sub-agent trust](../multi-agent-trust/sub-agent-trust.md) — the same trust-boundary discipline applied to agent-to-agent and MCP connections.

> **Note — apply normal vulnerability management to AI frameworks too.** Separate from this control's provenance and trust focus, the AI frameworks you build on (LangChain, LangGraph, LlamaIndex, and similar) have an active and severe CVE history and should be covered by your existing vulnerability management. Monitor advisories, pin versions, and patch reachable criticals quickly, since exploitation windows are now measured in hours. This is deliberately not a standalone control, because generic vulnerability management is universal security practice rather than an AI-specific guardrail. The one AI-specific wrinkle worth knowing is that, in these frameworks, prompt injection can be the delivery vector for a framework exploit — for example, the LangChain serialization flaws trigger through LLM-output fields — so framework-processed LLM output should be treated as untrusted wherever it is serialized or deserialized.

## Source grounding

- **Primary — OWASP MCP Top 10, MCP04:2025 (Software Supply Chain Attacks & Dependency Tampering).** Covers third-party MCP components in trusted execution paths, with mitigations including pinning, provenance, and pre-pull analysis. Supporting from the same body: MCP09 (Shadow MCP Servers).
- **Supporting — NIST SP 800-161 (Supply Chain Risk Management)** and **NIST SSDF (SP 800-218)** for pinning, provenance, and integrity verification; **NCSC supply-chain guidance** for the dependency inventory in any suitable format, reducing dependency complexity, and the rapid-detection rationale that makes package aging work; **MITRE ATLAS (ML Supply Chain Compromise)**.
- **Category anchor — OWASP MCP Top 10 / MITRE ATLAS.**

See [reference/sources.md](../../reference/sources.md) for full citations.
