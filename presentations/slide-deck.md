# Slide Deck: Agent Identity — Human Anchoring, Delegation, and Provenance

*Text content for slide deck. Ready to drop into Keynote or PowerPoint.*

---

## Slide 1 — Title

**Agent Identity: Human Anchoring, Delegation, and Provenance**

Brandon Wesley Beyer
Individual Submission — IETF

---

## Slide 2 — The Problem

Agents act on behalf of humans across platforms.

Identity systems treat them as clients or service accounts.

**Missing:**
- Human anchoring
- Explicit delegation
- Durable provenance

---

## Slide 3 — Why This Matters

Without a coherent identity model:

- Agents can be **impersonated**
- Agents can be **laundered or wrapped**
- Replication is **unbounded**
- Provenance is **lost** across platforms
- Relying parties **cannot evaluate trustworthiness**

---

## Slide 4 — The Gap

OAuth, SCIM, PKI, and decentralized identity frameworks provide credentials for software, but none express:

- Who the agent **represents**
- What authority was **delegated**
- Under what **constraints**
- How provenance **survives replication**

---

## Slide 5 — Architectural Stance

**A constitutional identity layer:**

- Human principal as root of trust
- Explicit, verifiable delegation chains
- Durable constitutional constraints
- Transport-agnostic identity structures
- Compatible with OAuth, SCIM, mTLS, DIDs

---

## Slide 6 — Core Constructs

```
Human Identity Root
        |
        | (issues delegation)
        v
  Delegation Chain
  [ delegator · delegate · scope · constraints · validity ]
        |
        | (governed by)
        v
  Constitutional Contract
  [ structural · authority · replication · revocation rules ]
        |
        v
  Agent Identity → Relying Party
```

---

## Slide 7 — What This Is Not

- Not a new protocol
- Not a replacement for OAuth or SCIM
- Not a behavioral safety system
- Not a tracking mechanism
- Not a centralized trust authority

---

## Slide 8 — Drafts Available

| Document | Status |
|---|---|
| Problem Statement | Individual submission, Informational |
| Architecture | Individual submission, Informational |

Both available at: *[repository URL]*

---

## Slide 9 — What We're Asking

- Community review of the drafts
- Dispatch guidance
- Interest in a BOF
- Assessment of WG viability

---

## Slide 10 — Next Steps

- Refine drafts based on community feedback
- Explore protocol binding requirements (OAuth, SCIM, PKI, DIDs)
- Evaluate cross-WG collaboration opportunities
- Submit to IETF datatracker

---

*Speaker notes and extended talking points can be added per slide. This text version maps 1:1 to the slide deck.*
