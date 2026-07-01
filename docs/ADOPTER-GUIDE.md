# Adopter Guide

> **Status: skeleton.** This guide is scaffolded in Milestone 0 (the
> foundation phase) before `template/` contains real policy/governance
> content. Sections marked **TODO (Phase 7)** will be filled in once the
> de-branded policy templates exist and can be walked through concretely.
> Do not treat this document as complete or ready for external adopters yet.

## Who this guide is for

An NGO/nonprofit board member, executive director, or operations lead who
wants to adopt the Golden Records governance-as-code commons for their own
organization.

## Prerequisites

- [Copier](https://copier.readthedocs.io/) installed (`pipx install copier`
  or `pip install copier`; current stable is the 9.x series — see
  `docs/ARCHITECTURE.md` for why we target `_min_copier_version: "9.3.0"`).
- Git installed and a destination location (empty local directory, or an
  empty repo on GitHub/GitLab per your `vcs_platform` choice) to render into.
- The following on hand before you start (see `copier.yml` for the full
  question list): your organization's legal name, EIN, state of
  incorporation, principal address, and a primary contact email.

## Step 1 — Render your copy

```
copier copy --trust gh:US-Council/golden-records path/to/your-new-repo
```

Copier will walk you through the question schema defined in `copier.yml`,
one question at a time, showing the help text for each. Answers are
validated as you go (for example, the EIN field must match `NN-NNNNNNN`).

**TODO (Phase 7):** Walk through each question group (organization identity,
fiscal & tax-exempt status, board & governance, operating posture, contact)
with a worked example, and show what the rendered output looks like for a
sample organization.

## Step 2 — Review before you commit

**Do not treat the rendered output as ready-to-adopt without review.** See
`docs/NOT-LEGAL-ADVICE.md` — every rendered governance document should be
reviewed by your board and, where appropriate, by counsel before adoption.

**TODO (Phase 7):** Provide a concrete review checklist once `template/`
contains real bylaws/resolution/policy artifacts to check.

## Step 3 — Commit and customize

Initialize your own repository (or add to an existing one) with the
rendered output, and customize freely — it's yours now.

**TODO (Phase 7):** Document recommended repo structure/branch protection
once there is real template content to reference.

## Step 4 — Staying current

See `docs/UPGRADE-GUIDE.md` for how to pull future improvements from the
commons via `copier update`.

## Getting help

- Questions about the commons itself: open an issue on
  `US-Council/golden-records`.
- Security or data-leak concerns: see `SECURITY.md`.
- Legal/tax questions about your specific rendered documents: consult your
  own counsel/CPA — see `docs/NOT-LEGAL-ADVICE.md`.
