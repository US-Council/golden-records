# Upgrade Guide

> **Status: skeleton.** This guide is scaffolded in Milestone 0 before
> `template/` contains real policy/governance content, so it cannot yet
> walk through a concrete before/after upgrade diff. The mechanism and
> the critical gotcha below are accurate and complete now; worked examples
> are **TODO (Phase 7)**.

## What `copier update` does

When the `golden-records` commons template improves — a policy is
clarified, a compliance checklist is added, boilerplate language is fixed —
adopters pull those improvements into their already-rendered repo with:

```
copier update --trust
```

This is **not** a re-render from scratch. Copier performs a **3-way merge**:

1. It looks at your repo's `.copier-answers.yml` (or whatever
   `_copier_conf.answers_file` was set to) to find out which template
   version you last updated from, and what answers you gave.
2. It re-renders the template at that *old* version with your saved
   answers — this is the "base."
3. It renders the template at the *new* version with the same answers —
   this is the "theirs" side.
4. It diffs base → theirs, and applies that diff on top of your actual
   current files ("yours," which may include manual edits) — a standard
   3-way merge, same concept as a git merge.

If you kept a rendered file exactly as generated, this update is usually
clean. If you (or your counsel) hand-edited a rendered file, Copier will
try to merge the upstream change around your edits, same as git would.

## ⚠️ The dirty-tree gotcha

**`copier update` requires a clean git working tree to run safely**, because
the 3-way merge relies on git's own merge machinery. If you have
uncommitted changes when you run `copier update`, you will get an error
(or, worse, an update that's hard to reason about because it's unclear
which changes are "yours" vs. uncommitted noise).

**Before every `copier update`:**

```
git status              # confirm working tree is clean
git commit -am "..."    # commit any pending local changes first
copier update --trust
```

**After the update:**

```
git status               # review what changed
git diff                 # review the actual diff
```

Look specifically for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) in
any file you'd previously customized — Copier will insert these when it
can't cleanly reconcile your edit with the upstream change, exactly like a
failed git merge. Resolve them manually before committing.

**Do not run `copier update` on a dirty tree "just to see what happens."**
If the merge goes sideways on a dirty tree, you can end up with local
edits, upstream changes, and merge artifacts tangled together with no
clean way to tell them apart. Commit first, always.

## Pinning vs. tracking

Adopters can either:

- **Pin to a specific semver tag** (e.g. `v0.2.0`) and upgrade deliberately
  by running `copier update --trust` and choosing to move to a new tag when
  ready. This is the recommended default — predictable, reviewable
  upgrades.
- **Track `main`** for the latest unreleased changes. Not recommended for
  most adopters; only for those actively co-developing the commons.

**TODO (Phase 7):** Add a worked example showing a real 3-way merge on a
populated `template/` file (e.g. a policy document), including what a
conflict-marker resolution looks like in practice.

## Getting help

If an update produces a merge result you don't understand, open an issue on
`US-Council/golden-records` with your Copier version (`copier --version`),
the template version you're updating from/to, and the specific file where
the merge looks wrong.
