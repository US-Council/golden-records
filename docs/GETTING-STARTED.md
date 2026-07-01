# Getting Started

An end-to-end walkthrough for a nonprofit board, executive director, or
operations lead rendering their **own** governance-as-code repository from
the Golden Records commons for the first time.

**This is not legal advice.** Nothing in this walkthrough, or in the
repository it produces, should be treated as adopted until your board — and,
where appropriate, your counsel — has reviewed it. See
[`docs/NOT-LEGAL-ADVICE.md`](NOT-LEGAL-ADVICE.md) before you rely on
anything you render below.

If you want the conceptual picture first — what the commons is, how the
draft→adopt governance model works, what stays in your private repo versus
what came from upstream — read [`docs/ADOPTER-GUIDE.md`](ADOPTER-GUIDE.md)
alongside or before this document. This guide is the mechanics; that one is
the "why."

## Before you start

Have ready:

- Your organization's **full legal name**, exactly as filed with the IRS
  and your state.
- Your **EIN** (format `NN-NNNNNNN`).
- Your **state of incorporation**.
- Your **principal office address**, as filed.
- A **primary contact email** for governance and security matters.
- A decision on which platform your board will use for governance-as-code:
  **GitHub or GitLab** (`vcs_platform` — see Step 2). You can pick either
  regardless of which platform hosts the *commons* itself; see
  [`docs/FAQ.md`](FAQ.md#does-my-organization-have-to-use-github-because-the-commons-is-on-github).

You do **not** need bylaws, a board roster, or any policy content ready yet
— those are things you fill in or customize *after* rendering, not
questions Copier asks you.

## Step 1 — Install Copier

Golden Records is consumed via [Copier](https://copier.readthedocs.io/), a
template-rendering CLI. This commons targets `_min_copier_version: "9.3.0"`
(set in [`copier.yml`](../copier.yml)); any current 9.x release satisfies
that. Install it with either:

```bash
# pipx (recommended if you don't already manage Python tools with uv)
pipx install copier

# uv (recommended if you already have uv installed)
uv tool install copier
```

Either method gives you an isolated, up-to-date `copier` command without
polluting a project's Python environment. Confirm the install:

```bash
copier --version
```

## Step 2 — Render your copy

From wherever you want your new governance repository to live locally:

```bash
copier copy --trust gh:USCouncil/golden-records ./our-records
```

- `--trust` tells Copier you trust this template source. Golden Records
  doesn't ship Copier tasks or migrations, but the question schema does use
  Jinja expressions (computed defaults, validators) — `--trust` is the
  documented, unambiguous way to allow that on every current Copier
  release, and it's the form used throughout this repo's own docs.
- `gh:USCouncil/golden-records` is Copier's shorthand for
  `https://github.com/USCouncil/golden-records.git`. Copier checks out the
  latest semver git tag by default, so you get the latest **released**
  version of the commons, not whatever is on `main` at the moment (see
  `docs/UPGRADE-GUIDE.md` for pinning vs. tracking).
- `./our-records` is the destination — it will be created if it doesn't
  exist. Use whatever path or repository name your organization prefers;
  nothing about the render depends on this specific name.

Copier will print the `_message_before_copy` banner from `copier.yml`
(a reminder of what to have ready and that this isn't legal advice), then
walk you through every question in the schema, one at a time, showing each
question's help text and validating your answer as you go.

### The question schema, group by group

The full, authoritative list — with exact help text, defaults, and
validators — is [`copier.yml`](../copier.yml). What follows is a guide to
what each group is asking and why, so you're not answering blind.

**Organization identity** (8 questions)

| Variable | What it's asking | Notes |
|---|---|---|
| `org_name` | Full legal name, exactly as filed | Required. This is the string substituted everywhere your organization's name appears in the rendered output. |
| `org_short` | Short name/acronym | Defaults to an acronym auto-derived from `org_name`'s initials (e.g. "Rivertown Community Trust" → `RCT`) — accept the default or override it. |
| `ein` | IRS EIN, `NN-NNNNNNN` | Required; validated by regex. Copier will re-prompt if the format doesn't match. |
| `state` | State (or DC) of incorporation | A fixed choice list of all 50 states + DC. |
| `incorp_statute` | The nonprofit-incorporation statute citation used in boilerplate | Auto-fills for Ohio, Delaware, California, New York, and Texas; for any other state, type the citation yourself (e.g. "`[State] Nonprofit Corporation Act`"). |
| `charter_no` | State charter/registration number | Optional — leave blank if not yet issued. |
| `principal_address` | Principal office address as filed | Required. |
| `registered_agent_name` / `registered_agent_type` | Registered agent, if you have one, and whether it's a person or a service entity | Optional — leave the name blank if not yet designated. |

**Fiscal & tax-exempt status** (3 questions)

| Variable | What it's asking | Notes |
|---|---|---|
| `fiscal_year_end` | e.g. "December 31" or "June 30" | Defaults to "December 31." |
| `irs_determination_date` | Date of your 501(c)(3) determination letter | Optional — leave blank if pending. |
| `public_charity_classification` | Your public-charity classification under the IRC | Defaults to the common `509(a)(1)/170(b)(1)(A)(vi)` classification; change it if yours differs (e.g. a supporting organization or private foundation). |

**Board & governance** (3 questions)

| Variable | What it's asking | Notes |
|---|---|---|
| `board_size` | Number of governing-board seats | Defaults to 2 — almost every organization should raise this to its real board size; it drives the quorum-count math shown in your rendered `GOVERNANCE.md`. |
| `board_quorum_rule` | `majority`, `two-thirds`, or `custom` | Feeds directly into the quorum language and director-count arithmetic in `GOVERNANCE.md`. Pick `custom` if your bylaws use a rule that doesn't map to the other two, and expect to hand-edit the resulting placeholder language. |
| `treasurer_title` | Title of your financial-oversight officer | Defaults to "Treasurer"; change to "CFO," "VP of Finance," etc. if that's your organization's actual title. |

**Operating posture** (4 questions)

| Variable | What it's asking | Notes |
|---|---|---|
| `vcs_platform` | `gitlab` or `github` | Determines which CI workflow, PR/MR template, and terminology ("pull request" vs. "merge request") is rendered throughout. See `docs/FAQ.md` for the cross-platform governance-parity question. |
| `ai_agents_enabled` | Does your org use AI agents in its workflow? | Toggles AI-governance policy content (`policies/ai-governance-board.md`, `policies/ai-responsible-use-research.md`) and the AI-agent rules section of `GOVERNANCE.md`/`CLAUDE.md`. Defaults to `true`. |
| `federal_grants_focus` | Does your org pursue/manage US federal grants? | Toggles a substantial block of 2 CFR 200 / FAR compliance policies and templates — 28 files in the current template. Defaults to `true` — if your org has no federal funding, set this to `false` to avoid rendering compliance scaffolding you don't need. |
| `include_bylaws_example` | Include a fully worked example bylaws document? | Defaults to `false` (only a skeleton/checklist). See `docs/ADOPTER-GUIDE.md` for why this defaults off. |

**Contact** (2 questions)

| Variable | What it's asking | Notes |
|---|---|---|
| `primary_contact_email` | Primary governance/security contact | Required; validated as a well-formed email address. |
| `org_website` | Organization website URL | Optional. |

When you're done, Copier prints the `_message_after_copy` banner — the
same "review before you commit, read the guides, `copier update` later"
reminder repeated at the end of this walkthrough.

## Step 3 — What gets rendered

Copier writes a complete repository to your destination path. The shape
depends on your answers (`federal_grants_focus`, `ai_agents_enabled`,
`include_bylaws_example`, and `vcs_platform` all add or remove files), but
every render includes at minimum:

```
our-records/
├── .copier-answers.yml         records your answers + template version — see UPGRADE-GUIDE.md; never hand-edit
├── README.md                   public-facing description of this repository
├── GOVERNANCE.md               the full governance model, parameterized with your board size/quorum/platform
├── CLAUDE.md                   AI agent operating rules for this repository
├── COPYRIGHT.md                copyright notice + template provenance statement
├── board-roster.md / .yml      your board of directors (human- and machine-readable) — starts empty, fill it in
├── POLICY-INDEX.md             index of every rendered policy
├── policies/                   adopted-policy content, parameterized with your org's data
├── templates/                  board-approved document templates and forms
├── minutes/, resolutions/, voting/, agreements/, compliance/
│                                empty (with README.md placeholders) — these fill up as your board acts
├── .gitleaks.toml              leak-guard config scoped to YOUR repo (secrets only — see docs/SECURITY.md in your render)
└── .github/  or  .gitlab/      CI workflow + governance-change PR/MR template, matching your vcs_platform choice
```

If `include_bylaws_example: true`, you'll also get `bylaws.md` — a
generic, fully worked (but non-binding) example. If `false` (the default),
your board adds its own adopted bylaws directly at that path.

**Every file is now yours.** Nothing about the rendered output phones home
to USCouncil or the commons — Copier ran locally, and `_message_before_copy`
already told you that. Customize freely.

## Step 4 — Commit and initialize your repository

Turn the rendered output into a real repository:

```bash
cd our-records
git init
git add .
git commit -m "Initial render from Golden Records commons"
```

Then create the actual GitHub or GitLab repository (matching the
`vcs_platform` you chose in Step 2) and push. **This repository should be
private.** Unlike the commons — which is deliberately public and contains
zero real organization data — your rendered repository is going to
accumulate your real EIN, real board member names, real minutes, and real
signed agreements. Set repository visibility to private at creation.

## Step 5 — Adopt your governance model

Rendering the repository does not, by itself, adopt anything. Two things
still need to happen before `GOVERNANCE.md` is more than a description of
a process you intend to follow:

1. **Populate `board-roster.md` / `board-roster.yml`** with your actual
   directors and officers, and grant them write access on your
   `vcs_platform` project under their real identities — `GOVERNANCE.md`
   requires directors to use real, verified identities on governance
   pull/merge requests, not anonymous or shared accounts.
2. **Configure branch protection** for `main`, and for the `board/*` and
   `admin/*` branch-prefix pattern, per
   [`docs/CI-AND-BRANCH-PROTECTION.md`](CI-AND-BRANCH-PROTECTION.md). This
   is a manual step in your GitHub or GitLab project settings — Copier
   cannot configure it for you, and it's the platform enforcement layer
   that makes "no direct pushes to `main`" and "quorum approval on
   `board/` branches" actually stick rather than being process discipline
   alone.

Only after both are done does your first real `board/` pull/merge request
— for example, one that adds your bylaws and formally adopts
`GOVERNANCE.md` itself — function the way the document describes: a motion,
a vote via approval, and an adoption via merge.

## Step 6 — Have your board (and counsel) ratify

Everything up to this point is mechanical. Before treating any rendered
document as your organization's actual governance:

- Have your board formally review and adopt `GOVERNANCE.md`,
  `board-roster.md`, and every policy you intend to rely on — via the
  `board/` pull/merge request process the document itself describes.
- Have counsel licensed in your state review anything with legal
  consequence: bylaws, resolutions, and any policy touching your 501(c)(3)
  status or federal compliance obligations.
- Do not treat a clean `copier copy`, a passing leak-guard CI check, or
  the mere existence of a file in this repository as a substitute for that
  review. See [`docs/NOT-LEGAL-ADVICE.md`](NOT-LEGAL-ADVICE.md).

## Next steps

- **Understand the model in depth:** [`docs/ADOPTER-GUIDE.md`](ADOPTER-GUIDE.md).
- **Stay current with the commons:** [`docs/UPGRADE-GUIDE.md`](UPGRADE-GUIDE.md)
  — read this *before* your first `copier update`, especially the
  dirty-working-tree gotcha.
- **Contribute an improvement back:** [`docs/CONTRIBUTING-POLICIES.md`](CONTRIBUTING-POLICIES.md).
- **Common questions:** [`docs/FAQ.md`](FAQ.md).
