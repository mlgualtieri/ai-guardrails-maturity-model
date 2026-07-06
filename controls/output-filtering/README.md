# 4. Output Filtering & Content Controls

**Source.** Category default: NIST AI RMF Playbook — Measure 2.5 (output validity and reliability measurement), superseded per-control where a more direct authority applies (see each control's Source grounding).

## What this category defends against

This category governs what leaves the system. Once the model has produced a response, the controls here decide whether that response — or the data inside it — is allowed to reach the user or a downstream system. The threats span leaking secrets and PII in responses, emitting content that is harmful regardless of purpose, delivering fabricated information with unwarranted confidence, and drifting outside the agent's sanctioned operational lane. The common thread is that model output is treated as untrusted by default, and it is checked against a boundary before it is delivered.

## Crawl/walk/run

The governing axis is **what kind of boundary is being enforced on outputs**. The category advances from universal boundaries that every deployment shares to deployment-specific boundaries that each team must define.

| Level | Controls | Boundary being enforced |
|-------|----------|-------------------------|
| L1 (crawl) | PII/secret scrubbing · Toxicity/harmful-content filtering | *Universal* — don't leak secrets; don't emit content harmful regardless of purpose. Largely provider-default. |
| L2 (walk) | Grounding/hallucination checks · Topic/scope enforcement | *Deployment-specific* — enforce *your* accuracy bar and *your* operational lane. Requires custom policy. |

## Controls in this category

- [PII / sensitive data scrubbing](pii-sensitive-data-scrubbing.md) — `[Both / L1 / High]` — output-side scan-and-redact for secrets and PII before a response leaves the system.
- [Toxicity and harmful-content filters](toxicity-harmful-content-filters.md) — `[Both / L1 / Medium]` — block universally harmful output (abuse, harassment, hate, obscene content, dangerous/violent recommendations) at provider and application levels.
- [Grounding & hallucination mitigation](grounding-hallucination-mitigation.md) — `[Both / L2 / Medium]` — design-first practices that keep high-stakes outputs grounded (faithful to authoritative context), reducing fabrication.
- [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md) — `[Both / L2 / High]` — keep the agent in its sanctioned lane, enforced on both input and output.
