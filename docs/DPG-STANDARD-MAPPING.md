# DPG Standard Mapping

This document maps `golden-records` against the
[Digital Public Good Standard](https://www.digitalpublicgoods.net/standard)
maintained by the Digital Public Goods Alliance (DPGA). It is the artifact
referenced by DPG Standard indicator 5 (Documentation) and is intended to
be kept current as the repository — and the DPG Standard itself — evolve.

Indicator names, descriptions, and the 9A/9B/9C breakdown of indicator 9
below were confirmed against digitalpublicgoods.net/standard on 2026-07-01.

| # | Indicator | What it requires | Satisfied by | Status |
|---|-----------|-------------------|--------------|--------|
| 1 | Relevance to Sustainable Development Goals | Demonstrate a clear, evidenced connection between the solution and one or more UN SDGs. | `README.md` (SDG framing: SDG 16 — Peace, Justice & Strong Institutions, via transparent nonprofit governance; SDG 17 — Partnerships for the Goals, via a shared open commons for NGO governance infrastructure). | In progress — SDG framing exists in README; a dedicated, evidence-backed writeup is needed for formal DPG submission. |
| 2 | Use of Approved Open Licenses | License the solution under an open license approved by the Open Source Initiative (for software) or a Creative Commons license (for content), per the DPGA's approved license list. | Dual licensing: `LICENSE` (Apache-2.0, OSI-approved, covers `copier.yml` logic, CI, scripts) and `LICENSE-CONTENT` (CC-BY-4.0, DPGA-approved content license, covers `template/` and `docs/`). | Met. |
| 3 | Clear Ownership | The solution must have a clearly identified, named legal or organizational owner responsible for it. | `CODEOWNERS` (commons maintainers), `README.md` (states US-Council as commons steward), `SECURITY.md` (reporting channel identifies who is responsible). | In progress — CODEOWNERS currently uses the placeholder `@US-Council/maintainers` team pending GitHub org provisioning (see Step 1 findings in the Milestone 0 report); will be Met once the org/team exists and CODEOWNERS is updated to the real handle. |
| 4 | Platform Independence | The solution must be usable without being tied to a single proprietary platform or vendor. | `copier.yml` `vcs_platform` variable (choice: gitlab/github) lets adopters render for either platform; Copier itself is a standalone, vendor-neutral CLI tool (not tied to GitHub); rendered output (`template/`) contains no platform-specific automation beyond what an adopter explicitly opts into. | Met for the template mechanism itself. Ongoing — future phases must ensure any platform-specific CI added to `template/` stays optional/parameterized, not hard-coded to one vendor. |
| 5 | Documentation | The solution must be documented sufficiently for a new user/adopter to understand, install, and use it. | `README.md` (what/who/how), `docs/ARCHITECTURE.md`, `docs/ADOPTER-GUIDE.md`, `docs/UPGRADE-GUIDE.md`, inline `help:` text on every `copier.yml` question, this mapping document. | In progress — foundation docs exist; `ADOPTER-GUIDE.md` and `UPGRADE-GUIDE.md` are intentionally skeletons in Milestone 0, to be completed once `template/` payload exists (later phase) so the walkthrough can reference real rendered output. |
| 6 | Mechanism for Extracting Data | Non-personal, non-identifiable data produced/managed by the solution must be extractable in open, interoperable formats. | Not yet applicable at the Milestone 0 stage — no data-producing runtime component exists yet; this is a governance-document template, not a data system. Copier's own `.copier-answers.yml` output is plain YAML (open, interoperable) and is itself extractable/diffable. | Phase-N — will be revisited if/when the commons adds any data-bearing component (e.g. a compliance-tracking tool); currently out of scope for a documentation-and-policy template. |
| 7 | Adherence to Privacy and Applicable Laws | The solution must comply with relevant data protection and privacy laws for the jurisdictions it operates in. | The commons repo itself collects no personal data. `copier.yml` collects only organization-level operational data (EIN, address, contact email) supplied voluntarily and locally by the adopter for their own use — it is never transmitted to US-Council or any third party by the tooling. `SECURITY.md` and this repo's leak-guard explicitly forbid real PII (director names, etc.) from ever being committed to the *commons* repo. | Met for the commons repo. Adopters are responsible for their own privacy posture once they render and populate their own repo — flagged in `docs/NOT-LEGAL-ADVICE.md`. |
| 8 | Adherence to Standards and Best Practices | The solution should follow relevant open standards and established best practices for its domain. | Copier itself is the established open-source standard for template-based repository generation with update/merge support; `copier.yml` follows Copier's documented schema conventions; CI follows standard GitHub Actions conventions; secret/leak scanning follows the `gitleaks` open-source standard tool and config format. | Met for tooling conventions. Ongoing — as `template/` fills in governance content in later phases, that content should be benchmarked against recognized nonprofit governance best-practice sources (e.g. BoardSource, National Council of Nonprofits) and cited. |
| 9A | Data Privacy and Security | The solution must incorporate adequate measures to protect data privacy and security, appropriate to the sensitivity of the data involved. | `.gitleaks.toml` + `.gitleaks-denylist.txt` + `.github/workflows/leak-guard.yml` (automated, CI-enforced, blocks merge on any real-org-data or secret pattern); `SECURITY.md` (private vulnerability/leak reporting channel via GitHub Security Advisories). | Met for the commons repo's own security posture. |
| 9B | Do No Harm by Design — Prevention of and Response to Misuse, including Inappropriate and Illegal Content | The solution must have safeguards against enabling or hosting inappropriate or illegal content. | `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1, defines and enforces acceptable contribution behavior); `CODEOWNERS` (review gate on all changes, including `template/` content, before merge); `CONTRIBUTING.md` (leak-guard-must-pass contribution contract). | Met at the process-governance level. Ongoing — as third-party contributions to `template/` policy content grow, review rigor should scale accordingly (flagged for later phases). |
| 9C | Do No Harm by Design — Protection from Harassment | The solution must include protections that guard users/contributors against harassment. | `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1 in full, including Enforcement Guidelines); `SECURITY.md` (private reporting channel usable for both security issues and Code of Conduct concerns escalation path). | Met. |

## Summary

Of the 9 indicators (11 counting the 9A/9B/9C split), **Milestone 0
establishes the structural mechanism to satisfy 6 fully (2, 4-partial, 7,
8-partial, 9A, 9C)** and lays the groundwork for the remainder. The clearest
gaps for a future DPG submission are:

- **Indicator 3 (Clear Ownership):** blocked on the US-Council GitHub
  organization being provisioned (manual step, outside API access — see
  the Milestone 0 report) and `CODEOWNERS` being updated from the
  placeholder team handle to the real one.
- **Indicator 1 (SDG Relevance):** needs a dedicated, evidence-backed
  writeup beyond the README's framing paragraph — appropriate for the
  phase that also produces `ADOPTER-GUIDE.md` content.
- **Indicator 5 (Documentation):** `ADOPTER-GUIDE.md` and
  `UPGRADE-GUIDE.md` are intentionally skeletons pending the `template/`
  payload; must be completed once policy/template content exists.
- **Indicator 6 (Data Extraction):** currently not applicable; revisit if
  the commons ever grows a data-bearing component.

This mapping should be re-verified against
[digitalpublicgoods.net/standard](https://www.digitalpublicgoods.net/standard)
before any formal DPG registration submission, as indicator language and
sub-indicator structure (9A/9B/9C) may continue to evolve.
