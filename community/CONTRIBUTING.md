# Contributing

Thank you for your interest in this work. Community review and feedback are essential to producing a well-grounded architectural model and problem statement.

## Ways to Contribute

**Open an Issue** for:
- Editorial errors or unclear text in either draft
- Specific architectural concerns or questions
- Use cases that should inform the architecture

**Open a Discussion** for:
- Broader architectural debates where multiple approaches are viable
- Interoperability questions related to specific binding technologies (OAuth, SCIM, PKI, DIDs)
- Questions about scope, non-goals, or problem framing

**Submit a Pull Request** for:
- Proposed text changes to the draft XML (editorial fixes, clarifications)
- Additions or corrections to the `docs/` summary documents
- New or improved binding notes in `bindings/`

## Issue Templates

Three issue templates are available when opening a new issue:

- **Text Issue / Editorial Bug** — errors or unclear passages in the draft text
- **Technical Comment** — architecture and design feedback
- **Implementation Feedback** — notes from real-world implementation attempts

## Discussion Template

A discussion template is available for structured architectural debates. Copy the template into your discussion post when opening a significant design question.

## Draft Text Contributions

The primary draft documents are authored in IETF xml2rfc v3 XML format. Substantive changes to draft text should:

1. Be discussed in a GitHub Issue or Discussion before a PR is submitted
2. Reference the specific section anchor being modified
3. Preserve the document's format and style conventions
4. Not introduce new undefined terms without explanation

## Scope

Please keep contributions within the defined scope of this work:
- Human anchoring, delegation semantics, and provenance for software agents
- Transport-agnostic architectural constructs that complement existing identity systems

Out of scope: protocol definitions, wire formats, credential schemas, agent behavioral safety, runtime policy enforcement.

## Code of Conduct

This project follows the [IETF Guidelines for Conduct](https://www.rfc-editor.org/rfc/rfc7154). Please be respectful and constructive in all interactions.

---

## GitHub Deployment Note

This file lives in `community/` for organizational clarity. When setting up the GitHub repository:

- **\`CONTRIBUTING.md\`** should be copied to the repo root, `docs/`, or `.github/` so GitHub surfaces it automatically on the Issues and Pull Requests pages.
- **Issue templates** (`ISSUE_TEMPLATE_*.md`) should be placed in `.github/ISSUE_TEMPLATE/` and renamed (e.g., `bug-report.md`, `technical-comment.md`, `implementation-feedback.md`) for GitHub to offer them automatically when contributors open new issues.
- **Discussion templates** are not natively supported by GitHub Discussions in the same way; link to `DISCUSSION_TEMPLATE_architectural-debate.md` in your pinned discussion post for contributors to copy manually.
