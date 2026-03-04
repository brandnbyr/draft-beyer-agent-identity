# SCIM Binding Notes

**Reference:** RFC 7643 — System for Cross-domain Identity Management: Core Schema
**Reference:** RFC 7644 — System for Cross-domain Identity Management: Protocol

These notes describe how the constructs defined in `draft-beyer-agent-identity-architecture-00` may be expressed using SCIM. This is not a normative binding specification; it is an analysis document to inform future binding work.

---

## Overview

SCIM (System for Cross-domain Identity Management) is a provisioning protocol designed to make user and group identity data portable across systems. It defines a schema for identity resources and a REST-based protocol for managing them. SCIM's provisioning-oriented model makes it a natural fit for expressing agent identity as a managed resource — but SCIM was designed for human users and groups, not for agents with delegation chains, constitutional contracts, or provenance requirements.

---

## Mapping Analysis

### Human Identity Root → SCIM User Resource

SCIM's core `User` resource represents a human identity with attributes such as `userName`, `name`, `emails`, and organizational associations. This maps reasonably well to the human identity root as a managed entity, with important caveats:

- SCIM User resources carry persistent identifiers (`id`, `externalId`) that may introduce linkability across contexts if reused without care — in tension with the architecture's privacy requirement.
- SCIM does not express the *role* of the human as the root of agent authority; it represents user attributes for provisioning purposes.

**Binding consideration:** A binding might define a SCIM extension schema for a `HumanIdentityRoot` resource that represents the architectural role — potentially context-scoped, with limited linkable attributes — rather than reusing the full User resource directly.

---

### Agent Identity → SCIM Extension Resource

SCIM does not natively define a resource type for software agents. However, SCIM's extension mechanism allows custom resource types and schemas to be defined. An `Agent` resource type could be defined with attributes expressing:
- Reference to the human identity root (by SCIM resource reference)
- Agent role and capabilities
- Instance identifier and lineage
- Reference to the governing delegation chain and constitutional contract

**Binding consideration:** A binding specification would define an `Agent` schema (e.g., `urn:ietf:params:scim:schemas:extension:agentidentity:2.0:Agent`) with the required attributes and a corresponding resource endpoint. SCIM's existing referencing mechanisms (`$ref`) can express relationships between agents and human identity roots.

---

### Delegation Chains → SCIM Complex Attribute or Separate Resource

SCIM supports complex multi-valued attributes and resource references, which could express individual delegation steps. A delegation chain could be represented as:
- A complex attribute on the `Agent` resource listing delegation steps in sequence, or
- A separate `DelegationChain` resource type with references to the delegating and delegated parties

The tradeoffs are significant: embedding the chain as an attribute is simpler but limits chain depth and makes revocation of individual steps more complex. A separate resource type is richer but requires additional provisioning protocol interactions.

**Binding consideration:** A binding would need to define the delegation chain representation, the schema for individual steps (delegator, delegate, scope, constraints, validity), and how chain updates (revocation, modification) are propagated via SCIM patch operations.

---

### Constitutional Contracts → SCIM Extension Resource

Constitutional contracts have no SCIM equivalent. They represent a governance document that constrains the structure and scope of all delegation chains under a given human identity root.

**Binding consideration:** A `ConstitutionalContract` resource type could be defined, referenced by the `Agent` and `DelegationChain` resources. SCIM's `READ` and filtering capabilities would allow relying parties to retrieve and verify the applicable contract for a given agent.

---

### Replication and Instance Management → SCIM Multi-Instance Model

SCIM manages identity resources as unique entities — it does not natively model the idea of one agent being instantiated as multiple authorized copies. However, SCIM's `Group` or bulk resource management patterns could support managing a set of authorized instances under a parent agent identity.

**Binding consideration:** A binding would define how agent instances are represented as distinct SCIM resources related to a canonical agent identity, and how constitutional contract replication constraints are enforced by the provisioning service at instance creation time.

---

### Provenance → SCIM Meta and Audit Extension

SCIM's `meta` attribute captures creation and modification timestamps and the resource's location. This is a minimal provenance record. The richer provenance model required by the architecture — capturing delegation chain state at the time of an action, instance lineage, and human identity root reference — exceeds what SCIM `meta` provides.

**Binding consideration:** A provenance extension schema could be defined, capturing the key provenance claims that relying parties need to verify accountability. Alternatively, provenance records could be expressed as immutable SCIM resources created at action time and referenced by relying parties.

---

### Revocation → SCIM DELETE and PATCH Operations

SCIM supports `DELETE` for resource removal and `PATCH` for partial updates, including deactivation via the `active` attribute. These provide a reasonable mechanism for expressing revocation:
- Revoking a delegation step: PATCH the delegation chain resource to mark the step inactive, or DELETE the step resource
- Cascading revocation: the provisioning service propagates the revocation to downstream delegation steps

**Binding consideration:** A binding would define the revocation event model and how provisioning clients are expected to respond to revocation notifications. SCIM's event feed or push notification patterns (if applicable in the deployment) would need to support timely revocation propagation.

---

## Key Gaps to Address in a Binding Specification

1. Agent resource type definition with full delegation chain reference support
2. HumanIdentityRoot resource type with privacy-preserving identifier model
3. ConstitutionalContract resource type with constraints schema
4. Multi-instance model for replicated agents with replication constraint enforcement
5. Provenance extension schema capturing action-time delegation chain state
6. Revocation cascade model for chain-level invalidation
7. Cross-domain federation — SCIM's cross-domain model needs extension to support agent identity portability outside the provisioning relationship

---

## Relevant SCIM Extensions to Consider

- **RFC 7643** — Core Schema (User, Group, EnterpriseUser)
- **RFC 7644** — Protocol (REST operations, filtering, bulk operations)
- SCIM extension schema mechanism for custom resource types
- SCIM event model for push notification of resource changes (if standardized)
