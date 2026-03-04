# Problem Statement Summary

**Full draft:** `draft-beyer-agent-identity-problem-statement-00`

## One-Sentence Version

Existing identity systems authenticate software but provide no consistent way to connect an agent's actions to the human who authorized it, the scope of that authorization, or a verifiable chain of custody across platforms.

## The Core Gap

Software agents now act on behalf of people — drafting messages, initiating transactions, negotiating with other agents — without continuous human supervision. But the identity layer underneath these ecosystems was built for a different era: it authenticates *software components* and *network endpoints*, not the humans behind them.

As a result:

- A relying party can verify that *an agent* made a request, but not *which human* authorized it or under what constraints.
- Delegation happens implicitly, embedded in application logic or inferred from context — not expressed in a portable, inspectable form.
- Agents can be replicated or re-hosted freely, with no mechanism to distinguish authorized instances from unauthorized copies.
- When agents move across platforms, the provenance of their actions — who authorized what, under which conditions — is routinely lost.

## Five Problem Dimensions

**Lack of human anchoring.** Identity systems bind to software, not to the person behind it. There is no durable, verifiable link from an agent's actions back to a responsible human.

**Unscoped or implicit delegation.** Authority is granted through application-specific logic, not portable semantics. No consistent way exists to express the scope, duration, or conditions of a delegation across ecosystem boundaries.

**Uncontrolled replication.** Agents can be copied or instantiated without consent or lineage tracking. Ecosystems cannot tell authorized instances from unauthorized copies.

**Loss of provenance.** As agents traverse platforms, the record of who authorized them and under what conditions is not preserved. Accountability becomes difficult or impossible post-hoc.

**Fragmented interoperability.** Every platform handles agent identity differently. No shared model exists for expressing human anchoring, delegation, or provenance across organizational or technical boundaries.

## A Sixth Dimension: Lack of Constitutional Constraints

The early draft surfaces one additional dimension worth calling out explicitly: existing identity systems focus on *access control* (who can access what) but not on *constitutional constraints* (under what rules an agent may operate). There is no standard way to attach durable, verifiable behavioral boundaries to an agent's identity — even when such constraints are central to the human principal's expectations. This gap is addressed in the architecture by the constitutional contract construct.

## Requirements for a Solution

The problem statement defines the following high-level requirements that any architectural solution must satisfy:

**MUST requirements:**
- There MUST be a way to represent the human principal as a cryptographic root of trust for the agents that act on their behalf.
- Agents MUST have cryptographic identities that can be bound to the human root and verified by relying parties, independent of any single platform or vendor.
- Delegation from a human to an agent, and between agents, MUST be represented as explicit, signed structures that can be inspected and validated.
- Identity and delegation information MUST be designed to survive copying, wrapping, and cross-platform transport, so relying parties can evaluate provenance even when agents move between ecosystems.
- It MUST be possible to revoke or update delegations and to express lifecycle events (creation, suspension, retirement) in a form relying parties can consume.
- The identity and delegation model MUST avoid introducing global identifiers or unnecessary linkability across contexts.

**SHOULD requirements:**
- The model SHOULD support pseudonymous use and minimize disclosure of information not required for a given interaction.
- The model SHOULD be capable of being carried over existing authentication and authorization mechanisms (OAuth, mTLS, SCIM, decentralized identity) without requiring a single new monolithic protocol.

## What This Is Not

This document does not propose a protocol. It defines the problem space that an architectural model must address — the gaps, risks, and requirements that any solution must satisfy.

## Relationship to the Architecture Draft

The architecture draft (`draft-beyer-agent-identity-architecture-00`) responds directly to this problem statement by defining:
- A **human identity root** (addresses: lack of human anchoring)
- **Delegation chains** with explicit scope and revocation (addresses: implicit delegation)
- **Replication constraints** (addresses: uncontrolled replication)
- A **provenance model** portable across contexts (addresses: loss of provenance)
- A **transport-agnostic** binding model (addresses: fragmented interoperability)
