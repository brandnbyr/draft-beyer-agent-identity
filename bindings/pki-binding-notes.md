# PKI / mTLS Binding Notes

**Reference:** RFC 5280 — Internet X.509 Public Key Infrastructure Certificate and CRL Profile
**Reference:** RFC 8705 — OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens

These notes describe how the constructs defined in `draft-beyer-agent-identity-architecture-00` may be expressed using X.509 Public Key Infrastructure (PKI) and mutual TLS (mTLS). This is not a normative binding specification; it is an analysis document to inform future binding work.

---

## Overview

PKI provides a well-established framework for binding cryptographic keys to identities through signed certificates and certificate chains. The PKI trust model — in which a certificate authority (CA) attests to the binding between a subject and a public key — has structural similarities to the agent identity architecture's delegation chain model. However, standard X.509 PKI was designed for authenticating machines, services, and humans in network contexts, not for expressing human-to-agent delegation, constitutional contracts, or cross-context provenance.

---

## Mapping Analysis

### Human Identity Root → CA-Signed Certificate or Self-Signed Root

In a PKI binding, the human identity root could be represented as:

- A **CA-issued end-entity certificate** for the human, where the CA attests to the binding between the human's identity and their key pair, or
- A **self-signed root certificate** that the human controls, used as the top-level trust anchor for all agent certificates in their delegation chain

The self-signed root model is architecturally closer — it makes the human the root of trust for their own agent ecosystem without relying on a third-party CA's interpretation of "human identity." The CA-issued model integrates more naturally with existing PKI infrastructure.

**Binding consideration:** A binding would need to define which model is required, how the human identity root certificate is distinguished from other certificates (e.g., via a custom extension), and how it is transported to relying parties for verification.

---

### Agent Identity → X.509 End-Entity Certificate

An agent identity maps naturally to an X.509 end-entity certificate, where:
- The **Subject** identifies the agent (with care about linkability)
- The **Public Key** is controlled by the agent instance
- The **Issuer** is the human identity root or an intermediate in the delegation chain
- **Extensions** carry delegation scope, constraints, and constitutional contract reference

X.509's existing extension mechanism (`subjectAltName`, custom OID extensions) provides a path for carrying architectural attributes that standard certificate fields do not cover.

**Binding consideration:** A binding would define a certificate profile with required and optional extensions for expressing agent role, delegation scope, instance identifier, and constitutional contract reference. Privacy-sensitive fields (linkable identifiers) must be handled carefully.

---

### Delegation Chains → X.509 Certificate Chains

X.509 certificate chains are the PKI model's native representation of a trust hierarchy, making this the most natural mapping in the architecture. A delegation chain can be expressed as a chain of certificates:

```
Human Identity Root Certificate (self-signed or CA-signed)
        |
        | signs
        v
Agent Certificate (intermediate or end-entity)
        |
        | signs (if sub-delegating)
        v
Sub-Agent Certificate (end-entity)
```

Each certificate in the chain expresses who signed it (the delegating party), scope constraints (via extensions), and validity period. The chain can be verified by any relying party with access to the root certificate.

**Gaps:**
- Standard X.509 does not carry arbitrary delegation scope or constitutional contract constraints in a structured, machine-readable form.
- X.509 chains typically authenticate identity, not authorization scope. Scope constraints would require custom extensions.
- Chain depth limits (`pathLenConstraint` in the BasicConstraints extension) partially address sub-delegation limits but are coarse.

**Binding consideration:** A binding would define a set of custom certificate extensions for expressing delegation scope, constitutional contract reference, and replication constraints. It would also define how relying parties should interpret chain depth constraints as delegation depth limits.

---

### Constitutional Contracts → Certificate Policy Extensions

X.509 Certificate Policies (`certificatePolicies` extension) provide a mechanism for attaching policy identifiers and policy qualifiers to certificates. This maps partially to constitutional contracts:
- A policy OID could identify the applicable constitutional contract
- Policy qualifiers could carry contract parameters or a URI reference to the contract document

**Binding consideration:** A binding would define a constitutional contract policy OID and the structure of policy qualifiers (or an external contract document format). Relying parties would need a mechanism to retrieve and verify the contract referenced by the policy extension.

---

### Replication and Instance Management → Subject Key Identifier

X.509's `subjectKeyIdentifier` extension uniquely identifies a certificate's public key. In the agent identity context, each agent instance would have a distinct key pair and thus a distinct certificate — enabling relying parties to distinguish instances.

Constitutional contract replication constraints (maximum instances, permitted contexts) have no direct X.509 equivalent and would need to be carried in custom extensions or an out-of-band registry.

**Binding consideration:** A binding would define how agent instance certificates are structured relative to the canonical agent identity, and how replication constraints are expressed and enforced at certificate issuance time.

---

### Provenance → Certificate Transparency + Audit Extensions

X.509 Certificate Transparency (RFC 9162) provides a tamper-evident log of issued certificates, which partially addresses provenance — relying parties can verify that a certificate was publicly logged and detect misissued certificates. However, CT logs capture issuance-time provenance, not action-time provenance (which human authorized which agent to take which action at which time).

**Binding consideration:** A binding would need to define how action-time provenance is captured — either through signed audit records that reference the relevant certificate chain, or through a provenance extension in certificates used for specific interaction types (e.g., mTLS client certificates).

---

### Revocation → CRL / OCSP

X.509 provides two standard revocation mechanisms:
- **CRL** (Certificate Revocation List) — a signed list of revoked certificate serial numbers, published by the CA
- **OCSP** (Online Certificate Status Protocol, RFC 6960) — a real-time query interface for checking certificate status

These map reasonably well to delegation chain revocation: revoking an agent certificate (a delegation step) can be expressed as adding it to the CRL or returning a revoked status via OCSP. Cascade revocation (revoking a parent certificate invalidating all child certificates) is handled naturally by PKI chain validation — if any certificate in the chain is revoked, the entire chain fails.

**Binding consideration:** A binding would define revocation policies for delegation chain certificates, including acceptable CRL/OCSP staleness, whether OCSP stapling is required for latency-sensitive contexts, and how constitutional contract revocation events translate to certificate revocation actions.

---

### mTLS Integration

mTLS (mutual TLS) is a natural deployment mechanism for the agent identity architecture in network contexts. An agent presents its certificate chain during the TLS handshake, and the relying party validates the chain against the human identity root. RFC 8705 (OAuth mTLS) provides a precedent for using mTLS certificates as client authentication credentials that are also bound to access tokens.

A PKI binding could leverage mTLS as the presentation layer: the agent authenticates via mTLS using its delegation chain certificate, and the relying party verifies the chain (including scope, constraints, and revocation) before accepting the interaction.

---

## Key Gaps to Address in a Binding Specification

1. Custom certificate extension set for delegation scope, constitutional contract, instance lineage
2. Certificate profile for human identity root (self-signed vs. CA-issued model)
3. Constitutional contract policy OID and external document format
4. Replication constraint expression at certificate issuance
5. Action-time provenance mechanism beyond issuance-time CT logging
6. Revocation cascade semantics and acceptable staleness policy
7. Privacy analysis of subject identifiers across context boundaries

---

## Relevant Standards to Consider

- **RFC 5280** — X.509 Certificate and CRL Profile
- **RFC 6960** — Online Certificate Status Protocol (OCSP)
- **RFC 9162** — Certificate Transparency Version 2.0
- **RFC 8705** — OAuth 2.0 Mutual-TLS Client Authentication
- **RFC 5755** — Attribute Certificates (alternative model for delegation attributes)
