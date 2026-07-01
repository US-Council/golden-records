# Do No Harm By Design

This is the standalone statement for DPG Standard indicator 9 ("Do No Harm
By Design"), covering its three sub-indicators (9A/9B/9C) plus a
domain-specific addition this project treats as equally load-bearing: the
legal-disclaimer posture required by the fact that this solution's content
is compliance/legal-template language. It is summarized in
[`DPG-SUBMISSION.md`](DPG-SUBMISSION.md) under indicator 9; this document
is the fuller, standalone version for a reviewer or auditor who wants the
full reasoning in one place.

Requirement text (Standard §9, confirmed 2026-07-01 against
[`DPG-Standard/standard.md`](https://github.com/DPGAlliance/DPG-Standard/blob/main/standard.md)):
an overarching principle requiring anticipation and prevention of harm,
broken into 9A (Data Privacy & Security), 9B (Inappropriate & Illegal
Content), and 9C (Protection from Harassment).

## 9A — Data privacy and security: the PII / no-real-org-data boundary

`golden-records` is architected so the commons repository itself never
contains personal or organization-identifying data — not as an operational
policy that could be forgotten, but as a structural boundary enforced by
CI.

- **What the boundary is.** No real EIN, no real director/officer name, no
  real street address, no real charter number, no signed governance PDF,
  no actual meeting minutes or votes. Every value that looks like
  organization data in this repository is either a Copier template
  variable (`{{ variable }}`) rendered per-adopter, or a clearly fictitious
  placeholder. This is stated as "the absolute rule" in `CONTRIBUTING.md`.
- **Why it matters specifically for this solution.** Unlike most
  open-source projects, the payload here is *governance content* — the
  single most dangerous failure mode is a maintainer accidentally
  committing real organization data while drafting or de-branding example
  material, which would then sit in the commit history of a public repo
  permanently. `docs/ARCHITECTURE.md` treats this as core architecture,
  not a bolt-on: "this is treated as core architecture, not bolt-on
  tooling."
- **How it's enforced, not just declared.** See "The leak-guard
  enforcement mechanism" below — this is the technical control that makes
  the boundary real rather than aspirational.
- **What's out of scope.** The commons repo's own posture is what's
  certified here. Once an adopting organization renders their own copy and
  populates it with their real EIN, board roster, and address, that
  *rendered* repository's security and privacy posture becomes the
  adopting organization's own responsibility — `SECURITY.md` states this
  scope boundary explicitly.

## The leak-guard enforcement mechanism

Two independent, CI-blocking checks run on every push and pull request via
`.github/workflows/leak-guard.yml`:

1. **`gitleaks` pattern scan** (`.gitleaks.toml`) — extends gitleaks'
   standard secret-detection ruleset (`[extend] useDefault = true`, catching
   AWS keys, private keys, generic API tokens) with two custom
   organization-identifier rules: a US EIN shape detector (`\d{2}-\d{7}`)
   and a US SSN shape detector (`\d{3}-\d{2}-\d{4}`). These catch
   *structured* identifiers by shape, regardless of the specific value, so
   a not-yet-denylisted real EIN would still be caught.
2. **Denylist scan** (`.gitleaks-denylist.txt`) — a plain-text,
   one-literal-per-line list of specific forbidden strings, checked against
   every tracked file on every push/PR. This is the extensible,
   no-regex-required safety net: any contributor who spots a real-world
   identifier that must never appear appends one line, no code change
   required.

Both layers must pass before a pull request can be merged — enforced by
CI, not by reviewer trust alone, per `CONTRIBUTING.md`'s "The absolute
rule: zero real organization data" section. This is the same mechanism
this milestone's own worktree was scanned against before commit (see the
verification step at the end of this document).

## 9C — Human-in-the-loop governance: AI cannot adopt or vote

Because this commons is explicitly designed for nonprofits that use AI
agents as part of their own governance operations (see
`template/GOVERNANCE.md.jinja`'s framing: "AI agents prepare agendas, draft
minutes, generate financial reports, manage voting records, track action
items, and support board decision-making on a continuous basis"), the
do-no-harm design has to address a harm vector specific to that use case:
an AI agent silently becoming the *de facto* decision-maker of a nonprofit
board. The rendered governance template forecloses this by rule, not by
convention:

- **AI agents do not hold votes.** `template/GOVERNANCE.md.jinja`, in its
  rules for who may propose vs. adopt governance actions, states plainly:
  "AI agents authorized by the Board Chair may prepare [motions], draft
  frontmatter, assemble supporting materials, and carry out mechanical
  edits. However, **a human director or officer must be the [motion]
  author of record, or must explicitly endorse the [motion] in writing in
  its description.** AI agents do not hold votes and cannot author
  governance actions unattended."
- **A defined prohibited-actions list, not a vague aspiration.** The
  rendered `ai-governance-board.md` policy (gated behind the
  `ai_agents_enabled` Copier variable) enumerates concrete actions no AI
  agent may take without explicit human authorization: access or transact
  against financial accounts; execute payments or financial commitments;
  file tax returns, legal documents, or regulatory submissions; modify
  articles of incorporation or bylaws; communicate externally on behalf of
  the board; or grant/revoke system access permissions.
- **Service/bot accounts don't count toward quorum.** `template/GOVERNANCE.md.jinja`:
  "Service, bot, or admin accounts may open or maintain [a motion], but
  they do not count toward director quorum and must not be the only named
  proposer of a board action."
- **The audit trail makes this checkable, not just asserted.** Because
  every governance action is a git commit and every adoption is a merge
  (see indicator 1's evidence in `DPG-SUBMISSION.md`), whether a human
  director was in fact the author of record or explicit endorser of any
  given policy change is itself independently verifiable after the fact,
  not just a policy statement trusted at face value.

This addresses 9C's "protection from harassment" requirement in its literal
sense (Contributor Covenant 2.1, `CODE_OF_CONDUCT.md`) and, in this
project's specific domain, a broader institutional-safety sense: the people
governed by decisions this template's output records are protected from
having consequential governance decisions made *for* them by a system that
cannot be held accountable the way a named human director can.

## 9B — Inappropriate and illegal content: a narrow surface by design

The solution has no open content-ingestion channel — no user comments, no
public file uploads, no data collection from end users of a rendered
governance repo. The only route by which content enters `template/` is a
reviewed pull request against the commons itself, gated by `CODEOWNERS` and
subject to `CONTRIBUTING.md`'s leak-guard-must-pass contract. This
structurally narrows the 9B attack surface relative to platforms that host
arbitrary user-generated content; what governance is required is
process-level (review gates), not a content-moderation pipeline, and that
process-level governance is in place.

## The legal-disclaimer posture: not-legal-advice framing

This is treated as a do-no-harm measure in its own right, specific to this
project's domain, alongside 9A/9B/9C rather than instead of them. The
single most realistic harm vector for a governance-template commons is not
a data breach — it's a nonprofit board adopting a generic, un-reviewed
document as if it were legal advice tailored to their situation, in a
jurisdiction (US state nonprofit law) that varies significantly
state-to-state.

`docs/NOT-LEGAL-ADVICE.md` addresses this directly and is surfaced at the
point of use, not buried:

- Stated plainly: "Nothing in this repository... is legal, tax, or
  accounting advice. It is not a substitute for advice from a licensed
  attorney, CPA, or other qualified professional."
- Surfaced at generation time: `copier.yml`'s `_message_before_copy` and
  `_message_after_copy` prompts both reference it, so an adopter sees the
  disclaimer at the moment they run `copier copy`, not only if they happen
  to browse `docs/`.
- No attorney-client relationship is created by use of the repository —
  stated explicitly, to avoid ambiguity for adopters unfamiliar with that
  distinction.
- Concrete guidance, not just a disclaimer: "Have a licensed attorney in
  your state review any bylaws, resolution, or governance policy before
  your board adopts it" and "Have a CPA or tax professional review anything
  touching your 501(c)(3) status... before you rely on it."
- An explicit statement that CI passing (leak-guard) or a successful
  `copier copy` is **not** legal sign-off — engineering safeguards and
  legal sufficiency are different things, and the document says so in
  those words, to prevent an adopter from conflating "the tool didn't
  error" with "counsel has reviewed this."

## Verification for this milestone's own changes

The files added under `docs/dpga/` by this milestone were scanned with the
same leak-guard mechanism described above (`gitleaks` + the denylist) prior
to commit, and reference no real-world reference-adopter-specific data —
any planned or in-progress reference adoption is referred to only
generically, as "the reference adopter," consistent with the leak-guard
boundary this document describes.
