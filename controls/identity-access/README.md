# 1. Identity & Access Controls

**Source.** Per-control primaries are the NIST SP 800-53 Rev 5 AC family (AC-6 Least Privilege, AC-5 Separation of Duties, AC-2 Account Management), NIST SP 800-207 (Zero Trust Architecture), and the OWASP Non-Human Identity Top 10. NIST AI RMF (Govern) anchors identity governance. CyberArk — Securing AI Agents is a vendor reference, used here as illustrative only rather than as a primary authority.

## What this category defends against

An AI agent is an actor in your systems, and its identity is its reach. When an agent runs under a shared or over-privileged account, three things break at once: a single compromise becomes total compromise, actions can no longer be attributed to a specific actor, and an agent that is merely mistaken — not attacked — acts with far more privilege than its task requires. These controls bound what an agent's identity can do and for how long, so that a compromised, hijacked, or malfunctioning agent carries the smallest possible blast radius.

## This category advances on two dimensions

Most categories progress along a single axis. Identity does not, and the model states this explicitly rather than implying one clean gradient. The crawl/walk/run below braids **two axes**: privilege *scope* — a bounded identity, then a decomposition of that identity by operation — and privilege *duration* — no idle standing privilege, then automated grant-and-revoke bound to a task. Both begin at L1 as a static baseline, scope matures at L2, and duration matures at L3.

## Crawl/walk/run

| Level | Control | Progression logic | Axis |
|-------|---------|-------------------|------|
| L1 (crawl) | Scoped service accounts + No standing admin access | Static least-privilege: a bounded identity, with no idle elevated rights | scope + duration (baseline of each) |
| L2 (walk) | Separation of read vs. write identities | Functional decomposition of the identity by operation | scope |
| L3 (run) | Just-in-time (JIT) authorization | Temporal least-privilege: automated grant and revoke | duration |

## Controls in this category

- [1.1 Scoped service accounts (NHIs)](scoped-service-accounts.md) — `[Both / L1 / Low]`. Every agent receives its own dedicated, least-privilege non-human identity, which is the structural prerequisite for the rest of the category.
- [1.2 No standing admin access](no-standing-admin-access.md) — `[Both / L1 / Low]`. That identity carries no permanent, always-on elevated privilege it is not actively using, and it is the companion to scoped accounts.
- [1.3 Separation of read vs. write identities](separation-read-write-identities.md) — `[Both / L2 / Low]`. The identity is decomposed by operation so a compromised read path cannot cause destructive action.
- [1.4 JIT authorization](jit-authorization.md) — `[Both / L3 / Low]`. Elevated access is granted at the moment of need, scoped to the task, and revoked the instant it completes.
