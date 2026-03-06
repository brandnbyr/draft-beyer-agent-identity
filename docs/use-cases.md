# Use Cases

This document presents concrete scenarios that motivate the agent identity architecture defined in `draft-beyer-agent-identity-architecture-00`. Each use case identifies the actors involved, the problem that arises in the absence of a coherent identity layer, and the architectural constructs that address it.

Each use case maps to one or more of the six problem dimensions identified in the problem statement: (1) lack of human anchoring, (2) unscoped or implicit delegation, (3) uncontrolled replication, (4) loss of provenance, (5) fragmented interoperability, and (6) absence of constitutional constraints.

---

## UC-1: Personal AI Assistant Acting on Behalf of a User

**Context.** A person authorizes an AI assistant to manage their calendar, send messages on their behalf, and make purchases within a spending limit. The assistant interacts with calendar services, messaging platforms, and e-commerce APIs using the person's credentials or session tokens.

**Actors.** Human principal (person); AI assistant agent; calendar service; messaging platform; e-commerce API.

**Without this architecture.** The assistant typically operates using ambient credentials — OAuth tokens, session cookies, or API keys — that are not scoped to the assistant's specific role. Relying parties (the calendar service, the messaging platform) cannot determine whether a given request was initiated by the person directly or by the assistant acting autonomously. There is no machine-readable record of what the person authorized the assistant to do, or under what constraints. If the assistant's token is compromised or the assistant acts outside the intended scope, there is no reliable signal for the relying party to detect the deviation.

**With this architecture.** The human principal issues a delegation to the assistant that explicitly scopes its authority: specific services, permitted action types, a spending cap, and a validity period. Each interaction by the assistant carries a reference to this delegation, allowing relying parties to verify the assistant's authority independently. When the principal revokes the assistant's access, the delegation becomes invalid and all relying parties that enforce it will reject subsequent requests.

**Constructs exercised.** Human identity root; delegation chain (scoped, time-limited); revocation; provenance.

**Problem dimensions addressed.** (1) Lack of human anchoring — the relying party can now verify which human authorized the assistant. (2) Unscoped delegation — the delegation carries explicit scope rather than relying on ambient credentials.

---

## UC-2: Enterprise Agent with Nested Sub-Delegation

**Context.** An enterprise employee authorizes an orchestration agent to complete a multi-step procurement workflow. The orchestration agent, in turn, delegates specific subtasks to specialized sub-agents: one to query vendor catalogs, one to generate purchase orders, and one to submit the orders for approval. Each sub-agent interacts with different internal systems.

**Actors.** Human employee; orchestration agent; vendor query sub-agent; purchase order sub-agent; approval submission sub-agent; procurement system; approval system.

**Without this architecture.** Sub-delegation is handled within the application layer — the orchestration agent spawns sub-agents using shared credentials or implicit permissions inherited from the top-level session. Each sub-agent has access to everything the orchestration agent has access to, regardless of its specific task. There is no portable record linking a sub-agent's actions back to the employee who authorized the workflow. If a sub-agent produces an anomalous action, it is difficult to determine whether it was within the scope of what the employee intended.

**With this architecture.** The employee issues a delegation to the orchestration agent that defines the overall scope and the rules under which it may sub-delegate. Each sub-agent receives a delegation from the orchestration agent that is constrained to its specific subtask. Each delegation step references the prior one, forming an inspectable chain that terminates at the employee's identity root. Any relying party — the procurement system, the approval system — can trace the full chain and verify that the sub-agent requesting an action has a legitimate delegation from a human principal.

**Constructs exercised.** Human identity root; delegation chains (nested); constitutional contracts (sub-delegation rules); provenance.

**Problem dimensions addressed.** (2) Unscoped delegation — each sub-agent is constrained to its specific role. (6) Absence of constitutional constraints — the rules under which sub-delegation is permitted are defined at the top level and propagate down the chain.

---

## UC-3: Agent Migration Across Platforms

**Context.** A human principal uses an agent on Platform A (a personal productivity service) that drafts and schedules content on their behalf. They switch to Platform B (a different productivity service) and migrate the agent. The agent continues operating on Platform B, accessing the same types of content services it used on Platform A.

