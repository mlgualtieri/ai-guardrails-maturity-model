# PII / sensitive data scrubbing

`[Applies To: Both / Maturity Level: L1 / Alert Fatigue Risk: High]`

**Level:** [Level 1 — Basic](../../levels/level-1-basic.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Before the model's output reaches the user or a downstream system, scan it for sensitive patterns — SSNs, API keys, passwords, access tokens, internal hostnames, financial and health identifiers — and redact or block them. This is the output-side half of data-loss prevention: even where sensitive content reaches the model's context or is generated, it must not leave in the response.

## Scope note: this control is one half of a bidirectional risk

OWASP frames sensitive-information disclosure as *bidirectional*. Data can leak on the way **in** — into training data or context — or on the way **out**, in responses. This control is specifically the **output-side** guard. The input-side half is handled elsewhere, by data-access scoping ([Data Access & Handling](../data-access/README.md), Category 2) and prompt/input filtering ([Prompt & Input Filtering](../prompt-input-filtering/prompt-injection-detection.md), Category 3). This control should be read as covering the outbound direction only, and not the input direction it does not touch.

## Why it sits at this level

If an agent can emit secrets or PII in its responses from day one, that is a baseline-grade exposure rather than a sophisticated one. It sits at the L1 floor alongside the other controls that bound what the system can leak.

## What implementing it looks like

- Run output DLP scanning in the response path.
- Use a **sliding-window buffer** for streamed responses so that entities split across chunks are still caught — a card number or token can be split across two chunks, which naive per-token redaction misses entirely.
- Add custom recognizers for organization-specific identifiers such as employee IDs, account numbers, and internal schemes.

Tooling (not sources): Microsoft Presidio and similar DLP libraries automate detection and support custom regex and context recognizers.

## Source grounding

- **Primary — OWASP LLM02:2025, Sensitive Information Disclosure.** LLM02 covers PII, financial details, health records, confidential business data, and security credentials; it explicitly covers the output direction (applications risk exposing sensitive data *through their output*); and OWASP's prevention guidance names this control's two-part mitigation — scrub or mask data before it reaches the model, and strictly filter outputs. This is a more direct primary than the category's NIST AI RMF Measure 2.5 default.
- **Supporting — NIST AI RMF Measure 2.5** (output validity and reliability measurement) and **NIST SP 800-53 SI-family** (system and information integrity, output handling) for the general DLP grounding.

For the full reference list, see [sources](../../reference/sources.md).

## Honest limits and trade-offs

- **Expect noise, even at the L1 floor.** Pattern-based output scanning is noisy — it false-positives on secret-shaped strings and hostname-shaped labels — and it is one of only two L1 controls tagged High, the other being full logging. Streaming responses make naive per-token redaction genuinely hard, because an entity can be split across chunks; the recognized mitigation is a sliding-window buffer that accumulates chunks before scanning. At L1, the expectation is simply that *output scanning is present*, with buffering and tuning to control noise as maturity that comes later. The noise is architectural and manageable, which is why the High alert-fatigue tag does not disqualify L1 placement.
- **Don't confuse this with OWASP LLM05.** Improper handling of model outputs is separately **OWASP LLM05, Improper Output Handling** — related, but distinct. LLM05 concerns downstream injection and execution risk from unsanitized output, whereas this control concerns sensitive-data leakage. Keep the two distinct.

## Related controls

- [Toxicity and harmful-content filters](toxicity-harmful-content-filters.md) — the other L1 universal-harm output control.
- [Grounding & hallucination mitigation](grounding-hallucination-mitigation.md) and [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md) — the L2 deployment-specific walk-tier of this category.
- Input-side halves of the bidirectional risk: [Data Access & Handling](../data-access/README.md) (Category 2) and [Prompt & Input Filtering](../prompt-input-filtering/prompt-injection-detection.md) (Category 3).
