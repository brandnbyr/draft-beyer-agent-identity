# Agent Identity: Human Anchoring, Delegation, and Provenance

**An open architectural standard for human-anchored agent identity, explicit delegation, and verifiable provenance.**

---

## The Problem

When a software agent takes an action today, there is no consistent, verifiable way to answer a basic question:

**Who is actually responsible for this?**

Was it the human? The agent acting within its intended scope? The agent acting outside it? A second agent it delegated to without the human's knowledge? An unauthorized copy of the original agent? A compromised instance? A bad actor who constructed a plausible-looking delegation chain?

Current identity systems cannot distinguish between these cases. They authenticate software components, devices, and network endpoints — but they have no model for representing the human behind an agent, the scope of authority that human actually granted, or the chain of delegation that led to a specific action.

The result is an accountability vacuum. Agents act. Actions propagate. And the connection between those actions and the humans responsible for them is invisible, unverifiable, and increasingly easy to exploit.

This is not a gap in any single product or protocol. It is a structural absence at the architectural level. And as agents become more capable and more autonomous, the consequences grow.

---

## The Gap

Existing identity systems — OAuth, SCIM, mTLS, even DIDs — were designed for a world where software authenticates to services. They answer the question "is this client who it claims to be?" They do not answer:

- **Who authorized this agent, and what exactly did they authorize?**
- **Is this delegation chain valid, or was it constructed without the human's knowledge?**
- **Is this a legitimate instance, or an unauthorized copy?**
- **Did this action fall within the agent's intended scope, or outside it?**
- **Can any of this be verified across platforms, or only within a single system?**

Without answers to these questions, ecosystems cannot detect impersonation, cannot prevent unauthorized replication, cannot enforce delegation boundaries, and cannot trace actions back to responsible humans.

The attack surface this creates is not hypothetical. An agent acting on behalf of a malicious actor looks identical to an agent acting on behalf of a legitimate user. A forged delegation chain looks identical to a valid one. An unauthorized copy looks identical to the original. There is no architectural mechanism to tell them apart.

---

## This Repository

This repository contains drafts and supporting materials for an architectural standard addressing human-anchored agent identity.

The core contributions are:

1. **A problem statement** — defining the structural gaps in current identity systems as they relate to autonomous and semi-autonomous agents acting on behalf of humans

2. **An architecture draft** — defining three foundational constructs:
   - A **Human Identity Root** — a stable, privacy-preserving representation of the human who ultimately bears responsibility for an agent's actions
   - **Delegation Semantics** — explicit, portable, scoped authority that travels with the agent across platforms, with revocation at the chain level
   - **Provenance Model** — a durable record connecting agent actions to the delegation chain and human identity root that authorized them

3. **Binding notes** — how the architecture maps to and complements existing systems: OAuth, SCIM, PKI, DIDs

---

## Scope and Non-Goals

This architecture does not define a protocol, wire format, or enforcement mechanism. It provides a conceptual foundation that existing systems may bind to incrementally — without breaking existing infrastructure or requiring protocol changes.

It is transport-agnostic. It does not introduce new global identifiers. It does not require centralized trust anchors. Existing credential systems — including X.509, OAuth tokens, and DIDs — may serve as bindings to the architectural constructs defined here.

The goal is infrastructure. Like TCP/IP. Like X.509. Like Git. Something the world builds on, that nobody owns.

---

## Repository Structure

```
agent-identity/
│
├── drafts/
│   ├── draft-beyer-agent-identity-problem-statement-00.xml
│   ├── draft-beyer-agent-identity-architecture-00.xml
│   └── README.md
│
├── docs/
│   ├── problem-statement-summary.md
│   ├── architecture-summary.md
│   ├── design-principles.md
│   ├── threat-model.md
│   └── faq.md
│
├── presentations/
│   ├── bof-outline.md
│   └── dispatch-intro.md
│
├── bindings/
│   ├── oauth-binding-notes.md
│   ├── scim-binding-notes.md
│   ├── pki-binding-notes.md
│   └── did-binding-notes.md
│
└── README.md
```

---

## Status

Individual submissions seeking community review and dispatch guidance.

Both drafts are in active development. Feedback, issues, and discussion are welcome.

If you work on identity, security, agent systems, or distributed architectures — and especially if you've been thinking about the accountability gap this work addresses — this is an open conversation.

---

## Author

Brandon Wesley Beyer
Independent
brandnbyr@icloud.com

---

*"Agents already act on behalf of humans at scale. The question is no longer if, nor when; but how do we maintain accountability?"*