**Actors.** Human principal; agent (running on Platform A, then Platform B); content services (Platform A-hosted and Platform B-hosted).

**Without this architecture.** When the agent is re-instantiated on Platform B, it loses its provenance — there is no portable record linking the Platform B instance to the original delegation the human granted on Platform A. Platform B has no standard way to verify that this agent was authorized by the same person who operated it on Platform A. The agent must re-authenticate from scratch using Platform B's own identity model, which may not reflect the original scope the human intended. Cross-platform relying parties have no consistent way to evaluate the agent's authority.

**With this architecture.** The agent carries a portable delegation chain rooted in the human principal's identity. When the agent is instantiated on Platform B, it presents this chain as evidence of its authorization. The chain is platform-independent and verifiable without a call to Platform A. The provenance model records that the Platform B instance is a continuation of the authorized agent, not a new one. Relying parties on Platform B can validate the delegation and confirm it covers the requested actions.

**Constructs exercised.** Human identity root; delegation chain (portable); provenance (cross-platform); replication model.

**Problem dimensions addressed.** (4) Loss of provenance — the delegation and provenance travel with the agent across platform boundaries. (5) Fragmented interoperability — both platforms evaluate the same portable identity constructs rather than platform-specific credentials.

---

## UC-4: Unauthorized Agent Replication

**Context.** A software vendor ships an agent that customers authorize to act on their behalf. A malicious actor obtains a copy of an authorized agent instance — its credential material, configuration, and delegation token — and re-deploys it independently to act without the original human principal's ongoing consent.

**Actors.** Human principal (original authorizer); authorized agent instance; unauthorized agent replica; relying parties.

**Without this architecture.** Because the agent's authorization is typically expressed as a long-lived token or embedded credential, copying the agent effectively copies its authority. Relying parties cannot distinguish the authorized instance from the unauthorized copy. The human principal has no mechanism to restrict how many instances may operate simultaneously or to express that replication requires explicit consent.

**With this architecture.** The delegation and constitutional contract governing the agent specify replication constraints — for example, that only a bounded number of simultaneous instances are permitted, or that new instances require a fresh delegation step signed by the human principal. A replica that lacks a valid delegation signed by the human identity root cannot satisfy the authorization requirements of a properly enforcing relying party. The human principal can revoke the delegation, invalidating all instances simultaneously.

**Constructs exercised.** Human identity root; constitutional contracts (replication constraints); delegation chain; revocation.

**Problem dimensions addressed.** (3) Uncontrolled replication — the constitutional contract defines the rules under which the agent may be replicated, and instances without valid delegations cannot act.

---

## UC-5: Revocation Propagation Across a Delegation Chain

**Context.** A human principal authorizes a top-level agent to conduct a negotiation with a third-party service. The top-level agent sub-delegates portions of the negotiation to three specialized agents. Midway through the negotiation, the human principal decides to terminate the process — either because terms have changed or because the agent's behavior was anomalous — and wants all agents in the chain to lose authority immediately.

**Actors.** Human principal; top-level negotiation agent; three sub-agents; third-party service.

**Without this architecture.** The top-level agent's token and each sub-agent's derived token are independent artifacts. Revoking the top-level token may not automatically invalidate the sub-agents' tokens, particularly if they are cached, long-lived, or held by third-party services. The human principal has no reliable way to broadcast revocation across the full delegation tree in a form that relying parties will evaluate consistently.

**With this architecture.** Each delegation step in the chain references the step above it. Revocation at any step — including the top-level delegation from the human principal — propagates to all downstream steps. Relying parties that evaluate the full chain will reject requests from any sub-agent whose chain includes a revoked step. The revocation can be expressed in a form (a revocation list, a signed revocation event, or a short-lived credential model) that fits within existing protocol bindings such as OAuth token introspection or DID-based revocation.

**Constructs exercised.** Delegation chains (nested); revocation (chain-wide propagation).

**Problem dimensions addressed.** (2) Unscoped delegation — authority is explicit and traceable. (1) Lack of human anchoring — the human principal's intent to terminate is reflected immediately in the delegation state.

---

## UC-6: Regulated Context — Healthcare Agent Acting for a Patient

