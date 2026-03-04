# BOF Proposal Outline

**Session title:** Agent Identity: Human Anchoring, Delegation, and Provenance
**Type:** Birds of a Feather (BOF)
**Proposed venue:** IETF (area TBD — likely Security or Applications)

---

## Problem

Autonomous and semi-autonomous agents increasingly act on behalf of human principals across platforms and services. Existing identity systems treat agents as clients or service accounts and do not provide human anchoring, explicit delegation, or durable provenance. This creates risks including impersonation, agent laundering, uncontrolled replication, and inconsistent cross-platform semantics.

## Motivation

The absence of a coherent identity model for agents is an Internet-scale problem. Without a standard way to represent who an agent represents, what authority it has, and under what constraints it operates, relying parties cannot evaluate trustworthiness across ecosystems. This gap affects authentication, authorization, privacy, and security.

## Scope

The BOF will explore whether the IETF should develop:
- A constitutional identity architecture for agents
- A standard model for delegation chains
- A transport-agnostic representation of provenance
- Requirements for protocol bindings (OAuth, SCIM, mTLS, decentralized identity)

The BOF will **not** define a protocol.

## Out of Scope

- Behavioral safety
- Content controls
- Runtime policy engines
- Surveillance or tracking mechanisms
- Replacing OAuth, SCIM, or PKI

## Goals

- Validate the problem statement
- Assess community interest
- Determine whether a working group is appropriate
- Identify potential protocol binding candidates

## Non-Goals for the BOF

- Selecting a protocol
- Defining a wire format
- Choosing a cryptographic suite

## Proposed Deliverables (if WG chartered)

- Problem Statement *(already drafted — individual submission)*
- Architecture *(already drafted — individual submission)*
- Requirements for protocol bindings
- Optional: expanded threat model and privacy considerations

## Expected Attendees

Security ADs, OAuth WG participants, SCIM participants, identity architects, decentralized identity contributors (DIF/W3C), platform engineers, and researchers working on agent systems.

## Existing Work

Two individual Internet-Drafts are available for review prior to the BOF:

| Document | Status |
|---|---|
| `draft-beyer-agent-identity-problem-statement-00` | Individual submission, Informational |
| `draft-beyer-agent-identity-architecture-00` | Individual submission, Informational |

Both documents are available in this repository under `drafts/`.

## Agenda (proposed, 90 minutes)

| Time | Item |
|---|---|
| 0:00–0:10 | Introduction and problem framing |
| 0:10–0:25 | Problem statement overview |
| 0:25–0:45 | Architecture overview |
| 0:45–1:00 | Protocol binding landscape (OAuth, SCIM, PKI, DIDs) |
| 1:00–1:20 | Open discussion: scope, interest, WG viability |
| 1:20–1:30 | Hum / next steps |
