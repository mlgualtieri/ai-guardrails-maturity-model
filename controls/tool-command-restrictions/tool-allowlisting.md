# Tool & operation allowlisting (default-deny)

`[Applies To: Agentic / Maturity Level: L2 / Alert Fatigue Risk: Medium]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Bound both *which tools* an agent may use and *what each permitted tool may do*, default-deny at both layers. First, define an explicit list of the tools an agent is permitted to use and deny everything not on it — an agent cannot invoke a tool simply because the tool exists or because the model reached for it, and only pre-approved tools are callable. Then, within each permitted tool, constrain the operations it may perform: prefer an allowlist of permitted operations and deny the rest, falling back to a blocklist of known-dangerous operations only where an allowlist is impractical. This is application allowlisting applied to agents, at two granularities — the tool set and the operations inside each tool.

## Why it sits at this level

This is the entry-level agentic control. The moment an agent is given tools, bounding which tools it can use, and what those tools can do, is the first restriction to impose. It sits at L2 rather than L1 because it applies only once an agent takes actions, which is the L2 "agentic workflows in production" threshold. Within this category it is the floor: restrict the tool set and its operations first, then contain *where* execution runs ([5.2](sandboxed-execution.md)), then bound *how often* tools are called ([5.3](rate-limiting-tool-calls.md)). It maps to two of OWASP LLM06's root causes at once — *excessive functionality* (too many tools) and *excessive permissions* (a permitted tool that can do too much).

The Medium alert-fatigue rating reflects that an overly tight tool allowlist can block a tool the agent legitimately needs, generating friction until the registry is right-sized. A well-scoped operation allowlist, by contrast, rarely false-positives — nothing legitimate needs `rm -rf /`. The noise is a tuning cost, not a stream of false alerts.

## The two layers, and why both are needed

The tool layer and the operation layer are different granularities, enforced in different places, and a system needs both:

- **Which tools (the tool registry).** Maintain an explicit registry of approved tools per agent or per task, deny-by-default for anything unlisted. Remove deprecated and unused tools — OWASP flags leftover enabled plugins as a common failure mode. Prefer narrowly-scoped tools over open-ended ones; a tool such as "run arbitrary shell command" or "fetch arbitrary URL" dramatically expands the attack surface, so scope the capability to the task rather than exposing the general primitive.
- **What a permitted tool can do (the operation filter).** A shell, database, or code-execution tool can be legitimately allowlisted because the agent needs it, and it will still require operation-level restriction to prevent its destructive subset. Allowlist the tool, then restrict its operations.

## The preference order for operations IS the control

If you have heard that blocklists are weak, this ordering is why that concern does not sink the control. It is not a menu of equal options; it is a strict preference with a forced fallback.

1. **Prefer an allowlist of operations.** Enumerate the permitted operations and deny the rest, a default-deny posture. This is the strong model, and it should be the default wherever the set of legitimate operations *can* be defined.
2. **Fall back to a blocklist only when an allowlist is impractical.** Some tools are inherently open-ended — a general shell, an arbitrary-code interpreter, a free-form database connection — where one genuinely cannot enumerate every legitimate operation in advance. There, the only option is to deny the known-dangerous subset (`rm -rf`, `DROP TABLE`, `iptables -F`, credential export, disk wipe, fork bombs). This is a fallback *forced by the tool's nature*, not a co-equal choice.
3. **Know the blocklist's limits.** A blocklist cannot be a primary boundary: no one can enumerate every dangerous operation, and obfuscation defeats naive string-matching (`rm -rf` versus `rm --recursive --force` versus a base64-encoded variant). Where a blocklist is forced, it must therefore be backed by sandboxing ([5.2](sandboxed-execution.md)) and least-privilege identity, so that the agent does not have permission to perform the destructive operation in the first place. **The blocklist catches mistakes; the sandbox and permission model contain attacks.**

## What implementing it looks like

- Maintain an explicit registry of approved tools per agent or per task, with deny-by-default for anything unlisted, and prune deprecated or unused tools.
- Define an operation allowlist wherever the legitimate set is enumerable.
- Where it is not enumerable, blocklist destructive operations, matching on **normalized/canonical form** rather than raw strings so as to resist trivial obfuscation.
- Always pair the fallback blocklist with sandboxing ([5.2](sandboxed-execution.md)) and a least-privileged execution identity, so that a bypass does not amount to a successful attack.

## Source grounding

- **Primary — OWASP LLM06:2025 (Excessive Agency).** Addresses two root causes. *Excessive functionality*: an agent has excessive agency when it can act outside the least-privilege boundary of its task — OWASP's own example is a mail-summarizing assistant given an extension that can also *send* mail when it only needs to *read*, and the fix is an extension that implements mail-reading only. *Excessive permissions*: open-ended operations such as arbitrary shell execution dramatically expand attack surface and must be constrained. The allowlist-preferred / least-functionality stance is OWASP's own direction.
- **Supporting — NIST SP 800-53 CM-7 (Least Functionality).** Provide only essential capabilities and restrict unnecessary or prohibited functions; allowlisting the tool set and the operations within it is CM-7 applied to agent tools. **AC-6 (Least Privilege)** supplies the underlying principle, and **SC-7 / SC-39 (isolation)** ground the defense-in-depth pairing with sandboxing.
- **Category anchor — Anthropic, Building Effective Agents.** Practitioner grounding for how bounded tool sets are built in real agent systems.

## Honest limits and trade-offs

- **Allowlisting only works because it is enforced outside the model.** A common misconception is that this is just telling the model which tools to use. It is not, and the distinction is the entire point. Instructing a model on which tools or operations to prefer is not allowlisting, because the model can be manipulated, by way of prompt injection, into wanting the wrong one. The registry that refuses to dispatch an unlisted tool, and the filter that refuses a disallowed operation, hold even when the model is compromised. This is the category's "defense is architectural, not prompt-based" principle applied at the tool and operation boundaries.
- **A determined attacker will obfuscate around a blocklist, and you should not expect otherwise.** That is exactly why a blocklist is a forced fallback rather than the primary boundary. The division of labor is the thing to internalize: the blocklist catches mistakes; the sandbox and permission model contain attacks. Never trust a blocklist to stop a determined adversary on its own — back it with isolation ([5.2](sandboxed-execution.md)) and least-privilege identity, so that a bypass lands inside a throwaway environment with no privilege to do damage.

## Related controls

- [5.2 Sandboxed execution environments](sandboxed-execution.md) — contains what an allowed tool can reach when it runs, and is the real containment boundary behind any blocklist fallback.
- [5.3 Rate limiting on tool call volume](rate-limiting-tool-calls.md) — bounds how often allowed tools are called.
- [Identity & Access Management](../identity-access/README.md) — least-privilege identity reinforces this at the tool and operation layers, ensuring the agent cannot perform a destructive operation even if a filter is bypassed.
