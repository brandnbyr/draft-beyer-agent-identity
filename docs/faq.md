# Frequently Asked Questions

---

## General

**Q: What problem does this work address?**

A: Existing identity systems authenticate software components and network endpoints, but they don't provide any consistent way to represent the human who authorized an agent, the scope of that authorization, or the provenance of an agent's actions across platforms. As agents increasingly act without continuous human supervision — across communication, automation, and decision-making contexts — this gap creates real accountability and security problems. This work defines the problem and an architectural model to address it.

---

**Q: Is this a new protocol?**

A: No. The problem statement is explicitly not a protocol. The architecture draft also does not define a protocol, wire format, or credential schema. It defines conceptual constructs — a human identity root, delegation chains, constitutional contracts, and a provenance model — that existing systems can bind to. Binding specifications (how OAuth, SCIM, PKI, DIDs, etc. implement these constructs) are separate work.

---

**Q: Why bring this to the IETF?**

A: The IETF is the appropriate venue because the problem is inherently cross-platform and cross-ecosystem. Agent identity problems arise precisely because agents move across organizational and technical boundaries. A solution requires a shared architectural model that any platform can implement — and the IETF process, with its emphasis on rough consensus and running code, is well-suited to producing one.

---

**Q: What is the current status of these documents?**

A: Both documents are version -00 individual submissions. They have not been submitted to the IETF datatracker. The goal at this stage is community review and dispatch guidance — establishing whether the problem statement is well-scoped and whether the architectural approach has enough support to form a working group or adopt as an individual submission stream.

---

## Architecture

**Q: What is a "human identity root"?**

A: The human identity root is an architectural role — not a global identifier, not an account, and not a credential. It represents the human who ultimately bears responsibility for an agent's actions. The architecture does not prescribe how it is instantiated, stored, or authenticated; it defines what it must do: provide a stable anchor for delegation chains and provenance that existing identity systems can bind to.

---

**Q: What is a "constitutional contract"?**

A: A constitutional contract is the governance layer that defines the rules under which a human may delegate authority to agents: permissible delegation depth, authority constraints, replication limits, and revocation conditions. It is not a runtime policy engine and does not prescribe enforcement mechanisms. Think of it as the durable "terms of engagement" that governs a human's agent ecosystem.

---

**Q: How does delegation work?**

A: Authority flows from the human identity root through an explicit, inspectable delegation chain. Each step in the chain expresses who delegated to whom, under what scope, for what duration, and under what constraints. An agent that has received delegated authority may sub-delegate a constrained subset of that authority to another agent — but cannot expand scope beyond what it received. Any step in the chain may be revoked by the entity that issued it, which invalidates all downstream steps.

---

**Q: What happens when an agent is replicated?**

A: Replication is treated as a normal property of agent ecosystems — agents are copied, instantiated, and migrated across platforms routinely. The architecture requires that replicated instances remain within the scope of the authority granted by the human identity root and that replication does not create new authority. Constitutional contracts may express explicit replication constraints (e.g., maximum instances, permitted contexts).

---

**Q: Does this require global identifiers?**

A: No. One of the explicit design principles is that the architecture must avoid global identifiers and cross-context correlation mechanisms. The human identity root is an architectural role that can be bound to context-specific representations. Privacy is a structural requirement, not an afterthought.

---

## Interoperability

**Q: Does this replace OAuth / SCIM / PKI / DIDs?**

A: No. The architecture is explicitly designed to complement these systems, not replace them. It sits above existing mechanisms as a conceptual layer, and existing systems serve as bindings. OAuth can express delegation semantics; SCIM can represent agent identity; PKI and DIDs can instantiate the human identity root. None of these protocols need to be modified to adopt the architectural model.

---

**Q: Does adopting this architecture require changes to existing protocols?**

A: No. Adoption is incremental. A binding specification defines how an existing protocol implements the architectural constructs, and platforms can implement that binding alongside their existing infrastructure. There is no flag-day migration requirement.

---

**Q: What about platforms that use their own proprietary identity systems?**

A: The architecture is transport-agnostic and does not prescribe specific credential formats or key structures. Any system that can represent the required properties — a human identity root, explicit delegation semantics, scope constraints, and portable provenance — can implement a compliant binding.

---

## Scope and Non-Goals

**Q: Does this cover agent "behavior" or AI safety?**

A: No. The architecture addresses identity, delegation, and provenance — the question of *who authorized an agent and under what conditions*, not *how an agent makes decisions*. Agent cognition, behavioral safety systems, and content governance are explicitly out of scope.

---

**Q: Does this define how revocation information is distributed?**

A: No. The architecture defines the conceptual model — revocation applies at the delegation-chain level and invalidates downstream steps. How revocation information is distributed, how fresh it must be, and how relying parties check it are all binding-layer concerns.

---

**Q: Does this work apply to non-AI software agents?**

A: Yes. The architecture applies to any software agent acting on behalf of a human — automated scripts, bots, workflow orchestrators, and AI-powered agents alike. The motivating use cases are increasingly AI-driven, but the architectural constructs are general.

---

## Contributing

**Q: How can I provide feedback?**

A: Open a GitHub Issue for specific questions, concerns, or proposed changes to the draft text. Use GitHub Discussions for broader architectural conversations — design alternatives, use case analysis, or interoperability questions. Formal IETF comments may be submitted via the datatracker once these documents are submitted.

---

**Q: Is there a mailing list?**

A: Not yet. Once this work is submitted to the IETF datatracker and seeks dispatch guidance, it will be directed to an appropriate area mailing list. Watch this repository for updates.
