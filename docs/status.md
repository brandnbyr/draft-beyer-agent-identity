# Project Status

**Last updated:** 2026-03-06

## Draft Status

| Document | Version | Status |
|---|---|---|
| `draft-beyer-agent-identity-problem-statement-00` | -00 | Individual submission, not yet on IETF datatracker |
| `draft-beyer-agent-identity-architecture-00` | -00 | Individual submission, not yet on IETF datatracker |

Both drafts are available in `drafts/` and as HTML renders. Community review is ongoing via GitHub Issues and Discussions.

---

## Completed

- **Problem statement and architecture drafts** — Both -00 documents written and available for review.
- **Threat model** — 13 threats (T1–T13) documented with description, impact, mitigation, and residual risk.
- **Design principles** — 10 principles derived from the architecture, with 6 foundational philosophical grounding statements.
- **Binding notes** — Analysis documents for OAuth 2.0, SCIM, PKI/mTLS, and DIDs/VCs, each identifying key gaps for a future binding specification.
- **Community infrastructure** — Issue templates (bug-report, technical-comment, implementation-feedback), issue chooser config, CONTRIBUTING.md, and Discussion template in place.
- **BOF and Dispatch materials** — BOF outline, slide deck, and Dispatch introduction prepared.
- **Repo hygiene** — Removed committed `.DS_Store` artifacts; fixed corrupted `.github/CONTRIBUTING.md`; aligned all CONTRIBUTING files to deployed template names.
- **RFC reference fix** — Moved RFC 2119 and RFC 8174 from Normative to Informative References in the problem statement. The document is Informational and uses no RFC 2119 key words; having them as normative was incorrect per IETF process.
- **Sixth problem dimension** — Added "Absence of Constitutional Constraints" as a sixth problem dimension (Section 2) and corresponding current limitation (Section 3) to the problem statement draft, reconciling it with `docs/problem-statement-summary.md`.

---

## Known Remaining Issues

| Issue | Location | Priority |
|---|---|---|
| No use cases document | `docs/` | Medium — useful before a BOF |
| `[repository URL]` placeholders unfilled | `presentations/slide-deck.md`, `presentations/dispatch-intro.md` | Medium — fill before sharing publicly |
| README tree stale (missing `.github/`, old template names) | `README.md` | Low |
| BOF outline area designation too vague vs. dispatch intro | `presentations/bof-outline.md` | Low |
| VC Data Model 2.0 not referenced in architecture draft | `drafts/draft-beyer-agent-identity-architecture-00.xml` | Low |
| Slide deck missing a threats/urgency slide | `presentations/slide-deck.md` | Low |

---

## Next Steps

1. **Submit to IETF datatracker** — Both drafts are ready for initial submission as individual submissions.
2. **Fill `[repository URL]` placeholders** in slide deck and dispatch intro.
3. **Add `docs/use-cases.md`** — Concrete scenarios that motivate the architecture; strengthens the BOF case.
4. **Seek dispatch guidance** — Submit to IETF Dispatch (likely Security or ART area) to get direction on WG home or BOF approval.
5. **Begin binding specification work** — The four binding notes provide a ready roadmap for requirements documents.
