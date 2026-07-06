# 6. Audit Logging & Observability

**Source.** Category default source: CISA/NCSC — Guidelines for Secure AI System Development (logging & monitoring in secure operation and maintenance). Per-control primary is the NIST SP 800-53 AU family.

## What this category defends against

Audit logging and observability provide the evidentiary substrate for every other guardrail: without a trustworthy record of what an AI system did, an organization cannot attribute actions, reconstruct decisions, debug failures, detect abuse, or respond to incidents. This category defends against silent misbehavior — a compromised or drifting agent whose actions leave no reconstructable trail, logs that can be quietly altered to hide an attack, records discarded before they are needed, or anomalous behavior that no one notices because nothing is watching the record.

## The capture-then-detect pattern

The category advances in two stages. First a **capture** cluster at L1 establishes the record and makes it dependable: capture enough metadata to reconstruct and debug decisions (6.1), then protect that record from tampering and retain it for the right amount of time (6.2). Then a **detect** tier at L3 builds on that substrate: agent trace logging (6.3) extends the capture pattern to the multi-step agentic chain, and behavioral anomaly detection (6.4) consumes those traces, along with cost and rate signals, to alert on deviation. Detection is impossible for what was never captured, so the L3 detection layer depends entirely on the L1 logging floor. This mirrors the visibility-then-anomaly progression used in cost governance.

## Crawl/walk/run

| Level | Control | Progression logic |
|-------|---------|-------------------|
| L1 | 6.1 Decision & debug logging | Capture: sufficient scrubbed metadata to reconstruct and debug every decision — the observability precondition for everything above |
| L1 | 6.2 Immutable log storage & retention | Protect and keep: append-only, tamper-evident, separated storage makes the captured record dependable, and retention keeps it as long as required and no longer |
| L3 | 6.3 Agent trace logging | Detect (instrumentation): extend capture to the multi-step agent chain — reasoning, tool calls, intermediate decisions |
| L3 | 6.4 Behavioral anomaly detection | Detect (alerting): baseline agent behavior and alert on deviation, consuming traces, cost, and rate signals |

The two L1 controls interlock as a cluster — capture (6.1), then protect and retain (6.2) — and pair with scoped identity, which gives *who* while logging gives *what they did*.

## Controls in this category

- [Decision & debug logging](decision-debug-logging.md) — `[Both / L1 / High]`
- [Immutable log storage & retention](immutable-log-storage.md) — `[Both / L1 / Low / Process]`
- [Agent trace logging](agent-trace-logging.md) — `[Agentic / L3 / Medium]`
- [Behavioral anomaly detection](behavioral-anomaly-detection.md) — `[Both / L3 / High]`
