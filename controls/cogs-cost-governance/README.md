# 11. COGS & Cost Governance

**Source.** FinOps Foundation (FinOps for AI / Tokenomics) and NIST AI RMF (Measure/Manage), with supporting grounding in OWASP LLM10 (Unbounded Consumption).

## What this category defends against

This category defends against **cost abuse** — an attacker or a runaway agent driving catastrophic spend, the pattern OWASP names Denial-of-Wallet. AI systems bill by consumption (tokens, calls, reasoning steps), so a crafted input that makes an agent loop, or a hijacked workflow that fires expensive tool storms, can drain a budget in minutes. The category builds both the observability to see that spend and the enforcement machinery to stop it.

## Scope discipline (category framing)

Cost is in this model **only where it is a guardrail, not where it is mere optimization.** The security-relevant thread is cost abuse (Denial-of-Wallet / runaway spend). The progression is: **see the spend (L2 visibility) → act on it and defend against abuse (L3 control).**

Pure cost-*optimization* techniques — semantic caching, model routing / right-sizing, prompt compression — are **techniques, not controls.** They reduce baseline spend and shrink the blast radius of a cost attack, but they defend against nothing on their own, so they are folded into the controls as efficiency levers rather than standing as guardrails in their own right.

This mirrors the Audit Logging shape: **capture at the floor → detect and defend higher up.** Visibility (11.1) is the sensor; abuse defense (11.2) is the response built on it.

## Crawl/walk/run

| Level | Control | Progression logic |
|-------|---------|-------------------|
| L2 | [Cost visibility / tracking](cost-visibility-tracking.md) | The floor: instrument AI usage so spend is observable per request / workflow / feature / agent step. You cannot govern, cap, or defend what you cannot see. |
| L3 | [Cost control & abuse defense (Denial-of-Wallet)](cost-control-abuse-defense.md) | Act on the visibility: hard budget caps that stop spend, anomaly alerting on spikes, and attribution fine enough to localize the abuse. |

## Controls in this category

- [Cost visibility / tracking](cost-visibility-tracking.md) — `[Both / L2 / Low]`
- [Cost control & abuse defense (Denial-of-Wallet)](cost-control-abuse-defense.md) — `[Both / L3 / Medium]`
