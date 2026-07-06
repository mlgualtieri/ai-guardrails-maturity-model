# AI Guardrails Maturity Model

This is a practitioner reference for assessing and advancing an enterprise AI security posture, and
it covers both conversational and agentic AI deployments. It defines four progressive maturity
levels — Basic, Intermediate, Advanced, and Leading Edge — and it assigns each of **36 guardrail
sub-controls** to the level at which that control should first appear. There are thirteen
categories in all, running from Identity & Access through Multi-Agent Trust.

The model is built for practitioners. Every control names a primary authoritative source you can
take to a stakeholder, and every non-obvious leveling decision is explained so you can adapt it to
your own environment rather than take it on faith. It is worth being clear about what this is not:
it is not a certification and it is not a compliance framework. It is a self-assessment and planning
aid, and it should be used as one.

- **The whole model on one page:** [maturity-model.md](maturity-model.md) — the four levels, the
  design principle, the applicability convention, and the full catalog of all 36 controls with
  links to every detail page.
- **The sources:** [reference/sources.md](reference/sources.md) — organized by authority tier.
- **Release history:** [CHANGELOG.md](CHANGELOG.md) — what each version of the model adds or changes.

---

## How to read this model

Two points shape how the model is meant to be used, and getting them right matters more than any
individual control.

**This is not a bingo card.** The goal is not to implement all 36 controls. Many controls will be
irrelevant to a given system, and that is expected, not a gap. A control tagged `Agentic` does not
apply to a conversational bot with no tools; memory controls do not apply to a system with no
persistent memory; model-provenance verification does not apply if you consume a hosted model rather
than self-host weights. Each control states when it should appear *if it is relevant to your
architecture*, and a control that does not fit your system is simply not used. Scoping the model
down to what actually applies is the first step, not a shortcut.

**The controls build on each other; the basics are the foundation, not a phase to move past.** The
levels are cumulative. A higher level assumes every applicable control at the levels beneath it is
already in place, because the advanced controls depend on the basic ones — anomaly detection is only
as trustworthy as the logging it reads, just-in-time access assumes a scoped identity to grant access
to, and rogue-agent containment assumes a kill switch to invoke. Reaching a higher level does not
retire the Basic controls; it stands on them. An organization does not "graduate" past least
privilege or audit logging, and a single missing L1 control means L1 work remains no matter how many
advanced controls are in place.

---

## What this model contributes vs. what it cites

This model stands on a great deal of work done across the wider information security community. The
individual controls map to named authoritative sources — **NIST** (AI RMF, SP 800-53, and the
800-series), **OWASP** (Top 10 for LLM Applications, MCP Top 10, Non-Human Identity Top 10, and the
cheat sheets), **MITRE** (ATLAS, ATT&CK), **CISA/NCSC** (Secure AI Development Guidelines), the
**Cloud Security Alliance** (MAESTRO), **ISO/IEC**, and the **FinOps Foundation** — along with
peer-reviewed research for specific empirical claims. Full attribution lives in the
[Reference Library](reference/sources.md), and every control names its own primary source.

What the model contributes that those sources do not is the **maturity leveling**. That means the
crawl/walk/run sequencing of these controls across L1 through L4, the consolidation of overlapping
industry guidance into a single non-redundant catalog, and the AI-specificity discipline that
separates a genuine AI guardrail from general security practice. None of the cited sources publish a
maturity leveling of these controls. Where the model places a control at a given level, that
placement is an engineering judgment grounded in the cited practice, and it is not a claim that the
source itself prescribes that level.

---

## How to use this repo

There are two ways in, and it is worth picking the one that matches what you are doing.

**"Assess my organization."** Start at [maturity-model.md](maturity-model.md), then work the level
pages in order:

- [Level 1 — Basic](levels/level-1-basic.md)
- [Level 2 — Intermediate](levels/level-2-intermediate.md)
- [Level 3 — Advanced](levels/level-3-advanced.md)
- [Level 4 — Leading Edge](levels/level-4-leading-edge.md)

Each level page states what an organization at that level can affirmatively say about its AI
security, lists the controls that first appear there with links to each, and names the key gap to
the next level. Your organization sits at the highest level for which every applicable control at
that level, and at every level beneath it, is genuinely in place.

**"Look up a control."** Use the [full catalog](maturity-model.md#full-catalog) or the category
overview pages under [`controls/`](controls/). Every control has a detail page that covers what it
is, why it sits at its level, how to implement it, its verified sources, its honest limits and
trade-offs, and its related controls.

A reader can enter from either direction and work through the whole model without hitting a dead
end. The catalog links down to the controls, each control links up to its level and across to its
siblings, and each level links back down to its controls.

---

## Scope & limitations

These are stated plainly, so that the model is used for what it is and not oversold.

- **This is a leveling and consolidation of existing guidance, not new primary research.** The
  contribution is the sequencing, the consolidation into a non-redundant catalog, and the
  AI-specificity discipline. The underlying controls are not the contribution, and they are
  attributed to their sources throughout.
- **It is not a certification, an audit standard, or a compliance framework.** Reaching "Level 3"
  is a self-assessment aid rather than an attestation. It does not map one-to-one to SOC 2, ISO/IEC
  42001, the EU AI Act, or any regulatory regime, though many of the controls do support those
  obligations.
- **Levels are directional, and they are not a guarantee of security.** An organization that
  implements every L4 control is not "secure." It is well-postured against the risks this model
  enumerates. No control set is complete, adversaries adapt, and several controls here — prompt
  injection and scope detection among them — are explicitly probabilistic rather than hard
  boundaries.
- **The model is a snapshot of a fast-moving field.** The sources, tools, and threats that are
  current as of mid-2026 will move. The threats named as examples (MCP supply-chain attacks,
  framework CVEs) and the tooling named as examples (agent-identity products) are illustrative of a
  moment, not permanent fixtures.
- **Applicability is deployment-specific.** The model spans conversational and agentic AI, and not
  every control fits every system. It assumes the reader exercises judgment about what applies.
- **It does not replace a threat model for your specific system.** This is a checklist-grade
  maturity aid. A real deployment still needs its own threat modeling, which will surface risks that
  sit outside this catalog.

---

## License

This work is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**
license — see [LICENSE](LICENSE) for the terms and [NOTICE](NOTICE) for attribution. You are free
to share and adapt the material for any purpose, including commercially, so long as you give
appropriate credit. The license covers this work's original contribution — the leveling, the
consolidation into a non-redundant catalog, and the prose. It does not relicense the cited standards
and frameworks, which remain the property of their respective publishers; see the
[Reference Library](reference/sources.md) for attribution.
