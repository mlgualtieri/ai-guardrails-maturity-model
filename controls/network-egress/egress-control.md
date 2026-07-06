# Egress control for AI agents

`[Applies To: Agentic / Maturity Level: L3 / Alert Fatigue Risk: Medium]`

**Level:** [Level 3 — Advanced](../../levels/level-3-advanced.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Control and constrain where an agent can send data and make outbound connections. Agents that browse the web or call external APIs are given an allowlist of permitted destinations, and outbound traffic to anything else is blocked at the network layer. The point is containment: a compromised or injection-hijacked agent should not be able to exfiltrate data to an arbitrary endpoint.

## Why one control (the collapse)

Egress filtering, DNS monitoring, and API-gateway enforcement are three *mechanisms* for one idea: bounding and inspecting what leaves. DNS monitoring catches tunneling and C2 beaconing that bypasses payload inspection. The API gateway is the chokepoint that enforces logging, rate limits, and outbound policy on tool calls. Both are *how* egress control is implemented rather than separate guardrails, which is why this is one control rather than three.

## Why it sits at this level

Egress control is a general network discipline, but it is the key containment against the AI-specific threat this model keeps returning to: prompt injection's end goal is often exfiltration, and egress control is what stops a hijacked agent from shipping data out even after other layers fail. It sits at L3 because effective egress control for agents — destination allowlisting together with DNS inspection and a gateway chokepoint — is mature network engineering beyond the baseline. It is the defense-in-depth partner to the injection controls ([3.1](../prompt-input-filtering/prompt-injection-detection.md), [3.3](../prompt-input-filtering/indirect-prompt-injection-protection.md)) and the [tool restrictions](../tool-command-restrictions/README.md) in category 5: those layers try to prevent and detect the hijack, and egress control contains the payoff when they do not.

## What implementing it looks like

- Maintain a destination allowlist for agent outbound traffic, deny-by-default.
- Route AI-to-external calls through an API gateway that logs, rate-limits, and enforces outbound policy — the chokepoint for what leaves via tool calls.
- Monitor and inspect DNS queries for tunneling and C2 beaconing.
- Place agents in network segments with controlled egress.

Tooling in this space includes egress proxies and firewalls, API gateways, and DNS-layer security. These are implementation choices rather than sources.

## Source grounding

- **Primary — NIST SP 800-53 SC-7 (Boundary Protection) + SC-7(10) (prevent exfiltration).** Applied to agent egress: the boundary-protection and exfiltration-prevention controls are the direct basis for allowlisting and inspecting agent outbound traffic.
- **CISA/NCSC Guidelines for Secure AI System Development.** Directs controlling agent network access as part of securing an AI system's operation.
- **Category anchor:** CISA/NCSC + NIST SC-7. See [reference/sources.md](../../reference/sources.md).

## Honest limits and trade-offs

Be clear-eyed that this is general network discipline, not a novel AI mechanism — you may already run egress control elsewhere, and much of that carries over directly. What makes it matter for agents is its role as the key containment against injection-driven exfiltration: it is the layer that stops a hijacked agent from shipping data out when your injection detection fails. Treat it as the backstop, not the first line — it does not prevent the hijack, it limits where the payoff can go.

## Related controls

- [Prompt injection detection (3.1)](../prompt-input-filtering/prompt-injection-detection.md) and [indirect prompt injection protection (3.3)](../prompt-input-filtering/indirect-prompt-injection-protection.md) — egress control is the containment layer behind these injection controls, stopping exfiltration when detection fails.
- [Tool & Command Restrictions (category 5)](../tool-command-restrictions/README.md), including [sandboxed execution](../tool-command-restrictions/sandboxed-execution.md) — restricts what an agent can do, whereas egress control restricts where its output can go.
- [Audit Logging (category 6)](../audit-logging/README.md) — the API-gateway chokepoint's logging ties into the audit-logging controls.
