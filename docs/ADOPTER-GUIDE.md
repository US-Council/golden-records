# Adopter Guide

## Who this guide is for

An NGO/nonprofit board member, executive director, or operations lead
evaluating or adopting the Golden Records governance-as-code commons for
their own organization. This document is the conceptual companion to
[`docs/GETTING-STARTED.md`](GETTING-STARTED.md) — read that one for the
concrete `copier copy` walkthrough, step by step. Read this one for what
you're actually adopting and why the model is shaped the way it is.

## What the commons is

`golden-records` is a **Copier template**, not a nonprofit's governance
repository. It is the shared source that many organizations render *from*.
No single NGO's real governance data lives in the commons itself — every
value that would identify a real organization (an EIN, a director's name,
an address, a signed minute) is either a Copier template variable or a
generic placeholder, enforced by automated `leak-guard` CI on every push
and pull request (see `SECURITY.md` and `CONTRIBUTING.md` in the commons
repo).

When you run `copier copy`, Copier reads the commons' question schema
(`copier.yml`), asks you the questions, and renders a complete,
organization-specific copy of `template/` into a new repository — with
your organization's data filled in, and nobody else's. That rendered
repository is entirely yours: private, self-hosted (on GitHub or GitLab, at
your choice), and disconnected from the commons except for the optional,
opt-in `copier update` mechanism described in
[`docs/UPGRADE-GUIDE.md`](UPGRADE-GUIDE.md). See
[`docs/ARCHITECTURE.md`](ARCHITECTURE.md) for the full mechanics of how
Copier does this (the `_subdirectory: template` split, semver tagging,
dual licensing).

## The draft → adopt governance model

The single idea underneath everything you render is: **your governance
repository is a legal record, and git is the record-keeping system.**

Concretely, once you've rendered your copy, your organization's own
`GOVERNANCE.md` (see `template/GOVERNANCE.md.jinja` for the full text you'll
get) establishes a document lifecycle:

```
draft → proposed → review → adopted → amended → superseded → archived
```

And it maps that lifecycle onto ordinary pull/merge requests, not a
separate voting tool:

- **A pull/merge request is a motion.** Someone opens one, on a `board/`
  or `admin/` branch, proposing a change — a new policy, an amended
  bylaw, a resolution.
- **Approval is a vote.** A director approving the PR/MR is the electronic
  equivalent of voting "aye," under your bylaws' electronic-consent or
  written-consent provisions.
- **Merging is adoption.** The merge commit's timestamp — not the date
  written in the document, not the date of any meeting — is the legally
  operative adoption date. Everything on `main` is, by definition, in
  force; everything not on `main` is not.

Two branch prefixes carry different weight: `board/` branches require your
board's quorum (majority, two-thirds, or a custom rule you specify at
render time via `board_quorum_rule`); `admin/` branches require one
authorized officer, for routine administrative material like minutes and
filings. This distinction is fixed across every organization using the
commons — it's an interoperability convention, not something you
customize per-render.

The full mechanics — branch naming, review periods, the YAML frontmatter
every document carries, amendment process, audit-trail guarantees — are in
your rendered `GOVERNANCE.md` itself once you've run `copier copy`. This
guide won't restate that document; go read your own copy of it as the
primary source once it exists. If your organization uses AI agents as part
of its workflow (`ai_agents_enabled: true`), your rendered `CLAUDE.md` sets
the hard limit that governs them: **agents draft; only real, named
directors adopt.**

## What to fill in vs. what to leave alone

Copier renders a **starting point**, not a finished governance repository.
Roughly, three categories of content land in your copy:

1. **Structural/mechanical content you should leave as rendered**, unless
   you have a specific reason to change it: `GOVERNANCE.md`'s branch
   naming scheme and lifecycle states, the YAML frontmatter standard, the
   leak-guard CI wiring. These are the interoperability layer — the parts
   that let `copier update` reconcile cleanly and that other organizations
   using the commons will recognize.
2. **Policy and template content you should review and adapt**: everything
   under `policies/` and `templates/`. This is real governance language —
   conflict-of-interest, whistleblower protection, travel, data
   management, and (if `federal_grants_focus: true`) a substantial block
   of 2 CFR 200 compliance policies. It reflects common nonprofit practice,
   parameterized with your organization's data, but it is generic by
   construction. Your board should read every policy it intends to adopt,
   not merely merge what Copier rendered.
3. **Content you must originate yourself**: your actual bylaws (unless you
   opted into `include_bylaws_example: true`, which gives you a generic,
   non-binding worked example rather than your organization's real,
   board-adopted bylaws), your board roster, and every minute, resolution,
   vote, and signed agreement your organization produces going forward.
   Nothing in the commons can originate these for you — they are
   inherently organization-specific.

`include_bylaws_example` defaults to `false` deliberately: bylaws are the
single document most likely to be treated as "done" the moment it exists in
the repo, and a generic example bylaws document is the least appropriate
place for that to happen. If you do include the example, treat it as a
drafting aid your counsel marks up, not a document your board merges as-is.

## How your organization's real data stays private

This is the property that makes the commons safe to keep public: **data
flows one direction, and it stops at your repository boundary.**

- The commons repo (`US-Council/golden-records`) contains zero real
  organization data, by construction and by CI enforcement (`leak-guard`,
  covered in `SECURITY.md` and `CONTRIBUTING.md`). Nothing you type into
  `copier copy` is ever transmitted back to the commons, to US-Council, or
  to any third party — Copier renders entirely on your own machine, from a
  template it downloaded, into a directory you control.
- Your rendered repository — which does contain your EIN, your directors'
  names, your address, your actual bylaws, minutes, and resolutions — is
  **yours**, hosted wherever you choose, and should be **private**. It is
  never pushed, synced, or reported back to the commons.
- The only channel that ever moves content *from* your repository *toward*
  the commons is if you choose to **contribute an improvement back** —
  and even then, you are contributing a generic policy correction or a new
  `copier.yml` variable, not your organization's real data. See
  [`docs/CONTRIBUTING-POLICIES.md`](CONTRIBUTING-POLICIES.md) for that
  path and its own leak-guard requirement.
- The only channel that ever moves content *from* the commons *toward*
  your repository after the initial render is `copier update` — a
  deliberate, adopter-initiated pull of upstream template improvements,
  never automatic, never silent. See
  [`docs/UPGRADE-GUIDE.md`](UPGRADE-GUIDE.md).

In short: draft in your own private repository, using org-specific content
that never leaves it; adopt through your own board's process; and pull
generic improvements from the commons only when you choose to.

## This is not legal advice

Every point above describes a mechanism, not a legal opinion. See
[`docs/NOT-LEGAL-ADVICE.md`](NOT-LEGAL-ADVICE.md): nothing rendered from
this commons — bylaws, policy language, resolution templates — should be
treated as adopted until your board, and where appropriate your counsel,
have reviewed it. "Common nonprofit practice" is not the same as "correct
for your organization."

## Getting help

- Questions about the commons itself: open an issue on
  `US-Council/golden-records`.
- Security or data-leak concerns: see `SECURITY.md`.
- Legal/tax questions about your specific rendered documents: consult your
  own counsel/CPA — see `docs/NOT-LEGAL-ADVICE.md`.
- The concrete rendering steps: [`docs/GETTING-STARTED.md`](GETTING-STARTED.md).
- Staying current after you've adopted: [`docs/UPGRADE-GUIDE.md`](UPGRADE-GUIDE.md).
- Common questions: [`docs/FAQ.md`](FAQ.md).
