# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this repository — including in the Copier
template logic, the `copier.yml` question schema, the leak-guard CI workflow, or any
generic policy artifact — please report it privately rather than opening a public issue.

**How to report:**

- Use [GitHub Security Advisories](../../security/advisories/new) for this repository
  ("Report a vulnerability" under the Security tab). This creates a private disclosure
  channel visible only to maintainers until a fix is ready.
- If GitHub Security Advisories are unavailable to you, email the maintainers at the
  address listed in this repository's `CODEOWNERS` file.

Please include:

- A description of the vulnerability and its potential impact.
- Steps to reproduce (or a proof-of-concept, if applicable).
- Any suggested remediation, if you have one.

We aim to acknowledge reports within 5 business days and to provide a remediation
timeline once the issue is triaged.

## Reporting Leaked or Real Organization Data

This repository is a **generic, vendor-neutral commons**. It must never contain real
organization-specific data — no live EINs, charter numbers, real director or officer
names, signed governance PDFs, or actual meeting minutes/resolutions/votes. Every value
that resembles organization data in this repo is a placeholder, an example, or a
Copier template variable rendered from a fictitious sample.

Automated leak-guard scanning (`.github/workflows/leak-guard.yml`, powered by
[gitleaks](https://github.com/gitleaks/gitleaks) and `.gitleaks.toml`) runs on every
push and pull request specifically to catch this class of mistake before merge.

**If you find real organization data that slipped through CI** — for example, in a
commit history, a fork, or an adopter's contribution back upstream — report it the
same way as a security vulnerability (see above), and additionally flag it as
"data leak" in the report so it can be prioritized. Do not open a public issue
describing the leaked data, since that would republish it.

## Supported Versions

This repository is versioned via semver git tags consumed by `copier update`. Security
fixes are applied to the `main` branch and released as a new patch or minor tag.
Adopters should run `copier update` regularly to pick up fixes.

## Scope

This policy covers the commons repository itself (template logic, CI, documentation).
It does not cover the security posture of any individual organization's *rendered*
copy of this template — each adopting organization is responsible for the security of
their own fork/render, including any secrets, credentials, or real governance data they
add after rendering.
