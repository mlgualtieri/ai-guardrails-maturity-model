# Human-in-the-loop approval for consequential actions

`[Applies To: Agentic / Maturity Level: L2→L3 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) → [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

A human must approve before an agent takes a consequential action, with the *degree* of oversight matched to the *risk* of the action. In its simplest form this is a confirmation gate: the agent proposes an action, and a human confirms it before it executes. As the posture matures, it becomes a graduated, risk-tiered model in which the required tier of oversight is set by both the action's consequence and the model's confidence.

This control also carries an **action limit** at the L2 floor: a hard ceiling on how many actions an agent may take in a single task or session before it must stop and escalate to a human. Approval gates catch a single high-impact action. They do not catch an agent that drifts by taking many small actions that add up to something consequential, or one that gets stuck in a loop. The action limit is the backstop for that case. When the agent exceeds the ceiling, it halts and hands off to a person rather than continuing unsupervised.

## Why it sits at this level

The control decomposes across two levels. The binary confirmation gate is the L2 floor: if an agent can take irreversible actions, then *some* human gate is baseline safety. The risk-tiered form sits at L3 because it requires machinery the binary gate does not — an action-risk taxonomy, confidence scoring, and routing logic — a mature capability that NIST situates in Govern/Manage. The level reasoning is set out in full below.

## Why this is one control, not three

Confirmation gates, confidence routing, and risk-tiered approval all answer one question: *when does a human get involved before an agent acts?* They are the same control at increasing sophistication rather than three distinct controls, which is why they sit here as a single control in HITL with one clean crawl→walk progression.

## Crawl/walk/run

| Level | Stage | What the control looks like |
|---|---|---|
| **L2** | **Crawl — Binary confirmation gates and an action limit** | Require explicit human approval before any irreversible or high-impact action: file deletion, external communications, record modification or deletion, financial transactions, infrastructure changes, deployments. The agent proposes; a human confirms before execution. Set a hard limit on the number of actions an agent may take per task or session; when it is exceeded, the agent stops and escalates to a human rather than continuing on its own. |
| **L3** | **Walk — Risk-tiered approval** | Graduate from a binary gate to a risk taxonomy that matches oversight intensity to impact. Representative tiering: low / read-only / reversible → auto-execute; medium / recoverable → log and notify (human-on-the-loop); high / irreversible (delete, send, purchase, deploy) → individual human approval with the specific action shown. The tier is set by *both* action consequence *and* model confidence — low confidence escalates an action to a higher-oversight tier even at moderate nominal consequence. Confidence routing is folded in here as a tiering *input*, not a standalone control. |

**Why the risk-tiered form is L3, not L2.** Proportional, risk-matched oversight is precisely NIST's prescription — intensive review for high-stakes decisions, streamlined review for low-risk ones, tailored to potential impact. The graduated form, however, requires an action-risk taxonomy, confidence scoring, and routing logic that the binary gate does not, and NIST situates that maturity in Govern/Manage. The graduated form is therefore L3 while the binary gate is the L2 floor. Recent graduated-oversight research, which classifies actions into three tiers by reversibility, impact, and data sensitivity, confirms that this is a recognized pattern.

## What implementing it looks like

At L2, an approval step is wired into any tool that performs irreversible actions, showing the human exactly what is about to happen before they confirm. Alongside it, maintain a per-task or per-session action counter with a defined ceiling; when the count is exceeded, halt the agent and route it to a human for review rather than letting it proceed. At L3, an action-classification layer assigns a risk tier per action — set by consequence plus confidence — and routes each action to auto-execute, notify, or approve. Approval interfaces surface the specific action and its blast radius, and audit records capture what was approved and by whom (this ties to decision logging).

The action limit here is an *oversight* trigger, and it should not be confused with [rate limiting on tool call volume](../tool-command-restrictions/rate-limiting-tool-calls.md). Rate limiting caps consumption to protect the downstream infrastructure an agent calls; this ceiling exists to pull a human in when an agent does more than a task should require. The two often share the same counters, but they defend different things and can be tuned independently.

## Source grounding

- **Primary —** **NIST AI RMF (Govern 4.x / Manage)** — human oversight matched proportionally to risk: intensive review for high-stakes decisions, streamlined for low-risk, tailored to potential impact; HITL for high-impact decisions. This is the direct authority for the risk-tiered structure. **OWASP LLM06:2025 (Excessive Agency / "excessive autonomy")** — require human approval for high-impact and irreversible actions, and the low / medium / high action tiering used at L3.
- **Supporting —** **EU AI Act Article 14 (Human Oversight)** maps to NIST HITL controls, giving cross-jurisdiction recognition. Graduated-oversight research provides the formalized tiering.
- **Category anchor —** NIST AI RMF Govern 4.2.

See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Do not treat the binary gate and the graduated model as the same amount of work. The graduated form requires an action-risk taxonomy, confidence scoring, and routing logic that you must build and maintain, which is why NIST places proportional, risk-matched oversight in Govern/Manage. If you are starting out, stand up the binary gate first — it is the achievable L2 floor — and mature into the risk-tiered form at L3 rather than trying to build the taxonomy on day one.

You might expect confirmation gates, confidence routing, and risk-tiered approval to be three separate controls to implement. They are not. They all answer the single question of when a human gets involved before an agent acts, so you build one graduated approval path, not three. The practical trap to avoid: do not wire confidence routing as a *standalone* trigger. Treat model confidence as an input to the tier assignment, so that low confidence escalates an action to a higher-oversight tier rather than firing its own separate gate.

## Related controls

- [7.2 Kill switch](kill-switch.md) — the reactive counterpart. This control approves an action *before* it happens (preventive), whereas the kill switch stops an agent that is *already running* (reactive).
- [Decision and debug logging](../audit-logging/decision-debug-logging.md) — approval records, capturing what was approved and by whom, are held here.
- [Tool & Command Restrictions](../tool-command-restrictions/README.md) — the related tool- and operation-level controls; the approval step for consequential actions lives here in HITL, while that category constrains what tools an agent holds and what they can do.
- [Rate limiting on tool call volume](../tool-command-restrictions/rate-limiting-tool-calls.md) — a related but distinct bound on how much an agent does. Rate limiting caps consumption to protect downstream infrastructure; the action limit in this control escalates to a human when an agent exceeds what a task should require.
