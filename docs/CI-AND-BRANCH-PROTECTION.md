# CI and Branch Protection

This document covers two distinct things — do not conflate them:

1. **The commons repo** (`USCouncil/golden-records`, this repository) — how
   *it* should be configured to keep the public template free of real
   organization data.
2. **An adopter's rendered repo** — how *they* should wire up the
   leak-guard shipped into `template/` (see `ARCHITECTURE.md` and
   `SECURITY.md`) as a required check, and how the `board/` / `admin/`
   branch-prefix governance model in `GOVERNANCE.md` maps onto branch
   protection settings.

Branch protection and required status checks are **repository/project
settings on GitHub or GitLab**, not something Copier or a CI workflow file
can configure for you. They must be set manually (or via each platform's
own Terraform/API tooling, which is out of scope here) by someone with
admin rights on the repository. This document tells you *what* to set;
it is not itself the mechanism that sets it.

## 1. The commons repo (`USCouncil/golden-records`)

This is a public repository whose core invariant is **zero real
organization-specific data** (see `CONTRIBUTING.md`, `SECURITY.md`). The
GitHub settings below exist to make violating that invariant impossible to
merge, not just detectable after the fact.

In the repository's **Settings → Branches → Branch protection rules**,
add a rule for `main`:

- **Require a pull request before merging.** Disables direct pushes to
  `main` for everyone, including admins if "Include administrators" is
  also enabled (recommended).
- **Require status checks to pass before merging**, with both jobs from
  `.github/workflows/leak-guard.yml` selected as required:
  - `gitleaks pattern scan`
  - `literal org-identifier denylist scan`
- **Require branches to be up to date before merging**, so the checks
  above run against the actual merge result, not a stale base.
- **Require review from Code Owners.** `CODEOWNERS` already scopes
  mandatory review onto `copier.yml`, `.gitleaks.toml`,
  `.gitleaks-denylist.txt`, and `.github/workflows/` — the exact paths
  where a leak or a weakened safety net would do the most damage.
- **Do not allow force pushes. Do not allow deletions.** Preserves the
  audit trail of what was reviewed and merged.

These are manual GitHub organization/repository settings, not something
committed to this repo as code — there is no GitHub-native way to express
branch protection as a version-controlled file at the time of writing.
Whoever provisions the `USCouncil` GitHub organization (see the Milestone 0
report referenced in `DPG-STANDARD-MAPPING.md`, indicator 3) should apply
this configuration as part of that setup, and re-apply it if the repository
is ever transferred or recreated.

## 2. An adopter's rendered repo

Once an organization runs `copier copy` and renders their own repository
(see `ADOPTER-GUIDE.md`), they get a leak-guard scoped to *their* needs:
gitleaks' default secret-detection ruleset, with no org-identifier rules
and no denylist, because their real EIN, director names, and address
legitimately belong in their own private repo (see the header comment in
their rendered `.github/workflows/leak-guard.yml` or `.gitlab-ci.yml`, and
`SECURITY.md`).

### Wiring the leak-guard as a required check

**GitHub adopters** (`vcs_platform: github`): in **Settings → Branches**,
add a protection rule for `main` and require the `gitleaks secret scan
(default rules)` status check (from the rendered
`.github/workflows/leak-guard.yml`) before merging.

**GitLab adopters** (`vcs_platform: gitlab`): in **Settings → Repository →
Protected branches**, protect `main`. Then, in **Settings → Merge
requests**, enable **"Pipelines must succeed"** so the `leak-guard` job
(from the rendered `.gitlab-ci.yml`) blocks merge on failure. GitLab
evaluates pipeline success at the merge-request level rather than via a
named required-check list, so there is no separate job name to select —
enabling the setting is sufficient as long as `leak-guard` is the only (or
a required) stage in the pipeline.

### Mapping `board/` / `admin/` branch governance onto branch protection

`GOVERNANCE.md` (as rendered) defines two branch prefixes with different
approval requirements:

