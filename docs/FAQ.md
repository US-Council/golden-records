# Frequently Asked Questions

## Adoption

### Does my organization have to use GitHub, because the commons is on GitHub?

No. `US-Council/golden-records` (the commons) happens to be hosted on
GitHub, but the `vcs_platform` question in `copier.yml` lets you render
your own copy for **GitHub or GitLab**, independent of where the commons
itself lives. Set `vcs_platform: gitlab` and Copier renders
`.gitlab-ci.yml` and a `.gitlab/merge_request_templates/governance-change.md`
instead of `.github/workflows/leak-guard.yml` and a GitHub PR template;
your rendered `GOVERNANCE.md` and `README.md` also swap every "pull
request" for "merge request" and every "PR" for "MR" throughout, using
Jinja logic keyed off that one variable (see the top of
`template/GOVERNANCE.md.jinja` for exactly how). This is deliberate
platform independence — see `docs/ARCHITECTURE.md` and
`docs/DPG-STANDARD-MAPPING.md` (indicator 4).

You can even fetch the commons from GitHub (`gh:US-Council/golden-records`)
while rendering a `vcs_platform: gitlab` copy — the source platform and
your rendered target platform are unrelated. An adopter whose organization
runs its own infrastructure on GitLab internally simply sets
`vcs_platform: gitlab` and gets a repository that matches their own
tooling and terminology, while still pulling the template from GitHub.

### What if my organization uses neither GitHub nor GitLab?

`vcs_platform` currently only supports `gitlab` and `github` — those are
the two platforms with native, well-documented branch-protection and
PR/MR-approval primitives that `docs/CI-AND-BRANCH-PROTECTION.md` maps the
governance model onto. If your organization uses something else (e.g.
Bitbucket, a self-hosted Gitea instance), you can still render with
whichever of the two choices is the closer analog and adapt the CI/PR
template files by hand afterward, or open an issue proposing a third
`vcs_platform` choice if there's enough shared need to justify maintaining
it in the commons.

### Do I need bylaws, a board roster, or policies ready before I run `copier copy`?

No. The question schema (`copier.yml`) only asks for identity, fiscal, and
board-structure facts (legal name, EIN, state, board size, and similar) —
see `docs/GETTING-STARTED.md` for the full walkthrough. Bylaws, board
roster entries, and policy adoption all happen *after* rendering, through
your own board's `board/`-branch pull/merge request process.

### What does `include_bylaws_example` actually give me if I set it to `true`?

A generic, non-binding, fully worked example bylaws document
(`bylaws.md`), useful as a drafting aid or a "what does a complete document
look like" reference. It is not legal advice and is not your
organization's actual bylaws merely because it exists in your rendered
repo — see `docs/NOT-LEGAL-ADVICE.md`. It defaults to `false` precisely
because bylaws are the document most likely to be mistaken for "already
done." See `docs/ADOPTER-GUIDE.md` for the fuller reasoning.

### Can I change my answers after I've already rendered — e.g., we outgrew `board_size: 2`?

Yes, two ways. The lower-friction one: hand-edit the rendered files that
reference board size directly (mainly `GOVERNANCE.md`'s quorum-count
language and `board-roster.yml`) — Copier doesn't require you to re-run
anything for a simple data change. The more thorough one: hand-editing the
`board_size` value in `.copier-answers.yml` itself is **not** recommended
(that file is Copier's own bookkeeping, and editing it by hand can
desynchronize `copier update`'s 3-way merge base); instead run
`copier update --trust` on a clean tree without `-A/--skip-answered` or
`--defaults`, and Copier re-prompts every question, showing your current
answer as the default for each — press Enter to keep it, or type a new
`board_size`. That regenerates the board-size-dependent text consistently
across every file that uses it (not just the ones you remembered to check
by hand), through the same 3-way merge described in
`docs/UPGRADE-GUIDE.md`, even if the template version itself hasn't
changed.

## Federal grants and AI governance

### What does `federal_grants_focus: true` actually add?

A substantial block of content — 28 files in the current template —
covering 2 CFR 200 / FAR compliance: policies like cost-sharing,
export-control, suspension-and-debarment, and pre-award spending, plus
templates like the CUI system security plan and federal award disclosure
report. If your organization has no federal funding and no plans to pursue
it, set this to `false` and skip the compliance scaffolding entirely — you
can always re-render or `copier update` into it later if that changes,
since it's a normal Copier variable, not a one-time irreversible choice.

### What does `ai_agents_enabled: true` change?

It renders `policies/ai-governance-board.md` and
`policies/ai-responsible-use-research.md`, and adds an AI-agent-specific
clause to `GOVERNANCE.md`'s "Who Can Propose Changes" section: AI agents
may draft pull/merge requests and prepare supporting material, but a human
director or officer must be the named author of record or explicit
endorser — agents don't hold votes and can't adopt governance actions
unattended. Your rendered `CLAUDE.md` carries the detailed operating rules
for any agent working in your repository.

## Staying current

### Is `copier update` mandatory?

No. Rendering is a one-time snapshot unless you choose to stay connected.
Many adopters will render once, customize heavily, and never run
`copier update` again — that's a valid choice, not a broken one. The
tradeoff is that you won't automatically benefit from later commons
improvements (clarified language, new compliance content, bug fixes in
boilerplate). See `docs/UPGRADE-GUIDE.md` for what you're opting into if
you do stay connected, including the dirty-tree requirement and the
diff3-style conflict markers Copier uses when your customization and an
upstream change touch the same lines.

### What Copier version do I need?

`copier.yml` sets `_min_copier_version: "9.3.0"`. Any current Copier 9.x
release satisfies that — install with `pipx install copier` or
`uv tool install copier` and confirm with `copier --version`. See
`docs/GETTING-STARTED.md` Step 1.

## Contributing back

### I fixed a typo/wording issue in my own rendered repo. How do I get that fix into the commons?

See [`docs/CONTRIBUTING-POLICIES.md`](CONTRIBUTING-POLICIES.md) for the
full pathway. In short: you re-express the fix in the corresponding
`template/*.jinja` file (using Copier variables, not your organization's
literal data), not by copy-pasting your rendered file — the commons must
never contain real organization data, even accidentally, from any adopter.

### Can I contribute a whole new policy my organization wrote?

Yes, if it's original content (or content you have the rights to
contribute under the commons' licensing — see
`CONTRIBUTING.md`/`README.md`) and generic enough to benefit other
adopters, not specific to your organization's unique circumstances. Open
an issue first if you're unsure whether it's a good fit — see
`docs/CONTRIBUTING-POLICIES.md`.

## Legal

### Is any of this legal advice?

No — see `docs/NOT-LEGAL-ADVICE.md` for the full statement. The short
version: this commons gives you a strong, structured starting point, not a
legal opinion about what your specific organization needs. Have your board
and, where appropriate, your counsel review every document before treating
it as adopted.

### Does using this template create any relationship with US-Council or the commons maintainers?

No attorney-client relationship, no ongoing obligation, and no data
relationship — `copier copy` runs entirely on your own machine, and
nothing you enter is transmitted to US-Council or any third party. The only
ongoing connection, if you choose it, is the optional `copier update`
mechanism pulling template improvements you explicitly request.

## Getting help

Still stuck? Open an issue on `US-Council/golden-records` for questions
about the commons itself, or see `SECURITY.md` for security/data-leak
concerns that shouldn't be discussed in a public issue. For questions
about your own rendered repository's specific legal or tax situation,
consult your own counsel/CPA — not the commons maintainers.
