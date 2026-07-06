# 5. Tool & Command Restrictions

**Source.** Category primary source: OWASP LLM06:2025 (Excessive Agency). Supporting: NIST SP 800-53 CM-7 (Least Functionality) and AC-6 (Least Privilege). Category anchor: Anthropic — Building Effective Agents (practice).

## What this category defends against

Once an agent can take actions in the world — call tools, run commands, execute code — the question is no longer *what it says* but *what it can reach and do*. This category is the structured response to **OWASP LLM06: Excessive Agency**, which decomposes the risk into three root causes:

- **Excessive functionality** — the agent can reach tools beyond what its task requires.
- **Excessive permissions** — the tools it holds operate with broader privilege than needed.
- **Excessive autonomy** — high-impact actions proceed without human approval.

The controls map onto these root causes: tool & operation allowlisting addresses excessive *functionality* (which tools) and excessive *permissions* (what each tool may do); sandboxing contains the blast radius; and rate limiting (L3) bounds runaway autonomy by capping how much an agent can consume. The *excessive autonomy* root cause — requiring human approval before a high-impact action — is addressed by a control that lives in Human-in-the-Loop, cross-referenced below.

## Defense is architectural, not prompt-based

The foundational principle for the whole category: **you cannot prompt your way to safe agency.** "Be careful" instructions in a system prompt are insufficient, because the model can be tricked — via prompt injection — into wanting to use a tool or run a command it should not. Every control here must be *enforced outside the model*, not requested of it. "Tell the model which tools to use" is not a control; a tool registry that refuses to invoke anything unlisted is.

## Crawl/walk/run

The category progresses by *what it restricts*: first the set of tools and what those tools can do, then where execution runs, then how often it can act.

| Level | Control | What it restricts |
|-------|---------|-------------------|
| L2 | Tool & operation allowlisting (5.1) | The **set of tools** an agent may reach *and* **what each permitted tool can do** (default-deny; blocklist as a forced fallback for open-ended tools). |
| L2 | Sandboxed execution (5.2) | **Where execution runs** — an isolated, ephemeral environment with no host or production reach. |
| L3 | Rate limiting on tool call volume (5.3) | **How often / how much** an agent can call, protecting downstream infrastructure. |

Read as a sentence: restrict *which tools and what they can do* (allowlisting, L2) → contain *where execution runs* (sandboxing, L2) → restrict *how often* (rate limiting, L3).

## A note on confirmation gates

The human-approval step that addresses OWASP LLM06's *excessive autonomy* root cause lives in Human-in-the-Loop as [7.1 Human approval for consequential actions](../hitl/human-approval-consequential-actions.md), where the approval-workflow controls sit together. This category covers the other three root causes — excessive functionality, excessive permissions, and runaway autonomy — while the autonomy root cause remains part of its organizing principle.

## Controls in this category

- [5.1 Tool & operation allowlisting (default-deny)](tool-allowlisting.md) — `[Agentic / L2 / Medium]`
- [5.2 Sandboxed execution environments](sandboxed-execution.md) — `[Agentic / L2 / Low]`
- [5.3 Rate limiting on tool call volume](rate-limiting-tool-calls.md) — `[Agentic / L3 / Low]`
