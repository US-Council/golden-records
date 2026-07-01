# DPG Submission Dossier (Drafted)

**Status: draft, prepared for human review. Not submitted.** This document
is the complete, evidence-backed answer set for a Digital Public Goods
Alliance (DPGA) nomination of `golden-records`, organized to mirror the
DPGA's own application question set so a human submitter can copy these
answers directly into the webapp at
[app.digitalpublicgoods.net](https://app.digitalpublicgoods.net) with
minimal editing. See [`SUBMISSION-CHECKLIST.md`](SUBMISSION-CHECKLIST.md)
for what must be true before anyone actually submits, and
[`DPG-STANDARD-MAPPING.md`](../DPG-STANDARD-MAPPING.md) for the
indicator-by-indicator status summary this dossier is drawn from.

Sources for the question structure and indicator requirements (all
confirmed 2026-07-01): the [DPG Standard](https://www.digitalpublicgoods.net/standard),
[`DPG-Standard/standard.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard.md),
[`DPG-Standard/standard-questions.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard-questions.md),
and the [DPG Review Policy](https://github.com/DPGAlliance/publicgoods-candidates/blob/main/help-center/dpg-review-policy.md).

---

## General information

| Field | Draft answer |
|---|---|
| Solution name | Golden Records |
| Aliases | `USCouncil/golden-records`; "the golden-records commons" |
| Solution category | **Open Software** (Copier question schema, CI, tooling — Apache-2.0) and **Open Content** (governance/policy template corpus and documentation — CC-BY-4.0). The application's category field is multi-select; both apply. |
| Short description (tweet-length) | "A public, vendor-neutral commons of governance-as-code templates — bylaws, policies, and compliance checklists — that any US nonprofit can render into their own repository via Copier, then own and maintain independently." |
| Website | `https://uscouncil.org` (USCouncil, the maintaining organization) |
| Where solution's source is hosted | **Pending.** Intended: `https://github.com/USCouncil/golden-records`. The repository does not yet have a public remote — see `SUBMISSION-CHECKLIST.md`, item 2. This field cannot be completed until the repo is pushed and public. |
| Alternate contact email | **To be supplied by the human submitter.** DPGA requires a verified email tied to an authorized representative of the applicant; this is a human decision, not something drafted here (see `SUBMISSION-CHECKLIST.md`, item 7). |

---

## Indicator 1 — Relevance to Sustainable Development Goals

**Requirement (Standard §1):** "DPGs must show a connection to advancing
progress on the United Nations SDGs."

**SDGs selected:** SDG 16 (Peace, Justice and Strong Institutions), SDG 17
(Partnerships for the Goals).

**SDG 16 — Peace, Justice and Strong Institutions.** Target 16.6 calls for
"effective, accountable and transparent institutions at all levels"; target
16.7 calls for "responsive, inclusive, participatory and representative
decision-making." A nonprofit board that cannot produce an auditable,
tamper-evident record of its own governance decisions cannot credibly claim
to be an accountable institution — and small-to-midsize nonprofits
routinely lack the in-house capacity to build that infrastructure
themselves. `golden-records` makes institutional accountability
structural rather than aspirational:

- `template/GOVERNANCE.md.jinja` defines governance as code: "Each policy
  is a file. Each amendment is a commit. Each adoption is a merge... Git
  history is the immutable audit trail. Every change to every document is
  cryptographically linked and timestamped." Board approvals become the
  vote; the merge commit becomes the legally operative adoption record.
- The rendered governance corpus (39 source policies, distributed across
  federal-compliance, research-integrity, HR/operations, risk/legal, and
  AI-governance clusters — see `docs/POLICY-CLUSTER-MANIFEST.md`) gives an
  adopting nonprofit a working conflict-of-interest policy, document
  retention policy, delegation-of-authority policy, and code of ethics from
  day one, each explicitly cross-referenced to the IRS Form 990 governance
  questions that indirectly measure institutional accountability
  (Part VI, Lines 12 and 14).
- This is capacity-building infrastructure, not a one-off document: the
  Copier `copier update` mechanism means an adopter's governance posture
  improves as the commons improves, without re-drafting from scratch.

**SDG 17 — Partnerships for the Goals.** Target 17.16 calls for a "Global
Partnership for Sustainable Development, complemented by multi-stakeholder
partnerships that mobilize and share knowledge, expertise, technology and
financial resources." `golden-records` is a literal instance of this: it is
a shared, open commons that lets many independent nonprofits pool the cost
of governance-engineering (legal-template drafting, compliance mapping,
policy maintenance) that each would otherwise duplicate in isolation. The
dual-license structure (Apache-2.0 for tooling, CC-BY-4.0 for content) and
the `copier update` 3-way-merge upgrade path are specifically designed so
that an improvement contributed by, or for, one adopting organization
benefits every other adopter — the definition of a shared resource
"multiplying the collective impact of philanthropic capital, official
development assistance, and public expenditure," in the DPGA's own
articulation of its mission.

**Evidence:** `README.md` ("Digital Public Good posture" section),
`template/GOVERNANCE.md.jinja`, `docs/POLICY-CLUSTER-MANIFEST.md`,
`template/POLICY-INDEX.md.jinja` (federal/tax citation mapping per policy).

---

## Indicator 2 — Use of Approved Open Licenses

**Requirement (Standard §2):** software requires an OSI-approved license;
content requires a Creative Commons license (CC-BY and CC-BY-SA
"encouraged").

**Evidence of license use:**

- `LICENSE` — Apache License, Version 2.0. OSI-approved. Covers
  `copier.yml` logic, CI workflows (`.github/workflows/`), and tooling.
- `LICENSE-CONTENT` — Creative Commons Attribution 4.0 International
  (CC-BY-4.0). One of the two DPG-Standard-encouraged CC variants. Covers
  `template/` (the rendered policy/governance content) and `docs/`.
- `README.md`'s "Licensing" table and `docs/ARCHITECTURE.md`'s
  "Why dual licensing" section document the file-by-file boundary and the
  rationale (code vs. prose) for a human reviewer.

**Status: Met.** Both applicable content categories (software, content) are
covered by DPGA-approved licenses.

---

## Indicator 3 — Clear Ownership

**Requirement (Standard §3):** ownership must be unambiguous, evidenced via
copyright, trademark, or similar public documentation.

**Who owns this digital solution:** USCouncil.

**Evidence of ownership:** `README.md` states USCouncil as commons steward
and links `https://uscouncil.org`. `CODEOWNERS` names the responsible
review authority for changes to the commons. `SECURITY.md`'s reporting
channel identifies who is responsible for the repository.

**Do we own all of the code, content, and/or data:** Yes — the commons
contains no third-party code, content, or data requiring redistribution
rights; the policy corpus was authored/de-branded specifically for this
commons (see `docs/DEBRANDING-SPEC.md`).

**Status: Gap, pending a provisioning step.** `CODEOWNERS` currently
references a **placeholder** team handle (`@USCouncil/maintainers`) because
the USCouncil GitHub organization has not yet been provisioned — this is a
manual, outside-of-API step, not an engineering task. Ownership itself
(USCouncil as the legal/organizational owner) is not in question; what's
pending is the *public, verifiable evidence* of it (a real org/team handle
DPGA reviewers can click through to). See `SUBMISSION-CHECKLIST.md`,
item 1.

---

## Indicator 4 — Platform Independence

**Requirement (Standard §4):** if mandatory dependencies impose
restrictions beyond the core license, the project must show independence
from closed components, or open alternatives usable without significant
changes.

**Core technologies depended on:** [Copier](https://copier.readthedocs.io/)
(open source, MIT-licensed, standalone CLI — not a hosted SaaS or vendor
product), Git (open, distributed, no single vendor), Jinja2 templating
(open source). No proprietary runtime, database, or hosted service is
required to author, render, or consume the template.

**Does the solution use closed components creating proprietary
dependency:** No. The one place a platform choice exists at all —
GitHub vs. GitLab — is exposed as the `vcs_platform` Copier variable in
`copier.yml`, so an adopter explicitly chooses their platform rather than
having one hard-coded into the rendered output. `docs/ARCHITECTURE.md`
documents this as a deliberate design constraint (`_subdirectory: template`
keeps commons-only, GitHub-shaped tooling out of the rendered payload).

**Status: Met.**

---

## Indicator 5 — Documentation

**Requirement (Standard §5):** for software, documentation sufficient for
an unfamiliar technical person to launch and run it; for content,
compatible tools/usage instructions.

**Where documentation lives:** `README.md` (what/who/how), `docs/ARCHITECTURE.md`
(topology and rationale), `docs/ADOPTER-GUIDE.md` (adoption walkthrough),
`docs/UPGRADE-GUIDE.md` (`copier update` mechanics and the dirty-tree
gotcha), `docs/CI-AND-BRANCH-PROTECTION.md` (CI requirements), inline
`help:` text on every `copier.yml` question, and `docs/DPG-STANDARD-MAPPING.md`
(this DPG alignment itself, which is itself a form of documentation).

**Status: In progress, one concrete gap.** The upgrade mechanism, CI
requirements, and architecture are fully and accurately documented today.
`docs/ADOPTER-GUIDE.md` and `docs/UPGRADE-GUIDE.md` are explicitly marked
"skeleton" in their own headers, with `TODO (Phase 7)` markers — they were
scaffolded before `template/` had real content to walk through. `template/`
now has a substantial policy corpus (39 de-branded policies, a worked
bylaws example, governance/CLAUDE.md/README templates — see git history
M1–M3), so those TODOs are ready to be resolved, but doing so is explicitly
in scope for the sibling onboarding milestone (owning
`docs/GETTING-STARTED.md`, `docs/UPGRADE-GUIDE.md`,
`docs/ADOPTER-GUIDE.md`, `.copier-answers.yml.example`, `CONTRIBUTING.md`),
not this one. Do not submit until that milestone lands — see
`SUBMISSION-CHECKLIST.md`, item 4.

---

## Indicator 6 — Mechanism for Extracting Data and Content

**Requirement (Standard §6):** non-PII data/content should be designed for
export or import in a non-proprietary format.

**Does the solution collect/use/generate non-PII data or content:** Yes —
its entire output *is* content: rendered governance documents (policies,
bylaws, checklists).

**Extraction mechanism:** Trivially satisfied by construction, not by
add-on tooling. Every artifact this solution produces — from the template
source (`template/*.md.jinja`) to the rendered output in an adopter's repo
— is plain Markdown, from the moment it exists. There is no database, no
proprietary binary format, and no export step to design, because nothing
is ever stored in a closed format in the first place. The one piece of
structured metadata Copier itself generates — the answers file
(`.copier-answers.yml`, per `_copier_conf.answers_file`) — is plain,
diffable, open YAML. `git log` and `git diff` are themselves the
extraction/audit interface (see `template/GOVERNANCE.md.jinja`'s framing of
git history as "the minute book").

**Status: Met.** This is a documentation-and-policy template with no
data-bearing runtime component, so there is no proprietary store to design
extraction *out of* — the content is born open.

---

## Indicator 7 — Adherence to Privacy and Applicable Laws

**Requirement (Standard §7):** design and development must comply with
privacy law and other relevant legal obligations.

**Relevant laws / regime:** U.S. state and federal privacy law is the
primary frame, since the solution and its adopters are US-nonprofit-
focused. The commons repository itself is architected to be out of scope
for most data-protection obligations by design — it processes no personal
data of natural persons.

**Evidence of adherence:** The commons repo collects no personal data of
any kind. `copier.yml` collects only organization-level operational data
(legal name, EIN, state, address, contact email) supplied voluntarily and
locally by the adopter, for rendering *their own* repository — it is never
transmitted to USCouncil, the DPGA, or any third party by the tooling
itself (Copier runs entirely on the adopter's own machine).
`SECURITY.md` and the leak-guard CI (`gitleaks` + `.gitleaks-denylist.txt`,
enforced by `.github/workflows/leak-guard.yml` on every push/PR) explicitly
forbid real personal data — director names, EINs, addresses — from ever
being committed to the *commons* repository.

**Status: Met for the commons repository.** Adopters are responsible for
their own privacy posture once they render and populate their own repo
with real organizational data — this boundary is explicitly flagged in
`docs/NOT-LEGAL-ADVICE.md` and is not a claim this submission makes on
adopters' behalf.

---

## Indicator 8 — Adherence to Standards and Best Practices

**Requirement (Standard §8):** projects must align with applicable
standards, best practices, and/or guiding principles.

**Open standards adhered to (tooling):** Copier's documented template
schema conventions; standard GitHub Actions / GitLab CI conventions;
`gitleaks`, the open-source standard secret-scanning tool, using its
standard TOML rule format; Git itself; the Developer Certificate of Origin
(DCO) for contribution provenance (`CONTRIBUTING.md`); Contributor Covenant
2.1 for community conduct (`CODE_OF_CONDUCT.md`).

**Domain standards adhered to (content):** the governance/policy corpus
under `template/` is systematically mapped to the regulatory frameworks
that actually govern US nonprofit compliance, per policy, in
`template/POLICY-INDEX.md.jinja`:

- **2 CFR 200** (Uniform Guidance) — cited across the 18-policy
  federal-compliance cluster (cost accounting, procurement, subaward
  monitoring, time and effort reporting, and more).
- **IRS Form 990** — code of ethics (Part VI general-conduct basis),
  conflict-of-interest policy (Part VI, Line 12), document retention
  (Part VI, Line 14).
- **NSF PAPPG** (Proposal & Award Policies & Procedures Guide) and NIH
  policy notices — research-integrity cluster (RCR training, current &
  pending support, malign foreign talent recruitment program compliance).
- **NIST SP 800-171 / 800-53**, **DFARS**, **FAR 52.204-21** — cybersecurity
  policy.
- **Bayh-Dole Act**, **OSHA**, **EPA regulations**, **ITAR/EAR**, **Byrd
  Anti-Lobbying Amendment**, **42 CFR Part 50 Subpart F** — remaining
  federal-compliance and research-integrity policies.

**Status: Met, with one honest ongoing gap.** Tooling conventions and
domain regulatory citations are both substantively in place — this is not
a thin claim. What remains partial: general nonprofit-governance
best-practice sources outside of federal regulatory citations (e.g.
BoardSource, National Council of Nonprofits) are the *structural* model
the templates follow but are not yet explicitly cited inline the way the
2 CFR 200 / Form 990 / PAPPG citations are. Flagged as a future,
non-blocking content-quality improvement, not a Standard-compliance
blocker — the Standard requires adherence to standards and best practices,
which is demonstrated; it does not require exhaustive inline citation of
every practice source.

---

## Indicator 9 — Do No Harm By Design

See [`DO-NO-HARM.md`](DO-NO-HARM.md) for the standalone statement this
section summarizes.

### 9A — Data Privacy and Security

**Does the solution allow collection/storage/distribution of PII:** No, by
design, for the commons repository (see indicator 7). Adopters' rendered
repos may come to contain organization-operational data, which is their
own responsibility to secure.

**How privacy & security is ensured:** dual-layer, CI-enforced leak-guard
— `gitleaks` pattern rules (`.gitleaks.toml`, including custom EIN/SSN
shape-detection rules) plus a literal-string denylist
(`.gitleaks-denylist.txt`) — blocks any real-organization-data or secret
pattern on every push and pull request before merge, per
`.github/workflows/leak-guard.yml`. `SECURITY.md` provides a private
GitHub Security Advisories channel for both vulnerability reports and
leaked-data reports. The rendered `cybersecurity.md` policy in
`template/policies/` further gives adopters a working infosec policy
mapped to NIST SP 800-171/800-53.

**Status: Met.**

### 9B — Inappropriate, Misleading, and Illegal Content

**Does the solution allow collection/storage/distribution of content:**
Yes — the solution's entire purpose is distributing governance/policy
content — but that content is exclusively commons-authored, reviewed
template language. There is no open user-content channel (no comments, no
file uploads, no data ingestion from the public) through which a third
party could inject illegal or inappropriate material into a rendered
adopter repo.

**How this is handled:** `CODEOWNERS` gates every change to `template/`
content behind maintainer review before merge. `CONTRIBUTING.md` sets the
contribution contract (leak-guard must pass; DCO sign-off required).
`CODE_OF_CONDUCT.md` (Contributor Covenant 2.1) defines and enforces
acceptable contribution behavior for anyone proposing changes.

**Status: Met at the process-governance level.**

### 9C — Protection from Harassment

**Does the solution facilitate user/contributor interactions:** Yes,
through normal open-source contribution channels (issues, pull requests).

**How this is handled:** `CODE_OF_CONDUCT.md` adopts Contributor Covenant
2.1 in full, including its Enforcement Guidelines. `SECURITY.md`'s private
reporting channel doubles as an escalation path for Code of Conduct
concerns, not just security vulnerabilities.

**Status: Met.**

### The domain-specific do-no-harm addition: legal-disclaimer posture

Because this solution's content is compliance/legal-template language for
501(c)(3) organizations, the single largest realistic harm vector isn't a
security bug — it's an adopter treating a generic template as sufficient,
adopted legal advice without qualified review. `docs/NOT-LEGAL-ADVICE.md`
exists specifically to prevent that: it states plainly that nothing in the
repository is legal, tax, or accounting advice, that no attorney-client
relationship is created by use of the repository, and that every rendered
document should be reviewed by a licensed attorney (state nonprofit law
varies) and, where applicable, a CPA before a board adopts it. This
disclaimer is referenced from `README.md`, `copier.yml`'s
`_message_before_copy` and `_message_after_copy` prompts (so an adopter
sees it at the moment of generation, not buried in a doc they may never
open), and `docs/ADOPTER-GUIDE.md`.

**Status: Met**, and treated as load-bearing for this submission's honesty
about the domain's actual risk profile — see `DO-NO-HARM.md` for the full
statement.
