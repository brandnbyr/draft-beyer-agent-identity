# Architecture Summary

**Full draft:** `draft-beyer-agent-identity-architecture-00`

## One-Sentence Version

The architecture defines a transport-agnostic identity layer — built on a human identity root, explicit delegation chains, constitutional contracts, and a portable provenance model — that existing systems can bind to without protocol changes.

## What This Document Does

This document defines an **architectural model**, not a protocol. It does not specify wire formats, credential schemas, or enforcement mechanisms. Instead, it provides a conceptual foundation that existing identity and authorization systems (OAuth, SCIM, PKI, DIDs) can bind to in order to support accountable, human-aligned agent ecosystems.

---

## Core Constructs

### Human Identity Root
The foundational concept of the architecture. The human identity root represents the person who ultimately bears responsibility for an agent's actions. It is an **architectural role**, not a global identifier or account — meaning it deliberately avoids cross-context tracking while still providing a stable anchor for delegation chains and provenance.

### Agent Identity
An agent identity represents a software agent authorized to act on behalf of a human. It is distinct from the human identity root and can be instantiated multiple times, provided each instance remains traceable back to the human identity root through a valid delegation chain.

### Delegation Chains
Authority flows from the human identity root to one or more agents through an explicit, inspectable delegation chain. Each step in the chain expresses:
- **Scope** — what the agent is permitted to do
- **Duration** — temporal validity constraints
- **Constraints** — additional conditions on authority
- **Revocation** — ability to invalidate the step and all downstream steps

Delegation chains support **nested delegation**: an agent that has received authority may sub-delegate a constrained subset of that authority to another agent. The chain remains fully traceable to the human identity root at every level.

### Constitutional Contracts
Constitutional contracts are the governance layer that defines the *rules* under which delegation can occur. They express durable, high-level constraints: permissible delegation depths, authority limits, replication rules, and revocation conditions. They are architectural constructs, not protocol artifacts — they inform how systems *reason about* authority without specifying how that reasoning is encoded or transmitted.

### Replication and Provenance
The architecture treats replication as a normal property of agent ecosystems. Agents may be copied, instantiated, or migrated across platforms, but replicated instances must remain within the scope of the authority granted by the human identity root. Critically, **replication does not create new authority**.

Provenance is the durable record of how an agent's actions relate to the human identity root and delegation chain. It must remain portable across platform boundaries, enabling relying parties to verify: which human authorized the agent, which delegation chain applied, whether the agent acted within scope, and how instances relate to one another.

---

## How the Pieces Fit Together

```
Human Identity Root
        |
        | (issues delegation)
        v
  Delegation Chain
  ┌─────────────────────────┐
  │  delegator              │
  │  delegate               │
  │  capabilities           │
  │  constraints            │
  │  validity / revocation  │
  └───────────┬─────────────┘
              |
              | (governed by)
              v
  Constitutional Contract
  ┌─────────────────────────┐
  │  structural constraints │
  │  authority constraints  │
  │  replication rules      │
  │  revocation conditions  │
  └─────────────────────────┘
              |
              v
    Agent Identity (acting in ecosystem)
              |
              v
    Relying Party (verifies delegation chain,
                   provenance, and constraints)
```

---

## Interoperability Positioning

The architecture sits *above* existing systems — it does not compete with them:

```
        Constitutional Identity Layer
     (human anchoring, delegation, provenance)
                      |
         ┌────────────┼────────────┐
         v            v            v
       OAuth        SCIM        PKI / DIDs
```

Existing credential systems, key structures, and authorization frameworks can serve as bindings for the architectural constructs without modification to their underlying protocols.

---

## Agent Lifecycle Model

The architecture defines a full lifecycle for agent identity and delegation, not just a static snapshot. Each phase has specific identity implications:

**Creation** — A human principal issues a delegation to instantiate an agent. The delegation binds the agent identity to the human identity root and specifies capabilities and constraints. The constitutional contract is established at this stage.

**Operation** — When interacting with relying parties, an agent presents its identity, delegation chain, and constitutional contract. The relying party validates signatures, evaluates constraints, and determines whether to trust the agent for the requested action.

**Replication** — Agents may be replicated across devices or platforms. Each instance must carry the same identity, delegation chain, and constitutional contract. Replication does not create new authority — it extends the existing delegation to additional instances within the bounds of the constitutional contract.

**Revocation** — A human principal or authorized delegate may revoke a delegation at any time. Revocation must be expressible in a way that relying parties can evaluate even in offline or asynchronous contexts. Revocation at any step invalidates all downstream steps.

**Retirement** — When an agent is retired, its identity and delegation become invalid. Audit trails must remain verifiable without enabling cross-context tracking or correlation of the human principal.

---

## Requirements for Protocol Bindings

Any protocol binding that implements this architecture must:
- Preserve the human-anchored trust model
- Preserve the semantics of delegation chains and constitutional constraints
- Avoid introducing centralized trust anchors that undermine sovereignty
- Support revocation and lifecycle governance
- Provide mechanisms for relying parties to validate signatures, provenance, and constraints

---

## Non-Goals

The architecture explicitly does not define:
- Wire formats or protocol messages
- Credential schemas or key structures
- Runtime policy enforcement engines
- Behavioral safety or agent cognition systems
- Centralized trust anchors or new global identifiers

These are left to binding specifications that reference this architecture.

---

## Relationship to the Problem Statement

Every construct in the architecture maps directly to a gap identified in the problem statement:

| Problem | Architectural Response |
|---|---|
| No human anchoring | Human identity root |
| Implicit delegation | Explicit delegation chains with scope and constraints |
| Uncontrolled replication | Replication constraints via constitutional contracts |
| Loss of provenance | Portable provenance model |
| Fragmented interoperability | Transport-agnostic binding model |
