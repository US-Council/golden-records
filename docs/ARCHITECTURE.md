# Architecture

## What this repository is

`golden-records` is a **Copier template** — a source repository that
generates other repositories. It is not itself a nonprofit's governance
repo; it is the commons that nonprofits render *from*. One adopting
organization runs `copier copy` against this repo to produce their own
concrete governance-as-code repository, answers a schema of questions
(`copier.yml`), and gets back a fully parameterized set of policies,
bylaws skeletons, resolution templates, and compliance checklists with
their own organization's data filled in — and none of anyone else's.

## Topology

```
golden-records/                  (this repo — the commons / template SOURCE)
├── copier.yml                   question schema, defaults, validators
├── template/                    the COPIER PAYLOAD — rendered into adopters' repos
│   ├── {{ _copier_conf.answers_file }}.jinja   tracks answers for `copier update`
│   ├── policies/                 (added in later phases)
│   ├── governance/               (added in later phases)
│   └── ...
├── docs/                        commons-facing documentation (this file, DPG mapping, etc.)
├── .github/workflows/           CI for the COMMONS repo itself (leak-guard)
├── .gitleaks.toml / .gitleaks-denylist.txt   anti-leak safety net
└── README.md / CONTRIBUTING.md / etc.
```

Everything outside `template/` (docs, CI, this file, contributing guides)
describes and governs the *commons itself*. Everything inside `template/`
is payload — the actual Jinja-templated files that get rendered into an
adopter's new repository when they run `copier copy gh:USCouncil/golden-records their-repo/`.

## Why `_subdirectory: template`

Copier supports rendering from a subdirectory of the source repo rather
than the repo root (`_subdirectory` in `copier.yml`). We use this
deliberately:

- **Clean separation of concerns.** Commons infrastructure — this repo's
  own CI, its own contributing guide, its own leak-guard config — must
  never leak into an adopter's rendered output. If we rendered from repo
  root, an adopter would get *our* `CODEOWNERS`, *our* leak-guard workflow
  (which references commons-specific paths), and *our* commons-facing
  README, all of which are wrong in their repo.
- **The commons repo can evolve its own tooling independently** of the
  template payload. We can change how leak-guard works, add commons-level
  CI, or restructure `docs/` without every change becoming a diff an
  adopter has to merge via `copier update`.
- **`template/` is exactly what gets rendered, nothing more, nothing less.**
  This makes it easy to reason about "what will an adopter actually see"
  by just looking at that one directory.

## The Copier flow

1. **Author** (commons maintainers) edits `copier.yml` and files under
   `template/`, using Jinja syntax (`{{ variable }}`) in both file contents
   and file/directory *names* where needed (e.g. the answers file itself:
   `template/{{ _copier_conf.answers_file }}.jinja`).
2. **Tag a release.** Copier templates are versioned via semver git tags
   (e.g. `v0.1.0`). Adopters pin to a tag (or track `main` at their own
   risk) so their `copier update` runs are predictable.
3. **Adopt.** A new organization runs:
   ```
   copier copy --trust gh:USCouncil/golden-records path/to/their-new-repo
   ```
   Copier walks the `copier.yml` question schema, validates answers (e.g.
   the EIN regex validator), renders every file in `template/` with Jinja,
   and writes a `.copier-answers.yml` (or equivalent, per
   `_copier_conf.answers_file`) into the destination recording exactly
   what was answered and which template version was used.
4. **Customize.** The adopter's org-specific board, counsel, and staff
   modify the rendered repo freely — it's now theirs.
5. **Upgrade.** When the commons template improves (a policy gets
   clarified, a new compliance checklist is added, a bug in boilerplate
   language is fixed), the adopter runs:
   ```
   copier update --trust
   ```
   Copier performs a **3-way merge**: it diffs the old template version
   against the new template version, and applies that diff on top of the
   adopter's current (possibly customized) files, using the recorded
   answers file to know what "unmodified template output" looked like at
   each version. See `UPGRADE-GUIDE.md` for the dirty-tree gotcha and other
   practical notes.

## Why dual licensing

This repo mixes two fundamentally different kinds of content, and a
single license would be the wrong fit for at least one of them:

- **`LICENSE` — Apache-2.0**, covering *code*: the Copier question schema
  logic embedded in `copier.yml` (Jinja expressions, validators), CI
  workflows, shell scripts, and any future tooling. Apache-2.0 is the
  right choice here for the same reasons it's a common choice for
  developer tooling: an explicit patent grant, clear contributor terms,
  and broad compatibility with other software licenses adopters' own
  codebases might use.
- **`LICENSE-CONTENT` — CC-BY-4.0**, covering *content*: the policy
  language, bylaws skeletons, resolution templates, and documentation
  prose in `template/` and `docs/`. This is written material, not code —
  attribution-based reuse (CC-BY) is the standard, well-understood license
  for this category, and it's what the DPG Standard's "approved open
  license" indicator expects to see for non-software content (see
  `DPG-STANDARD-MAPPING.md`, indicator 2).

Every file in the repo falls under one or the other; `README.md` documents
the header convention used to make this unambiguous file-by-file.

## Why the leak-guard exists at the architecture level (not just as a CI afterthought)

Because this repo's payload is *governance content*, the single most
dangerous failure mode is a maintainer accidentally committing real
organization data (a real EIN, a real board member's name, a signed PDF)
while drafting or de-branding example material. That data would then be
in the CI history of a **public** repository forever. `.gitleaks.toml` and
`.gitleaks-denylist.txt`, enforced by `.github/workflows/leak-guard.yml`
on every push and PR, exist to make that class of mistake fail loudly and
immediately rather than silently ship. This is treated as core
architecture, not bolt-on tooling — see `CONTRIBUTING.md` for the
contributor-facing contract.
