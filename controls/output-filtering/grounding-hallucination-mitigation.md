# Grounding & hallucination mitigation

`[Applies To: Both / Maturity Level: L2 / Alert Fatigue Risk: Medium]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

For outputs where factual accuracy matters, take deliberate steps to keep the model's responses grounded and to reduce fabrication, rather than trusting a fluent-but-possibly-invented answer. This is primarily a *design-and-input-stage* discipline, not a magic output-verifier: the most reliable leverage on hallucination is applied *before and during* generation, and not as a bolt-on check afterward.

## Honest framing: what this control is and is not

There is no general, reliable mechanism to *verify* that an arbitrary LLM output is factually true, and so this control does not promise a factuality gate. What it promises is *deliberate hallucination-reduction practices* scoped to high-stakes outputs. Two failure modes are worth distinguishing:

- **Faithfulness** — is the output supported by the source material or context you provided? This is practically checkable wherever you have sources to check against.
- **Factuality** — is the output objectively true in the world? This generally *cannot* be verified by an automated check, because the check would need to already know ground truth. If your source is wrong, a faithful answer is still wrong.

The control keeps outputs grounded, meaning faithful to authoritative context. It does not, and cannot, guarantee truth.

## Techniques — a menu, design-first

- **Prompt and system-prompt engineering (primary, most implementable).** Constrain the model to its authoritative context, instruct it to answer "I don't know" rather than guess, require it to cite or refuse, and discourage confident fabrication. This reduces hallucination at the source, and it is what competent teams actually do first.
- **Provide authoritative context.** Give the model the trusted material it should reason from, so that it is not filling gaps from training-data patterns.
- **Deterministic checks where the output structure allows.** Validate that cited references and identifiers actually exist, check structured outputs against schemas and known values, and flag claims that have no supporting source. These checks are concrete, non-probabilistic, and cheap.
- **Human review for the highest-stakes outputs** (ties to [HITL 7.1](../hitl/human-approval-consequential-actions.md)).
- **RAG / retrieval grounding — a more advanced implementation path.** Retrieval-augmented generation, which grounds responses in trusted sources, is a legitimate and more sophisticated technique, but it is *one option* rather than the definition of the control, and it brings its own complexity. It is not required to satisfy this control.

## Why it sits at this level

It sits in the *deployment-specific correctness* walk-tier of Output Filtering, where L1 addresses universal harm and L2 addresses your accuracy and scope bar. It is scoped to high-stakes outputs — legal, medical, financial, security — so it is L2-selective rather than a universal floor. Medium alert-fatigue.

It is deliberately **capped at "deliberate grounding practices"** and does not expand to L3. Building a sophisticated automated-verification version is exactly what would push it to L3, and that is precisely what this control does *not* attempt, because there is no reliable general mechanism to verify factuality.

## Why it is a security control, not just quality

Under OWASP LLM09 (Misinformation), hallucinated output is dangerous precisely because it is delivered *authoritatively* — a coding assistant confidently recommending a deprecated crypto function, a compliance bot fabricating a regulation, or a model suggesting a non-existent package name that an attacker can then register (slopsquatting). The same principle runs through the whole model: **treat LLM output as untrusted by default.**

## What implementing it looks like

Start with prompt and system-prompt design and with authoritative context. Add deterministic checks — citation and reference existence, schema validation — wherever output structure permits. Route high-stakes ungrounded outputs to human review, and design the UX to signal uncertainty rather than false confidence. Adopt retrieval grounding if and when the added sophistication is warranted. Scope all of this to outputs where accuracy carries real consequence.

## Source grounding

- **Primary — OWASP LLM09:2025 (Misinformation).** Hallucination is the core cause; mitigations include grounding in trusted sources, automatic output validation, citation verification, human oversight for high-stakes, and communicating limitations. **NIST AI RMF (Measure 2.x — validity and reliability)** provides the validation framing.
- **Supporting —** peer-reviewed RAG fact-checking research for the faithfulness-versus-factuality distinction (grounding verifies faithfulness, not factuality); **NIST AI 600-1** for confabulation as a named GenAI risk.
- **Category anchor —** OWASP GenAI Security Project.

For the full reference list, see [sources](../../reference/sources.md).

## Honest limits and trade-offs

- **This does not verify that outputs are true, and the control says so plainly.** It distinguishes faithfulness — supported by the provided context, and therefore checkable — from factuality, which is objective truth in the world and generally not auto-verifiable, because the checker would need to already know ground truth. The control promises grounding and faithfulness, not truth. So do not promise a factuality gate you cannot deliver — it would be aspirational and un-implementable.
- **Treat this as a security control, not just quality.** Hallucinated output is delivered authoritatively (OWASP LLM09) and can cause real harm — a fabricated regulation, a deprecated crypto recommendation, or a non-existent package name that an attacker registers (slopsquatting). It embodies the model-wide principle of treating output as untrusted.
- **Expect it to stay capped at L2.** It is scoped to deliberate design practices for high-stakes outputs. A sophisticated automated-verification version is what would make it L3, and the control deliberately does not attempt that.

## Related controls

- Walk-tier of Output Filtering alongside [Acceptable-use & scope enforcement](acceptable-use-scope-enforcement.md).
- The human-review path ties to [HITL — human approval for consequential actions](../hitl/human-approval-consequential-actions.md) (7.1).
- Embodies the "treat output as untrusted" principle shared with [prompt injection detection](../prompt-input-filtering/prompt-injection-detection.md) (3.1).
