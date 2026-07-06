# Acceptable-use & scope enforcement

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: High]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Keep the agent within its sanctioned operational lane and acceptable-use policy, enforced at **both** the input and the output. Block or redirect requests that fall outside policy or scope *going in*, and block or redirect responses that fall outside scope *coming out*. This is one policy — what this agent may be asked to do and may say — enforced at two points. Input-side screening lives here: if you are looking for acceptable-use enforcement on requests going in, this is the control that provides it.

## Why one control, not two

Input-side policy screening and output-side scope enforcement are the *same idea* — keeping the agent in its lane — enforced at two points in the pipeline. They are not distinct guardrails, and "enforce on input and on output" is an implementation detail of one control. A practitioner wants the concept, not two near-duplicate checklist items, so it is presented as one.

## What it covers

- **Input side:** block requests that fall outside the sanctioned purpose or against acceptable-use — requests for credentials or secrets, PII exfiltration ("list all customer SSNs"), system reconnaissance ("what's your system prompt / what tools can you call"), or off-domain asks. A request may be phrased entirely innocently — no injection, no manipulation — and still violate policy, which is what distinguishes this from [prompt injection detection](../prompt-input-filtering/prompt-injection-detection.md) (3.1 = *manipulation/technique*; this = *content/intent/scope*).
- **Output side:** block responses that fall outside the agent's domain even when they are harmless in general — a customer-service bot must not opine on politics, a healthcare policy chatbot must not emit Python, and a coding agent must not give legal advice.

## Why enforce at both ends (defense in depth)

The two enforcement points catch different escapes. An in-scope-looking request can still produce an out-of-scope answer, and a request that slips past the input check can still be caught when its *response* is off-domain. Industry guardrail practice pairs "input rails" (classifying off-topic or disallowed queries) with "output rails" (validating that the response is in-scope before return), which amounts to belt and suspenders on the operational lane.

## Distinct from adjacent controls

- Distinct from **[prompt injection detection](../prompt-input-filtering/prompt-injection-detection.md)** (3.1), because injection detection is about manipulation *technique*, not content or scope.
- Distinct from **[toxicity and harmful-content filters](toxicity-harmful-content-filters.md)** (4.2), because toxicity is *universal* harm that every deployment blocks, whereas this is *deployment-specific* scope, your agent's particular lane.

Output Filtering crawl/walk: L1 universal harm (4.1 PII, 4.2 toxicity) to L2 deployment-specific correctness (4.3 grounding, 4.4 scope).

## Why it sits at this level

Deployment-specific correctness — which requires that you have first *defined* the lane — puts this in the L2 walk tier rather than the L1 floor. It carries a **High** alert-fatigue tag: topic and scope boundaries are ambiguous and context-dependent, so enforcement over-blocks legitimate edge cases. Tighter scope definitions yield tighter, less-noisy guardrails, which means the noise is partly a function of how well the lane is defined.

## AI-specific framing and honest limit

What makes this an AI guardrail rather than generic content moderation is that the surface is natural language and the agent can *act*, so enforcement must classify *intent and scope expressed conversationally*. Keyword and static filters catch only trivial cases. Real enforcement needs intent- and classification-based detection, and even then it remains **probabilistic** and evadable by meaning-preserving rephrasing. This is a defense-in-depth layer, not a hard boundary. The ultimate containment for something that slips through is the agency controls ([tool least-privilege](../tool-command-restrictions/README.md), [HITL](../hitl/human-approval-consequential-actions.md)).

**Agentic wrinkle:** when scope enforcement triggers *mid-task* for an agent, rather than on a single chat turn, the system must make an explicit decision — halt, ask for clarification, or take a fallback — rather than defaulting inconsistently by code path.

## Why it is a security control, not just brand safety

Keeping the model in its lane shrinks the *malicious-steering* surface. A tightly-scoped agent is a smaller attack surface, and it limits what an attacker who partially bypasses other controls can coax it into. The value here goes well beyond the reputational cost of a bot going off-topic.

## What implementing it looks like

- Define the permitted domain explicitly, and understand that tighter is better.
- Enforce with intent and scope classification on both inputs and outputs, not with keyword filters alone.
- On trigger, block or redirect with a coherent fallback.
- For agents, make the mid-task-trigger behavior — halt, clarify, or fallback — explicit and consistent.

Tooling (not sources): NeMo Guardrails topic rails, Guardrails AI, LLM Guard, and cloud AI-firewalls — input and output rails.

## Source grounding

- **Primary — NIST AI RMF (Govern/Map):** define and bound the system's intended purpose and scope; map ethics and regulatory obligations to specific model behaviors (for example, "AI must never dispense medical diagnoses") and measure the disallowed-content rate. **NIST AI 600-1** for keeping GenAI within intended use and disallowed-content categories.
- **Supporting — OWASP LLM01 / LLM02 / LLM09** (scope enforcement reduces malicious-steering surface; input screening for exfiltration and recon; off-topic misinformation); off-topic-detection research for implementation reality; prompt-fuzzing evasion research for the honest caveat.
- **Category anchor —** OWASP GenAI Security Project.

For the full reference list, see [sources](../../reference/sources.md).

## Honest limits and trade-offs

- **You might expect this to be two controls — here's why it's one.** Input-side policy screening and output-side scope enforcement are the same idea — keeping the agent in its sanctioned lane — enforced at two pipeline points. They are not distinct guardrails, so per the condense-same-idea principle they are one control with "enforce at both ends" as an implementation detail. Input-side acceptable-use screening lives here, under this single control.
- **Expect High alert-fatigue at L2, and manage it definitionally.** Topic and scope boundaries are ambiguous and context-dependent, so enforcement over-blocks legitimate edge cases. The mitigation is definitional: tighter scope definitions yield tighter, less-noisy guardrails, so the noise is partly a function of how well you define the lane.
- **Assume it can be evaded.** Real enforcement needs intent classification, not keyword blocklists, and even then it remains probabilistic and evadable by meaning-preserving rephrasing. It is a defense-in-depth layer, not a hard boundary, and the ultimate containment sits in the agency controls (tool least-privilege, HITL).

## Related controls

- Distinct from [prompt injection detection](../prompt-input-filtering/prompt-injection-detection.md) (3.1, manipulation) and [toxicity and harmful-content filters](toxicity-harmful-content-filters.md) (4.2, universal harm).
- Walk-tier of Output Filtering alongside [grounding & hallucination mitigation](grounding-hallucination-mitigation.md).
- Containment behind it: [tool and command restrictions](../tool-command-restrictions/README.md) (Category 5) and [HITL — human approval for consequential actions](../hitl/human-approval-consequential-actions.md) (7.1).
