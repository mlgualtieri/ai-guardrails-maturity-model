# Model provenance verification

`[Applies To: Both / Maturity Level: L4 / Alert Fatigue Risk: Low]`

**Level:** [Level 4 — Leading Edge](../../levels/level-4-leading-edge.md) · **Category:** [overview](README.md) · **Model:** [catalog](../../maturity-model.md#full-catalog) · [sources](../../reference/sources.md)

> **Applies only if you self-host model weights. N/A for pure API consumers.** If you only call a hosted model (OpenAI, Anthropic, Google, and similar), the provider handles this and you never touch a weights file. This control is for organizations that download and run open models themselves.

## What it is

Verify that the model weights and adapters you pull are genuinely what they claim to be and have not been tampered with. There are three parts:

- **Integrity** — checksum or hash against a trusted reference, to establish whether the bits were altered.
- **Authenticity** — signed model cards or attestation, to establish whether the artifact is really from the claimed publisher.
- **Safe formats** — prefer **safetensors** over legacy **pickle**. Pickle-format models can execute arbitrary code on load, which is a real and exploited attack vector, whereas safetensors is designed so that loading cannot execute code.

## Why it sits at this level

Model weights are a unique AI artifact, and the load-time code-execution threat is AI-specific. It sits at L4 because only the self-hosting subset needs it, since the majority of deployments consume a hosted model and never see a weights file. It is part of the Supply Chain progression: [9.1 vets what you connect](pre-integration-vetting-agent-resources.md) at L2, and then the model artifact itself is verified at L4.

## What implementing it looks like

- Compare a downloaded artifact's checksum or hash against the value published by a trusted source before loading it.
- Prefer signed model cards and publisher attestation, and treat unsigned or unverifiable artifacts as untrusted.
- Standardize on the safetensors format for stored and distributed weights, and avoid loading pickle-format artifacts from untrusted or unverified sources, given the on-load code-execution risk.

## Honest limits and trade-offs

**This control only applies if you self-host model weights.** If you only call a hosted model, it does nothing for you — skip it, and the scope note at the top of the page tells you as much. Do not let the narrow scope fool you into skipping it if you *do* download and run open weights, though. For that subset the threat is real and exploited: model-weight tampering and, in particular, pickle-format artifacts that execute arbitrary code on load. If you pull weights, checksum them, prefer signed artifacts, and standardize on safetensors. The narrowness of who this applies to says nothing about how much it matters to those it does.

## Related controls

- [9.1 Pre-integration vetting of agent resources](pre-integration-vetting-agent-resources.md) — the earlier surface in the Supply Chain progression: vetting what you connect, before the model artifact itself is verified.

## Source grounding

- **Primary — MITRE ATLAS** — ML supply-chain compromise and model tampering; the authoritative catalog of the adversarial technique this control defends against.
- **Supporting — NIST SSDF (SP 800-218)** and **NIST SP 800-161** — provenance and integrity of software and supply-chain artifacts, applied here to the model weights.
- **Safe-format standard — Hugging Face safetensors** — referenced as the neutral format standard for loading weights without code execution (the pickle-vs-safetensors distinction), not as a vendor product.

The AI-specific slice is model-weight integrity together with the pickle code-execution threat. See [reference/sources.md](../../reference/sources.md) for full citations.
