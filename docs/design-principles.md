# Design Principles

These principles govern the architecture for human-anchored agent identity, delegation, and provenance. They are derived from both the problem statement and the architecture draft and should guide any binding specification, extension, or implementation that references this work.

## Foundational Principles (from the Architecture Draft)

The architecture draft names six core principles that form its philosophical grounding. These underpin all ten design principles below:

**Sovereignty** — The human principal is the root of trust. Agents derive their authority from explicit delegation, not from platform-issued identifiers or credentials. No platform can grant an agent authority that the human did not extend.

**Constitutionality** — Agents operate under explicit, durable constraints that define the scope and limits of their delegated authority. These constraints are not runtime policy — they are embedded in the identity structure itself.

**Provenance** — Identity structures must carry verifiable information about origin, delegation, and constraints, enabling relying parties to evaluate trustworthiness at the point of interaction.

**Interoperability** — The architecture must be transport-agnostic and capable of being carried over existing authentication and authorization mechanisms without requiring protocol changes.

**Auditability** — Delegation and provenance must be verifiable without requiring disclosure of unnecessary information. Accountability and privacy are not in tension — they must coexist by design.

**Minimality** — Identity structures should contain only the information required to support delegation, provenance, and constraint evaluation. No unnecessary data should be introduced.

---

## 1. Human Authority Is the Root of All Agent Authority

Every agent derives its authority from a human. There is no legitimate agent action that cannot be traced back to a responsible human who authorized it. The human identity root is not optional, advisory, or inferrable — it is the mandatory starting point of every delegation chain.

*Implication: Systems must not treat agents as autonomous principals with independent authority. Authority flows from humans; agents carry it.*

---

## 2. Delegation Must Be Explicit

Delegation of authority from a human to an agent — or from an agent to a sub-agent — must be expressed explicitly. Implicit delegation inferred from context, embedding in tokens, or application-specific logic does not satisfy this architecture.

*Implication: Binding specifications must define a portable, inspectable delegation representation. Delegation must be readable by relying parties across ecosystem boundaries.*

---

## 3. Scope Is Always Constrained

No delegation step grants unlimited authority. Every step in a delegation chain expresses a defined scope — the types of actions permitted, the contexts in which they are valid, and any temporal or conditional constraints. A sub-delegation may only grant a subset of the authority received; it cannot expand scope.

*Implication: Relying parties must be able to verify that an agent's action falls within the scope of its delegation chain before accepting it.*

---

## 4. Provenance Must Be Portable

The record of how an agent's actions relate to its human identity root and delegation chain must survive transport across platform, organizational, and protocol boundaries. Provenance that is lost in transit is provenance that cannot be relied upon.

*Implication: Provenance must not be stored only in application-layer state or session context. It must travel with the agent or be independently verifiable.*

---

## 5. Replication Does Not Create Authority

Copying, instantiating, or migrating an agent does not grant new authority. Replicated instances carry exactly the authority expressed in the delegation chain that covers them. Unauthorized replication — creating instances outside the scope of the constitutional contract — produces agents with no legitimate standing.

*Implication: Systems must treat replication as an operation that requires explicit authorization, and relying parties must be able to distinguish authorized instances from unauthorized copies.*

---

## 6. Revocation Is a First-Class Operation

Any delegation step may be revoked by the entity that issued it. Revocation invalidates not only the revoked step but all downstream steps that depend on it. Revocation must be interpretable across contexts without relying on transport-specific mechanisms.

*Implication: Binding specifications must address revocation propagation. A relying party must be able to determine whether a delegation chain remains valid at the time of an interaction.*

---

## 7. The Architecture Is Transport-Agnostic

The architecture does not prescribe wire formats, credential schemas, or protocol mechanisms. It defines conceptual constructs and their required properties. Any existing identity, authorization, or credential system may implement a binding to these constructs without modifying its underlying protocol.

*Implication: Adoption should be incremental. Platforms can bind to the architecture using their existing infrastructure.*

---

## 8. Privacy Is a Structural Requirement

The architecture must not introduce global identifiers, cross-context correlation mechanisms, or unnecessary linkability as a side effect of expressing human anchoring or delegation. Privacy is not a feature to add later — it constrains the design of every construct.

*Implication: The human identity root is an architectural role, not a persistent global identifier. Binding specifications must preserve context separation.*

---

## 9. The Architecture Complements, Does Not Replace

This architecture does not replace OAuth, SCIM, PKI, DIDs, or other existing systems. It provides a conceptual layer that these systems can bind to. The goal is interoperability through shared semantics, not consolidation through a new centralized authority.

*Implication: Adoption should not require ecosystem-wide changes. Early adopters should be able to implement bindings alongside existing infrastructure.*

---

## 10. Accountability Must Be Verifiable, Not Just Asserted

Saying that an agent is authorized is not sufficient. A relying party must be able to *verify* the authorization — inspect the delegation chain, confirm the scope, check revocation status, and trace provenance to the human identity root. Unverifiable assertions provide no meaningful accountability.

*Implication: Delegation chains must be cryptographically sound or otherwise independently verifiable, not merely self-reported.*
