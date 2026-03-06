# Agent Identity: Human Anchoring, Delegation, and Provenance

This repository contains drafts, discussions, and supporting materials for an IETF effort to define a constitutional identity layer for autonomous and semi-autonomous agents.

## Status

Individual submissions seeking community review and dispatch guidance. Both documents are in early draft (version -00) and have not been submitted to the IETF datatracker.

## Drafts

| Document | Description |
|----------|-------------|
| `draft-beyer-agent-identity-problem-statement-00.xml` | Problem statement: the gaps in existing identity systems that motivate this work |
| `draft-beyer-agent-identity-architecture-00.xml` | Architecture: human identity root, delegation chains, constitutional contracts, and provenance |
| `draft-beyer-agent-identity-problem-statement-00.html` | HTML render of problem statement (human-readable, no tooling required) |
| `draft-beyer-agent-identity-architecture-00.html` | HTML render of architecture draft (human-readable, no tooling required) |

## Goals

- Define **human anchoring**: a durable relationship between a software agent and the responsible human who authorized it.
- Define **delegation chains**: explicit, verifiable, and scoped authority flows from human to agent.
- Define **provenance**: a portable record of how an agent's actions relate to its authorization chain.
- Provide a **transport-agnostic** identity architecture that complements existing systems (OAuth, SCIM, PKI, DIDs) without requiring protocol changes or global identifiers.

## Repository Structure

```
agent-identity/
│
├── README.md
│
├── .github/
│   └── ISSUE_TEMPLATE/
│       ├── bug-report.md
│       ├── technical-comment.md
│       ├── implementation-feedback.md
│       └── config.yml
│
├── drafts/
│   ├── draft-beyer-agent-identity-problem-statement-00.xml
│   ├── draft-beyer-agent-identity-architecture-00.xml
│   ├── draft-beyer-agent-identity-problem-statement-00.html
│   └── draft-beyer-agent-identity-architecture-00.html
│
├── docs/
│   ├── problem-statement-summary.md
│   ├── architecture-summary.md
│   ├── design-principles.md
│   ├── threat-model.md
│   ├── faq.md
│   └── status.md
│
├── presentations/
│   ├── bof-outline.md
│   ├── slide-deck.md
│   └── dispatch-intro.md
│
├── bindings/
│   ├── oauth-binding-notes.md
│   ├── scim-binding-notes.md
│   ├── pki-binding-notes.md
│   └── did-binding-notes.md
│
└── community/
    ├── CONTRIBUTING.md
    ├── ISSUE_TEMPLATE_bug-report.md
    ├── ISSUE_TEMPLATE_design-question.md
    ├── ISSUE_TEMPLATE_use-case.md
    └── DISCUSSION_TEMPLATE_architectural-debate.md
```

GitHub Issues and Discussions are enabled via the repository settings and do not require files in this tree.

## Contributing

Community review is welcome. See [`community/CONTRIBUTING.md`](community/CONTRIBUTING.md) for contribution guidelines, issue templates, and discussion structure.

## Author

Brandon Wesley Beyer — Independent
brandnbyr@icloud.com
