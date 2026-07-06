# Indirect prompt injection protection

`[Applies To: Agentic / Maturity Level: L3 / Alert Fatigue Risk: High]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

In agentic workflows, the injected instruction arrives from the *environment* — a malicious email, a poisoned web page, a crafted document, a compromised tool's output — rather than from the user. This control scans and treats as untrusted **all** external and retrieved content before and as it enters the context window, which means retrieved documents, tool outputs, memory retrievals, and fetched pages, and not merely the user message.

## Why it sits at this level

Direct injection ([3.1](prompt-injection-detection.md), L2) screens the user's prompt. Indirect injection is a harder problem, because the hostile content is embedded in data the agent legitimately fetches mid-task, which makes the attack surface every external source the agent touches. It is genuinely AI- and agentic-specific, and it is named in OWASP LLM01 as the indirect sub-class, which places it at L3. It is tagged High alert-fatigue.

## What implementing it looks like

Scan and segregate all retrieved and tool-returned content before it enters the context window. Mark provenance — user versus fetched versus tool — so that fetched content is not interpreted as instructions. Apply guardrail-model screening to retrieved content, not only to user input. Pair all of this with tool least-privilege and HITL, which provide the containment.

Monitoring everything loaded into the context window — not just the user turn — is part of *how* this control is implemented, because it defines the scanning surface, rather than a separate guardrail.

Tooling (implementations, not authorities): the same guardrail stack as 3.1, applied to retrieved content.

## The honest limit

Indirect injection inherits 3.1's framing: detection is probabilistic, and it is not a boundary. The real containment for indirect injection is architectural — treat all retrieved content as untrusted, apply least-privilege on tools ([category 5](../tool-command-restrictions/README.md)), require HITL on consequential actions ([7.1](../hitl/human-approval-consequential-actions.md)), and enforce context segregation so that untrusted fetched content is never interpreted as instructions.

This is also where the sub-agent-output-trust idea lands. A sub-agent's output is simply another untrusted external input to the calling agent, and it should be treated as one.

## Source grounding

- **Primary —** OWASP LLM01:2025 — the indirect prompt-injection sub-class, with context segregation and privilege limitation named as controls.
- **Supporting —** NIST AI 600-1 — data-poisoning and injection via external content.
- **Category anchor —** OWASP GenAI Security Project.

## Honest limits and trade-offs

As with direct injection, do not treat the scanning of retrieved content as a boundary. It is a probabilistic layer, and it inherits the honest limit from 3.1. Keep the real containment architectural: untrusted-content handling, tool least-privilege (category 5), HITL (7.1), and context segregation. Monitoring everything loaded into the context window — not just the user's message — is part of how you implement this control, defining the scanning surface, rather than a separate control.

## Related controls

- Escalation of [3.1 Prompt injection detection](prompt-injection-detection.md) (direct → indirect).
- Containment via [tool & command restrictions](../tool-command-restrictions/README.md) (category 5) and [7.1 Human approval for consequential actions](../hitl/human-approval-consequential-actions.md).
- Absorbs the Multi-Agent Trust "don't trust sub-agent output" idea — a sub-agent's output is untrusted external input: [sub-agent trust](../multi-agent-trust/sub-agent-trust.md).
- Ties to [8.2 Memory poisoning prevention](../memory-state/memory-poisoning-prevention.md) — poisoned content that gets stored.
- Full source list: [reference/sources.md](../../reference/sources.md).
