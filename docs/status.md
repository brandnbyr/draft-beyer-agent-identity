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
- **Repository URL placeholders filled** — Replaced `[repository URL]` with the live GitHub URL in `presentations/slide-deck.md` and `presentations/dispatch-intro.md`.
- **README tree updated** — Added `.github/ISSUE_TEMPLATE/` folder with correct template names and `docs/status.md` to the repository structure diagram.
- **BOF venue language aligned** — `presentations/bof-outline.md` now matches `presentations/dispatch-intro.md` in naming Security/ART area and listing OAuth WG adoption as a path.
- **VC Data Model 2.0 reference added** — Added W3C VC Data Model v2.0 as an informative reference in `drafts/draft-beyer-agent-identity-architecture-00.xml`.
- **Threats/urgency slide added** — Added "What Goes Wrong" slide (Slide 4) to the slide deck covering agent laundering, shadow agents, scope creep, revocation non-propagation, and cross-platform misrepresentation. Deck is now 11 slides.

---

## Known Remaining Issues

| Issue | Location | Priority |
|---|---|---|
| No use cases document | `docs/` | Medium — useful before a BOF |

---

## Next Steps

1. **Submit to IETF datatracker** — Both drafts are ready for initial submission as individual submissions.
2. **Add `docs/use-cases.md`** — Concrete scenarios that motivate the architecture; strengthens the BOF case.
3. **Seek dispatch guidance** — Submit to IETF Dispatch (likely Security or ART area) to get direction on WG home or BOF approval.
4. **Begin binding specification work** — The four binding notes provide a ready roadmap for requirements documents.
