# DID Binding Notes

**Reference:** W3C DID Core 1.0 — Decentralized Identifiers
**Reference:** W3C Verifiable Credentials Data Model 2.0

These notes describe how the constructs defined in `draft-beyer-agent-identity-architecture-00` may be expressed using Decentralized Identifiers (DIDs) and Verifiable Credentials (VCs). This is not a normative binding specification; it is an analysis document to inform future binding work.

---

## Overview

DIDs are self-sovereign, cryptographically verifiable identifiers that do not require a centralized registry or certificate authority. A DID resolves to a DID Document that contains public keys and service endpoints, enabling the DID controller to prove control over the identifier. The DID ecosystem, together with Verifiable Credentials, provides arguably the strongest conceptual alignment with the agent identity architecture of any of the candidate binding technologies — particularly around decentralization, human control, and privacy-preserving design.

---

## Why DIDs Are a Strong Candidate

The DID model shares several core properties with the agent identity architecture:

- **Self-sovereign control** — the human controls the root key, not a third-party authority
- **No global linkable identifiers** — DIDs can be context-specific (pairwise DIDs) to prevent cross-context tracking
- **Cryptographic verifiability** — all claims are signed and independently verifiable
- **Decentralized** — no centralized trust anchor required
- **Composable with VCs** — Verifiable Credentials provide a structured claim format that can carry delegation semantics, scope constraints, and provenance

---

## Mapping Analysis

### Human Identity Root → DID (Controller)

The human identity root maps directly and cleanly to a DID controlled by the human. The human generates a key pair, publishes a DID Document with the public key (to any supported DID method — ledger, web, peer, etc.), and uses the private key to sign delegation credentials.

**Privacy consideration:** The DID method matters significantly for privacy. A shared ledger-based DID (e.g., `did:ion`, `did:web`) may be linkable across contexts. Pairwise DIDs (e.g., `did:peer`) created specifically for each delegation relationship better satisfy the architecture's privacy requirement.

**Binding consideration:** A binding would recommend or require the use of context-specific DIDs for the human identity root, and define how the DID Document's verification methods are used to sign delegation credentials.

---

### Agent Identity → DID (Agent-Controlled)

Each agent can be assigned its own DID, controlled by the agent (or the platform running the agent). The agent DID is not the human identity root's DID — it is a separate identifier whose authority derives from a delegation credential issued by the human.

**Binding consideration:** A binding would define the relationship between the human's DID and the agent's DID — expressed through a Verifiable Credential rather than DID Document structure — and how the agent's DID is presented to relying parties alongside the delegation credential chain.

---

### Delegation Chains → Verifiable Credential Chains

This is where the DID/VC model provides the richest support. A delegation chain can be expressed as a chain of Verifiable Credentials:

```
Human Identity Root DID
  |
  | issues
  v
Delegation Credential 1 (VC)
  {
    issuer: human-did
    subject: agent-did
    claims: {
      scope: [...],
      constraints: {...},
      validUntil: "...",
      subDelegationPermitted: true/false
    }
    proof: [human-signature]
  }

Agent DID (if sub-delegating)
  |
  | issues (within granted scope)
  v
Delegation Credential 2 (VC)
  {
    issuer: agent-did
    subject: sub-agent-did
    claims: {
      scope: [...],   // subset of scope in Credential 1
      constraints: {...},
      validUntil: "...",
      parentDelegation: credential-1-id
    }
    proof: [agent-signature]
  }
```

The chain is verifiable end-to-end: each credential's issuer DID can be resolved to verify the signature, and the `parentDelegation` reference links the chain together.

**Binding consideration:** A binding would define the VC schema for delegation credentials, the required and optional claims, how scope is expressed in a machine-readable format, and how relying parties traverse and verify the chain.

---

### Constitutional Contracts → Verifiable Credential or DID-Linked Document

Constitutional contracts can be expressed as:
- A signed **Verifiable Credential** issued by the human to themselves, expressing the governance rules for their agent ecosystem, or
- A document linked from the human's **DID Document** via a service endpoint, expressing constitutional constraints that all delegation credentials under that DID must respect

