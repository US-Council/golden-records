# DPG Eligibility Questionnaire (Drafted)

This is a drafted self-assessment against the Digital Public Goods
Alliance's (DPGA) pre-nomination eligibility screen, completed for
`golden-records` ahead of a formal DPG Standard submission.

## Sources and a caveat on question wording

The DPGA publishes a standalone "Eligibility Tool" at
[digitalpublicgoods.net](https://www.digitalpublicgoods.net/) (linked from
the [DPG Standard page](https://www.digitalpublicgoods.net/standard) and the
[FAQ](https://www.digitalpublicgoods.net/frequently-asked-questions)) that
lets a prospective applicant "quickly determine if [their] digital solution
is far off from becoming a digital public good" before committing to a full
nomination. The DPGA does not publish the tool's exact question wording as
static text (it is an interactive form) — the questionnaire below is
reconstructed from the DPG Standard's own baseline gating criteria, which
the DPGA explicitly identifies as the load-bearing pre-screen factors: an
approved open license "first and foremost," availability of the
documentation the review will require, and current privacy/security
posture. See the FAQ page for that framing.

Because the live tool's exact copy isn't reproducible here, **re-run the
actual Eligibility Tool at nomination time** and reconcile its output
against this document before proceeding to
[`DPG-SUBMISSION.md`](DPG-SUBMISSION.md). This questionnaire is a
preparation aid, not a substitute for the tool.

Primary sources consulted (all confirmed 2026-07-01):
- [digitalpublicgoods.net/standard](https://www.digitalpublicgoods.net/standard) — the 9 indicators
- [digitalpublicgoods.net/frequently-asked-questions](https://www.digitalpublicgoods.net/frequently-asked-questions) — eligibility tool, submission guide, application process framing
- [DPGAlliance/DPG-Standard, `standard.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard.md) — full indicator requirement text
- [DPGAlliance/DPG-Standard, `standard-questions.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard-questions.md) — the actual application question set
- [DPGAlliance/publicgoods-candidates, `dpg-review-policy.md`](https://github.com/DPGAlliance/publicgoods-candidates/blob/main/help-center/dpg-review-policy.md) — review/nomination process mechanics

## Self-assessment

| Gating question | Answer for `golden-records` |
|---|---|
| Is the solution openly licensed under a DPGA-approved license? | Yes. Dual-licensed: [`LICENSE`](../../LICENSE) (Apache-2.0, OSI-approved, covers code) and [`LICENSE-CONTENT`](../../LICENSE-CONTENT) (CC-BY-4.0, a DPGA-approved Creative Commons license, covers content). Both license categories the Standard asks about (software, content) are covered. |
| Which DPG solution category does it fall under? | Both **Open Software** (the Copier question schema, CI, tooling) and **Open Content** (the governance/policy template corpus under `template/` and the documentation under `docs/`). The application's multi-select category field supports declaring both. |
| Does the solution have a clear, identifiable owner? | Partial. US-Council is the intended and stated owner (see `README.md`), but `CODEOWNERS` currently points at a placeholder team handle (`@US-Council/maintainers`) pending GitHub organization provisioning. See [`DPG-STANDARD-MAPPING.md`](../DPG-STANDARD-MAPPING.md) indicator 3 and [`SUBMISSION-CHECKLIST.md`](SUBMISSION-CHECKLIST.md). |
| Is the solution live/functional, not just a concept? | Yes. `copier.yml` is a working question schema; `template/` contains a substantial, renderable policy/governance corpus (39 source policies de-branded and distributed across five clusters — see `docs/POLICY-CLUSTER-MANIFEST.md`); CI (`leak-guard`) runs on every push/PR. |
| Is documentation available and sufficient for an unfamiliar user to understand and use the solution? | Mostly. `README.md`, `docs/ARCHITECTURE.md`, and inline `copier.yml` `help:` text are complete. `docs/ADOPTER-GUIDE.md` and `docs/UPGRADE-GUIDE.md` are intentionally marked "skeleton" pending the sibling onboarding milestone that walks through the now-populated `template/` payload concretely. |
| Does the solution show a credible connection to one or more SDGs? | Yes. SDG 16 (Peace, Justice and Strong Institutions) via transparent, auditable nonprofit governance infrastructure, and SDG 17 (Partnerships for the Goals) via a shared, git-native governance commons that lets many nonprofits pool governance-engineering investment instead of duplicating it. Full evidence-backed writeup in [`DPG-SUBMISSION.md`](DPG-SUBMISSION.md), indicator 1. |
| Does the solution avoid closed/proprietary mandatory dependencies? | Yes. [Copier](https://copier.readthedocs.io/) is itself open source (MIT), git is open, and `copier.yml`'s `vcs_platform` variable makes the one platform-shaped choice (GitHub vs. GitLab) adopter-selectable rather than hard-coded. |
| Does the solution have privacy/security measures appropriate to what it handles? | Yes, for the commons repo itself: dual-layer `leak-guard` CI (`gitleaks` pattern rules + a literal-string denylist) blocks any real organization data or secret pattern on every push/PR; `SECURITY.md` defines a private disclosure channel. The commons collects no PII; `copier.yml` collects only the *adopter's own* operational data, locally, never transmitted to US-Council or any third party. |
| Is there a do-no-harm posture appropriate to the solution's domain? | Yes, with a domain-specific addition: because the content produced is compliance/legal-adjacent governance language, `docs/NOT-LEGAL-ADVICE.md` provides the mandatory "this is not legal, tax, or accounting advice — consult your own counsel" framing on top of the standard 9A/9B/9C safeguards. Full detail in [`DO-NO-HARM.md`](DO-NO-HARM.md). |
| Is there a named individual able to represent the project and attest to indicators 7–9 (privacy, harm-prevention)? | Not yet finalized. The DPG application requires a title, name, and contact for the authorized representative submitting on the project's behalf — this is a human decision to be made at submission time, not something this prep work can predetermine. Tracked in [`SUBMISSION-CHECKLIST.md`](SUBMISSION-CHECKLIST.md). |

## Reading this table

Every "Yes" above still needs re-verification against the live tool and the
current `digitalpublicgoods.net/standard` page at submission time, since
both the Standard and the tool are maintained independently of this repo
and may change. The two "Partial" / "Not yet finalized" rows are the
project's honest, current gaps — they are the same gaps tracked in
[`DPG-STANDARD-MAPPING.md`](../DPG-STANDARD-MAPPING.md) and gated in
[`SUBMISSION-CHECKLIST.md`](SUBMISSION-CHECKLIST.md).

**Bottom line:** the project clears the eligibility bar the DPGA describes
as most load-bearing pre-screen factors (open license, functioning
solution, available documentation) today. It is not yet ready to submit,
because ownership provisioning and the human decision on who submits are
still open — not because the solution itself falls short of the Standard.
