# Contributing to Golden Records

Thank you for considering a contribution to the Golden Records commons.
This repository is a public, vendor-neutral template that many NGOs will
render their own governance repositories from — changes here ripple out to
every adopter via `copier update`. Please read this whole document before
opening a pull request; the rules below (especially the leak-guard
requirement) are non-negotiable.

**Already adopted the commons and want to port an improvement from your own
rendered repo back upstream?** See
[`docs/CONTRIBUTING-POLICIES.md`](docs/CONTRIBUTING-POLICIES.md) — it walks
through the adopter-specific path, including how to re-genericize content
that currently contains your organization's real data before it can go
anywhere near this repo.

## What kinds of contributions we welcome

- **Policy and template improvements** — corrections, clarifications, or
  additions to the generic governance/policy content under `template/`.
- **New `copier.yml` variables** — when a genuinely common nonprofit
  governance need isn't yet parameterized.
- **Documentation improvements** — to `docs/`, this file, or any top-level
  doc.
- **Tooling/CI improvements** — to `.github/workflows/`, `.gitleaks.toml`,
  etc.

## The absolute rule: zero real organization data

This is a **generic commons**, not any one organization's actual governance
repo. Every value that looks like organization data in this repository —
names, EINs, addresses, charter numbers, dates, dollar amounts — must be
either:

- a Copier template variable (`{{ variable_name }}`), or
- a clearly fictitious/generic placeholder or example value.

**Never commit:**
- A real EIN, charter number, or other government-issued identifier.
- A real person's name in a governance/board context (director, officer,
  registered agent).
- A real street address.
- Real board minutes, resolutions, votes, or signed documents (PDFs,
  scans, or transcriptions of them).

If you're de-branding or adapting real-world source material into a
generic template, **triple-check every field** before committing. It is
extremely easy to leave one real value behind while generalizing everything
else around it.

### The leak-guard safety net

Every push and pull request runs `.github/workflows/leak-guard.yml`, which
performs two independent checks and **fails the build on any hit**:

1. **`gitleaks` pattern scan** (`.gitleaks.toml`) — catches structured
   identifiers like EIN-shaped strings (`NN-NNNNNNN`) regardless of the
   exact value, plus gitleaks' standard secret-detection ruleset.
2. **Denylist scan** (`.gitleaks-denylist.txt`) — a plain-text, one-literal-
   per-line list of specific forbidden strings (real org names, real
   people's names, a real address, etc.), checked against every tracked
   file.

**Your PR must pass both before it can be merged.** This is enforced by CI,
not by trust — but please still self-check before pushing.

**Extending the denylist:** if you're aware of an additional real-world
identifier that must never appear in this repo, add it as a new line to
`.gitleaks-denylist.txt`. No regex or code changes needed — see the
comments at the top of that file for the format.

## Development workflow

1. Fork the repository (or create a branch, if you have write access).
2. Make your change. If you're touching `template/`, remember it's
   Jinja-templated — file *and directory* names can contain
   `{{ variable }}` syntax, and Copier will render both content and paths.
3. If you added or changed a `copier.yml` variable, update
   `docs/ADOPTER-GUIDE.md` and/or `docs/DPG-STANDARD-MAPPING.md` if the
   change affects documentation coverage or DPG alignment.
4. Run `gitleaks detect --config .gitleaks.toml` locally if you have it
   installed, to catch issues before pushing (CI will re-run this
   regardless).
5. Commit with sign-off (see DCO section below).
6. Open a pull request. CODEOWNERS will be requested for review
   automatically on sensitive paths (see `CODEOWNERS`).

## Developer Certificate of Origin (DCO)

All commits must be signed off, certifying that you wrote the contribution
or otherwise have the right to submit it under this repo's licenses (see
"Licensing" below). Add the `Signed-off-by` trailer with:

```
git commit --signoff
```

This appends a line like:

```
Signed-off-by: Your Name <you@example.com>
```

Pull requests with unsigned commits will not be merged. If you forgot to
sign off, you can amend: `git commit --amend --signoff` (for the most
recent commit) or use an interactive rebase for multiple commits.

## Licensing of contributions

This repository is dual-licensed (see `README.md` for the file-type
breakdown):

- Code (scripts, CI, `copier.yml` logic) is licensed **Apache-2.0**
  (`LICENSE`).
- Content (policy language, templates, documentation prose) is licensed
  **CC-BY-4.0** (`LICENSE-CONTENT`).

By contributing, you agree your contribution is licensed under the license
that applies to the type of file you're changing.

## Code of Conduct

All contributors are expected to follow `CODE_OF_CONDUCT.md` (Contributor
Covenant 2.1).

## Questions

Open an issue, or see `SECURITY.md` if your question involves a security
or data-leak concern that shouldn't be discussed publicly.
