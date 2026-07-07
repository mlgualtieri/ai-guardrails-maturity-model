# 7. Human-in-the-Loop (HITL) Controls

**Source.** NIST AI RMF (Govern 4.x / Manage — human oversight). Supporting: OWASP LLM06:2025 (Excessive Agency / "excessive autonomy").

## What this category defends against

Autonomous, action-taking agents can take consequential steps faster than a human can review them — deleting files, sending external communications, modifying records, making purchases, changing infrastructure, or deploying code. HITL controls keep a human in ultimate control of that autonomy at two ends: they insert a human decision *before* an agent takes a high-impact action, and they preserve the ability to *rapidly disable* an agent that is already misbehaving at scale. Together they answer how a person stays in the loop when the system can act on its own.

The category contains two controls: **7.1 approval before an action** (preventive — a single approval control spanning a simple confirmation gate up to risk-tiered approval) and **7.2 kill switch** (rapid emergency disable of the capability or deployment). Prevent and kill — the two ends of keeping a human in ultimate control.

You will not find a separate "mid-task interruption / termination" control (pause, roll back, or kill a single running task) here — it was **cut**, because pausing one run is operational agent-ops convenience rather than a distinct security guardrail, and its legitimate core is already covered: 7.1 approval gates stop a bad run from taking irreversible actions unapproved, sandboxing and rate limiting bound a runaway, and the 7.2 kill switch disables the capability where you truly need to stop the agent.

## Controls in this category

- **[7.1 Human-in-the-loop approval for consequential actions](human-approval-consequential-actions.md)** — `[Agentic / L2→L3 / Low]`. A human approves before an agent takes a consequential action, with the degree of oversight matched to the risk of the action — a binary confirmation gate at L2, graduating to risk-tiered approval at L3. The L2 floor also sets a limit on how many actions an agent may take before it must stop and escalate to a human.
- **[7.2 Kill switch (rapid capability / deployment disable)](kill-switch.md)** — `[Agentic / L2 / Low]`. The ability to rapidly disable a tool, capability, or an entire agent deployment — a system-scoped emergency stop for when something is wrong at scale.
