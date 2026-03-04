# OAuth 2.0 Binding Notes

**Reference:** RFC 6749 — The OAuth 2.0 Authorization Framework

These notes describe how the constructs defined in `draft-beyer-agent-identity-architecture-00` may be expressed using OAuth 2.0 and its extensions. This is not a normative binding specification; it is an analysis document to inform future binding work.

---

## Overview

OAuth 2.0 is a widely-deployed authorization framework that governs how clients act on behalf of resource owners. It has natural overlap with several constructs in the agent identity architecture, particularly around delegation semantics and scope. However, OAuth was not designed to express human anchoring, delegation chains, or provenance as first-class artifacts — gaps that a binding specification would need to address.

---

## Mapping Analysis

### Human Identity Root → Resource Owner

In OAuth, the **resource owner** is the entity (typically a human) who grants access to protected resources. This maps approximately to the human identity root, but with important limitations:

- OAuth's resource owner is represented implicitly through the authorization grant — the identity root is not durably expressed or verifiable by the relying party post-grant.
- OAuth does not define a portable representation of the resource owner that travels with the access token to relying parties who were not party to the original authorization flow.

**Binding consideration:** A binding would need to define a verifiable, portable representation of the human identity root — potentially as a signed claim within a JWT access token or as a reference to a separate attestation document.

---

### Delegation Semantics → Scopes + Token Exchange (RFC 8693)

OAuth's **scope** parameter expresses the permissions an access token conveys, which partially maps to delegation scope in the architecture. RFC 8693 (Token Exchange) provides a mechanism for exchanging tokens in delegation and impersonation scenarios, which maps more closely to explicit delegation steps.

**Gaps:**
- OAuth scopes are flat string values — they do not express the structured delegation constraints (duration, contextual limits, revocation conditions) that the architecture requires.
- Token Exchange supports single-level delegation but does not natively express multi-level delegation chains with intermediate agents.
- The `may_act` and `act` claims in RFC 8693 provide some delegation chain expressibility but are not designed to carry full constitutional contract constraints.

**Binding consideration:** A binding might extend JWT claims to carry a structured delegation chain object, with each step expressing delegator, delegate, scope, constraints, and a validity period. The `act` claim in RFC 8693 could serve as a starting point.

---

### Constitutional Contracts → Authorization Server Policy (Partial)

The constitutional contract — the durable governance layer constraining how authority may be delegated — has no direct OAuth equivalent. Authorization server policies govern what scopes may be granted and to which clients, which partially captures the spirit but not the structure.

**Binding consideration:** Constitutional contracts might be expressed as a separate document type referenced by the authorization server or embedded as a signed claim set within the initial authorization grant. The contract would need to be verifiable by relying parties independent of the authorization server.

---

### Provenance → Structured JWT Claims

JWT access tokens (RFC 9068) carry claims about the token's origin, subject, and issuer, which provide a partial provenance record. However, standard JWT claims do not capture the full provenance model required by the architecture — particularly the delegation chain history and instance lineage.

**Binding consideration:** A binding would define additional JWT claims to express:
- The human identity root reference (non-correlating, context-scoped)
- The delegation chain summary or reference
- Instance lineage (for replicated agents)
- Provenance integrity protection (the chain of claims must be integrity-protected end-to-end, not just the terminal token)

---

### Replication Constraints → Client Registration (Partial)

OAuth client registration defines the set of authorized redirect URIs and other properties of a client identity. Multiple instances of the same client (e.g., a mobile app with many installations) are treated equivalently under the same client ID. This does not address the architecture's replication model.

**Binding consideration:** A binding would need to define how individual agent instances are distinguished within the OAuth client model, and how constitutional contract replication constraints are expressed and enforced during registration or token issuance.

---

### Revocation → Token Revocation (RFC 7009) + Introspection (RFC 7662)

OAuth token revocation (RFC 7009) and introspection (RFC 7662) provide mechanisms for invalidating tokens and checking their current validity. These map reasonably well to revocation at the token level but do not address chain-level revocation (revoking a delegation step that invalidates all downstream tokens issued under that step).

**Binding consideration:** A binding would need to define how chain-level revocation events propagate to relying parties — either through token introspection extended with chain status, or through a separate revocation registry for delegation steps.

---

## Key Gaps to Address in a Binding Specification

1. Portable, verifiable representation of the human identity root in access tokens
2. Structured delegation chain claims (multi-level, not just act/may_act)
3. Constitutional contract expression and verifiability by relying parties
4. Chain-level revocation propagation beyond single-token invalidation
5. Instance lineage for replicated agent clients
6. Cross-context provenance that survives token exchange boundaries

---

## Relevant OAuth Extensions to Consider

- **RFC 8693** — Token Exchange (delegation and impersonation semantics)
- **RFC 9068** — JWT Profile for OAuth 2.0 Access Tokens (structured claims)
- **RFC 7519** — JSON Web Token (JWT claim framework)
- **RFC 7009** — OAuth 2.0 Token Revocation
- **RFC 7662** — OAuth 2.0 Token Introspection
- **RFC 9396** — OAuth 2.0 Rich Authorization Requests (structured authorization details)
