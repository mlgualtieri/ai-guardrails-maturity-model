# Immutable log storage & retention

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: Low / Process]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Hold the audit log correctly for its whole life: protect it so it cannot be tampered with, and keep it for the right length of time. Two halves of one job. Store audit logs in append-only, tamper-evident storage, held separately from the AI system that generates them, so that a compromised agent — or a compromised admin path — cannot modify or delete its own audit trail; integrity is verifiable, for example via cryptographic hashing, and tampering attempts are detectable. Then retain those logs as long as applicable requirements demand, and no longer than there is a basis to: define the retention period per applicable framework, enforce it, and dispose of logs on schedule.

## Why it sits at this level

Logging (decision & debug logging) is only trustworthy if the logs themselves cannot be silently altered, because otherwise the entire audit trail, and every control that relies on it for evidence, is undermined. Immutability is what makes the L1 logging substrate *dependable*, so it belongs at the floor alongside it. Retention sits here for the same reason it is a floor obligation rather than an advanced capability: any organization subject to SOC 2, HIPAA, PCI-DSS, or FedRAMP already carries log-retention duties, and AI logs fall under the same rules as application and access logs. Alert fatigue is Low, because the protection half is infrastructure configuration rather than a source of ongoing alerts, and the retention half is a process and policy control rather than a technical alert stream — which is why the control also carries the `Process` tag.

## What implementing it looks like

For protection: append-only / WORM storage (S3 Object Lock, Azure Blob immutability, write-once media); a log store isolated from the audited system and from general admin access; cryptographic integrity (signed hashes) so that modification is detectable even if access controls are bypassed; alerting on tamper attempts; and dedicated log-management access held separate from system administration.

For retention: a defined retention schedule per applicable framework; automated enforcement and disposal; and long-term retrievability where multi-year retention applies (format migration, readable archives).

**Threat-model note.** The most common real-world failure is not missing log ACLs; it is *overly broad administrative privileges that bundle log-management access as a side effect*. This links the control directly to the Identity controls, because separating and restricting who can touch the log store is the same least-privilege principle as scoped identities, no standing admin, and read/write separation. The L1 cluster — identity, logging, log protection and retention — interlocks.

## Source grounding

- **Primary (protection) — NIST SP 800-53 AU-9 (Protection of Audit Information).** The base control protects audit information and audit tools from unauthorized access, modification, and deletion, and alerts on tampering. Its named enhancements map directly onto this control:
  - **AU-9(1) Hardware Write-Once Media** — append-only / immutable storage.
  - **AU-9(2) Store on Separate Physical Systems** — separation ensures a compromise of the audited system does not compromise the audit records, which is this control's exact rationale.
  - **AU-9(3) Cryptographic Protection** — signed-hash integrity and tamper-evidence.
- **Primary (retention) — NIST SP 800-53 AU-11 (Audit Record Retention).** Retain audit records for an organization-defined period consistent with the records-retention policy, to support after-the-fact investigation of incidents and to meet regulatory and organizational retention requirements — and retain them only until no longer needed for administrative, legal, audit, or operational purposes. That wording is exactly the keep-for-compliance / dispose-when-unneeded balance. **AU-11(1)** adds long-term retrievability where multi-year retention applies.
- **Supporting (AI-specific) — NSA/CISA/FBI 2025 AI Data Security joint guidance**, which explicitly calls for immutable logging for AI systems.
- **Supporting (cross-standard convergence) — ISO/IEC 27001:2022** 8.15 (logging) and 5.33 (protection of records), which independently require log protection.
- **Supporting (retention period-setting authorities) — SOC 2, HIPAA, PCI-DSS, GDPR, FedRAMP**, the frameworks that set the concrete retention windows AU-11 points to; and the **GDPR storage-limitation principle / NIST privacy controls** for the "no longer than necessary" half.
- **Category anchor — CISA/NCSC Secure AI Development Guidelines** (log protection in secure operation).

See the [sources reference](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

- **You can verify the protection half concretely rather than take it on faith.** Against NIST AU-9 the checks are specific: write-once (append-only) media per AU-9(1); the log store held on separate systems from the audited system per AU-9(2), whose stated rationale is that a compromise of the audited system does not compromise the audit records; and cryptographic protection per AU-9(3), so that modification is detectable even where access controls are bypassed. Use those as your acceptance criteria.
- **Watch for a gap in the cross-standard convergence.** ISO/IEC 27001:2022 8.15 and 5.33 independently require log and record protection, so the convergence is real — but ISO does *not* mandate the real-time tamper-alerting that NIST AU-9 does. If you are leaning on ISO alone, you will not get tamper-alerting for free; source it from the NIST controls.
- **The apparent conflict between "keep logs long enough" and "don't keep personal data longer than necessary" mostly dissolves in practice.** AU-11 resolves it in its own wording: retain per the records-retention policy to support investigations and meet regulatory requirements, but only until the records are no longer needed for administrative, legal, audit, or operational purposes. Because decision & debug logging scrubs sensitive data by default, retention here governs scrubbed decision-metadata rather than a sensitive-data archive, which is what lets long-retention minimums and data minimization coexist. A two-window split — one period for compliance, another for personal data — was considered and left out as unnecessary nuance for a floor control, so do not feel obliged to build one at L1.

## Related controls

- [Decision & debug logging](decision-debug-logging.md) — this control protects and retains the substrate that decision & debug logging creates. Together they form the L1 Audit Logging cluster: capture enough, then protect it and keep it the right amount of time.
- [Identity & Access Control](../identity-access/README.md) and [scoped service accounts](../identity-access/scoped-service-accounts.md) — least-privilege access to the log store is the same principle; the most common failure is over-broad admin privileges that bundle log access.