**Context.** A patient authorizes an AI agent to coordinate care on their behalf: requesting referrals from their primary care provider, scheduling specialist appointments, and retrieving lab results from a hospital system. Each interaction involves a healthcare provider operating under regulatory requirements (e.g., HIPAA) for access control and audit logging.

**Actors.** Patient (human principal); care coordination agent; primary care provider system; specialist scheduling system; hospital lab system; compliance auditor.

**Without this architecture.** The agent accesses healthcare systems using OAuth tokens scoped to the patient's account. From the perspective of each healthcare provider, the requests appear to originate from the patient's OAuth client, not from an agent acting with bounded authority. There is no portable record of what the patient authorized the agent to do, under what constraints, or how the agent's actions relate to the patient's broader care intent. An auditor reviewing access logs cannot distinguish the patient's direct actions from the agent's actions. If the agent is compromised and accesses records outside the patient's care context, there is no delegation record to show the anomaly.

**With this architecture.** The patient issues a delegation that explicitly scopes the agent to specific record types, specific provider relationships, and a defined care episode. Each interaction carries the delegation chain and provenance, which healthcare systems can log and evaluate. A compliance auditor can reconstruct the full authorization record: who delegated what to which agent, when, and under what constraints. Constitutional contracts can encode that the agent may not re-delegate to other agents, ensuring the human patient remains the unique root of all healthcare access.

**Constructs exercised.** Human identity root; delegation chain (scoped, constrained); constitutional contracts (no re-delegation); provenance (auditable); revocation.

**Problem dimensions addressed.** (1) Lack of human anchoring — providers and auditors can verify the patient is the authorizing principal. (4) Loss of provenance — the delegation and access record are portable and auditable across provider systems. (6) Absence of constitutional constraints — re-delegation is prohibited at the constitutional level, not merely by application policy.

---

## UC-7: Competing Agent Ecosystems — Cross-Organizational Interoperability

**Context.** Organization A deploys an agent that negotiates supply chain terms with Organization B's procurement system. Each organization has its own identity infrastructure: Organization A uses OAuth 2.0 with enterprise IdP, Organization B uses PKI-based client certificates. The agent from Organization A must present evidence of its authority to Organization B's system in a form that Organization B can verify without calling back to Organization A's infrastructure in real time.

**Actors.** Organization A (deploying organization); agent (from Organization A); Organization B (relying party); each organization's identity infrastructure.

**Without this architecture.** The agent from Organization A carries credentials that are meaningful within Organization A's ecosystem. Organization B's system must either trust Organization A's identity infrastructure implicitly (federation), call back to Organization A to validate the credentials (online verification), or reject the agent's requests. There is no shared semantic model for expressing what the agent is authorized to do, who authorized it, or how Organization B should evaluate the scope of its authority.

**With this architecture.** The agent carries a delegation chain rooted in the authorizing human principal at Organization A. The chain is expressed using transport-agnostic constructs that can be carried in an OAuth token, a verifiable credential, or a PKI certificate extension. Organization B's system can evaluate the chain using whatever binding its infrastructure supports, without requiring federation or real-time callbacks. The delegation's scope is expressed in a portable semantic form that both organizations can interpret consistently.

**Constructs exercised.** Delegation chain (portable); human identity root; transport-agnostic binding; provenance.

**Problem dimensions addressed.** (5) Fragmented interoperability — the delegation chain provides a common semantic layer above Organization A's OAuth infrastructure and Organization B's PKI. (4) Loss of provenance — the authorizing human and scope are expressed portably and verifiable offline.

---

## Summary: Use Cases to Problem Dimensions

| Use Case | (1) Human Anchoring | (2) Implicit Delegation | (3) Replication | (4) Provenance | (5) Interoperability | (6) Constitutional Constraints |
|---|---|---|---|---|---|---|
| UC-1: Personal assistant | ✓ | ✓ | | | | |
| UC-2: Enterprise nested delegation | | ✓ | | ✓ | | ✓ |
| UC-3: Cross-platform migration | | | ✓ | ✓ | ✓ | |
| UC-4: Unauthorized replication | | | ✓ | | | ✓ |
| UC-5: Revocation propagation | ✓ | ✓ | | | | |
| UC-6: Healthcare regulated context | ✓ | | | ✓ | | ✓ |
| UC-7: Cross-org interoperability | | | | ✓ | ✓ | |
