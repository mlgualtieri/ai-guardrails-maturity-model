# 3. Prompt & Input Filtering

**Source.** OWASP LLM01:2025 (Prompt Injection) — the #1 LLM risk. Supporting: OWASP LLM Prompt Injection Prevention Cheat Sheet; adversarial-evasion research. Category anchor: OWASP GenAI Security Project.

## What this category defends against

Prompt injection is OWASP LLM01 — the highest-ranked LLM risk, and unchanged as #1 since the list debuted in 2023. It covers attempts to override the model's instructions, whether those attempts arrive directly in the user's message (jailbreaks, "ignore previous instructions," instruction-hierarchy manipulation) or indirectly through content the agent fetches mid-task (a malicious email, a poisoned web page, a crafted document, a compromised tool's output). This category is the input-screening tripwire that detects such attempts before the primary model acts on them.

## The architecture, stated honestly

Prompt injection cannot be "solved" by input filtering. Detection is one probabilistic layer — it reduces successful-attack volume and catches known patterns, but it is not a boundary and must never be presented as one. The real containment for injection is architectural: treat model output as untrusted, apply least privilege to tools ([category 5](../tool-command-restrictions/README.md)), and require human approval on consequential actions ([7.1](../hitl/human-approval-consequential-actions.md)). This category is the tripwire, and the agency controls are the containment. The controls interlock across categories.

The former control 3.2 ("acceptable-use / content-policy enforcement on inputs") has been merged into 4.4 ([Acceptable-use & scope enforcement](../output-filtering/acceptable-use-scope-enforcement.md)), because input-side policy screening and output-side scope enforcement are the same idea — keeping the agent in its sanctioned lane — enforced at two points in the pipeline. As a result, this category holds 3.1 (injection detection) plus the L3 indirect-injection control; the policy/scope idea now lives in 4.4.

## Crawl/walk/run

| Level | Control | Progression logic |
| --- | --- | --- |
| L2 | 3.1 Prompt injection detection | Screen the user's prompt (and fetched context) for override/jailbreak attempts — the production baseline for any LLM deployment. |
| L3 | 3.3 Indirect prompt injection protection | Extend screening to injection from the environment — every external source the agent touches (retrieved docs, tool output, memory, fetched pages). Harder than direct injection because the hostile content is embedded in data the agent legitimately fetches mid-task. |

## Controls in this category

- [3.1 Prompt injection detection](prompt-injection-detection.md) — `[Both / L2 / High]`
- [3.3 Indirect prompt injection protection](indirect-prompt-injection-protection.md) — `[Agentic / L3 / High]`
