# Prompt injection detection

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: High]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

This is a detection layer that screens inputs for attempts to override the model's instructions — jailbreaks, "ignore previous instructions" attacks, and instruction-hierarchy manipulation. It is typically implemented as an inline classifier or "guardrail model" (LLM-as-judge) that inspects prompts before the primary model acts on them, and it flags or blocks the inputs it judges to be likely injection attempts.

## Why it sits at this level

Prompt injection is OWASP LLM01 — the #1 LLM risk, and it has remained #1 since the list debuted in 2023. Any production LLM deployment faces it, which is what makes a detection layer an L2 baseline rather than an advanced capability. It is tagged High alert-fatigue because injection detectors are inherently noisy: they false-positive on legitimate inputs that happen to resemble attacks, and they require ongoing tuning to stay useful.

## The honesty point this control must carry

Injection *detection* is fundamentally imperfect, and it cannot be presented as a solved boundary. Researchers have demonstrated roughly 100% evasion against production detectors — Azure Prompt Shield and Meta Prompt Guard among them — and the literature is explicit that filter-based defenses are vulnerable to adaptive attacks purpose-built to evade them, while also adding inference overhead. This is one probabilistic layer within defense-in-depth, and it is never a guarantee.

Detection reduces the volume of successful attacks and catches known patterns, but it is a tripwire rather than a boundary. The real mitigation is architectural, and the controls interlock:

- 3.1 detects what it can (this control).
- Category 5 (tool restrictions) limits what a hijacked agent can actually do.
- 7.1 (HITL) places a human gate on consequential actions.

The posture is a tripwire paired with containment. Do not present an injection detector as *solving* prompt injection — it does not. Treat it as a tripwire and put the real containment in the architecture around it: tool least-privilege and a human gate on consequential actions.

## What implementing it looks like

Run an inline classifier or guardrail model that screens user prompts *and* any retrieved or fetched context — RAG documents, tool output, web pages, email bodies — before the primary model ever sees it. OWASP is explicit that regex and pattern filters do not reliably catch injection, and that a trained model catches what regex misses. The detector should always be paired with the architectural controls, and it should never be deployed standalone.

Tooling (implementations, not authorities): AI-firewall and guardrail options include LLM Guard (Protect AI — open-source input/output scanners, including a prompt-injection scanner), Lakera Guard, Prompt Security, and Rebuff; open guardrail models include Llama Guard, ShieldGemma, IBM Granite Guardian, and Meta Prompt Guard; and NVIDIA NeMo Guardrails provides orchestration. It is worth noting that "LLM Guard" (Protect AI) and "Prompt Guard" / "Llama Guard" (Meta) are entirely different projects.

## Source grounding

- **Primary —** OWASP LLM01:2025 (Prompt Injection) — the #1 risk; distinguishes direct and indirect sub-classes; lists controls including input screening, context segregation, privilege limitation, and output filtering. OWASP LLM Prompt Injection Prevention Cheat Sheet — establishes the guardrail-model / LLM-as-judge pattern and input-plus-output screening, and is explicit that it sits *alongside*, not *in place of*, deterministic controls.
- **Supporting (honesty grounding) —** adversarial-evasion research demonstrating roughly 100% evasion against production detectors, establishing that detection is probabilistic rather than a boundary.
- **Category anchor —** OWASP GenAI Security Project.

## Honest limits and trade-offs

Do not treat injection detection as a solved boundary — it is not. Verified adversarial research shows roughly 100% evasion against production detectors such as Azure Prompt Shield and Meta Prompt Guard, and OWASP's own cheat sheet states that guardrail models sit *alongside*, not *in place of*, deterministic controls. Rely on this control only as a tripwire within defense-in-depth, and keep the real containment architectural: treat output as untrusted, apply tool least-privilege (category 5), and require HITL approval (7.1). That interlock is why 3.1 is only a tripwire, and the real containment lives in the architecture around it. The second thing to watch for is the High alert-fatigue tag — the detector is noisy, and it needs constant tuning to stay useful.

## Related controls

- Escalates to [3.3 Indirect prompt injection protection](indirect-prompt-injection-protection.md) at L3 (direct → indirect).
- Containment for a hijacked agent: [tool & command restrictions](../tool-command-restrictions/README.md) (category 5).
- Human gate on consequential actions: [7.1 Human approval for consequential actions](../hitl/human-approval-consequential-actions.md).
- Output handling: [4.4 Acceptable-use & scope enforcement](../output-filtering/acceptable-use-scope-enforcement.md) (the former input-side policy control now lives here).
- Full source list: [reference/sources.md](../../reference/sources.md).
