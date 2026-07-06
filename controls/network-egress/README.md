# 10. Network & Egress Controls

**Source.** CISA/NCSC Guidelines for Secure AI System Development (control agent network access). Supporting: NIST SP 800-53 SC-7 (Boundary Protection).

## What this category defends against

This category defends against injection-driven exfiltration. The end-goal of prompt injection is often to get data out, and egress control is the key containment that stops a hijacked agent from shipping data to an arbitrary endpoint when upstream detection fails. Where an agent browses the web or calls external APIs, the network layer decides what is actually allowed to leave. That containment holds even after other layers are bypassed, which is what makes it the load-bearing partner to the injection and tool-restriction controls elsewhere in the model.

**This category is a single control.** It does not decompose across maturity levels. Egress filtering, DNS monitoring, and API-gateway enforcement are three *mechanisms* for one idea — bound and inspect what leaves — rather than separate guardrails: DNS monitoring and the API gateway are *how* you implement egress control. Because there is a single control, there is no crawl/walk/run progression table for this category.

## Controls in this category

- [Egress control for AI agents](egress-control.md) — `[Agentic / L3 / Medium]`
