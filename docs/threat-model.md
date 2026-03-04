# Threat Model

This document describes the threat landscape that the human-anchored agent identity architecture is designed to address. It covers the attack surfaces introduced by autonomous agents, the adversarial conditions that motivated the architecture, and the residual risks that binding specifications must handle.

---

## Scope

This threat model covers the architectural constructs defined in `draft-beyer-agent-identity-architecture-00` and the problem space described in `draft-beyer-agent-identity-problem-statement-00`. It does not address threats specific to any single binding, protocol implementation, or deployment environment — those are the responsibility of binding specifications.

---

## Adversary Model

**Adversaries may be:**
- External attackers with no legitimate presence in the ecosystem
- Compromised agents acting outside their authorized scope
- Platforms or services attempting to exceed their delegated authority
- Principals attempting to manufacture or forge delegation chains
- Agents that have been legitimately authorized but are acting in bad faith

**Adversaries are assumed to:**
- Have network-level access to interactions between agents and relying parties
- Be capable of attempting to replay, forge, or modify delegation claims
- Attempt to replicate agents without authorization
- Attempt to obscure provenance to avoid accountability
- Attempt to exploit gaps in cross-context interpretation

---

## Threats

### T1 — Human Identity Spoofing
**Description:** An agent claims to be authorized by a human without a verifiable link to that human. The delegation chain is fabricated or not independently verifiable.

**Impact:** Relying parties accept actions from agents with no legitimate human authorization, creating false accountability.

**Architectural mitigation:** The human identity root must be cryptographically bound to the delegation chain. Relying parties must be able to verify the binding independently.

**Residual risk:** Binding specifications must define key management, attestation mechanisms, and binding verification procedures.

---

### T2 — Delegation Chain Forgery
**Description:** An attacker fabricates a delegation step, extending or modifying the scope of authority in a chain that was not issued by the claimed delegator.

**Impact:** An agent claims authority it was never granted, potentially accessing resources or performing actions outside its intended scope.

**Architectural mitigation:** Each delegation step must be integrity-protected (e.g., cryptographically signed by the delegating party). Modification of any step must be detectable.

**Residual risk:** Binding specifications must select appropriate integrity mechanisms and key structures.

---

### T3 — Delegation Replay Across Contexts
**Description:** A delegation chain issued for one context (platform, time period, or scope) is presented in a different context where it was not intended to apply.

**Impact:** An agent gains access or performs actions in contexts not authorized by the human identity root.

**Architectural mitigation:** Delegation steps may express contextual constraints (scope, duration, platform). Relying parties must verify that constraints are satisfied in their context.

**Residual risk:** Binding specifications must define how contextual constraints are represented and validated cross-context.

---

### T4 — Unauthorized Replication
**Description:** An agent is copied or instantiated without the authorization of the human identity root or in violation of constitutional contract replication constraints.

**Impact:** Unauthorized agents operate in the ecosystem, potentially with the appearance of legitimacy. The human identity root cannot account for or revoke these instances.

**Architectural mitigation:** Constitutional contracts express replication constraints. Relying parties must verify that an agent instance is authorized under the applicable contract.

**Residual risk:** Binding specifications must define how instance authorization is represented and how unauthorized copies are distinguishable.

---

### T5 — Scope Creep via Sub-Delegation
**Description:** An agent that has received a constrained delegation creates a sub-delegation that grants broader authority than it itself received.

**Impact:** Authority expands beyond what the human identity root intended, violating the principle that scope cannot be expanded through delegation.

**Architectural mitigation:** The architecture requires that each sub-delegation step remain within the scope of the authority received. Relying parties must validate the full chain, not just the most recent step.

**Residual risk:** Binding specifications must define scope representation clearly enough to support cross-step containment verification.

---

### T6 — Revocation Non-Propagation
**Description:** A delegation step is revoked by the issuing party, but the revocation is not available to relying parties at the time of an interaction — whether due to delay, network partition, or inconsistent distribution.

**Impact:** Revoked agents continue to successfully interact with relying parties after their authority has been withdrawn.

**Architectural mitigation:** Revocation applies at the delegation-chain level. The architecture requires that revocation be interpretable across contexts.

**Residual risk:** Binding specifications must address revocation distribution, freshness guarantees, and acceptable staleness windows.

---

### T7 — Provenance Stripping
**Description:** An agent or intermediary removes or replaces provenance information as actions move across platforms, severing the traceable link to the human identity root.

**Impact:** Post-hoc accountability becomes impossible. Misuse cannot be attributed to a responsible human.

**Architectural mitigation:** Provenance must be portable across context boundaries and integrity-protected such that stripping or modification is detectable.

**Residual risk:** Binding specifications must define provenance encoding and cross-context transport requirements.

---

### T8 — Privacy Erosion via Linkability
**Description:** The mechanisms used to represent the human identity root or delegation chains inadvertently introduce global identifiers or cross-context correlators, enabling tracking of human behavior across platforms.

**Impact:** The architecture, intended to preserve human authority, becomes a surveillance mechanism.

**Architectural mitigation:** The architecture explicitly prohibits global identifiers and requires that privacy be a structural property, not an add-on. Binding specifications must avoid unnecessary linkability.

**Residual risk:** Each binding must be independently audited for linkability risks specific to its identifier and credential model.

---

