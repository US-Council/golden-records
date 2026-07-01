# Golden Records

**A public, vendor-neutral commons of governance-as-code templates for
US nonprofit organizations — bylaws, board resolutions, policy language,
and compliance checklists, rendered for your organization via
[Copier](https://copier.readthedocs.io/).**

Golden Records is maintained by [USCouncil](https://uscouncil.org) as an
open commons: no single NGO's real governance data lives here. What lives
here is a generic, parameterized template that any 501(c)(3) (or
organization pursuing that status) can render into their *own* concrete,
organization-specific governance repository — then own, customize, and
maintain independently.

## Who this is for

- Small-to-midsize US nonprofit boards and staff who want their governance
  documents — bylaws, resolutions, conflict-of-interest policy, compliance
  checklists — managed as version-controlled, reviewable, auditable code
  instead of scattered Word docs and PDFs.
- Nonprofits that use (or plan to use) AI agents as part of their
  operations and want governance language that accounts for that.
- Organizations pursuing US federal grants who need compliance scaffolding
  aligned with 2 CFR 200.
- Organizations that want to start from a well-structured, commons-reviewed
  baseline rather than drafting from a blank page — while still getting
  their own counsel's review before adopting anything.

## What this is not

- **Not legal, tax, or accounting advice.** See
  [`docs/NOT-LEGAL-ADVICE.md`](docs/NOT-LEGAL-ADVICE.md). Every rendered
  document should be reviewed by your own board and counsel before
  adoption.
- **Not any specific organization's real governance data.** This repo is
  actively scanned on every push/PR (see "Leak-guard," below) to guarantee
  it never contains a real EIN, real director names, a real address, or
  any other organization-specific identifier. Everything here is generic
  or a Copier template variable.

## How to adopt

Golden Records is consumed via [Copier](https://copier.readthedocs.io/), a
template-rendering tool with first-class support for pulling in upstream
improvements later via 3-way merge (`copier update`) — so your rendered
repo isn't a one-time snapshot; it can stay current with the commons.

```
pipx install copier   # or: pip install copier
copier copy --trust gh:USCouncil/golden-records path/to/your-new-repo
```

You'll be asked a series of questions (organization name, EIN, state of
incorporation, board size, and more — see [`copier.yml`](copier.yml) for
the full schema), and Copier will render a complete, organization-specific
repository from the generic template in [`template/`](template/).

To pull in future commons improvements:

```
cd path/to/your-repo
copier update --trust
```

**Read [`docs/UPGRADE-GUIDE.md`](docs/UPGRADE-GUIDE.md) before your first
`copier update`** — there's an important gotcha about running it on a
dirty git working tree.

Full walkthrough: [`docs/ADOPTER-GUIDE.md`](docs/ADOPTER-GUIDE.md).
Architecture and rationale: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

## Digital Public Good posture

Golden Records is built from day one to align with the
[Digital Public Goods Alliance](https://www.digitalpublicgoods.net/)'s
DPG Standard (9 indicators, with indicator 9 further split into 9A/9B/9C).
See [`docs/DPG-STANDARD-MAPPING.md`](docs/DPG-STANDARD-MAPPING.md) for a
full indicator-by-indicator mapping of what's satisfied today and what's
tracked as a gap for future phases. In short: openly licensed (indicator
2), platform-independent by design (indicator 4, via the `vcs_platform`
Copier variable), documented (indicator 5), and secured against data
leakage and harassment by automated CI and a Code of Conduct (9A/9B/9C).
Formal DPG registration is a future milestone, not a claim made by this
README.

## Licensing

This repository is **dual-licensed** by file type:

| Covers | License | File |
|---|---|---|
| Code — `copier.yml` logic, CI workflows, scripts, tooling | [Apache-2.0](LICENSE) | `LICENSE` |
| Content — policy language, bylaws/resolution templates, documentation prose (everything under `template/` and `docs/`, plus this README) | [CC-BY-4.0](LICENSE-CONTENT) | `LICENSE-CONTENT` |

When in doubt about which license applies to a given file, ask: "is this
executable/logic, or is this prose a human is meant to read and adapt?"
The former is Apache-2.0; the latter is CC-BY-4.0. See
[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md#why-dual-licensing) for the
full rationale.

## Security & data-leak reporting

See [`SECURITY.md`](SECURITY.md). This repo runs automated `leak-guard` CI
(gitleaks + a literal-string denylist) on every push and pull request
specifically to prevent real organization data from ever landing here —
see [`CONTRIBUTING.md`](CONTRIBUTING.md#the-absolute-rule-zero-real-organization-data)
for the contributor-facing contract.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) — includes the DCO sign-off
requirement and the leak-guard rule every contribution must satisfy. All
contributors are expected to follow
[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) (Contributor Covenant 2.1).

## Status

This repository is in active early development (Milestone 0: foundation —
licensing, CI, documentation scaffolding, and the Copier question schema
are in place; the actual de-branded policy/template payload under
`template/` is being built out in subsequent phases). See
[`docs/DPG-STANDARD-MAPPING.md`](docs/DPG-STANDARD-MAPPING.md) for a
transparent view of what's done and what's pending.
