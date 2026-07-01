# DPG Standard Mapping

This document maps `golden-records` against the
[Digital Public Good Standard](https://www.digitalpublicgoods.net/standard)
maintained by the Digital Public Goods Alliance (DPGA). It is the artifact
referenced by DPG Standard indicator 5 (Documentation) and is intended to
be kept current as the repository — and the DPG Standard itself — evolve.

Indicator names, descriptions, and the 9A/9B/9C breakdown of indicator 9
below were confirmed against digitalpublicgoods.net/standard on 2026-07-01.

**Milestone 6 update (2026-07-01):** with M1–M4 landed (dual licensing,
CI/branch-protection, the 39-policy de-branded governance corpus, and the
AI-governance/human-in-the-loop rules in `template/`), this table's
statuses have been finalized against the current repository state. The
full evidence-backed submission dossier is in
[`docs/dpga/DPG-SUBMISSION.md`](dpga/DPG-SUBMISSION.md); the standalone
do-no-harm statement is in [`docs/dpga/DO-NO-HARM.md`](dpga/DO-NO-HARM.md);
the pre-submission eligibility self-check is in
[`docs/dpga/ELIGIBILITY-QUESTIONNAIRE.md`](dpga/ELIGIBILITY-QUESTIONNAIRE.md);
and the human go/no-go gate is
[`docs/dpga/SUBMISSION-CHECKLIST.md`](dpga/SUBMISSION-CHECKLIST.md). This
milestone prepares submission artifacts only — it does not submit anything.

| # | Indicator | What it requires | Satisfied by | Status |
|---|-----------|-------------------|--------------|--------|
| 1 | Relevance to Sustainable Development Goals | Demonstrate a clear, evidenced connection between the solution and one or more UN SDGs. | `README.md` (SDG framing) plus the full evidence-backed writeup in `docs/dpga/DPG-SUBMISSION.md` §1 (SDG 16 via `template/GOVERNANCE.md.jinja`'s git-native audit trail and the 39-policy governance corpus; SDG 17 via the shared, `copier update`-able commons model). | **Met.** Evidence-backed writeup complete; no longer just the README's framing paragraph. |
| 2 | Use of Approved Open Licenses | License the solution under an open license approved by the Open Source Initiative (for software) or a Creative Commons license (for content), per the DPGA's approved license list. | Dual licensing: `LICENSE` (Apache-2.0, OSI-approved, covers `copier.yml` logic, CI, scripts) and `LICENSE-CONTENT` (CC-BY-4.0, DPGA-approved content license, covers `template/` and `docs/`). | **Met.** |
| 3 | Clear Ownership | The solution must have a clearly identified, named legal or organizational owner responsible for it. | `CODEOWNERS` (commons maintainers), `README.md` (states US-Council as commons steward), `SECURITY.md` (reporting channel identifies who is responsible). | **Gap.** Ownership itself is not ambiguous (US-Council), but `CODEOWNERS` still uses the placeholder `@US-Council/maintainers` team pending GitHub org provisioning — a manual, outside-of-API step. Blocks public, verifiable evidence a reviewer can click through to. See `docs/dpga/SUBMISSION-CHECKLIST.md` item 1. |
| 4 | Platform Independence | The solution must be usable without being tied to a single proprietary platform or vendor. | `copier.yml` `vcs_platform` variable (choice: gitlab/github) lets adopters render for either platform; Copier itself is a standalone, vendor-neutral CLI tool (not tied to GitHub); rendered output (`template/`) contains no platform-specific automation beyond what an adopter explicitly opts into. | **Met.** The M3/M4 template payload (including CI shipped into `template/` per M4.3) confirms no hard-coded vendor dependency was introduced as real content landed. |
| 5 | Documentation | The solution must be documented sufficiently for a new user/adopter to understand, install, and use it. | `README.md` (what/who/how), `docs/ARCHITECTURE.md`, `docs/CI-AND-BRANCH-PROTECTION.md`, inline `help:` text on every `copier.yml` question, this mapping document. | **Gap.** `docs/ADOPTER-GUIDE.md` and `docs/UPGRADE-GUIDE.md` remain marked "skeleton" with `TODO (Phase 7)` markers — even though `template/` now has substantial real content (39 de-branded policies, worked bylaws example, governance templates) to walk through. Completing them is in scope for the sibling onboarding milestone (owns `docs/GETTING-STARTED.md`, `docs/ADOPTER-GUIDE.md`, `docs/UPGRADE-GUIDE.md`, `.copier-answers.yml.example`, `CONTRIBUTING.md`), not this one. See `docs/dpga/SUBMISSION-CHECKLIST.md` item 4. |
| 6 | Mechanism for Extracting Data | Non-personal, non-identifiable data produced/managed by the solution must be extractable in open, interoperable formats. | No proprietary store exists to extract from: every artifact (rendered `template/*.md.jinja` output, `.copier-answers.yml`) is born as plain, open Markdown/YAML. See `docs/dpga/DPG-SUBMISSION.md` §6 for the full argument. | **Met.** Reclassified from "not yet applicable" now that `template/` has real content to evaluate the claim against — the content is open-by-construction, not just theoretically extractable. |
| 7 | Adherence to Privacy and Applicable Laws | The solution must comply with relevant data protection and privacy laws for the jurisdictions it operates in. | The commons repo itself collects no personal data. `copier.yml` collects only organization-level operational data (EIN, address, contact email) supplied voluntarily and locally by the adopter for their own use — it is never transmitted to US-Council or any third party by the tooling. `SECURITY.md` and this repo's leak-guard explicitly forbid real PII (director names, etc.) from ever being committed to the *commons* repo. | **Met for the commons repo.** Adopters are responsible for their own privacy posture once they render and populate their own repo — flagged in `docs/NOT-LEGAL-ADVICE.md`. |
| 8 | Adherence to Standards and Best Practices | The solution should follow relevant open standards and established best practices for its domain. | Copier itself is the established open-source standard for template-based repository generation with update/merge support; `copier.yml` follows Copier's documented schema conventions; CI follows standard GitHub Actions conventions; secret/leak scanning follows the `gitleaks` open-source standard tool and config format. `template/POLICY-INDEX.md.jinja` now maps 39 policies to 2 CFR 200, IRS Form 990, NSF PAPPG, NIST SP 800-171/800-53, Bayh-Dole, and other regulatory frameworks per-policy. | **Met.** Tooling conventions and domain regulatory citations are both now substantively in place (see `docs/dpga/DPG-SUBMISSION.md` §8). One non-blocking gap remains: general nonprofit-governance best-practice sources (BoardSource, National Council of Nonprofits) are the structural model but aren't yet inline-cited the way regulatory sources are — flagged as a content-quality improvement, not a Standard-compliance blocker. |
| 9A | Data Privacy and Security | The solution must incorporate adequate measures to protect data privacy and security, appropriate to the sensitivity of the data involved. | `.gitleaks.toml` + `.gitleaks-denylist.txt` + `.github/workflows/leak-guard.yml` (automated, CI-enforced, blocks merge on any real-org-data or secret pattern); `SECURITY.md` (private vulnerability/leak reporting channel via GitHub Security Advisories). | **Met.** See `docs/dpga/DO-NO-HARM.md` for the full statement. |
| 9B | Do No Harm by Design — Prevention of and Response to Misuse, including Inappropriate and Illegal Content | The solution must have safeguards against enabling or hosting inappropriate or illegal content. | `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1, defines and enforces acceptable contribution behavior); `CODEOWNERS` (review gate on all changes, including `template/` content, before merge); `CONTRIBUTING.md` (leak-guard-must-pass contribution contract). No open user-content ingestion channel exists. | **Met at the process-governance level.** See `docs/dpga/DO-NO-HARM.md`. |
| 9C | Do No Harm by Design — Protection from Harassment | The solution must include protections that guard users/contributors against harassment. | `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1 in full, including Enforcement Guidelines); `SECURITY.md` (private reporting channel usable for both security issues and Code of Conduct concerns escalation path). Additionally, `template/GOVERNANCE.md.jinja` and the gated `ai-governance-board.md` policy enforce human-in-the-loop governance — AI agents cannot hold votes or author governance actions unattended. | **Met.** See `docs/dpga/DO-NO-HARM.md` for the full human-in-the-loop analysis. |

## Summary

**7 of 9 indicators (9 of 11 counting the 9A/9B/9C split) are now Met.**
Two genuine gaps remain, both tracked with owners and unblocking
conditions in `docs/dpga/SUBMISSION-CHECKLIST.md`:

- **Indicator 3 (Clear Ownership):** blocked on the US-Council GitHub
  organization being provisioned (manual step, outside API access) and
  `CODEOWNERS` being updated from the placeholder team handle to the real
  one.
- **Indicator 5 (Documentation):** `docs/ADOPTER-GUIDE.md` and
  `docs/UPGRADE-GUIDE.md` remain skeletons; completing them is explicitly
  in scope for the sibling onboarding milestone running in parallel, not
  this one.

Beyond the two Standard-indicator gaps above, there are also
**deployment-readiness gates that are not Standard indicators per se but
are required to actually submit**: the repository has no public git
remote yet, no semver tag has been cut, and no reference adoption has
completed end-to-end. All three are tracked in
`docs/dpga/SUBMISSION-CHECKLIST.md`, consistent with the decision to hold
submission until the reference-adopter milestone lands.

This mapping should be re-verified against
[digitalpublicgoods.net/standard](https://www.digitalpublicgoods.net/standard)
before any formal DPG registration submission, as indicator language and
sub-indicator structure (9A/9B/9C) may continue to evolve.