### T9 — Ecosystem Fragmentation Exploits
**Description:** An attacker exploits inconsistencies in how different platforms interpret delegation chains, replication constraints, or provenance, gaining authority in one ecosystem that would not be recognized in another.

**Impact:** Security guarantees are inconsistent across the ecosystem, and attackers can exploit the gaps between interpretations.

**Architectural mitigation:** The architecture provides a common conceptual model. The interoperability section defines the properties that must be consistent across bindings.

**Residual risk:** Adoption of common binding specifications (rather than divergent implementations) is essential to closing this gap.

---

---

### T10 — Agent Laundering
**Description:** An agent is wrapped, proxied, or re-hosted by an intermediary in a way that obscures its origin, delegation chain, or provenance. The relying party cannot tell whether it is interacting with the original agent, a modified derivative, or an unrelated implementation using the same branding or credentials.

**Impact:** Relying parties accept interactions from agents whose actual authorization, constraints, and human anchoring are unknown. Accountability is severed at the wrapping boundary.

**Architectural mitigation:** Delegation chains and provenance structures must be integrity-protected end-to-end. A wrapped or re-hosted agent that cannot produce an intact, verifiable chain back to the human identity root must be treated as unauthenticated.

**Residual risk:** Binding specifications must define how provenance integrity is preserved across proxy and gateway boundaries, and how relying parties detect chain breaks.

---

### T11 — Shadow Agents and Unauthorized Delegation
**Description:** An agent is created or configured to act on behalf of a human without that human's informed consent or ongoing visibility. These "shadow agents" accumulate permissions and perform actions the human did not intend, while appearing legitimate to relying parties.

**Impact:** Humans lose effective control over what acts in their name. The human identity root becomes a nominal construct rather than a meaningful accountability anchor.

**Architectural mitigation:** All delegation must originate explicitly from the human identity root and be cryptographically bound. Relying parties must reject agents that cannot trace their authority through a verifiable delegation chain issued by the human.

**Residual risk:** Binding specifications must define human-consent flows for delegation issuance and provide mechanisms for humans to enumerate and audit active delegations.

---

### T12 — Privilege Escalation via Replication and Wrapping
**Description:** By replicating agents across multiple environments, or by layering additional software around them, an attacker aggregates privileges or bypasses local controls in ways that are invisible at the identity layer. Each environment may trust the agent independently, creating an aggregate authority the human never intended.

**Impact:** An agent accumulates effective permissions across platforms that exceed the scope of any individual delegation, even while each individual interaction appears legitimate.

**Architectural mitigation:** Constitutional contracts express system-wide constraints on replication and authority scope. Relying parties must verify not only the local delegation step but the full chain and applicable contract constraints.

**Residual risk:** Binding specifications must define how cross-environment replication is tracked and how relying parties detect privilege aggregation violations.

---

### T13 — Cross-Platform Misrepresentation
**Description:** Because identity semantics differ across platforms, an agent may be treated as more trusted in one environment than another, even when the underlying delegation and constraints are unclear or inconsistent. An attacker exploits this asymmetry to gain access in a permissive environment and use that standing to influence a more restricted one.

**Impact:** Security guarantees degrade at ecosystem boundaries. Agents gain standing in contexts where their actual authority was never validated.

**Architectural mitigation:** The architecture's transport-agnostic model and consistent delegation chain semantics are designed to close this gap. Relying parties in any environment must interpret the same chain constructs in the same way.

**Residual risk:** Requires broad adoption of consistent binding specifications. Fragmented implementations remain exploitable until convergence is achieved.

---

## Out of Scope

The following threats are noted but are not within scope for the architectural layer:

- **Agent cognition or behavioral safety** — the architecture does not govern how an agent makes decisions internally.
- **Protocol-level attacks** (e.g., TLS downgrade, injection) — addressed by transport-layer security.
- **Credential issuance compromise** — the architecture assumes that the human identity root binding is secure; key compromise is a binding-layer concern.
- **Platform-specific enforcement failures** — the architecture defines what must be enforceable; how enforcement is implemented is platform-specific.

---

## Summary Table

| Threat | Architectural Mitigation | Binding Responsibility |
|---|---|---|
| T1 Human identity spoofing | Human identity root binding | Key management and attestation |
| T2 Delegation chain forgery | Integrity-protected delegation steps | Signature scheme and key structure |
| T3 Delegation replay | Contextual constraints in delegation | Constraint representation and validation |
| T4 Unauthorized replication | Constitutional contract constraints | Instance authorization representation |
| T5 Scope creep via sub-delegation | Scope containment requirement | Scope representation and chain validation |
| T6 Revocation non-propagation | Chain-level revocation model | Revocation distribution and freshness |
| T7 Provenance stripping | Portable, integrity-protected provenance | Provenance encoding and transport |
| T8 Privacy erosion | No global identifiers; structural privacy | Per-binding linkability audit |
| T9 Ecosystem fragmentation exploits | Common conceptual model | Adoption of shared binding specifications |
| T10 Agent laundering | End-to-end provenance integrity | Provenance preservation across proxies and gateways |
| T11 Shadow agents / unauthorized delegation | Human-rooted explicit delegation | Human-consent flows and delegation audit mechanisms |
| T12 Privilege escalation via replication/wrapping | Constitutional contract scope constraints | Cross-environment replication tracking |
| T13 Cross-platform misrepresentation | Consistent chain semantics across contexts | Convergent binding specification adoption |
