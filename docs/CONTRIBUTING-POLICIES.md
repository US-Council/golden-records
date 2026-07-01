# Contributing Back to the Commons

This document covers the specific pathway for an **adopting organization**
— one that has already rendered its own copy via `copier copy` — to
propose an improvement back to the `golden-records` commons: a corrected
policy, a clarified piece of boilerplate, a new `copier.yml` variable, or a
brand-new policy/template that other adopters would also benefit from.

For the general contribution rules that apply to *any* contributor
(fork/branch workflow, DCO sign-off, licensing), see
[`CONTRIBUTING.md`](../CONTRIBUTING.md) at the repository root — this
document doesn't repeat those; it's specifically about the adopter's path
from "I fixed something in my own rendered repo" to "that fix is now in
the commons for everyone."

## Why this path exists

Your rendered repository and the commons are two different codebases, only
connected through `copier copy`/`copier update`. If your board's counsel
improves the wording of a policy in your private repo, that improvement
stays in your repo forever, invisible to every other adopter, unless
someone deliberately ports it upstream. This document is that porting
path.

## The core distinction: your repo has real data, the commons must not

This is the single most important thing to internalize before opening a
pull request against `US-Council/golden-records`:

- **Your rendered repository** contains your real EIN, your real board
  member names, your real address, your real minutes and resolutions. That
  data belongs there and nowhere else.
- **The commons repository** must contain **zero** real organization data,
  from any adopter, ever. It is a public repository, and every value that
  could identify a real organization must be either a Copier template
  variable (`{{ variable_name }}`) or a clearly fictitious/generic
  placeholder.

When you port an improvement from your rendered repo back to the commons,
you are not copy-pasting a file — you are **re-genericizing** it. Take the
specific wording improvement (a clarified sentence, a corrected citation, a
missing procedural step) and reintroduce it into the *template* file
(`template/policies/whatever.md.jinja`), using `{{ org_name }}` and the
other Copier variables exactly as the surrounding template already does.
Never paste your rendered, org-specific file over the template file
directly — it will contain your organization's real data by construction.

If you're contributing a change derived from real-world source material
(e.g. wording your organization actually adopted after legal review) and
you're not sure how to generalize a specific passage, see
[`docs/DEBRANDING-SPEC.md`](DEBRANDING-SPEC.md) — the rulebook the commons'
own maintainers follow when turning real adopted policy language into
generic template content. It's written for bulk corpus work, but the
underlying rules (structural description instead of literal values,
`«PLACEHOLDER»`-style tokens, never spelling out a real identifier even as
an example) apply just as well to a single-file contribution.

## What kinds of contributions this covers

- **A correction or clarification** to existing policy/template language
  in `template/` that your board or counsel identified while adopting or
  operating under your rendered copy.
- **A new `copier.yml` variable**, when you hit a genuinely common
  nonprofit governance need that isn't yet parameterized (for example, a
  jurisdiction-specific requirement affecting many organizations, not just
  yours).
- **A new policy or template file**, if your organization developed
  governance content — written from scratch or substantially rewritten,
  not merely adapted from a source you don't have rights to share — that
  would benefit adopters generally.
- **Documentation improvements** to any doc under `docs/`, informed by
  something that was unclear or missing while you were adopting.

This does **not** cover: reporting a bug or security issue (see
`SECURITY.md` instead), or requesting support with your own rendered
repository's org-specific customizations (those are yours to maintain; see
`docs/ADOPTER-GUIDE.md`).

## The pathway, step by step

1. **Fork** `US-Council/golden-records` (or create a branch, if you have
   write access) — same as any other commons contribution.
2. **Locate the corresponding template file(s).** If you're porting a
   change from your rendered `policies/conflict-of-interest.md`, the file
   you edit in the commons is
   `template/policies/conflict-of-interest.md.jinja`. Re-express your
   change using Copier's Jinja variables, not your organization's literal
   values.
3. **Re-genericize, then triple-check.** Read the diff you're about to
   commit and ask, line by line: does this introduce anything that
   identifies a real organization? See
   [`CONTRIBUTING.md`](../CONTRIBUTING.md#the-absolute-rule-zero-real-organization-data)
   for the exhaustive "never commit" list (real EINs, real names in a
   governance context, real addresses, real minutes/resolutions/votes).
4. **Run the leak-guard checks locally before pushing**, if you have
   `gitleaks` installed:
   ```bash
   gitleaks detect --config .gitleaks.toml
   ```
   CI re-runs this regardless (`.github/workflows/leak-guard.yml`), but
   catching it yourself first is faster than waiting on a red pipeline.
5. **If you added or changed a `copier.yml` variable**, update
   `docs/GETTING-STARTED.md`'s question-schema tables and
   `docs/DPG-STANDARD-MAPPING.md` if the change affects documentation
   coverage.
6. **Commit with sign-off** (`git commit --signoff`) — the DCO
   `Signed-off-by` trailer is required on every commit; see
   `CONTRIBUTING.md` for the full DCO section.
7. **Open the pull request.** Describe:
   - What changed and why (the governance/operational need it addresses).
   - Whether it's a correction to existing content or genuinely new
     content.
   - For new template variables: what the default should be and why, and
     which existing rendered files (if any) would need it threaded
     through.
8. **CODEOWNERS review.** `CODEOWNERS` routes mandatory review to commons
   maintainers automatically on sensitive paths (`copier.yml`, the
   leak-guard config, CI workflows). Expect closer scrutiny on those paths
   than on a single policy wording fix.

## Licensing of your contribution

By opening a pull request against the commons, you agree your contribution
is licensed under whichever license applies to the file type you're
changing — **Apache-2.0** for code (`copier.yml` logic, CI, scripts) or
**CC-BY-4.0** for content (policy language, templates, documentation
prose). See [`README.md`](../README.md#licensing) for the full
Apache-2.0/CC-BY-4.0 breakdown by file type. This is the same licensing
contract every commons contributor accepts, adopter or not — see
`CONTRIBUTING.md`.

## PR checklist

Before you open the pull request, confirm:

- [ ] The change is made in `template/` (or `docs/`, or `copier.yml`), not
      copy-pasted from your rendered repo's org-specific files.
- [ ] Every value that could identify a real organization is either a
      Copier variable (`{{ variable }}`) or a clearly generic/fictitious
      placeholder — no real EIN, charter number, address, or person's name
      in a governance context.
- [ ] `gitleaks detect --config .gitleaks.toml` passes locally (or you've
      confirmed CI will catch it if you couldn't run it locally).
- [ ] If you added a `copier.yml` variable: `docs/GETTING-STARTED.md` and
      `docs/DPG-STANDARD-MAPPING.md` are updated accordingly.
- [ ] Commits are signed off (DCO `Signed-off-by` trailer present on every
      commit).
- [ ] The PR description explains the governance/operational need, not
      just the mechanical diff.
- [ ] You've read and agree to the licensing terms in
      `CONTRIBUTING.md`/`README.md` for the file type(s) you changed.

## Getting help

Questions about whether a specific contribution is a good fit for the
commons (versus something that should stay organization-specific in your
own rendered repo): open an issue on `US-Council/golden-records` and ask
before doing the work. Security or data-leak concerns: see `SECURITY.md`,
not a public issue.
