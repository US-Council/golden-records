# DPG Submission Go/No-Go Checklist

This is the human submitter's checklist. **Submission is a human decision
and a human action** — nothing in this milestone submits, creates an
account, or contacts the DPGA. This document exists so that when a human
decides to submit, they can verify every prerequisite is actually true
first, rather than discovering a gap mid-application.

## Gate items — all must be true before submitting

| # | Item | Status | Why it gates |
|---|---|---|---|
| 1 | The US-Council GitHub organization exists, and `CODEOWNERS` is updated from the placeholder `@US-Council/maintainers` handle to the real team. | **Partially done.** The `US-Council` GitHub organization exists (verified via API) and `CODEOWNERS` already uses the correct `@US-Council/maintainers` handle. The `maintainers` *team* itself has not yet been created under that org — a manual, outside-of-API provisioning step. | DPG Standard indicator 3 (Clear Ownership) requires verifiable, public evidence of ownership — a reviewer needs to be able to click through to a real team/org, not a placeholder. See `docs/DPG-STANDARD-MAPPING.md`, indicator 3. |
| 2 | The repository is pushed to a public remote (intended: `github.com/US-Council/golden-records`) and is publicly readable. | **Not done.** This worktree currently has no git remote configured at all (`git remote -v` returns nothing). | The DPGA application requires a "where is solution's source hosted" URL (see `DPG-SUBMISSION.md`, General Information). A private or nonexistent remote cannot be submitted. |
| 3 | CI (`leak-guard`) is green on the public `main` branch after the push in item 2. | Depends on item 2. | Same trust signal a DPGA reviewer or community scrutiny process (per the DPG Review Policy) would check first — a red pipeline on a public commons repo undermines every claim in `DPG-SUBMISSION.md` indicators 7 and 9A. |
| 4 | The sibling onboarding milestone's docs are merged: `docs/GETTING-STARTED.md`, a completed (non-skeleton) `docs/ADOPTER-GUIDE.md` and `docs/UPGRADE-GUIDE.md`, `.copier-answers.yml.example`, and `CONTRIBUTING.md` updates. | **Done.** Merged into `main`. | DPG Standard indicator 5 (Documentation) is now Met — see `docs/DPG-STANDARD-MAPPING.md`, indicator 5. |
| 5 | At least one live, end-to-end reference adoption exists — a real (though privacy-scrubbed per leak-guard) organization has run `copier copy` against the commons and is operating from the rendered output — consistent with the decision to hold submission until that milestone lands. | **Not done**, pending. | This is the strongest practical evidence the solution actually works as claimed for indicators 1 (SDG relevance — demonstrated impact, not just design), 5 (documentation — proven sufficient for a real unfamiliar adopter), and 8 (standards adherence — proven in practice, not just in template text). A DPG submission with zero adopters is weaker evidence than one with a working reference case, even though the Standard's review scope is nominally "the design of the core solution" rather than local implementations. |
| 6 | At least one semver git tag has been cut (e.g. `v0.1.0`) on the public repo. | **Not done** (depends on item 2). | `docs/UPGRADE-GUIDE.md` and `docs/ARCHITECTURE.md` both describe the adopt/upgrade flow as tag-based; a DPG reviewer evaluating "is this solution actually usable/stable" will look for a tagged release, not a moving `main` branch. |
| 7 | A named, authorized human representative (title, name, contact email) has been identified to submit on US-Council's behalf, and is ready to complete email verification with the DPGA webapp. | **Not done.** This is explicitly a human decision — see `DPG-SUBMISSION.md`, General Information and `ELIGIBILITY-QUESTIONNAIRE.md`. | The DPGA requires this to create the application at all, and this same individual's attestation is what indicators 7–9 (privacy, harm-prevention) rely on. Not a task an agent can complete on the organization's behalf. |
| 8 | The live DPGA Eligibility Tool has been re-run and its output reconciled against `docs/dpga/ELIGIBILITY-QUESTIONNAIRE.md`. | **Not done** — the tool is interactive and wasn't run as part of this prep milestone. | The Standard and the tool are maintained independently of this repo and may have changed since 2026-07-01, when the sources in this milestone were last confirmed. |
| 9 | `docs/dpga/DPG-SUBMISSION.md` has been re-checked, indicator by indicator, against the live `digitalpublicgoods.net/standard` page immediately before submitting. | **Not done** — by definition, cannot be done until the moment of submission. | Same reasoning as item 8: indicator language and the 9A/9B/9C sub-structure "may continue to evolve," per `docs/DPG-STANDARD-MAPPING.md`'s own closing note. |

