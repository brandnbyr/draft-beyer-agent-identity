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

## Slide 4 — What Goes Wrong

Without a coherent identity layer, these attacks become practical:

- **Agent laundering** — malicious agent wraps a legitimate one to inherit its authority
- **Shadow agents** — unauthorized replicas operate with no accountability link
- **Scope creep** — sub-delegation expands authority beyond what the human granted
- **Revocation non-propagation** — revoked delegation continues authorizing downstream agents
- **Cross-platform misrepresentation** — agent claims authority from one platform while acting on another

---

## Slide 5 — The Gap

OAuth, SCIM, PKI, and decentralized identity frameworks provide credentials for software, but none express:

- Who the agent **represents**
- What authority was **delegated**
- Under what **constraints**
- How provenance **survives replication**

---

## Slide 6 — Architectural Stance

**A constitutional identity layer:**

- Human principal as root of trust
- Explicit, verifiable delegation chains
- Durable constitutional constraints
- Transport-agnostic identity structures
- Compatible with OAuth, SCIM, mTLS, DIDs

---

## Slide 7 — Core Constructs

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

## Slide 8 — What This Is Not

- Not a new protocol
- Not a replacement for OAuth or SCIM
- Not a behavioral safety system
- Not a tracking mechanism
- Not a centralized trust authority

---

## Slide 9 — Drafts Available

| Document | Status |
|---|---|
| Problem Statement | Individual submission, Informational |
| Architecture | Individual submission, Informational |

Both available at: https://github.com/brandnbyr/draft-beyer-agent-identity

---

## Slide 10 — What We're Asking

- Community review of the drafts
- Dispatch guidance
- Interest in a BOF
- Assessment of WG viability

---

## Slide 11 — Next Steps

- Refine drafts based on community feedback
- Explore protocol binding requirements (OAuth, SCIM, PKI, DIDs)
- Evaluate cross-WG collaboration opportunities
- Submit to IETF datatracker

---

*Speaker notes and extended talking points can be added per slide. This text version maps 1:1 to the slide deck.*
