# Decision & debug logging (sufficient metadata, scrub-by-default)

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: High]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Capture enough structured metadata about inputs, outputs, tool calls, and retrieved context to (1) reconstruct every decision an agent made and (2) debug why it made them. Log the *shape, provenance, and outcome* of what happened — which tool was called with which parameters, what was retrieved from where, which policy checks fired, what the identity was, and when — rather than verbatim sensitive payloads. Sensitive data is scrubbed from logs by default, and verbatim retention is a conscious, justified exception.

The objective is not verbatim capture of everything. Verbatim capture is the wrong objective, because it maximizes data volume and turns the log store into a sensitive-data hoard. What this control requires is *sufficient* capture for decision-reconstruction and debugging, with minimization built in.

**Definition of "sensitive."** For the scrub-by-default scope, sensitive data means PII, secrets, and any data whose presence in logs would create security, regulatory, or compliance risk.

**Engineer as arbiter.** The responsible engineer determines what falls under "sensitive" and what, if anything, must be retained verbatim. At L1 the principle is only that verbatim retention of sensitive data is a conscious, justified exception rather than a silent default. "Justified" covers forensic and security value — preserving a prompt-injection or jailbreak string as attack evidence, for example — as well as explicit regulatory requirement. Formalized documentation and review of those verbatim-retention decisions is deferred to L2/L3 as a provisional higher-level control.

**Forensic-preservation caveat — do not over-scrub.** Blanket scrubbing can destroy evidence: for prompt injection, jailbreaks, and exfiltration probes, the exact malicious input *is* the forensic artifact. "Scrub-by-default" means scrubbing sensitive data that is not security-relevant, while adversarial inputs are preserved as security evidence under the "justified exception" clause.

## Why it sits at this level

It is the observability precondition for everything above it: behavioral anomaly detection (L3), agent trace logging (L3), and incident response (L4) all require this substrate. It pairs with scoped identity — identity gives *who*, this gives *what they did* — and together they form the attribution foundation. The L1 expectation is that decision-reconstruction logging is present with scrub-by-default, and tuning and analysis maturity come later.

## What implementing it looks like

Structured event records per agent action (tool name, parameters, retrieval source/IDs, policy-check outcomes, identity, timestamp); redaction or tokenization of sensitive fields in the logging pipeline; and a deliberate, minimized path for any verbatim retention. In practice this means OpenTelemetry-based tracing together with DLP/redaction libraries operating in the log pipeline.

## Source grounding

- **Primary — NIST SP 800-53 AU-2 (Event Logging).** Requires a rationale for why logged events are adequate to support after-the-fact investigations, and frames logging as the selection of a *sufficient subset* rather than the capture of everything, explicitly to avoid burden. The "engineer selects what's logged, with documented rationale" model maps to AU-2's requirement that designated roles select logged events with a sufficiency rationale, so even the softer L1 principle has a NIST hook, and formalized documentation is the AU-2 rationale requirement maturing at higher levels.
- **Primary — NIST SP 800-53 AU-3 (Content of Audit Records).** Defines record content as type/when/where/source/outcome/identity, which is precisely decision-reconstruction metadata. The AU-2/AU-3 discussion also warns that audit trails can reveal PII, especially when recording inputs.
- **Primary — NIST SP 800-53 AU-3(3) (Limit PII Elements).** NIST explicitly names limiting PII in audit records, which grounds scrub-by-default as a named enhancement rather than mere hygiene.
- **Supporting — NSA/CISA/FBI 2025 AI Data Security joint guidance** ("log all model inference activity") for the AI-specific application.
- **Supporting — NIST SP 800-92** (log management) for the sufficiency/balance principle.
- **Category anchor — CISA/NCSC Secure AI Development Guidelines** (logging & monitoring in secure operation and maintenance).

See the [sources reference](../../reference/sources.md) for full citations.

## Honest limits and trade-offs

- **"Full logging" is a misnomer, and the distinction is load-bearing.** The real requirement is enough structured metadata to reconstruct and debug an agent's decisions, with sensitive data scrubbed by default — not verbatim capture of everything. This is consistent with NIST's own logging stance, since AU-2 frames logging as a sufficient subset rather than everything, and AU-3(3) names PII limitation directly.
- **Be aware of what the High alert-fatigue tag means at an L1 floor.** Here the "noise" is volume and sensitivity, not false positives. Decision-scoped, scrubbed logging *reduces* both relative to verbatim capture, which is a further reason to adopt the scrubbed posture rather than a reason to avoid the control.
- **Do not assume scrub-by-default destroys the evidence you need after an attack.** The forensic-preservation caveat covers exactly this. Scrub-by-default applies to sensitive data that is not security-relevant. Adversarial inputs such as prompt-injection and jailbreak strings are the forensic artifact and are preserved under the "justified exception" clause, with the engineer as the arbiter of that call.

## Related controls

- [Immutable log storage & retention](immutable-log-storage.md) — protects the substrate this control creates and governs how long the scrubbed decision-metadata is kept.
- [Agent trace logging](agent-trace-logging.md) — extends this control's scrub-by-default logging to the multi-step agent chain.
- [Behavioral anomaly detection](behavioral-anomaly-detection.md) — the L3 detection layer that consumes this record.
- [Identity & Access Control](../identity-access/README.md) and [scoped service accounts](../identity-access/scoped-service-accounts.md) — identity provides *who*, logging provides *what they did*; together the attribution foundation.