## What's already true (no further work needed on these)

- Dual open licensing (indicator 2) — `LICENSE` (Apache-2.0) and
  `LICENSE-CONTENT` (CC-BY-4.0) are both in place and correctly scoped.
- Platform independence (indicator 4) — Copier + git, no proprietary
  dependency, `vcs_platform` variable keeps the one platform-shaped choice
  adopter-selectable.
- Non-PII data extraction (indicator 6) — content is born in open,
  non-proprietary formats (Markdown, YAML); no proprietary store exists to
  design extraction out of.
- Privacy/legal adherence for the commons repo itself (indicator 7) — no
  PII collected by the commons; leak-guard CI enforces the boundary.
- Do-no-harm 9A/9B/9C plus the legal-disclaimer posture (indicator 9) — see
  `docs/dpga/DO-NO-HARM.md`.
- SDG relevance writeup (indicator 1) and standards/best-practices mapping
  (indicator 8) — evidence-backed in `docs/dpga/DPG-SUBMISSION.md`, not
  just asserted.

## Submission steps (once every gate item above is true)

1. Go to [app.digitalpublicgoods.net](https://app.digitalpublicgoods.net)
   and use the "Nominate a DPG" flow (linked from
   [digitalpublicgoods.net/frequently-asked-questions](https://www.digitalpublicgoods.net/frequently-asked-questions)),
   which starts at `app.digitalpublicgoods.net/signup`.
2. The authorized representative identified in gate item 7 creates an
   account and completes email verification — required before an
   application can be submitted.
3. Create a new application. Each application gets a unique ID and a
   public URL of the form `app.digitalpublicgoods.net/a/[ID]`.
4. Transcribe the answers from `docs/dpga/DPG-SUBMISSION.md` into the
   webapp form, section by section (General Information, then indicators
   1–9), re-verifying each against the live Standard page per gate item 9
   as you go — do not copy-paste blind.
5. Submit. Per the [DPG Review Policy](https://github.com/DPGAlliance/publicgoods-candidates/blob/main/help-center/dpg-review-policy.md),
   the content of the submitted application becomes publicly available and
   is licensed under Unlicense — this is a one-way publication step, not
   reversible by deleting the application later. Confirm every gate item
   above is genuinely satisfied before this step, not just documented as
   "planned."
6. Track review progress two ways: the application's own public URL, and
   the pull request the DPGA opens in
   [`DPGAlliance/publicgoods-candidates`](https://github.com/DPGAlliance/publicgoods-candidates)
   as the application progresses from nominee → candidate → fully vetted
   DPG. Expect roughly 1–2 months for the process, per the DPGA's own FAQ
   guidance, though this varies.
7. If review raises questions, the DPGA's technical team (or invited
   community reviewers, per the Review Policy) may request clarification
   directly on the application or the GitHub PR — route these back to the
   authorized representative from gate item 7, not to this repository's
   general issue tracker.

## Sources

All confirmed 2026-07-01:
[digitalpublicgoods.net/standard](https://www.digitalpublicgoods.net/standard),
[digitalpublicgoods.net/frequently-asked-questions](https://www.digitalpublicgoods.net/frequently-asked-questions),
[DPGAlliance/DPG-Standard `standard.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard.md),
[DPGAlliance/DPG-Standard `standard-questions.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard-questions.md),
[DPGAlliance/publicgoods-candidates `dpg-review-policy.md`](https://github.com/DPGAlliance/publicgoods-candidates/blob/main/help-center/dpg-review-policy.md).

## Overall submission-readiness read

**Not ready today — and that's the correct state given where the other
milestones stand.** Every gap on this checklist is either (a) a manual,
non-engineering provisioning step (the GitHub org, the public remote, the
named human representative) or (b) explicitly gated on a sibling milestone
that is intentionally still in flight (onboarding docs, reference
adoption). Nothing here reflects a flaw in the solution's design — the
Standard-indicator evidence in `DPG-SUBMISSION.md` is substantively strong
today. This checklist should be re-walked, top to bottom, once items 1, 2,
and 4–6 land; item 7 requires a decision from US-Council leadership that no
amount of further engineering work resolves.