The VC approach integrates naturally with the delegation chain model. The contract VC would be included or referenced in delegation credentials, allowing relying parties to verify that the delegation complies with the human's stated governance rules.

**Binding consideration:** A binding would define the schema for a constitutional contract VC, how it is referenced by delegation credentials, and how relying parties retrieve and verify it.

---

### Replication and Instance Management → Agent DID per Instance + VC Constraints

Each agent instance can be assigned a distinct DID, with a delegation credential that covers only that instance. Constitutional contract replication constraints can be encoded in the contract VC or directly in each delegation credential (e.g., a `maxInstances` claim or an explicit per-instance authorization).

This model provides strong instance-level verifiability: a relying party can verify not just that *an* agent is authorized, but that *this specific instance* holds a valid delegation credential from the human.

**Binding consideration:** A binding would define the per-instance DID model, how instance credentials are structured relative to the canonical agent identity, and how replication constraints are expressed and enforced.

---

### Provenance → Verifiable Presentations + Audit VCs

Provenance in the DID/VC model can be expressed through:
- **Verifiable Presentations (VPs)** — the agent assembles a signed presentation containing the delegation chain VCs and presents it to the relying party. The presentation is signed by the agent's private key, creating an action-time provenance record.
- **Audit VCs** — after an interaction, an audit VC can be issued that records the delegation chain state, the agent's DID, and the action performed, creating an immutable provenance record that can be verified independently.

**Binding consideration:** A binding would define the VP structure for presenting delegation chains, and optionally define an audit VC schema for post-interaction provenance records.

---

### Revocation → Credential Status (Status List 2021 / Revocation List)

The W3C VC ecosystem defines several credential status mechanisms, including Status List 2021, which provides an efficient, privacy-preserving way to check whether a credential has been revoked without revealing which credential is being checked.

Revocation of a delegation step is expressed by revoking the corresponding delegation credential. Cascade revocation (revoking a parent credential invalidates all child credentials that reference it) must be enforced by relying parties during chain verification — if any credential in the chain is revoked, the entire chain is invalid.

**Binding consideration:** A binding would specify which credential status mechanism is required, define acceptable freshness for status checks, and specify how relying parties perform chain-level revocation verification.

---

## Privacy Advantages Over Other Binding Candidates

The DID/VC model provides the strongest native support for the architecture's privacy requirements:

- **Pairwise DIDs** enable different identifiers per relationship, preventing cross-context correlation
- **Selective disclosure** (using BBS+ signatures or SD-JWT-style VCs) allows agents to present only the claims a relying party needs, reducing information exposure
- **No centralized CA or registry** means no single point of surveillance or control
- **Self-sovereign control** means the human can issue, manage, and revoke delegation credentials without third-party involvement

---

## Considerations and Challenges

**DID method diversity:** There are dozens of DID methods with different security, privacy, and availability properties. A binding specification would need to either define a required method or define a method-agnostic profile that any conformant method must satisfy.

**Ecosystem maturity:** The DID/VC ecosystem is younger than OAuth or PKI and is still evolving. Tooling, library support, and deployment experience are less mature.

**Interoperability with existing systems:** Organizations with existing OAuth or PKI infrastructure may face integration challenges. A binding might need to define bridge patterns (e.g., OAuth tokens that reference DID-based delegation chains).

---

## Key Gaps to Address in a Binding Specification

1. DID method requirements or method-agnostic profile for human identity root
2. Delegation credential schema (VC claim set for delegation steps)
3. Constitutional contract VC or DID-linked document schema
4. Per-instance DID model and replication constraint expression
5. Verifiable Presentation structure for presenting delegation chains to relying parties
6. Credential status mechanism selection and revocation cascade semantics
7. Bridge patterns for integration with OAuth and PKI environments

---

## Relevant Standards to Consider

- **W3C DID Core 1.0** — Decentralized Identifiers
- **W3C Verifiable Credentials Data Model 2.0** — VC and VP formats
- **W3C Status List 2021** — Credential revocation
- **DIF Presentation Exchange** — Structured presentation request/response
- **BBS+ Signatures** — Selective disclosure for VCs
- **SD-JWT** (IETF) — Selective disclosure for JWT-based VCs
- **did:peer** — Pairwise DID method for privacy-preserving contexts
