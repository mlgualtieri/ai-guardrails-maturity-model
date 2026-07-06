# Sandboxed execution environments

`[Applies To: Agentic / Maturity Level: L2 / Alert Fatigue Risk: Low]`

**Level:** [Level 2 — Intermediate](../../levels/level-2-intermediate.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

## What it is

Any code, command, or computer-use action an agent executes runs inside an isolated, ephemeral environment with no direct access to the host, production networks, other tenants, or durable infrastructure. The environment is disposable — created for the task, destroyed afterward — so that whatever the agent does, or whatever an attacker who has hijacked it does, is contained to a throwaway environment.

## Why it is the containment backbone

This is the control the others lean on. A blocklist (the fallback within [tool & operation allowlisting](tool-allowlisting.md)) catches mistakes but cannot contain a determined attack; sandboxing is what contains the attack. The principle converges across NIST, OWASP, and practitioner guidance: the model can always be manipulated by untrusted input, so safety comes from *bounding what its execution can reach* rather than from trusting it to behave. Sandboxing assumes compromise and makes that compromise survivable.

## Why it sits at this level

Sandboxing applies only to agents that *execute code or commands*, not to every deployment, so it is not an L1 control. For any code-executing agent, however, isolation is mandatory rather than optional from the start: it is the floor within that scenario, consistent with the L2 "running agentic workflows" threshold. Alert fatigue is Low because this is infrastructure, not an alerting filter.

## Choosing an environment — a menu, not a mandate

There is no single required technology. The following are suitable sandboxes for most agents:

- **Containers (Docker and similar)** — a sensible default for the majority of agent workloads; production systems, coding agents among them, use containers as their execution foundation. Containers are secure by default (namespaces, cgroups, capability restrictions), and most container security failures are *self-inflicted misconfigurations* — privileged mode, mounting the Docker socket — rather than inherent weaknesses.
- **Segmented cloud VMs** — an isolated VM with a scoped role, network-segmented from production.
- **Serverless functions (e.g., AWS Lambda, Cloud Functions)** — often the *easiest* strong sandbox, being ephemeral by nature, isolated per invocation, and running under a scoped execution role. Running the agent's code in a Lambda with a least-privilege role is a perfectly good sandbox for many teams. (Lambda itself runs on Firecracker microVMs, which provides strong isolation without your having to operate it.)

Hardware-level isolation (Firecracker/microVMs, gVisor, Kata Containers) is for the *genuinely-untrusted, high-risk tail*, and it is not a baseline requirement for every agent.

## General hardening callouts (deliberately scoped)

These are guide-rails, not a container-security tutorial:

- **Run as non-root.** This is the highest-leverage step for container sandboxes: a non-root process that is compromised or escapes lands with far less privilege, so it cannot install kernel modules, modify host system files, or reach other users' data. Per Docker's own guidance, even root *inside* a non-privileged container holds much less privilege than host root, which makes serious damage or host escape considerably harder.
- **Never run privileged / never mount the Docker socket.** `--privileged` disables isolation, and socket access is equivalent to host root. If a sandbox requires either, it is not a sandbox.
- **Ephemeral and one per session.** Destroy the environment after the task, and do not share it across users or tenants. This closes the persist-across-sessions and cross-tenant paths.
- **Keep real secrets out of the sandbox.** Inject short-lived, scoped credentials in-flight rather than baking API keys into the environment.

## Source grounding

- **Primary — NIST SP 800-53 SC-39 (Process Isolation) + SC-7 (Boundary Protection).** Isolate execution and bound what it can reach.
- **Supporting — NIST SP 800-190 (Application Container Security)** and the **OWASP Docker Security Cheat Sheet**: containers secure-by-default, run non-root, drop capabilities, avoid `--privileged`. **MITRE ATT&CK T1611 (Escape to Host)** names the escape threat the non-root / least-privilege guidance mitigates. **OWASP LLM06 (Excessive Agency)** supplies the *why* — bound the agent's downstream capability.
- **Category anchor — Anthropic, Building Effective Agents.**

Note that specific sandbox technologies (containers, serverless runtimes, microVMs, and hardware-isolation runtimes) are tools, not sources — they appear under "Choosing an environment" above, never in the grounding.

## Honest limits and trade-offs

You may have heard that containers are inadequate for isolation and that you need microVMs or hardware isolation. Do not over-rotate on that. The evidence is that most container-sandbox failures are *self-inflicted misconfigurations* (running privileged, mounting the Docker socket) rather than inherent weaknesses in the isolation primitives. Containers are secure by default and are a sensible baseline, and hardware isolation is reserved for the genuinely-untrusted, high-risk tail rather than mandated for every agent. The caveat runs the other way as well, and it is one to keep in mind: **NIST SP 800-190 predates the agentic threat model**, so supplement its container guidance with AI-specific reasoning about what a hijacked agent will attempt.

## Related controls

- [5.1 Tool & operation allowlisting](tool-allowlisting.md) — this sandbox is the real containment boundary behind that control's blocklist fallback; a bypassed blocklist lands inside the sandbox.
- [Network & Egress Controls](../network-egress/egress-control.md) — the network dimension of sandbox isolation.
- [Identity & Access Management](../identity-access/README.md) — supplies the scoped, least-privilege execution role the sandbox runs under.