| Prefix | Approval required | Review period |
|--------|-------------------|---------------|
| `board/` | Director quorum (per your `board_quorum_rule` — majority, two-thirds, or a custom threshold) | The period set by your bylaws (commonly 7 days), waivable only by unanimous written consent for documented emergencies |
| `admin/` | 1 officer (Chair, Executive Director, Secretary, or equivalent, with authority over the subject matter under the Delegation of Authority Policy) | None — may merge as soon as the required officer approves |

Neither GitHub nor GitLab natively enforces "N directors out of a
board of size X" as a distinct rule from "N reviewers" — both platforms
only know about required-approver *counts*, not board-specific quorum
semantics or *which* individuals hold a board seat this quarter. The
practical mapping is:

**GitHub:**
- Protect `main` as described in the adopter section above.
- Directors and officers are GitHub org members with write access;
  `CODEOWNERS` in the rendered repo can route review requests, but GitHub
  Code Owners approval is a single "did a code owner approve" gate, not a
  quorum count. To approximate quorum, set **"Require approvals"** to the
  numeric quorum threshold (e.g. the `board_quorum_count` value shown in
  your rendered `GOVERNANCE.md`) on the branch protection rule, and rely on
  the Secretary (per `GOVERNANCE.md`'s document-owner assignment) to verify
  by inspection that the specific approvers were directors then serving,
  not merely any N reviewers — GitHub cannot verify board membership for
  you.
- There is no native way to give `admin/` a lower approval count than
  `board/` on the *same* branch (`main`) via GitHub's per-branch rule,
  since both prefixes merge into the same protected `main`. If your
  organization wants the officer-only fast path for `admin/` enforced by
  the platform (not just by process discipline and PR review), consider
  GitHub's **rulesets** (Settings → Rules → Rulesets), which support
  branch-name-pattern-scoped rules — e.g. a ruleset targeting
  `refs/heads/admin/**` merges with 1 required approval, and a separate
  ruleset targeting `refs/heads/board/**` merges (or `main` itself) with
  the quorum count.

**GitLab:**
- Protect `main` in **Settings → Repository → Protected branches**, and
  additionally protect the `board/*` and `admin/*` branch patterns.
  GitLab's protected-branch UI accepts wildcard patterns directly, so this
  does not require a separate "rulesets" feature the way GitHub does.
- Under **Settings → Merge requests → Merge checks**, set **"All threads
  must be resolved"** and require the leak-guard pipeline to succeed (see
  above).
- Approval *counts* per protected pattern are configured via **Settings →
  Merge requests → Approval rules**, which supports scoping a rule to a
  specific protected branch (or wildcard). Create one rule requiring the
  quorum count of approvals, scoped to `board/*` merge targets is not
  directly expressible (GitLab approval rules apply to the *target*
  branch, i.e. `main`, not the source branch pattern) — so as with GitHub,
  platform-level enforcement is strongest at the `main` protection level,
  and the `board/` vs. `admin/` distinction for the *source* branch is
  enforced by process (the branch-naming convention itself, self-review
  discipline, and the Secretary's oversight role from `GOVERNANCE.md`)
  more than by a platform primitive. GitLab Premium/Ultimate tiers offer
  more granular approval-rule scoping (e.g. code owner sections per
  pattern) if your organization has access to them.

**Bottom line:** both platforms can reliably enforce "N approvals and a
passing leak-guard pipeline before merging into `main`." Neither platform
natively distinguishes "this approver is a director" from "this approver
is any repo collaborator," and neither cleanly gives a *lower* bar to
`admin/` branches merging into the same protected `main` without GitHub
rulesets or GitLab's higher-tier approval scoping. Where the platform
can't fully automate the distinction, `GOVERNANCE.md`'s process rules
(who is authorized to approve which prefix, and the Secretary's review
responsibility) remain the enforcement layer — the same way they would in
any board's paper-based governance before this repo existed.
