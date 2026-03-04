# Dispatch Introduction

*A short presentation for IETF Dispatch — typically 5 minutes. Goal: get directed to the right area or WG, or get BOF approval.*

---

## What We're Bringing

Two individual Internet-Drafts seeking dispatch guidance:

- `draft-beyer-agent-identity-problem-statement-00`
- `draft-beyer-agent-identity-architecture-00`

**Topic:** A constitutional identity layer for autonomous and semi-autonomous agents acting on behalf of human principals.

---

## The Problem in One Minute

Agents now act across platforms — drafting, transacting, delegating — without any consistent way to represent:
- **Who** authorized them (human anchoring)
- **What** they're allowed to do (explicit delegation)
- **How** their actions can be traced back (provenance)

Existing systems (OAuth, SCIM, PKI, DIDs) authenticate software. None express the human behind the agent.

---

## What We've Produced

**Problem Statement** — Identifies five structural gaps in current identity systems, a threat model, and explicit MUST/SHOULD requirements for any solution.

**Architecture** — Defines four constructs that address those gaps:
1. Human identity root
2. Delegation chains (explicit, scoped, revocable)
3. Constitutional contracts (durable governance constraints)
4. Provenance model (portable across platform boundaries)

The architecture is **transport-agnostic** — it's a conceptual layer that OAuth, SCIM, PKI, and DID systems can bind to. It does not define a protocol.

---

## Where We Think This Belongs

This work touches multiple areas:
- **Security** — identity, delegation integrity, threat model
- **Applications** — agent-to-agent interaction, ecosystem interoperability
- **ART** — authorization frameworks (OAuth), provisioning (SCIM)

We are open to direction. Possible paths:
- New BOF in Security or ART
- Adoption as individual submissions in an existing WG (e.g., OAuth WG)
- Cross-area coordination

---

## What We're Asking Dispatch

1. Is the problem well-scoped for IETF work?
2. Which area and WG is the right home?
3. Is there sufficient interest for a BOF?

---

## Status

- Both drafts: version -00, individual submissions, not yet on datatracker
- Community review ongoing via GitHub: *[repository URL]*
- No protocol defined — this is architecture and problem statement work

---

*Presentation time: ~5 minutes + questions. Slides available separately.*
